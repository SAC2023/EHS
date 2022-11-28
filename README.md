# EHS: Efficient Hashing Scheme for Persistent Memory 

This repository is for development of a hashing scheme for Persistent memory.


## Contents inside:

It contains the ESH implementation
and the Dash codes which shares the base code for implementation for Persistent memory.

Fully open-sourced under MIT license.

## Building

### Dependencies
We tested our build with Linux Kernel 5.4.9-47-generic and GCC 9.0. The Linux kernel versions recommended tobe newer than 4.17 and glibc >=2.29 to easily connect the codes to the migrated codes from Dash since both implementation use `MAP_FIXED_NOREPLACE` in the customized PMDK. 
