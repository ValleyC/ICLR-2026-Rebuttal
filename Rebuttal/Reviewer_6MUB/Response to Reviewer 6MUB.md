## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).

---

## **Response to Weakness 1:**

We thank the reviewer for this comment. However, we would like to ask if there is a misunderstanding or an oversight. **Table 1** (TSP-50/100) compares EDISCO with neural methods using **Concorde** as the exact baseline. we do not include LKH-3 in Table 1. **LKH-3** appears only in **Table 2** (TSP-500/1000), where it achieves 0.00% gap on both TSP-500 and TSP-1000.

Could the reviewer kindly clarify which specific table or result appears incorrect? We would be happy to investigate further and provide additional verification if needed.

Please see **Table 1** (TSP-50/100 without LKH-3) and **Table 2** (TSP-500/1000 with LKH-3).

---

## **Response to Weakness 2:**

We sincerely appreciate this important observation regarding hyperparameter sensitivity. We have strengthened the manuscript by adding a dedicated discussion paragraph addressing this concern.

We would like to clarify in three aspects:

**1. Optimal settings found and robust:** While we documented sensitivity in **Appendix H.7** for transparency, we identified optimal settings (α = 0.1, τ = 0.1) that work robustly across all problem sizes (TSP-50/100/500/1000, CVRP-20/50/100, ESTP-10/20/50) without per-problem tuning.

**2. Comparable to other deep learning methods:** Hyperparameter sensitivity is inherent to deep learning. Neural CO methods require tuning learning rates, network architectures, attention heads, embedding dimensions, etc. EDISCO's architectural stabilizers are analogous to batch normalization or layer normalization parameters in standard architectures. Although they are sensitive during initial design, they are stable once configured.

**3. Strong generalization validates robustness:** Our cross-distribution evaluation (Appendix H.3: Cross-Distribution Generalization) demonstrates that these settings generalize well to out-of-distribution instances (Cluster, Explosion, Implosion), showing the hyperparameters are not narrowly tuned to specific distributions.

We have added a **"Discussion on Hyperparameter Robustness"** paragraph at the end of **Appendix H.7** that explicitly addresses the coordinate collapse phenomenon when α >= 0.2 and contextualizes the sensitivity within standard deep learning practice. The thorough sensitivity analysis demonstrates our commitment to transparency and provides practitioners with clear guidance for implementation.

Please see **Appendix H.7** (complete sensitivity analysis with new discussion paragraph) and **Appendix H.3** (cross-distribution robustness validation).

---

## **Response to Weakness 3:**

We thank the reviewer for this suggestion. We have substantially strengthened the manuscript by adding both **stronger baselines** and **expanded Related Work discussions**.

**Baselines Added:**

We have included **Fast-T2T** [3] and **BQ-NCO** [4] as new baselines in our experimental results:

- **Fast-T2T**: Added to **Table 1** (TSP-50/100), **Table 2** (TSP-500/1000), **Appendix H.3: Cross-Distribution Generalization** (evaluation across Uniform/Cluster/Explosion/Implosion distributions), and **Appendix H.4: TSPLIB Evaluation**.

- **BQ-NCO**: Added to **Table 1** (TSP-100) and **Table 2** (TSP-500/1000), demonstrating its bisimulation quotienting approach for generalization.

**EDISCO maintains state-of-the-art performance** across all benchmarks, demonstrating the effectiveness of combining E(2)-equivariance with continuous-time diffusion.

**Related Work Expanded:**

We have significantly expanded our Related Work coverage in both **Section 2 (main text)** and **Extended Related Work (appendix)**, discussing 10+ recent methods including COExpander, DiffUCO, iSCO, RLSA, VAG-CO, GFlowNets, PO, BOPO, UTSP, UniCO, and GOAL. These additions clarify EDISCO's unique contribution—combining exact E(2)-equivariance with continuous-time categorical diffusion—and distinguish it from alternative paradigms (discrete-time diffusion, unsupervised methods, generalist solvers).

Please see the revised **Tables 1-2**, **Appendix H.3-H.4**, and **Section 2 (Related Work)** for detailed comparisons.