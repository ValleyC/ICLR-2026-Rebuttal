## **Response to Weakness 2:**

We sincerely appreciate the reviewer for highlighting these important recent related works. We have substantially strengthened our manuscript by adding additional **baselines** and **discussions** of these methods.

**1. Baselines Added:**

We have included **Fast-T2T** [3] and **BQ-NCO** [15] as new baselines in our experimental results:

- **Fast-T2T:** Added to **Table 1** (TSP-50/100), **Table 2** (TSP-500/1000), **Appendix: Cross-Distribution Generalization** (Evaluation across Uniform/Cluster/Explosion/Implosion distributions of TSP), and **Appendix: TSPLIB Evaluation**.

- **BQ-NCO:** Added to **Table 1** (TSP-100) and **Table 2** (TSP-500/1000), demonstrating its bisimulation quotienting approach.

**EDISCO maintains state-of-the-art performance** across all benchmarks. Please see the revised manuscript for detailed numerical comparisons.

**2. Discussions Added:**

We have significantly expanded our Related Work coverage in both the **Related Works (Section 2 in main text)** and **Extended Related Work in the appendix**:

1. **Diffusion-based methods (Section 2.1):** We now discuss **COExpander** [4]'s adaptive expansion method that scales to 10K nodes, **DiffUCO** [5]'s unsupervised continuous-time diffusion for graph problems. It further proves that EDISCO is the first to combine E(2)-equivariance with continuous-time categorical diffusion for geometric optimization.

2. **Sampling and variational approaches (Section 2.3):** We added discussions of **iSCO** [8]'s MCMC sampling, **RLSA** [9]'s regularized Langevin dynamics, **VAG-CO** [10]'s variational annealing, and **GFlowNets** [11]'s flow-based generative models. We clarify that these methods target non-geometric graph problems.

3. **RL methods (Appendix - Foundational NCO subsection):** We discuss **PO** [16] and **BOPO** [17] as recent RL training innovations using preference-based optimization, clarifying that these are improved training algorithms.

4. **Unsupervised and alternative paradigms (Section 2.1 & Appendix):** We added **UTSP** [18]'s data-efficient unsupervised learning method.

5. **Generalist approaches (Appendix - Alternative Architectures subsection):** We discuss **UniCO** [19]'s problem reduction strategy and **GOAL** [20]'s multi-task generalist model.

**These Additional Related Works Strengthened EDISCO's Contributions:**

EDISCO's unique contribution—**combining exact E(2)-equivariance with continuous-time categorical diffusion**—is clearly distinguished from:
- Diffusion methods lacking geometric awareness (DiffUCO, DISCO)
- Discrete-time diffusion formulations (DIFUSCO, T2T)
- Alternative paradigms (COExpander's expansion, RL-based methods)
- Unsupervised approaches (UTSP, DiffUCO)
- Generalist solvers sacrificing specialized geometric inductive bias (UniCO, GOAL)

Please see the **revised manuscript** for detailed comparisons in **Section 2 (Related Work)**, **Tables 1-2**, **Appendix: Cross-Distribution Generalization**, **Appendix: TSPLIB Evaluation**, and **Extended Related Work**.
