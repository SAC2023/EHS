# EHS: Efficient Hashing Scheme for Persistent Memory

This repository is for development of a hashing scheme for Persistent memory.

## Contents inside:

It contains the ESH implementation and the Dash codes which shares the base code for implementation for Persistent memory.

Fully open-sourced under MIT license.


## Building

### Dependencies
We tested our build with Linux Kernel 5.4.9-47-generic and GCC 9.0. The Linux kernel versions recommended tobe newer than 4.17 and glibc >=2.29 to easily connect the codes to the migrated codes from Dash since both implementation use MAP_FIXED_NOREPLACE in the customized PMDK. 

To get all external dependencies for Dash and CCEH, you can refer to the [customized PMDK](https://github.com/XiangpengHao/pmdk), [Dash](https://github.com/baotonglu/dash), [CCEH](https://github.com/DICL/CCEH), and [epoch manager](https://github.com/XiangpengHao/VeryPM), which are also open-sourced. 

### Compiling
This compliation is done under a `build` directory of the same source::
```EHS
git clone https://github.com/SAC2023/EHS.git
cd ESH
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release -DUSE_PMEM=ON .. 
make -j
```

## Running benchmark

As stated in our paper, we run the tests in a single NUMA node with 24 physical CPU cores. We pin threads to physical cores compactly assuming thread ID == core ID (e.g., for a dual-socket system, we assume cores 0-23 are located in socket 0, and cores 24-47 in socket 1).  To run benchmarks, use the `test_pmem` executable in the `build` directory. It supports the following arguments:

```bash
./build/test_pmem --helpshort
Usage: 
    ./build/test_pmem [OPTION...]

-index      the index to evaluate:dash-ex/dash-lh/cceh/level (default: "dash-ex")
-op         the type of operation to execute:insert/pos/neg/delete/mixed (default: "full")
-n          the number of warm-up workload (default: 0)
-p          the number of operations(insert/search/delete) to execute (default: 20000000)
-t          the number of concurrent threads (default: 1)
-r          search ratio for mixed workload: 0.0~1.0 (default: 1.0)
-s          insert ratio for mixed workload: 0.0~1.0 (default: 0.0)
-d          delete ratio for mixed workload: 0.0~1.0 (default: 0.0)
-e          whether to register epoch in application level: 0/1 (default: 0)
-k          the type of stored keys: fixed/variable (default: "fixed")
-vl         the length of the variable length key (default: 16)
```
Check out also the `run.sh` script for example benchmarks and easy testing of the hash tables. 

## Example program

To know how to integrate the Dash into your application, check out `example.cpp` under `src`.
The executable is `example` under your build directory. 
Also check `CMakeLists.txt` to know how to link with dependencies (customized PMDK and epoch manager) for correct build. 

## Miscellaneous

We noticed a possible `mmap` bug on our testing environment: `MAP_SHARED_VALIDATE` is incompatible with `MAP_FIXED_NOREPLACE` (since Linux 4.17).
To ensure safe memory mapping, we modified the original PMDK to use `MAP_SHARED` rather than `MAP_SHARED_VALIDATE`, which has the same functionality as the former one except for extra flag validation.
For a more detailed explanation and minimal reproducible code, please check out our [blog post](https://blog.haoxp.xyz/posts/mmap-bug/) about this issue.

## Contact

For any questions, please contact us at `btlu@cse.cuhk.edu.hk` and `tzwang@sfu.ca`.
