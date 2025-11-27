## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).
---

## **Response to Weakness 4:**

We sincerely appreciate this important observation. In the revised manuscript, we have added both empirical and theoretical justifications for our adaptive mixing strategy design:

**1. Theoretical Motivation:**

We now provide SNR-based theoretical justification **(added to Section 3.1 and Appendix H.6 "Theoretical Motivation" paragraph)** grounded in established diffusion model literature. Reverse diffusion exhibits a **monotonically increasing signal-to-noise ratio (SNR)** as time decreases [19]. The linear schedule $w(t) = t$ provides the simplest monotonic interpolation that aligns with this SNR progression. It favors stochastic exploration at high-noise timesteps and deterministic exploitation when the model's predictions become more reliable. This behavior is consistent with standard practice in diffusion models, where **linear noise schedules** remain widely adopted defaults due to their stability and simplicity.

**2. Comprehensive Empirical Validation:**

As the original submission already presented, **Appendix H.6** evaluates seven candidate schedules (linear, quadratic, cosine, exponential, square-root, constant, noise-free) and multiple switching rules across TSP-50/100/500. The **linear schedule with deterministic switching** consistently provides the best balance between solution quality, stability, and failure rate.

**3. Instance-Aware Strategies:**

We acknowledge that using instance-aware strategies is a good idea. However, such methods require additional per-instance tuning, reducing robustness and reproducibility. For clarity and consistency, and given that the existing schedule already delivers reliable, satisfying results across various scenarios, we adopt the linear + deterministic strategy as our default configuration. 

---

## **Response to Weakness 5:**

We sincerely appreciate this suggestion. To directly address this question, we have added a comprehensive ablation study in the revised manuscript (**Table 3**) that systematically removes combinations of EDISCO's three key components. In addition to our previous ablation study, which removed one component at a time, we now added **three more combinations** that keep only **one component at a time**: **(EGNN only, continuous-time diffusion only, and adaptive mixing strategy only)**. We have also chosen the **vanilla DIFUSCO** as the equivalent baseline. 

This design enables us to construct "DIFUSCO + EGNN" (the **EGNN Only** setting in **Table 3**) and isolate the architectural contribution of E(2)-equivariance:

| Model Variant | TSP-500 Gap | TSP-1000 Gap |
|---------------|-------------|--------------|
| **EDISCO (Full)** | **1.95%** | **2.85%** |
| w/o Mix Strategy | 2.44% | 3.41% |
| w/o Continuous-Time | 2.86% | 4.28% |
| w/o EGNN | 5.71% | 7.49% |
| EGNN Only | 3.58% | 5.06% |
| Continuous Only | 7.09% | 9.27% |
| Mix Only | 6.42% | 8.52% |
| Vanilla DIFUSCO | 9.41% | 11.24% |

"DIFUSCO + EGNN" achieves 3.58% & 5.06% gaps on TSP-500/1000, demonstrating **2.63× & 2.22×** improvements over vanilla DIFUSCO (9.41% & 11.24%). This isolates the architectural contribution of E(2)-equivariance.

---

## With the above answers, we welcome any further questions regarding this manuscript.