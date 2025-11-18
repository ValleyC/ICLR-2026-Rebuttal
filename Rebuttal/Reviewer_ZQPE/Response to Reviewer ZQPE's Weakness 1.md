## Thank you for these valuable comments. We have revised the manuscript, trying to address your concerns (changes marked in blue).

## **Response to Weakness 1:**

### (1) Node-Focused Geometric Tasks: Euclidean Steiner Tree

We now include extensive experiments on the **Euclidean Steiner Tree Problem (ESTP)**, demonstrating that EDISCO applies beyond routing to *node-selection* geometric optimization. Unlike TSP, ESTP requires deciding which candidate Steiner points to include to minimize tree length.

EDISCO substantially outperforms both classical heuristics and learning-based baselines (DIFUSCO [1], T2T [2], Fast-T2T [3]) across all problem sizes (Steiner-10/20/50). Please see **Appendix B** (Table 4).

---

### (2) Non-Geometric Problems (MIS, Max-Cut): Clarification of Scope

We clarify that **MIS, Max-Cut, Max Clique, and MVC are *not* geometric combinatorial optimization problems**. These problems operate on arbitrary graphs where only graph topology matters, without geometric structure. EDISCO is designed explicitly for geometric problems where solutions exhibit E(2) symmetries in Euclidean space. Without geometric structure, E(2)-equivariance provides no inductive bias, offering no advantage over general-purpose diffusion methods (DIFUSCO [1], T2T [2]) while unnecessarily introducing coordinate processing.

We respectfully believe this should not be considered a weakness. In fact, DIFUSCO's authors themselves identified **"explore the use of equivariant graph neural networks for further improvement of the diffusion models on geometrical NP-complete combinatorial optimization problems such as Euclidean TSP"** as a promising future direction [1]. This specialization yields **specialized benefits** for geometric problems.

Please see **Appendix A: Scope and Limitations**.

---

### (3) CVRP: Added Baselines, Positioning, and Clarifications

We have strengthened the CVRP section in three ways:

1. **Added state-of-the-art baseline:** We now include **HGS** [19], the leading CVRP heuristic, with reported optimal solutions for CVRP-20/50/100, and include missing runtime information.

2. **Clarified positioning relative to large-scale CVRP solvers:** We discuss GLOP [21] and NeuroLKH [20], which scale to 1000–7000 customers using partition-based strategies. Our CVRP scope (20-100 customers) is positioned as proof-of-concept for architectural extensibility to constrained routing, not as competing with large-scale specialists. Notably, competing diffusion methods (DIFUSCO [1], T2T [2], Fast-T2T [3]) have no reported CVRP experiments, highlighting the difficulty of scaling single end-to-end diffusion models.

3. **PO clarification:** After careful study, PO [14] is a training algorithm, not a solver architecture. It can be applied to POMO [11], Sym-NCO [12], AM [10] to improve training. Including it in our CVRP comparison would not be appropriate, as we are comparing solvers, not training methods.

Please see **Appendix C** (Table 5).

---

### (4) Asymmetric TSP (ATSP)

We clarify that **EDISCO does not support ATSP** due to an **architectural limitation** specific to E(2)-equivariant methods: EDISCO's E(2)-equivariant EGNN fundamentally **requires Euclidean coordinates** to preserve geometric symmetries, and any distance induced by coordinates is necessarily **symmetric**—incompatible with ATSP's **arbitrary asymmetric cost matrices**.

This architectural constraint is **stronger than other methods**: coordinate-based attention methods (AM [10], POMO [11], Pointer Networks [24]) are designed with positional encodings for Euclidean TSP, while graph-based diffusion methods (DIFUSCO [1], T2T [2], Fast-T2T [3]) use GNNs that could theoretically process cost matrices but have only been evaluated on coordinate-based Euclidean TSP. Current ATSP solvers (MatNet [22], GREAT [23]) incorporate directional edge information fundamentally incompatible with E(2)-equivariant coordinate encoders.

However, the **continuous-time diffusion framework is generalizable**. We identify two **promising future directions**: (1) replacing the encoder with ATSP-capable architectures (MatNet [22], GREAT [23]) while retaining continuous-time diffusion, or (2) learning coordinate embeddings from asymmetric cost matrices using **Finsler Multi-Dimensional Scaling** [25] (extends MDS to handle asymmetric dissimilarities via Finsler spaces) or neural metric learning to enable approximate E(2)-equivariance for ATSP instances with near-geometric structure.

Please see **Appendix A: Scope and Limitations**.
