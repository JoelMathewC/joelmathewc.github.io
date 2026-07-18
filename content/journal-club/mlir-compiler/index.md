---
title: "MLIR: A Compiler Infrastructure for the End of Moore's Law"
description: "A short review of the MLIR compiler infrastructure paper by Lattner et al."
date: 2026-07-12
---

**Paper:** *MLIR: A Compiler Infrastructure for the End of Moore's Law*
**Authors:** Chris Lattner, Mehdi Amini, Uday Bondhugula, Albert Cohen, Andy Davis, Jacques Pienaar, River Riddle, Tatiana Shpeisman, Nicolas Vasilache, and Oleksandr Zinenko

### Summary

MLIR is a compiler framework that is built to address several limitations of LLVM. LLVM was a compiler framework with an IR that mimicked assembly with types. However, the authors of the MLIR paper (who also were developers of LLVM) argue that most compilers use multiple IRs with different transformations performed at different stages of the IR. LLVM unfortunately provides only a single IR and doesn't provide any scope for customization to represent something like a machine learning graph. MLIR is built on the concept of dialects. A dialect is essentially an IR and MLIR allows you to define multiple IRs. While frameworks like LLVM were built for CPUs and could not adapt to heterogenous architectures, MLIR can work with different types of representation and compile to various architectures. 