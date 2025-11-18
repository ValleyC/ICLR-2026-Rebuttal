## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).

---

## **Response to Weakness 1:**
We sincerely appreciate this important observation. We have included extensive **out-of-distribution (OOD) experiments** following the evaluation protocol established by GLOP [1]:

The **Uniform (training baseline)**, **Cluster**, **Explosion**, and **Implosion** are evaluated on TSP-100. EDISCO achieves the lowest average performance drop across all distributions.

Please see **Appendix: Cross-Distribution Generalization** (complete table and analysis).

---

## **Response to Weakness 2:**
We sincerely thank the reviewer for this comment. We tried to address this in two ways:

**1. Added HGS baseline:** We now include **HGS** [5], the leading CVRP heuristic, with reported optimal solutions and complete runtime information for CVRP-20/50/100. We acknowledge that HGS achieves superior solution quality compared to neural methods. However, HGS requires substantially longer computation time (often hours for medium-sized instances), and is typically used in the neural CVRP literature as a ground truth or reference solution rather than a competing real-time solver (e.g., PO [8]).

**2. Clarified positioning:** Our CVRP scope (20-100 customers) serves as proof-of-concept for extending EDISCO's continuous-time diffusion framework to constrained routing problems, demonstrating architectural extensibility beyond unconstrained TSP. Notably, competing diffusion methods (DIFUSCO [2], T2T [3], Fast-T2T [4]) have no reported CVRP experiments, highlighting the difficulty of incorporating hard constraints into end-to-end diffusion models. Future work can explore partition-based strategies similar to GLOP [1] to scale to large-scale instances (1000+ customers).

Please see **Appendix C** (Table 5).

---

## **Response to Weakness 3:**
We thank the reviewer for this insightful observation.

We clarify that **EDISCO does not support ATSP** due to an **architectural limitation**: EDISCO's E(2)-equivariant EGNN fundamentally **requires Euclidean coordinates** to preserve geometric symmetries, and any distance induced by coordinates is necessarily **symmetric**—incompatible with ATSP's **arbitrary asymmetric cost matrices**.

This limitation is shared by all coordinate-based TSP solvers (AM [6], POMO [7], Pointer Networks [9], DIFUSCO [2], T2T [3], Fast-T2T [4]). Current ATSP solvers (MatNet [10], GREAT [11]) incorporate directional edge information fundamentally incompatible with E(2)-equivariant coordinate encoders.

However, the **continuous-time diffusion framework is generalizable**. We identify two complementary research directions:

1. **Replace the encoder**: Adopt ATSP-capable architectures (MatNet [10], GREAT [11]) while retaining continuous-time diffusion

2. **Learn coordinate embeddings**: Transform asymmetric cost matrices into approximate coordinate representations using **Finsler Multi-Dimensional Scaling** [12] (extends MDS to handle asymmetric dissimilarities via Finsler spaces) or neural metric learning to enable approximate E(2)-equivariance for ATSP instances with near-geometric structure

We have added a **"Scope and Limitations"** section in **Appendix A** discussing this design choice and its trade-offs. The specialization to coordinate-based geometric problems is deliberate and enables superior sample efficiency, strong generalization, and robust OOD performance.

Please see **Appendix A: Scope and Limitations**.

---

## **Response to Question 1:**
We have added complete runtime information to the CVRP experimental results in **Appendix C** (Table 5), enabling fair comparison across all methods including HGS, POMO, AM, and EDISCO.

Please see **Appendix C** (Table 5).

---

## **Response to Question 2:**
We appreciate this important question. EDISCO enforces demand constraints through a **hybrid approach** combining both learning and decoding:

1. **During Training:** The model learns to respect capacity constraints implicitly through the training data distribution. All training instances are feasible CVRP solutions where routes satisfy capacity constraints, providing implicit supervision for constraint-aware generation.

2. **During Decoding:** We employ **masked sampling** that explicitly enforces hard constraints. This is a widely used practice in diffusion-based COP solvers, including [2], [3], and [4].

You are correct that returning to the depot is not triggered solely by insufficient remaining capacity. Our decoding strategy allows the model to:
- Return to the depot **proactively** even with remaining capacity (learned behavior)
- Return when the capacity constraint would be violated (hard constraint)
- Balance route efficiency and capacity utilization through learned policy

This hybrid approach enables EDISCO to generate high-quality feasible solutions.

Please see **Section 4.3 (CVRP Implementation)** and **Appendix C** for details.
