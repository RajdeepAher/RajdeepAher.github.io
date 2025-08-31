## Report


### Aim

The aim of the project was to create a comprehensive set of benchmarks to evaluate how long various libraries take to execute and to determine whether performance is improving or slowing down over time. The broader goal was to implement a cohesive benchmarking framework that not only measures and records performance but also identifies bottlenecks for improvement. Additionally, we aimed to develop benchmarks that allow comparisons with other quantum software packages.

### Upon Conclusion

We now have a new repository, [`toqito-bench`](https://github.com/vprusso/toqito-bench), which contains benchmarks for **57 functions across 3 libraries**. A complete methodology has also been developed for the MATLAB-based library **QETLAB**. While it does not yet include the full set of benchmarks, the methodology provides a clear path for future contributions.

This work resulted in **21 pull requests** opened in `toqito-bench` and **7 pull requests** in `toqito` focusing on performance improvements. By carefully selecting functions that the entire library depended on, we achieved significant speedups in **6 core functions**.

There are still several future goals I would like to continue contributing to. What I enjoyed most during this project was working on performance improvements. Although I wish I had been able to dedicate more time to optimization, I am glad that, being open source, the project allows me to continue contributing even after the official program ends.



### Weekly Blogs
0. [Introduction and Community Bonding Period](https://rajdeepaher.github.io/2025/05/27/GSoC-Community-Bonding.html)  
1. [Week 1](https://rajdeepaher.github.io/2025/06/10/GSoC-Week-1.html)  
2. [Week 2](https://rajdeepaher.github.io/2025/06/19/GSoC-Week-2.html)  
3. [Week 3](https://rajdeepaher.github.io/2025/06/25/GSoC-Week-3.html)  
4. [Week 4](https://rajdeepaher.github.io/2025/07/03/GSoC-Week-4.html)  
5. [Week 5](https://rajdeepaher.github.io/2025/07/09/GSoC-Week-5.html)  
6. [Week 6](https://rajdeepaher.github.io/2025/07/18/GSoC-Week-6.html)  
7. [Week 7](https://rajdeepaher.github.io/2025/07/24/GSoC-Week-7.html)  
8. [Week 8](https://rajdeepaher.github.io/2025/07/29/GSoC-Week-8.html)  
9. [Week 9](https://rajdeepaher.github.io/2025/08/08/GSoC-Week-9.html)  
10. [Week 10](https://rajdeepaher.github.io/2025/08/12/GSoC-Week-10.html)  
11. [Week 11](https://rajdeepaher.github.io/2025/08/19/GSoC-Week-11.html)  
12. [Week 12](https://rajdeepaher.github.io/2025/08/24/GSoC-Week-12.html)  

### Logs of PR

| Pull Request | Description | Date | Status |
|--------------|-------------|------|--------|
| https://github.com/vprusso/toqito-bench/pull/9 | Initializing toqito-bench with basic structure and CI stub | 03/06/2025 | Closed and Merged |
| https://github.com/vprusso/toqito-bench/pull/11| Initialize environments in `python`, `julia` and `MATLAB`| 04/06/2025 | Closed and Merged | 
| https://github.com/vprusso/toqito-bench/pull/12 | Adding instructions on benchmark ID-ing| 30/06/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/13 | Adding `Makefile` and benchmark running instructions | 02/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/15 | Adding first set of benchmarks for `ketjl`| 05/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/14 | Adding first set of benchmarks for `toqito` and `QuTIpy`| 09/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/16 | Adding benchmark for `MATLAB`| 12/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/18 | Add `ruff` into the project | 18/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/19 | Add second set of benchmarks for `toqito` and `QuTIpy`| 22/07/2025 | Closed and Merged|
| https://github.com/vprusso/toqito-bench/pull/20 | Adding second set of benchmarks for `ketjl` | 26/07/2025 | Closed and Merged |
| https://github.com/vprusso/toqito-bench/pull/21 | Combining benchmark results | 08/08/2025 | Closed and Merged|
| https://github.com/vprusso/toqito/pull/1304 | optimising `toqito.states.bell`| 13/08/2025 | Closed and Merged |
| https://github.com/vprusso/toqito/pull/1305 | optimising `toqito.states.basis` | 14/08/2025 | Closed and Merged |
| https://github.com/vprusso/toqito/pull/1306 | optimising `toqito.states.max_entangled` | 15/08/2025 | Closed and Merged |
| https://github.com/vprusso/toqito/pull/1307 | optimising `toqito.channels.depolarizing_channel`| 17/08/2025 | Closed and Merged|
| https://github.com/vprusso/toqito/pull/1309 | optimising `toqito.perms.permute_systems`| 18/08/2025 | Draft|
| https://github.com/vprusso/toqito/pull/1310 | optimising `toqito.perms.swap`| 20/08/2025| Open |
| https://github.com/vprusso/toqito/pull/1311 | optimising `toqito.matrix_ops.partial_trace`| 21/08/2025 | Open|
| https://github.com/vprusso/toqito-bench/pull/22 | Index of all the functions benchmarked | 21/08/2025 |Closed and Merged|
|https://github.com/vprusso/toqito-bench/pull/23 | Adding a baseline run of all benchmarks to compare against| 23/08/2025| Closed and Merged|
|https://github.com/vprusso/toqito-bench/pull/24| Created a new branch as `toqito` imports were modified recently| 23/08/2025| Closed and Merged|
| https://github.com/vprusso/toqito/pull/1312 | regression analysis in `toqito` | 23/08/2025 | Open |



## Future Goals

1. We should prioritize writing down optimizations for functions based on how frequently they are imported within the repository.  
   The current status is as follows:

   | Function | Import Count | Optimization Status |
   |----------|--------------|----------------------|
   | `toqito.states.bell` | 30 | Done |
   | `toqito.states.basis` | 22 | Done |
   | `toqito.states.max_entangled` | 16 | Done |
   | `toqito.matrix_ops.tensor` | 16 | Done |
   | `toqito.matrix_props.is_square` | 16 | Not Useful |
   | `toqito.matrix_props.is_density` | 15 | Not Useful |
   | `toqito.matrix_props.is_positive_semidefinite` | 14 | Done |
   | `toqito.channels.depolarizing` | 12 | Done |
   | `toqito.perms.swap` | 11 | Done |
   | `toqito.perms.swap_operator` | 11 | Done |
   | `toqito.channels.partial_trace` | 10 | Done |
   | `toqito.matrix_ops.to_density_matrix` | 10 | Not Useful |
   | `toqito.perms.permute_systems` | 8 | Pending |
   | `toqito.channel_ops.kraus_to_choi` | 8 | Pending |
   | `toqito.rand.random_unitary` | 8 | Pending |
   | `toqito.matrices.pauli` | 7 | Pending |
   | `toqito.perms.permutation_operator` | 7 | Pending |
   | `toqito.channel_ops.apply_channel` | 6 | Pending |
   | `toqito.states.trine` | 6 | Pending |
   | `toqito.channels.partial_transpose` | 5 | Pending |
   | `toqito.channels.dephasing` | 5 | Pending |

2. Replace the existing `.md` documentation with proper [Read the Docs](https://readthedocs.org/) style documentation. Since functions already contain docstrings, this should integrate smoothly with Sphinx or similar tools.

3. The current [combine.yml](https://github.com/vprusso/toqito-bench/blob/master/.github/workflows/combine.yml) workflow does not run reliably. This needs debugging and fixes.

4. Introduce histogram-based visualizations to better interpret raw benchmark data. We could also run the regression-analysis workflow fortnightly and store generated plots for tracking performance trends over time.

5. While we have initial plan for `QETLAB` benchmarking laid down, I feel it does need improvements as running `MATLAB` in CI is cumbersome and does not feel scalable for benchmarking.

6. We may consider adding benchmarks for other libraries such as `Yao.jl` or `pennylane` in future.

