## **Welcome and thanks to the new AC**

We would like to express our most sincere gratitude to all the ACs for their time and efforts. While we were not aware of the severe OpenReview bug, we are very sorry to hear about the leak of information that disrupted the entire discussion process. 

We also sincerely thank all reviewers for their thorough evaluation and constructive feedback. Although no reviewers had the chance to respond to our rebuttals, we have substantially revised our manuscript (all changes marked in blue) and believe our responses comprehensively address all raised concerns.

---

## 1. Novelty, Solid Results, and Other Major Strengths

EDISCO is the **first method to combine E(2)-equivariance with continuous-time categorical diffusion** for **Geometric Combinatorial Optimization Problems (GCOPs)**. All reviewers particularly acknowledged the novelty in method design and solid practical results, among the following strengths:

- **Novel Architecture:** The integration of E(2)-equivariant graph neural networks (EGNN) with continuous-time diffusion is a principled approach that exploits the inherent symmetries of geometric problems.
- **Solid Empirical Results:** State-of-the-art performance across TSP-50/100/500/1000/10000 benchmarks, significantly outperforming prior diffusion methods (DIFUSCO, T2T).
- **Theoretical Grounding:** The continuous-time formulation enables flexible speed-quality trade-offs through advanced ODE solvers (PNDM, DEIS-2).
- **Comprehensive Evaluation:** Extensive experiments with ablation studies demonstrating each component's contribution.

---

## 2. How We Addressed the Concerns

### **Concern A: Extended Problem Scope Beyond Routing Problems (Raised by B483, ZQPE)**

To further demonstrate EDISCO's generalizability beyond routing problems, we added experiments on the **Euclidean Steiner Tree Problem (ESTP)**, a **node-selection problem** fundamentally different from routing problems like TSP and Capacitated Vehicle Routing Problem (CVRP). EDISCO achieves **2-4× better gaps** than all learning-based baselines. 

Please refer to:
- **Rebuttal:** Response to Reviewer B483's Weakness 1; Response to Reviewer ZQPE's Weakness 1
- **Manuscript:** Appendix B: Extension to Euclidean Steiner Tree Problem

---

### **Concern B: Missing Recent Baselines (Raised by ZQPE, 6MUB)**

We have added **Fast-T2T** and **BQ-NCO** as requested. EDISCO achieves **2.7-4.8× better gaps** than all baselines on TSP-500/1000.

Please refer to:
- **Rebuttal:** Response to Reviewer ZQPE's Weakness 2&3; Response to Reviewer 6MUB's Weakness 3
- **Manuscript:** Tables 1, 2; Table 4 (Appendix B); Appendix H.3: Cross-Distribution Generalization; Appendix H.4: TSPLIB Results

---

### **Concern C: Out of Distribution (OOD) Generalization on TSP (Raised by ZQPE, dpjP)**

We added comprehensive OOD experiments on the **Cluster**, **Explosion**, and **Implosion** distributions of TSP-100. EDISCO achieves the **lowest performance deterioration (4.2%)** vs 15-1960% for other methods.

Please refer to:
- **Rebuttal:** Response to Reviewer dpjP's Weakness 1; Response to Reviewer ZQPE's Weakness 2&3
- **Manuscript:** Appendix H.3: Cross-Distribution Generalization

---

### **Concern D: Asymmetric TSP (ATSP) Support (Raised by ZQPE, dpjP)**

We clarified that this is an **architectural limitation** shared by **ALL coordinate-based methods**: (AM, POMO, DIFUSCO, T2T, Fast-T2T), therefore, it might not be a specific weakness of EDISCO. We have now provided clarifications and two promising future directions in the revised manuscript.

Please refer to:
- **Rebuttal:** Response to Reviewer ZQPE's Weakness 1; Response to Reviewer dpjP's Weakness 3
- **Manuscript:** Appendix A: Scope and Limitations

---

### **Concern E: Theoretical Justification for Mixing Strategy (Raised by ZQPE)**

We added Signal-to-Noise Ratio (SNR)-based theoretical motivation. Comprehensive ablation demonstrates **4.8× improvement** over vanilla DIFUSCO, with each component contributing meaningfully.

Please refer to:
- **Rebuttal:** Response to Reviewer ZQPE's Weakness 4
- **Manuscript:** Appendix H.6: Evaluation on Adaptive Mixing Parameters

---

### **Concern F: Scalability and Computational Resources (Raised by B483)**

We added training resource analysis showing **O(kn)** memory complexity with graph sparsification. Peak memory (19.2-44.8GB) fits within a single A6000 GPU (48GB).

Please refer to:
- **Rebuttal:** Response to Reviewer B483's Weakness 2
- **Manuscript:** Appendix H.9: Training Resource Requirements
