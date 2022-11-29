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
To run benchmarks, use the `run` executable in the `build` directory. 

```ESH
./build/example --helpshort
Usage: 
    ./build/example [OPTION...]
Optoins are similar to others like Dash do.
```
Check out also the `run.sh` script for example benchmarks and easy testing of the hash tables. 

## Example program

To know how to integrate the Dash into your application, check out `example.cpp` under `src`.
The executable is `example` under your build directory. 
Also check `CMakeLists.txt` to know how to link with dependencies (customized PMDK and epoch manager) for correct build as per the recommendations of Dash. 

## Miscellaneous

Bugs on testing environment: can be solved in the same way other applications that use PMDK are solved.

## Contact

For any questions, please contact us at `xx@snu.ac.kr` and `yy@snu.ac.kr`.
