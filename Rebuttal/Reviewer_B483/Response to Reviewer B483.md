## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).

---

## **Response to Weakness 1:**
We are thankful for your comments, but we would like to point out that there might be significant oversights in your review. 

1. The manuscript had already been extended to other geometric problems when originally submitted. **In original submission,** Appendix A (**Appendix C in the revised manuscript**) already included comprehensive CVRP evaluation for CVRP-20/50/100 with demand constraints and capacity limits. For more extensive evaluations, we have now added **Appendix B** to the revised manuscript, containing experiments on the **Euclidean Steiner Tree Problem (ESTP)**. EDISCO substantially outperforms classical heuristics and learning-based baselines (DIFUSCO, T2T) across Steiner-10/20/50.

2. **Regarding missing comparisons with Concorde and LKH-3:** These comparisons already existed in almost **all the results tables** in the **original submission**. For example:
- **Concorde**: Table 1 (TSP-50/100) with wall-clock time and solution quality
- **LKH-3**: Table 2 (TSP-500/1000) with complete runtime information (46.28m for TSP-500, 2.57h for TSP-1000)

3. **Regarding non-Euclidean settings:** Thank you for this insightful comment. However, E(2)-equivariance fundamentally requires Euclidean coordinates to preserve geometric symmetries. This is an architectural choice, and we respectfully believe that this should not be viewed as a weakness.  We have added **Appendix A: Scope and Limitations**, which clarifies that EDISCO is specialized for Euclidean geometric problems. Extending to Manhattan distance or road networks, or even asymmetric TSP (ATSP), would require different architectural approaches, which we have identified as promising future directions in our discussions in **Appendix A: Scope and Limitations**. 

---

## **Response to Weakness 2:**

We appreciate these valuable concerns.

1. **Regarding 100K+ cities:** TSP-10000 results are the current upper limit of end-to-end neural methods. We have now added clarifications in the revised manuscript that scaling to 100K+ cities typically requires hierarchical decomposition (e.g., NeuroLKH, GLOP) rather than single end-to-end models. We position this as a limitation and discuss partition-based strategies as promising future work.

2. **Regarding memory requirements:** **In original submission,** Section 4.1 documented that we apply graph sparsification for TSP-500+ following DIFUSCO's protocol. The EGNN message passing is O(n²) for memory due to edge features without sparsification. **Added during rebuttal:** We have added comprehensive training resource analysis in **Appendix H.8 (Training Resource Requirements, Table 7)** showing peak memory usage (20-42GB) remains well within a single A6000 GPU's 48GB capacity across all problem scales. This demonstrates EDISCO's practical accessibility compared to methods requiring expensive multi-GPU infrastructure.

3. **Regarding architecture scaling:** **In original submission,** **Appendix H.8 (Model Efficiency, Table 16)** already provided comprehensive analysis of architecture scaling with three model sizes: EDISCO-Full (5.5M parameters), EDISCO-Medium (2.6M parameters), and EDISCO-Small (1.4M parameters). The results demonstrate that EDISCO-Medium achieves comparable performance to the full model with less than half the parameters, and EDISCO-Small (1.4M parameters) uses 3.8× fewer parameters than baselines while maintaining competitive performance. This shows EDISCO's architecture scales gracefully across different computational budgets.

4. **Regarding curriculum learning:** **In original submission,** Section 4.1 (Training Configuration) documented curriculum learning for large-scale problems. TSP-500/1000 are initialized from TSP-100 checkpoint, and TSP-10000 is initialized from TSP-500 checkpoint. Curriculum learning improves sample efficiency by transferring learned geometric patterns from smaller to larger instances, but is not strictly necessary—direct training on target problem sizes also converges successfully, albeit with more training data. 

---

## **Response to Question 1:**

We respectfully note that this is an oversight. **CVRP results already existed in the original submission Appendix A** (**Appendix C, Table 5** in the revised manuscript). EDISCO handles capacity constraints through a hybrid approach:

**Constraint Enforcement (from original submission):**
1. **During Training:** The model approaches the training data distribution, therefore, implicitly learn from legal training solutions
2. **During Decoding:** Explicit masked sampling that prevents capacity violations

Please see **Appendix C** for the complete CVRP methodology and results.

---

## **Response to Question 2:**

**Inference Time Scaling (from original submission):** Table 2 shows complete runtime information:
- TSP-500: 2.19m (50-step PNDM), 0.23m (5-step DEIS-2)
- TSP-1000: 6.84m (50-step PNDM), 0.75m (5-step DEIS-2)
- TSP-10000: 12.18m (50-step PNDM), demonstrating roughly O(n) scaling for inference time per problem

**Memory Usage:** **In original submission,** Section 4.1 documented graph sparsification (k-nearest neighbors for TSP-500+), enabling memory to scale as O(kn) rather than O(n²). For TSP-10000, we use k=100 resulting in ~10M edges. **Added during rebuttal:** We have added an explicit discussion in **Appendix H.8** providing detailed analysis of memory scaling and sparsification strategies.

---

## **Response to Question 3:**

1. **Regarding comparisons with Concorde and LKH-3:** We must respectfully point out another oversight here. These comparisons **already existed in the original submission** in almost all result tables. For example: **TSP-1000 example from original submission:**
- **Concorde** (exact): Gap 0.00%, Time 6.65h
- **LKH-3** (heuristic): Gap 0.00%, Time 2.57h
- **EDISCO-PNDM**: Gap 2.85%, Time 6.84m (22× faster than LKH-3)
- **EDISCO-DEIS-2**: Gap 4.42%, Time 0.75m (206× faster than LKH-3)

**Pareto Frontier Positioning:** EDISCO is different from traditional solvers. As discussed in **Appendix H.1**, compatibility with higher-order solvers enables EDISCO to trade modest solution quality (2-5% gap) for up to 200x speedup. This makes it suitable for real-time applications where near-optimal solutions with fast response times are preferable to optimal solutions requiring hours.