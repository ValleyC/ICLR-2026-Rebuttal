## Thank you for these valuable comments. We have revised the manuscript, trying to address your concerns (changes marked in blue).

## **Response to Weakness 1:**
---
### (1) Node-Focused Geometric Tasks: Euclidean Steiner Tree

We now include extensive experiments on the **Euclidean Steiner Tree Problem (ESTP)**, demonstrating that EDISCO applies beyond routing to *node-selection* geometric optimization. Unlike TSP, ESTP requires deciding which candidate Steiner points to include to minimize tree length.

**Results:**  
EDISCO achieves **1.53% (Steiner-10)**, **1.38% (Steiner-20)**, **1.56% (Steiner-50)** optimality gaps—substantially outperforming:
- **Steiner Insertion heuristic:** 3.44% → **1.53%**  
- **Learning-based baselines:** DIFUSCO [1] (5.73%), T2T [6] (4.20%), FastT2T [7] (2.29%)

Please see **Appendix B** (Table 4).

---

### (2) Non-Geometric Problems (MIS, Max-Cut): Clarification of Scope

We thank the reviewer for this insightful point. We clarify that **MIS, Max-Cut, Max Clique, and MVC are *not* geometric combinatorial optimization problems**. These problems operate on arbitrary graphs where node positions carry no semantic meaning, but only graph topology matters. EDISCO is designed explicitly for GCOPs where solutions exhibit E(2) symmetries in Euclidean space. Without geometric structure, E(2)-equivariance provides no inductive bias. Applying EDISCO to non-geometric problems would offer no advantage over general-purpose diffusion methods (DIFUSCO [1], T2T [6]) while unnecessarily introducing coordinate processing. 

We admit that this is one of the limitations of our method, but we respectfully believe that this should not be considered a weakness. In fact, the DIFUSCO's authors themselves identified **"explore the use of equivariant graph neural networks for further improvement of the diffusion models on geometrical NP-complete combinatorial optimization problems such as Euclidean TSP "** as a promising future direction in their conclusion [1].

This specialization is a deliberate design decision that yields **specialized benefits** for geometric problems. We have added a "Scope and Limitations" section to clarify this.

Please see “**Scope and Limitations**” section (Appendix A).

---

### (3) CVRP: Added Baselines, Positioning, and Clarifications

We have strengthened the CVRP section in three ways:

1. **Added state-of-the-art baseline:**  
   We now include **HGS** [2], the leading CVRP heuristic, with reported **0.00%** gaps at 1h/3h/5h for CVRP-20/50/100, and include missing runtime information.

2. **Clarified positioning relative to large-scale CVRP solvers:**  
   We discuss GLOP [3] and NeuroLKH [4], which scale to 1000–7000 customers using partition-based strategies. Our CVRP scope (20-100 customers) is positioned as proof-of-concept for architectural extensibility to constrained routing, not as competing with large-scale specialists. Large-scale CVRP (500-7000+ customers) universally requires partition-based strategies that divide the large-scale CVRPs into sub-TSP problems. Notably, competing diffusion methods (DIFUSCO [1], T2T [6], Fast-T2T [7]) have no reported CVRP experiments, highlighting the difficulty of scaling single end-to-end diffusion models.

3. **PO clarification:** We are sincerely thankful for pointing out the relevant literature that we overlooked. However, after careful study, PO [5] is a training algorithm, not a solver architecture. It can be applied to POMO [9], Sym-NCO [10], AM [8] to improve training. Including it in our CVRP comparison would not seem appropriate or fair, as we are comparing solvers, not training methods.

Please see **Appendix C** (Table 5).

---

### (4) Asymmetric TSP (ATSP)

We thank the reviewer for this important observation.  
We clarify that **EDISCO, in its current geometric EGNN formulation, does *not* support ATSP**, and this is a deliberate design choice aligned with the entire class of coordinate-based TSP solvers (AM [8], POMO [9], Pointer Networks [3], DIFUSCO [1], T2T [6], FastT2T [7]).

This limitation is **mathematical rather than methodological**:  
any distance induced by Euclidean coordinates is necessarily **symmetric**, while ATSP requires representing an **arbitrary asymmetric cost matrix**. Therefore, ATSP solvers must incorporate **directional edge information**, such as:
- full cost-matrix encoding (MatNet [14]), or  
- dual incoming/outgoing attention (GREAT [15])  

These mechanisms are fundamentally incompatible with E(2)-equivariant coordinate encoders [16].

However, the **continuous-time diffusion framework of EDISCO *is* generalizable**. Future work would be interesting by involving replacing the geometric EGNN with an ATSP-capable encoder (e.g., MatNet or GREAT) without changing the continuous-time dynamics.

This clarification is now included in **Appendix A (Scope and Limitations)**.