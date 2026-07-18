---
title: "TVM: An End-to-End Deep Learning Compiler System"
description: "A short review of the TVM paper by Chen et al."
date: 2026-07-13
---

**Paper:** *TVM: An End-to-End Deep Learning Compiler System* (OSDI 2018)  
**Authors:** Tianqi Chen, Thierry Moreau, Ziheng Jiang, Lianmin Zheng, Eddie Yan, Meghan Cowan, Haichen Shen, Leyuan Wang, Yuwei Hu, Luis Ceze, Carlos Guestrin, and Arvind Krishnamurthy

### Summary

TVM is a graph compiler for machine learning. It takes an input machine learning graph and performs graph level optimizations such as operator fusion and data layout optimizations. It then attempts to lower the fused operators down to backend schedules without relying on the need for handcrafter fused kernels. The backend schedules are essentially general abstractions that have been exposed for common hardware transformations like loop ordering, tiling, tensorization etc. The TVM paper also talks about using an ML (XGBoost) based cost model to infer hardware properties which can then be used for auto tuning. The search space exploration is performed using simulated annealing
