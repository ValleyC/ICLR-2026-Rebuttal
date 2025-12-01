## 3. Important Clarifications for a couple of "Weaknesses"

We would like to clarify about three "weaknesses":

### **(a) Weakness "Abnormal LKH Results":**
> *The results of LKH in Table 1 seems unright (not good enough as seen in literature).*

We respectfully clarify that **no LKH result was in Table 1**. Table 1 (TSP-50/100) compares EDISCO with neural methods using **Concorde** as the groundtruth. **LKH-3 appears only in Table 2** (TSP-500/1000). However, the LKH-3 gap is already 0.00% and cannot be better, because LKH-3 itself is used as the optimal **groundtruth**. For these reasons, we are confused about this weakness and have been looking forward to further clarification from the reviewer.

### **(b) Weakness "Limited Problem Scope":**
> *Evaluation restricted to TSP only. Missing: (a) other geometric COPs like Vehicle Routing Problem, Capacitated VRP...*

We respectfully clarify that **CVRP experiments were already presented in our original submission** (original Appendix A, now Appendix C). It may have been overlooked. Our original submission already included a comprehensive CVRP evaluation for CVRP-20/50/100 with demand constraints and capacity limits. In the revision, we **strengthened (not added)** this section by including the HGS baseline and complete runtime information. We have also added explicit references in the revised main text to direct readers to the appendices of these results to avoid overlooking them.

### **(c) Weakness "The evaluation scope is relatively limited":**
> *Provide some results on more types of GCOPs (or at least a subset of), e.g., node-focused tasks (MIS, Max Clique, Max Cut, Min Vertx Cover, etc.)*

We respectfully clarify that **MIS, Max-Cut, Max Clique, and MVC are not GCOPs**. These problems operate on arbitrary graphs where only the topology matters. The node positions do not carry semantic meaning, and there is no geometric structure in Euclidean space to exploit.

**EDISCO's scope was clearly defined from the beginning.** Our paper title explicitly states *"Geometric Combinatorial Optimization"*, and both the abstract and introduction consistently emphasize that EDISCO targets problems with inherent E(2) symmetries (rotation and translation invariance) in Euclidean space. This specialization is a deliberate architectural choice that enables strong inductive bias for geometric problems like TSP, CVRP, and ESTP.

To make this scope even more explicit, we have added **Appendix A: Scope and Limitations** in the revised manuscript, which provides a detailed discussion of why EDISCO is specialized for Euclidean geometric problems and identifies promising future directions for extending to other problem classes.

We respectfully believe that this might not be viewed as a weakness. Notably, the authors of DIFUSCO identified it as a promising future direction in the **Conclusion** section of their paper:
> *"**explore the use of equivariant graph neural networks for geometrical NP-complete combinatorial optimization problems such as Euclidean TSP**"*

This is precisely what EDISCO accomplishes.

---

## 4. Conclusion

We believe our revisions comprehensively address all reviewer concerns:

| Concern | Status | Summary |
|---------|--------|---------|
| **Problem scope** | ✓ Addressed | CVRP was already in the original submission and was enhanced in the revised manuscript; Added new ESTP experiments; Clarified non-geometric problems (MIS, Max-Cut) are outside the scope by design |
| **Missing baselines** | ✓ Addressed | Added Fast-T2T, BQ-NCO, HGS with complete comparisons |
| **OOD generalization** | ✓ Addressed | Demonstrated state-of-the-art robustness (4.2% deterioration vs 15-1960% for others) |
| **ATSP support** | ✓ Clarified | Architectural limitation shared by all coordinate-based methods; Future directions provided |
| **Theoretical justification** | ✓ Addressed | Added SNR-based motivation and comprehensive ablation |
| **Scalability** | ✓ Addressed | Added resource analysis showing O(kn) complexity and single-GPU accessibility |

---

## We sincerely hope the Area Chair considers our clarifications and detailed responses, and we are confident that the revised manuscript makes a significant contribution to the field.

## Thank you so much again for your valuable time and consideration. Please don’t hesitate to let us know if any further clarification is needed.