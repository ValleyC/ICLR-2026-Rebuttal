## We would like to thank the reviewer for the constructive and insightful comments. We have carefully revised the manuscript to address your concerns (changes marked in blue).

---

## **Response to Weakness 1:**
We sincerely appreciate this important observation. We have included extensive **out-of-distribution (OOD) experiments** in **Appendix H3**, following the evaluation protocol established by GLOP [1]: the **Uniform (training baseline)**, **Cluster**, **Explosion**, and **Implosion** are evaluated on TSP-100. EDISCO achieves the lowest average performance drop across all distributions:

| Method | Uniform | Cluster | Explosion | Implosion | Avg Gap | Avg Det. |
|--------|---------|---------|-----------|-----------|---------|----------|
| DIFUSCO | 1.01% | 2.87% | 1.38% | 2.80% | 2.02% | 132.7% |
| T2T | 0.18% | 1.50% | 0.15% | 2.60% | 1.11% | 687.0% |
| Fast-T2T | 0.06% | 1.18% | 0.03% | 2.50% | 0.94% | 1960.6% |
| GLOP | 0.09% | 0.17% | 0.07% | 0.08% | 0.10% | 15.0% |
| **EDISCO** | **0.04%** | **0.05%** | **0.03%** | **0.05%** | **0.04%** | **4.2%** |

Regarding CVRPLIB, our CVRP scope (20-100 customers) is positioned as a proof-of-concept for architectural extensibility to constrained routing, but not competing with large-scale specialists. Notably, competing diffusion methods (DIFUSCO [2], T2T [3], Fast-T2T [4]) have no reported CVRP experiments, highlighting the difficulty of scaling single end-to-end diffusion models. We have actively discussed future directions in **Appendix A** in the revised manuscript for applying the E(2) Equivariance to partition-based strategies similar to GLOP [1] to scale to large-scale instances (1000+ customers).

---

## **Response to Weakness 2:**
We sincerely thank the reviewer for this comment. We have now revised the CVRP experiments in **Appendix C** of the revised manuscript. We have included **HGS** [5], the leading CVRP heuristic, with reported optimal solutions and complete runtime information for CVRP-20/50/100:

| Method | Type | CVRP-20 Gap | CVRP-50 Gap | CVRP-100 Gap |
|--------|------|-------------|-------------|--------------|
| HGS | Heuristic | 0.00% (1h) | 0.00% (3h) | 0.00% (5h) |
| AM | RL+G | 4.97% | 5.86% | 7.34% |
| POMO | RL+G | 3.72% | 3.52% | 3.09% |
| **EDISCO** | **SL+G** | **1.41%** | **2.46%** | **3.17%** |

We acknowledge that HGS achieves superior solution quality compared to neural methods. However, HGS requires substantially longer computation time (often hours for medium-sized instances), and is typically used in the neural CVRP literature as a ground truth or reference solution rather than a competing real-time solver (such as PO [6]).

---

## **Response to Weakness 3:**
We thank the reviewer again for this insightful observation.

We clarify that **EDISCO does not support ATSP** due to an **architectural limitation**: EDISCO's E(2)-equivariant EGNN fundamentally **requires Euclidean coordinates** to preserve geometric symmetries, which is incompatible with ATSP's **arbitrary asymmetric cost matrices**.

This limitation is shared by all coordinate-based TSP solvers (AM [7], POMO [8], Pointer Networks [9], DIFUSCO [4], T2T [5], Fast-T2T [6]). Current ATSP solvers (MatNet [10], GREAT [11]) incorporate directional edge information, fundamentally incompatible with E(2)-equivariant coordinate encoders.

However, we believe that this might not be a weakness. We have added **Appendix A: Scope and Limitations** in the revised manuscript, which clarifies that EDISCO is specialized for Euclidean geometric problems. We have also actively discussed two **promising future directions** in **Appendix A**:

1. **Replace the encoder**: Adopt ATSP-capable architectures (MatNet [10], GREAT [11]) while retaining continuous-time diffusion.

2. **Learn coordinate embeddings**: Transform asymmetric cost matrices into approximate coordinate representations using **Finsler Multi-Dimensional Scaling** [12] (extends MDS to handle asymmetric dissimilarities via Finsler spaces) or neural metric learning to enable approximate E(2)-equivariance for ATSP instances with near-geometric structure.

---

## **Response to Question 1:**
We have added complete runtime information to the CVRP experimental results in **Appendix C**, enabling fair comparison across all methods, including HGS, POMO, AM, and EDISCO.

---

## **Response to Question 2:**
We appreciate this important question. EDISCO enforces demand constraints through a **hybrid approach** combining both learning and decoding:

1. **During Training:** The model learns to respect capacity constraints implicitly through the training data distribution. All training instances are feasible CVRP solutions where routes satisfy capacity constraints, providing implicit supervision for constraint-aware generation.

2. **During Decoding:** We employ **masked sampling** that explicitly enforces hard constraints. This is a widely used practice in diffusion-based COP solvers, including [2], [3], and [4].

It is correct that returning to the depot is not triggered solely by insufficient remaining capacity. Our decoding strategy allows the model to:
- Return to the depot **proactively** even with remaining capacity (learned behavior)
- Return when the capacity constraint would be violated (hard constraint)
- Balance route efficiency and capacity utilization through learned policy

This hybrid approach enables EDISCO to generate high-quality feasible solutions.

Please see **Section 4.3 (CVRP Implementation)** and **Appendix C** for details.

---

## With the above answers, we welcome any further questions regarding this manuscript.
