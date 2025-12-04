# Summarizing some improvements
## Improvements in `toqito.states.max_entangled`

`toqito`'s this module is concerned only with producing a maximally entangled bipartite pure state. The speciality of a maximally entangled state is that when measuring system A determines the state of system B.

### The original implementation of `toqito.states.max_entangled` in `toqito`

```python
def max_entangled_v1(dim: int, is_sparse: bool = False, is_normalized: bool = True) -> [np.ndarray, dia_array]:
    mat = eye_array(dim) if is_sparse else np.identity(dim)
    psi = np.reshape(mat, (dim**2, 1))
    if is_normalized:
        psi = psi / np.sqrt(dim)
    return psi
```
1. It first creates an $I_{d}$ identity matrix which can be written as $\sum_{i = 0}^{d - 1} \vert i\rangle \langle i \vert$

2. Next, we reshape $I_{d}$ into a vector of size $d^{2} \times 1$ (in row-major order) which effectively constructs $\sum_{i=0}^{d-1} \vert i\rangle \otimes \vert i \rangle$

3. We finally normalize to construct $\frac{1}{\sqrt{d}}\sum_{i=0}^{d-1} \vert i\rangle \otimes \vert i\rangle$



### The issues in original`toqito.states.max_entangled`

```python
def max_entangled_v1(dim: int, is_sparse: bool = False, is_normalized: bool = True) -> [np.ndarray, dia_array]:
    mat = eye_array(dim) if is_sparse else np.identity(dim)
    psi = np.reshape(mat, (dim**2, 1))
    if is_normalized:
        psi = psi / np.sqrt(dim)
    return psi

```
- Even when the goal is a vector, the function first constructs a full identity matrix (dense or sparse) and then reshapes it into a column vector. For large `dim`, this means allocating and initializing `dim × dim` memory, which is wasteful when only dim non-zero elements are needed.

- Similarly, the sparse branch uses `scipy.sparse.eye_array(dim)`, which still internally builds an identity structure (for matrices) before reshaping, instead of directly creating the sparse vector.

- Normalization is applied to the entire vector after construction. This requires going through all `dim^2` elements and is unnecessary

### Progressive improvements attempted

1. 
```python
def max_entangled_v2(dim: int, is_sparse: bool = False, is_normalized: bool = True) -> [np.ndarray, csr_array]:
    
    if is_sparse:
        indices = np.arange(dim) * (dim + 1)  # Diagonal indices in flattened form.
        data = np.ones(dim)
        psi = csr_array((data, (indices, np.zeros(dim, dtype=int))), shape=(dim**2, 1))
    else:
        # For dense: directly create vector with 1s at diagonal positions.
        psi = np.zeros((dim**2, 1))
        psi[::dim + 1] = 1  # Set diagonal elements to 1 using stride indexing.
        
        return np.divide(psi, np.sqrt(dim)) if is_normalized else psi
```

- Directly compute the diagonal positions (flattened index: `np.arange(dim) * (dim + 1)`) instead of creating an identity matrix and reshaping.
- For sparse: build a CSR sparse vector with only dim non-zero entries.
- For dense: use stride indexing `(psi[::dim+1] = 1)` to set diagonal entries directly.

This helps in avoiding the  `O(dim^2)` identity matrix creation.

2. 

```python
def max_entangled_v3(dim: int, is_sparse: bool = False, is_normalized: bool = True) -> [np.ndarray, csr_array]:
    
    norm_factor = 1 / np.sqrt(dim) if is_normalized else 1.0
    
    if is_sparse:
        # Diagonal positions in flattened form
        indices = np.arange(dim) * (dim + 1)
        data = np.full(dim, norm_factor)
        psi = csr_array((data, (indices, np.zeros(dim, dtype=int))), shape=(dim**2, 1))
    else:
        psi = np.zeros((dim**2, 1))
        psi[::dim + 1] = norm_factor
    
    return psi
```
- Apply normalization factor during construction rather than post-hoc. This helps in avoiding an extra pass of vectors for scaling.

3. 
```python
def max_entangled_v4(dim: int, is_sparse: bool = False, is_normalized: bool = True):
    
    norm_factor = 1 / np.sqrt(dim) if is_normalized else 1.0
    idx = np.arange(dim) * (dim + 1)

    if is_sparse:
        data = np.full(dim, norm_factor)
        psi = coo_array((data, (idx, np.zeros(dim))), shape=(dim**2, 1))
        return psi
    psi = np.zeros((dim**2, 1))
    psi[::dim + 1] = norm_factor
    return psi
```
- Use a single indexing array `(idx)` for both sparse and dense cases.
- Sparse: directly construct using `coo_array` from `(data, (idx, zeros))`.
- Dense: allocate zero array and set only the required `dim` entries.

### Profiling Results Summary

#### Test Setup:
- Dimensions tested: `4, 16, 64, 256, 1024`
- `1000` iterations per dimension
- Separate profiling for `is_sparse=True` and `is_sparse=False`
- Timing collected using `line_profiler` with per-line breakdown.

`is_sparse = True` 
Version | Total Time (s) | Speedup vs v1
-- | -- | --
v4 | 0.207 | 5.29×
v1 (original) | 1.096 | 1.00×
v3 | 2.671 | 0.41×
v2 | 3.402 | 0.32×

`is_sparse = False`

Version | Total Time (s) | Speedup vs v1
-- | -- | --
v3 | 0.228 | 3.49×
v4 | 0.239 | 3.33×
v2 | 0.736 | 1.08×
v1 (original) | 0.797 | 1.00×


### Final Solution

- For `dense` output `v3` is more or less equal to `v4` and `v4` is the clear best for `is_sparse = True`. Hence we choose our function such that, when `is_sparse=False` the `v3` branch is chosen and `v4` when `is_sparse= True`:

```python
def max_entangled_v5(dim: int, is_sparse: bool = False, is_normalized: bool = True) -> [np.ndarray, coo_array]:

    norm_factor = 1 / np.sqrt(dim) if is_normalized else 1.0
    idx = np.arange(dim) * (dim + 1)  # positions of nonzero entries in flattened form.

    if is_sparse:
        # Construct sparse vector directly.
        data = np.full(dim, norm_factor)
        psi = coo_array((data, (idx, np.zeros(dim))), shape=(dim**2, 1))
        return psi

    psi = np.zeros((dim**2, 1), dtype=float)
    psi[idx, 0] = norm_factor
    return psi
```

## Improvements in `toqito.channels.depolarizing`





 
