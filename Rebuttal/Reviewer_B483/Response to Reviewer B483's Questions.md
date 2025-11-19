## Thank you for these valuable questions

## **Response to Question 1:**

We respectfully note that this is an oversight. **CVRP results already existed in the original submission Appendix A** (**Appendix C in the revised manuscript**). EDISCO handles capacity constraints through supervised training and a decoder, and does not require any architectural modifications:

1. **During Training:** The model learns to approximate the training data's distribution. Therefore, it implicitly learn the constraints from the legal solutions in the training dataset.
2. **During Decoding:** Explicit masked sampling that hard prevents violations

However, we still believe this question is insightful. Our CVRP scope (20-100 customers) serves as proof-of-concept for extending EDISCO's continuous-time diffusion framework to constrained routing problems, demonstrating architectural extensibility beyond TSP. Notably, competing diffusion methods (DIFUSCO [2], T2T [3], Fast-T2T [9]) have no reported CVRP experiments, highlighting the difficulty of incorporating complex hard constraints into end-to-end diffusion models. We have actively discussed future directions in **Appendix A** in the revised manuscript for applying the E(2) Equivariance to partition-based strategies similar to GLOP [11] to scale to large-scale instances (1000+ customers).

## **Response to Question 2:**

**Table 2** shows complete runtime information:
- TSP-500: 2.19m (50-step PNDM), 0.23m (5-step DEIS-2)
- TSP-1000: 6.84m (50-step PNDM), 0.75m (5-step DEIS-2)
- TSP-10000: 12.18m (50-step PNDM), demonstrating roughly O(n) scaling for inference time per problem

**Memory Usage:** In the original submission, **Section 4.1** documented graph sparsification (k-nearest neighbors for TSP-500+), enabling memory to scale as O(kn) rather than O(n²). To better demonstrate the memory scaling pattern, we have added an explicit discussion in **Appendix H.9** that provides a detailed analysis of memory scaling and sparsification strategies.

## **Response to Question 3:**

We must respectfully point out another oversight here. These comparisons **already existed in the original submission** in almost all result tables. For example: **TSP-1000 example from original submission:**
- **Concorde** [5] (exact): Gap 0.00%, Time 6.65h
- **LKH-3** [6] (heuristic): Gap 0.00%, Time 2.57h
- **EDISCO-PNDM**: Gap 2.85%, Time 6.84m (22× faster than LKH-3)
- **EDISCO-DEIS-2**: Gap 4.42%, Time 0.75m (206× faster than LKH-3)

For **Pareto Frontier Positioning**, EDISCO is different from traditional solvers. As discussed in **Appendix H.1**, compatibility with higher-order solvers enables EDISCO to trade modest solution quality (2-5% gap) for up to 200x speedup. This makes it suitable for real-time applications where near-optimal solutions with fast response times are preferable to optimal solutions requiring hours.