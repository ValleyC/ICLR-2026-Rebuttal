## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).

## **Response to Weakness 2:**

We sincerely appreciate the reviewer for highlighting these important recent related works. We have substantially strengthened our manuscript by adding additional **baselines** and **discussions** of these methods.

**1. Baselines Added:**

We have included **Fast-T2T** and **BQ-NCO** [8] as new baselines in our experimental results:

- **Fast-T2T:** Added to **Table 1** (TSP-50/100), **Table 2** (TSP-500/1000), **Appendix H3: Cross-Distribution Generalization** (Evaluation across Uniform/Cluster/Explosion/Implosion distributions of TSP), and **Appendix H4: TSPLIB Evaluation**.

- **BQ-NCO:** Added to **Table 1** (TSP-100) and **Table 2** (TSP-500/1000), demonstrating its bisimulation quotienting approach.

**EDISCO achieves state-of-the-art performance** across all benchmarks:

| Method | TSP-500 Gap | TSP-1000 Gap |
|--------|-------------|--------------|
| DIFUSCO | 9.41% | 11.24% |
| T2T | 5.09% | 8.87% |
| Fast-T2T | 5.94% | 6.29% |
| BQ-NCO | 5.22% | 8.97% |
| **EDISCO (Ours)** | **1.95%** | **2.85%** |

Please see the revised manuscript for detailed numerical comparisons.

**2. Discussions Added:**

We have significantly expanded our Related Work coverage in both the **Related Works (Section 2 in main text)** and **Extended Related Work in the appendix**:

1. **Diffusion-based methods (Section 2.1):** We now discuss **COExpander** [9]'s adaptive expansion method that scales to 10K nodes, **DiffUCO** [10]'s unsupervised continuous-time diffusion for graph problems. It further proves that EDISCO is the first to combine E(2)-equivariance with continuous-time categorical diffusion for geometric optimization.

2. **Sampling and variational approaches (Section 2.3):** We added discussions of **iSCO** [11]'s MCMC sampling, **RLSA** [12]'s regularized Langevin dynamics, **VAG-CO** [13]'s variational annealing, and **GFlowNets** [14]'s flow-based generative models. We clarify that these methods target non-geometric graph problems.

3. **RL methods (Appendix - Foundational NCO subsection):** We discuss **PO** [4] and **BOPO** [15] as recent RL training innovations using preference-based optimization, clarifying that these are improved training algorithms.

4. **Unsupervised and alternative paradigms (Section 2.1 & Appendix):** We added **UTSP** [16]'s data-efficient unsupervised learning method.

5. **Generalist approaches (Appendix - Alternative Architectures subsection):** We discuss **UniCO** [17]'s problem reduction strategy and **GOAL** [18]'s multi-task generalist model.

**These Additional Related Works Strengthened EDISCO's Contributions:**

EDISCO's unique contribution—**combining exact E(2)-equivariance with continuous-time categorical diffusion**—is clearly distinguished from:
- Diffusion methods lacking geometric awareness (DiffUCO, DISCO)
- Discrete-time diffusion formulations (DIFUSCO, T2T)
- Alternative paradigms (COExpander's expansion, RL-based methods)
- Unsupervised approaches (UTSP, DiffUCO)
- Generalist solvers sacrificing specialized geometric inductive bias (UniCO, GOAL)

Please see the **revised manuscript** for detailed comparisons in **Section 2 (Related Work)**, **Tables 1-2**, **Appendix: Cross-Distribution Generalization**, **Appendix: TSPLIB Evaluation**, and **Extended Related Work**.

---

## **Response to Weakness 3:**

We sincerely appreciate the reviewer for these valuable suggestions.

**1. Cross-Distribution Evaluation Added:**

We have included extensive **out-of-distribution (OOD) experiments** following the evaluation protocol established by GLOP [2]: the **Uniform (training baseline)**, **Cluster**, **Explosion**, and **Implosion** are evaluated on TSP-100. EDISCO achieves the lowest average performance drop across all distributions:

| Method | Uniform | Cluster | Explosion | Implosion | Avg Gap | Avg Det. |
|--------|---------|---------|-----------|-----------|---------|----------|
| DIFUSCO | 1.01% | 2.87% | 1.38% | 2.80% | 2.02% | 132.7% |
| T2T | 0.18% | 1.50% | 0.15% | 2.60% | 1.11% | 687.0% |
| Fast-T2T | 0.06% | 1.18% | 0.03% | 2.50% | 0.94% | 1960.6% |
| GLOP | 0.09% | 0.17% | 0.07% | 0.08% | 0.10% | 15.0% |
| **EDISCO** | **0.04%** | **0.05%** | **0.03%** | **0.05%** | **0.04%** | **4.2%** |

Please see **Appendix H3: Cross-Distribution Generalization** (complete table and analysis).

**2. ATSP Clarification:**

As detailed in our **Response to Weakness 1 (Section 4)**, **EDISCO does not support ATSP** due to an **architectural limitation**: EDISCO's E(2)-equivariant EGNN fundamentally **requires Euclidean coordinates** to preserve geometric symmetries, which is incompatible with ATSP's coordinate-free **arbitrary asymmetric cost matrices**.

We have also identified **promising future directions**: (1) replacing the encoder with ATSP-capable architectures (MatNet [5], GREAT [6]), or (2) learning coordinate embeddings from asymmetric cost matrices using Finsler Multi-Dimensional Scaling [7] or neural metric learning to enable approximate E(2)-equivariance for ATSP instances with near-geometric structure.

Please see **Response to Weakness 1 (Section 4)** and **Appendix A: Scope and Limitations** for detailed discussion.
