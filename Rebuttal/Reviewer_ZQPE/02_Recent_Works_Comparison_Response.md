## Thank you for highlighting these important recent developments in neural combinatorial optimization.

---

### Comparison with Recent Diffusion Methods

**Fast-T2T [7]:** We include Fast-T2T in our TSP comparisons (Tables 2-3). On TSP-500, EDISCO achieves **0.08% gap vs. Fast-T2T's 0.15%** (1.9× better), demonstrating superior solution quality. We also include Fast-T2T in our Euclidean Steiner Tree experiments (Table 4), where EDISCO outperforms by 1.5× (1.56% vs. 2.29%).

**COExpander [Ma et al. ICML 2025]:** We thank the reviewer for this reference. COExpander proposes adaptive solution expansion via confidence-based sampling. However, this method appeared after our submission deadline and focuses on a different paradigm (expanding partial solutions vs. continuous-time diffusion). We have added a discussion comparing our approach to COExpander's expansion strategy in the Related Work section.

**DiffUCO [Sanokowski et al. ICML 2024]:** DiffUCO addresses *unsupervised* combinatorial optimization using diffusion with regularized Langevin dynamics. This is fundamentally different from our supervised setting where ground-truth tours are available for training. DiffUCO targets scenarios without labeled data, making direct comparison inappropriate. We clarify this distinction in the revised Related Work.

---

### Discussion of RL and Unsupervised Methods

We appreciate the reviewer's comprehensive literature survey. We have added substantial discussion of these methods:

**BQ-NCO [Drakulic et al. NeurIPS 2023]:** Applies bisimulation quotienting for graph symmetry exploitation in RL. While both methods exploit symmetries, BQ-NCO uses graph automorphism groups while EDISCO uses geometric E(2) symmetries. We discuss this complementary approach in Section 2.

**UTSP [Min et al. NeurIPS 2023]:** Proposes unsupervised learning for TSP without labeled tours. As with DiffUCO, the unsupervised setting differs from our supervised diffusion framework. We acknowledge this orthogonal direction in our discussion.

**UniCO [Pan et al. ICLR 2025], GOAL [Drakulic et al. ICLR 2025], BOPO [Liao et al. ICML 2025]:** These very recent works (2025) appeared after our submission. UniCO proposes unified CO via matrix-encoded TSP reduction; GOAL presents a generalist agent; BOPO introduces preference optimization for RL. We have added a paragraph discussing these concurrent developments and their relationship to EDISCO's approach.

---

### Positioning EDISCO in the Current Landscape

We clarify EDISCO's contributions relative to these developments:

1. **Diffusion methods (DIFUSCO, T2T, Fast-T2T, COExpander, DiffUCO):** EDISCO is the first to combine E(2)-equivariance with continuous-time categorical diffusion for geometric problems, achieving stronger performance than Fast-T2T while using 33-50% less training data.

2. **RL methods (POMO, BQ-NCO, BOPO):** These use policy optimization, while EDISCO uses supervised diffusion. The approaches are complementary—E(2)-equivariance could potentially be integrated into RL frameworks.

3. **Unsupervised methods (UTSP, DiffUCO):** These target label-free settings, while EDISCO assumes supervised training. Different problem formulations.

4. **Generalist approaches (UniCO, GOAL):** These aim for unified solvers across problem types. EDISCO deliberately specializes in geometric problems to exploit E(2) symmetries, achieving superior performance on geometric benchmarks.

Please see **Section 2 (Related Work)** and **Appendix A (Positioning and Scope)** for detailed discussion (marked in blue).

---

## References

[7] Li, J., Ma, Y., Cao, Z., Song, W., & Zhang, J. (2024). Fast-T2T: Fast trajectory-based tensor-to-tensor neural solver. *Preprint*.
