---
title: "An Introduction to LLVM"
description: "Summary of the original LLVM paper"
date: 2025-12-13
weight: 1
series: ["LLVM"]
series_order: 1
---

If you have ever considered compiler development as a profession, you have definitiely come face to face with a line in the job description that goes something like this

{{< alert cardColor="#E7A74A" textColor="#ffffff" iconColor="#ffffff" >}}Must have Low Level Virtual Machine (LLVM) experience{{< /alert >}}

Naturally, the next course of action is either (1) start farming or (2) learn LLVM. Blogpost on the former is in the works, meanwhile you may find some entertainment in exploring the latter through this article.

## The Beginning

LLVM was introduced in the paper ["LLVM: A Compilation Framework for Lifelong Program Analysis & Transformation"](https://dl.acm.org/doi/abs/10.5555/977395.977673) in 2004. The paper argues for program analysis at various stages of a program's lifetime. These analyses allows for 4 classes of optimizations
- _Interprocedural_: Those performed at link time. Eg: static debugging, static leak detection, etc.
- _Machine-Dependent_: Those performed at install time. Eg: program safety analyses
- _Dynamic_: Those performed at runtime
- _Profile-Guided_: Those performed between runs
Lifelong program analyses allows allows architects to have greater freedom to push hardware forward without having to worry about legacy applications being able to run on new systems. 
    
LLVM achieves this through an RISC-like instruction set based intermediate representation that is high-level enough for a whole class of optimizations and low level enough for being able to represent arbitrary programs and it should be of a format that other languages can lower to. While there is no formal proof for generality the paper argues that the IR is only slightly enriched in comparison to standard assembly. This intermediate representation presents the following novel features
- Type system that can be used to implement data types and associated operations
- Instructions for performing type conversions and address arithemetic
- Two exception handling instructions

LLVM is complementary to high-level virtual machines (like Java's JVM and .NET CLR) in that it does not support high-level constructs like classes. Additionally, its a more low-level register based VM that abstracts the underlying hardware whereas the high-level VMs are actually runtimes for respective codes.

LLVM is unique in that it provides the following benefits combined whereas other compilers tradeoff one or many
- Program analysis information is persistent and available throughout program lifetime
- Supports offline code generation for performance critical applications
- User based profiling and optimization
- Transparent runtime model which allows any language to be compiled 
- Uniform, whole program optimization

In comparison we can consider the following systems:
- Source-level compiler: Convert from one source language to another
- Interprocedural Optimizer: Optimizers that work at link time
- Profile guided optimizers
- High level virtual machines such as JVM and CLI
- Binary runtime optimization

### Instruction Set

- LLVM creates an abstraction over physical machine, so details like physical registers, pipelines
- LLVM is a load store architecture
- virtual registers are in SSA form (meaning that each register is written to once, this works because we have an infinite virtual registers)
  - there is a phi instruction
  - ssa allows for easy dataflow analysis
- 31 op codes
  - most opcodes are overloaded, add works on float and int
  - most instructions are in three address form
- Control flow graphs
  - Funciton in LLVM is a set of basic blocks 
  - each block is a sequence of llvm instructions
    - each block ends with one terminator (branches, return, unwind, invoke)
- provides instructions for typed memory allocations (malloc for reserving memory of specific type on the heap, alloca to allocate on the stack frame)
- Language independent type system
  - Each register has a type
  - supported primitve types - void, bool, signed/unsigned int of 8-64 bits and single- and double- precision floating point
  - Four derived types: pointer, array, structures, functions
  - supports cast instruction to convert a valye of one type to another
  - the system preserves type information during pointer arithemetic by using `getelementptr` instruction for loads which explicitly states the type that is being loaded from the pointer location
    ```
        <result-register> = getelementptr <type>* <mem-pointer-register>
    ```
  - In LLVM all memory operations occur through typed pointers, since there are no implicit access to memory, there is no need for address of operator
  - the call isntruction acts on a function pointer and typed arguments
  - Exceptions in LLVM are handled through invoke and unwind. Invoke works similar to a call, with an additional specification of a basic block that will be executed in the event of an exception. The unwind starts popping stackframes till an invoke block is encountered.
    - This approach allows for the exact logic for exception handling to come from the source language during lowering and LLVM just provides primitives to represent that without having an implicit mechanism of its own to deal with exceptions

### LLVM System Architecture
- User defined static compiler frontend converts source language to LLVM representation, it is expected that during this convertion users perform source language specific optimizations there itself as LLVM will not preserve source langauge semantics to exploit certain optimization opportunities
- The linker then performs a series of link time optimization. The code is then compiled down to a binary and the LLVM representation is saved alongside it (it is also possible for the code to be JIT'ed). The binary contains instrumentation to extract profile data which can be used to optimizr at runtime

### Tutorials

Now that we have discussed the basics of LLVM, the best way to understand some of these concepts is to build a small compiler in it. Over the next few posts in this series I details the steps in building a full-fledged compiler using LLVM.