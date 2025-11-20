## We would like to thank the reviewer for the detailed and insightful comments. We have thoroughly revised the manuscript to address your concerns (changes marked in blue).

## **Response to Weakness 1:**

### (1) Node-Focused Geometric Tasks: Euclidean Steiner Tree

We now include extensive experiments on the **Euclidean Steiner Tree Problem (ESTP)**, demonstrating that EDISCO applies beyond routing to *node-selection* geometric optimization. Unlike TSP, ESTP requires deciding which candidate Steiner points to include to minimize tree length.

EDISCO substantially outperforms both classical heuristics and learning-based baselines (DIFUSCO, T2T, Fast-T2T) across all problem sizes (Steiner-10/20/50). Please see **Appendix B** (Table 4).

---

### (2) Non-Geometric Problems (MIS, Max-Cut): Clarification of Scope

We clarify that **MIS, Max-Cut, Max Clique, and MVC are *not* geometric combinatorial optimization problems**. These problems operate on arbitrary graphs where only graph topology matters, without geometric structure. EDISCO is designed explicitly for geometric problems where solutions exhibit E(2) symmetries in Euclidean space. Without geometric structure, E(2)-equivariance provides no inductive bias, offering no advantage over general-purpose diffusion methods (DIFUSCO, T2T) while unnecessarily introducing coordinate processing.

We believe this might not be a weakness. In fact, DIFUSCO's authors themselves identified **"explore the use of equivariant graph neural networks for further improvement of the diffusion models on geometrical NP-complete combinatorial optimization problems such as Euclidean TSP"** as a promising future direction. This specialization yields **specialized benefits** for geometric problems.

Please see **Appendix A: Scope and Limitations**.

---

### (3) CVRP: Added Baselines, Positioning, and Clarifications

We have strengthened the CVRP section in **Appendix C** of the revised manuscript in three ways:

1. **Added state-of-the-art baseline:** We now include **HGS** [1], the leading CVRP heuristic, with reported optimal solutions for CVRP-20/50/100, and include missing runtime information.

2. **Clarified positioning relative to large-scale CVRP solvers:** We discuss GLOP [2] and NeuroLKH [3], which scale to 1000–7000 customers using partition-based strategies. Our CVRP scope (20-100 customers) is positioned as proof-of-concept for architectural extensibility to constrained routing, not as competing with large-scale specialists. Notably, competing diffusion methods (DIFUSCO, T2T, Fast-T2T) have no reported CVRP experiments, highlighting the difficulty of scaling single end-to-end diffusion models. We have actively discussed future directions in **Appendix A** in the revised manuscript for applying the E(2) Equivariance to partition-based strategies similar to GLOP [2] to scale to large-scale instances (1000+ customers).

3. **PO clarification:** Our study shows that PO [4] is a training algorithm, not a solver architecture. It can be applied to POMO, Sym-NCO, AM to improve training. Including it in our CVRP comparison would not be appropriate, as we are comparing solvers, not training methods.

---

### (4) Asymmetric TSP (ATSP)

We clarify that **EDISCO does not support ATSP** due to an **architectural limitation** specific to E(2)-equivariant methods: EDISCO's E(2)-equivariant EGNN fundamentally **requires Euclidean coordinates** to preserve geometric symmetries. This is incompatible with ATSP's coordinate-free **arbitrary asymmetric cost matrices**.

Actually, this is not limited to our method. Coordinate-based attention methods (AM, POMO, Pointer Networks) are designed with positional encodings for Euclidean TSP. Although graph-based diffusion methods (DIFUSCO, T2T, Fast-T2T) use GNNs that could theoretically process cost matrices, they have only been evaluated on coordinate-based Euclidean TSP. Current ATSP solvers (MatNet [5], GREAT [6]) incorporate directional edge information fundamentally incompatible with E(2)-equivariant coordinate encoders.

Again, we believe that this may not be a weakness. We have added **Appendix A: Scope and Limitations** in the revised manuscript, which clarifies that EDISCO is specialized for Euclidean geometric problems. We have also actively discussed two **promising future directions** in **Appendix A**: (1) replacing the encoder with ATSP-capable architectures (MatNet [5], GREAT [6]) while retaining continuous-time diffusion, or (2) learning coordinate embeddings from asymmetric cost matrices using **Finsler Multi-Dimensional Scaling** [7] (extends MDS to handle asymmetric dissimilarities via Finsler spaces) or neural metric learning to enable approximate E(2)-equivariance for ATSP instances with near-geometric structure.
