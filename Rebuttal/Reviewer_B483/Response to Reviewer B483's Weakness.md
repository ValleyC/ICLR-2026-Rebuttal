## Thank you for these valuable comments. We have revised the manuscript to address your concerns (changes marked in blue).

## **Response to Weakness 1:**
We are grateful for your comments, but we have to point out that there are significant oversights in your review.

**(a)** The manuscript had already been extended to other geometric problems when originally submitted. **In original submission,** Appendix A (**Appendix C in the revised manuscript**) already included comprehensive CVRP evaluation for CVRP-20/50/100 with demand constraints and capacity limits. For more extensive evaluations, we have now added **Appendix B** to the revised manuscript, containing experiments on the **Euclidean Steiner Tree Problem (ESTP)**. EDISCO substantially outperforms classical heuristics (GeoSteiner [11]) and learning-based baselines (DIFUSCO [3], T2T [4], Deep-Steiner [12]) across Steiner-10/20/50.

**(b)** Thank you for this insightful comment. We admit that this is one of the limitations of our method. However, E(2)-equivariance fundamentally requires Euclidean coordinates to preserve geometric symmetries. This is an architectural choice, and we respectfully believe that this should not be viewed as a weakness. We have added **Appendix A: Scope and Limitations**, which clarifies that EDISCO is specialized for Euclidean geometric problems. Extending to Manhattan distance or road networks, or even asymmetric TSP (ATSP), would require different architectural approaches, which we have actively explored as a promising future direction in **Appendix A: Scope and Limitations**.

**(c)** Our CVRP evaluation (**Appendix C**) demonstrates EDISCO's capability to handle capacity constraints through hybrid learning and masked decoding. However, we acknowledge that more complex real-world constraints (time windows, precedence) would require extensions to the constraint handling mechanism. The current masked sampling approach is compatible with hard constraints that can be verified locally during decoding. Time windows and precedence constraints represent promising future directions for extending EDISCO's constraint handling capabilities.

**(d)** The comparisons with Concorde and LKH already existed in almost **all the results tables** in the **original submission**. For example:
- **Concorde** [1]: Table 1 (TSP-50/100) with wall-clock time and solution quality
- **LKH-3** [2]: Table 2 (TSP-500/1000) with complete runtime information (46.28m for TSP-500, 2.57h for TSP-1000) 

## **Response to Weakness 2:**

We appreciate these valuable concerns.

**(a)** TSP-10000 results are the current upper limit of end-to-end neural methods. Notably, competing neural approaches (AM [8], POMO [9], DIFUSCO [3], T2T [4], Fast-T2T [5]) evaluate only up to TSP-10K. Hybrid methods (NeuroLKH [6], GLOP [7]) target large-scale routing problems, but also never explored TSP 100K+. We have added the training resource analysis in the revised manuscript (**Appendix H.9**) to show EDISCO's **memory scaling patterns** to prove that EDISCO's memory complexity is **O(kn)**. 

**(b)** In Section 4.1, we have clarified that we apply graph sparsification for TSP-500+ following DIFUSCO's [3] protocol. This is a standard approach in competing diffusion-based solvers [3], [4], and [5]. The EGNN message passing is O(n²) for memory due to edge features without sparsification. We have added a comprehensive training resource analysis in **Appendix H.9**, showing that peak memory usage (19.2-44.8GB) remains well within a single A6000 GPU's 48GB capacity across all problem scales. This demonstrates EDISCO's practical accessibility compared to methods requiring expensive multi-GPU infrastructure.

**(c)** **In the original submission,** **Appendix F.7 (Appendix H.8 in the revised manuscript)** already provided a comprehensive analysis of model sizes: EDISCO-Full (5.5M parameters), EDISCO-Medium (2.6M parameters), and EDISCO-Small (1.4M parameters). The results demonstrate that EDISCO-Medium achieves comparable performance to the full model with less than half the parameters, and EDISCO-Small (1.4M parameters) uses 3.8× fewer parameters than baselines while maintaining competitive performance. This shows EDISCO's architecture scales gracefully across different computational budgets.

**(d)** In Section 4.1, we have documented that curriculum learning is used for large-scale problems. TSP-500/1000 are initialized from the TSP-100 checkpoint, and TSP-10000 is initialized from the TSP-500 checkpoint. This is also a standard approach in competing diffusion-based solvers [3], [4], and [5]. Curriculum learning is used to improve training efficiency and for fair comparisons. But it is not strictly necessary. Direct training on target problem sizes also converges successfully with more training data and longer training time.