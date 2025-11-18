# Rebuttal: EDISCO and Coordinate-Free ATSP

## Response to Reviewer ZQPE's Question

**Reviewer's Concern:**
> "I'm curious how EDISCO performs on ATSP cases, e.g., those defined in MatNet (Kwon et al. NeurIPS 2021), as 2D-coordinates are absent for input and the inherent symmetries are largely removed."

We thank the reviewer for this insightful question. The short answer is: **EDISCO cannot handle MatNet's coordinate-free ATSP formulation, and this is a fundamental architectural constraint rather than a limitation**. We provide a detailed explanation below.

---

## 1. The Core Issue: MatNet's ATSP is Coordinate-Free

### 1.1 MatNet's ATSP Formulation

**Input**: Pure distance matrix D ∈ ℝ^(n×n) where d_ij ≠ d_ji
- **No 2D coordinates** provided
- Arbitrary asymmetric costs
- Example: TMAT-class instances [1]

**Architectural approach**:
- Matrix encoding with one-hot embeddings for rows/columns
- Standard GNN processes the encoded matrix
- No geometric assumptions

**Results** [1]:
- ATSP-50: 1.34% gap (single inference), 0.11% gap (×128 augmentation)
- ATSP-100: 3.24% gap (single inference), 0.93% gap (×128 augmentation)

### 1.2 Why This is Different from Euclidean TSP

Most neural TSP solvers (including ours) assume:
- Input: 2D coordinates (x_i, y_i) ∈ ℝ²
- Distances: Euclidean d_ij = ||p_j - p_i|| (inherently symmetric)
- Examples: POMO [2], Attention Model [3], DIFUSCO [4]

MatNet was the **first neural approach** to break this assumption by handling arbitrary asymmetric cost matrices without coordinates [1].

---

## 2. Why EDISCO Cannot Handle Coordinate-Free ATSP

### 2.1 E(2)-Equivariance Fundamentally Requires Coordinates

The core architecture of EDISCO relies on E(2)-equivariant graph neural networks (EGNNs) [5], which operate through **coordinate updates** at every layer:

```
For layer l:
  x_i^(l+1) = x_i^(l) + Σ_j (x_j^(l) - x_i^(l)) · φ(m_ij) / ||x_j^(l) - x_i^(l)||
```

**Critical observations**:

1. **Relative positions** (x_j - x_i) are fundamental to the computation
2. **Coordinate evolution** happens across ALL layers, not just initial encoding
3. **Equivariance property** f(Rx + t) = Rf(x) + t requires coordinate transformations

**Without coordinates, these operations are undefined.**

### 2.2 Comparison: DIFUSCO vs EDISCO

The reviewer might wonder: "DIFUSCO can handle both coordinate-based TSP and coordinate-free MIS [4], so why can't EDISCO?"

**Key architectural difference**:

| Component | DIFUSCO | EDISCO |
|-----------|---------|--------|
| **Encoder** | Modular (coords→features OR graph→features) | EGNN |
| **Processing** | Standard GNN on features | **Coordinate evolution through layers** |
| **Coordinates** | Used once for encoding (optional) | **Evolved at every layer** (mandatory) |

**DIFUSCO's flexibility** comes from:
```
Coordinates → [Encode once] → Features → [Standard GNN] → Output
```

**EDISCO's requirement**:
```
Coordinates → [EGNN Layer 1] → Updated Coordinates
           → [EGNN Layer 2] → Updated Coordinates
           → [EGNN Layer 3] → Updated Coordinates
           → Output
```

The coordinates are not just input—they are **transformed and updated through the network**, which is how E(2)-equivariance is maintained.

### 2.3 Cannot Be "Worked Around"

One might consider preprocessing approaches:

**Option 1: Multidimensional Scaling (MDS) [6]**
- Reconstruct approximate 2D coordinates from distance matrix
- **Problem**:
  - Only works for symmetric distances (d_ij = d_ji)
  - For ATSP, would need to symmetrize: D_sym = (D + D^T)/2
  - This loses the asymmetry information that defines the problem

**Option 2: Learned coordinate embedding**
- Train a network to predict coordinates from cost matrix
- **Problem**:
  - Arbitrary costs may have no meaningful 2D embedding
  - Even if embedded, E(2)-equivariance benefits are questionable for non-geometric costs
  - Loss of inductive bias that E(2)-equivariance provides

**Conclusion**: There is no principled way to apply E(2)-equivariant methods to coordinate-free problems.

---

## 3. This is Not a Limitation—It's a Design Choice

### 3.1 Different Architectures for Different Problems

The field has converged on two complementary approaches:

**Coordinate-Free Architecture** (for abstract costs):
- **Input**: Distance matrix D
- **Encoding**: Matrix embedding (MatNet) or directional edge features (GREAT [7])
- **Processing**: Standard GNN or specialized attention
- **Suitable for**: Network routing, abstract cost matrices, coordinate-free ATSP

**Geometric Architecture** (for Euclidean/geometric problems):
- **Input**: 2D/3D coordinates
- **Encoding**: Coordinate-based node embeddings
- **Processing**: E(2)-equivariant operations (EDISCO) or standard GNN (POMO, AM)
- **Suitable for**: Euclidean TSP, geometric routing, physical world problems

### 3.2 Why E(2)-Equivariance Requires Geometry

E(2)-equivariance encodes the symmetry: **solutions are invariant to rigid transformations**

For geometric problems:
- Rotate coordinates by θ → Optimal tour rotates by θ
- Translate by vector v → Optimal tour translates by v
- **This is only meaningful when coordinates represent physical space**

For coordinate-free ATSP:
- No coordinates → No notion of rotation/translation
- Costs are arbitrary → No geometric structure to exploit
- E(2)-equivariance is **not applicable**, regardless of architecture

**Analogy**: Using E(2)-equivariance for coordinate-free ATSP is like using CNN for non-image data—the inductive bias doesn't match the problem structure.

---

## 4. Addressing "Inherent Symmetries Are Largely Removed"

The reviewer's statement about symmetries requires clarification. There are **two distinct types of symmetry**:

### 4.1 Cost Symmetry (d_ij = d_ji)

**In Symmetric TSP**:
- d_ij = d_ji for all i, j
- This is a **problem property**, not a network property

**In ATSP**:
- d_ij ≠ d_ji (by definition)
- This symmetry is indeed broken

### 4.2 Geometric Symmetry (E(2) Equivariance)

**Key insight**: Geometric symmetry is **orthogonal to cost symmetry**

Even in ATSP (if coordinates exist), geometric transformations remain valid:
- **Rotation**: Rotate the problem → Solution rotates identically
- **Translation**: Shift the problem → Solution shifts identically
- **Scale**: Scale the problem → Solution scales identically

**Example**: Consider ATSP with wind effects
- Cities at coordinates: {(0,0), (1,0), (0,1), (1,1)}
- Wind from east increases eastward travel cost
- **Rotate entire instance 90°**: Wind now from north, costs rotate accordingly
- **Optimal tour rotates 90°** (E(2)-equivariance still holds!)

**The reviewer's concern** conflates these two symmetries:
- **Cost asymmetry** (d_ij ≠ d_ji): Problem-specific
- **Geometric symmetry** (E(2)): Coordinate-space property

For **MatNet's ATSP**:
- ✗ No cost symmetry (d_ij ≠ d_ji)
- ✗ No geometric symmetry (no coordinates!)

For **hypothetical geometric ATSP**:
- ✗ No cost symmetry (d_ij ≠ d_ji)
- ✓ Has geometric symmetry (E(2)-equivariance applies)

---

## 5. Scope and Contributions of EDISCO

### 5.1 What EDISCO is Designed For

✓ **Euclidean TSP** with 2D coordinates
✓ **Geometric routing problems** in physical space
✓ **Problems where E(2)-equivariance provides inductive bias**

### 5.2 What EDISCO Cannot Handle

✗ **Coordinate-free ATSP** (MatNet's formulation)
✗ **Abstract cost matrices** without geometric structure
✗ **Problems where no meaningful coordinate embedding exists**

### 5.3 Our Contribution

**Claim**: EDISCO demonstrates that **E(2)-equivariant diffusion models** achieve state-of-the-art performance on geometric TSP instances.

**Scope**: Euclidean TSP where:
- Coordinates are given as input
- Distances are symmetric (d_ij = ||p_j - p_i||)
- Geometric structure is meaningful

**We do NOT claim**:
- EDISCO can replace MatNet for coordinate-free problems
- E(2)-equivariance is universally applicable to all TSP variants
- Our approach works without coordinates

---

## 6. Complementary Approaches in the Literature

The neural combinatorial optimization literature has developed **specialized tools for different problem types**:

| Problem Type | Coordinates? | Representative Methods | Reference |
|--------------|--------------|----------------------|-----------|
| **Euclidean TSP** | ✓ Yes | POMO, AM, T2T, EDISCO | [2, 3, 8] |
| **Coordinate-free ATSP** | ✗ No | MatNet, GREAT | [1, 7] |
| **Graph problems (MIS, etc.)** | ✗ No | DIFUSCO, GNN-based | [4] |

**This specialization is healthy for the field**:
- Different architectures exploit different problem structures
- No single approach dominates all problem types
- Practitioners can choose based on problem characteristics

---

## 7. Related Work: Other Methods and ATSP

To provide context, we survey how other neural TSP solvers relate to coordinate-free ATSP:

**Cannot handle coordinate-free ATSP**:
- **Pointer Networks** [9]: RNN on city coordinates (Euclidean TSP)
- **Bello et al. RL** [10]: Policy network on coordinates (Euclidean TSP)
- **Joshi et al. GCN** [11]: Graph convolutions on 2D Euclidean graphs
- **Attention Model (AM)** [3]: Transformer encoder on coordinates
- **POMO** [2]: RL with multiple starting points (uses AM architecture, coordinates)
- **T2T** [8]: Diffusion on Euclidean TSP

**Can handle coordinate-free ATSP**:
- **MatNet** [1]: Matrix encoding for arbitrary cost matrices
- **GREAT** [7]: Dual attention heads for incoming/outgoing edges
- **Zhou et al.** [12]: Learning-guided MCTS with directional vectors
- **Gaile et al.** [13]: Unsupervised GNN with ILP-based loss

**Key observation**: All methods that handle coordinate-free ATSP use explicit mechanisms to encode directional edge information (D[i,:] and D[:,i] separately) or full matrix encoding. None rely on geometric inductive biases.

---

## 8. Theoretical Perspective: When is E(2)-Equivariance Beneficial?

### 8.1 Conditions for E(2)-Equivariance to Help

E(2)-equivariance provides benefits when:

1. **Geometric structure exists**: Problem defined on ℝ² or ℝ³
2. **Solutions respect rigid transformations**: Optimal solution transforms with input
3. **Sample efficiency matters**: Symmetry reduces effective hypothesis space

### 8.2 When E(2)-Equivariance Cannot Help

E(2)-equivariance provides **no benefit** when:

1. **No coordinates**: Problem defined on abstract graphs/matrices
2. **Non-geometric costs**: Distances don't reflect spatial relationships
3. **Symmetry doesn't hold**: Costs depend on absolute orientation (e.g., compass-based rules)

**MatNet's ATSP falls squarely in category 1**—no coordinates means E(2)-equivariance is undefined, not just unhelpful.

---

## 9. Clarifications for the Paper

Based on this discussion, we propose the following clarifications:

### 9.1 Scope Statement (Introduction)

**Add**: "EDISCO is designed for geometric TSP instances with 2D coordinates. For coordinate-free problems such as those with arbitrary asymmetric cost matrices [1], alternative approaches like matrix encoding networks are more appropriate."

### 9.2 Related Work (Add Subsection)

**Title**: "Neural Methods for Asymmetric TSP"

**Content**: "Unlike Euclidean TSP, the asymmetric TSP (ATSP) does not assume d_ij = d_ji. MatNet [1] addresses ATSP by encoding the full cost matrix using one-hot embeddings for cities, achieving 1.34% optimality gap on 50-node instances. GREAT [7] uses dual attention heads for incoming/outgoing edges. These methods handle abstract cost matrices without geometric coordinates. In contrast, EDISCO's E(2)-equivariant architecture requires coordinate input and is designed for geometric routing problems. Future work could explore whether E(2)-equivariance benefits geometric ATSP variants (e.g., with wind or elevation affecting symmetric base distances), though this represents a different problem formulation than MatNet's coordinate-free setting."

### 9.3 Limitations Section

**Add**: "EDISCO requires 2D coordinates as input and cannot handle coordinate-free problems such as MatNet's ATSP formulation [1]. This is an architectural constraint—E(2)-equivariance operates on coordinate space and is undefined without geometric structure. For problems where no meaningful coordinate embedding exists, matrix encoding approaches [1, 7] are more suitable."

---

## 10. Conclusion

### 10.1 Direct Answer to Reviewer

**Question**: "How does EDISCO perform on ATSP cases defined in MatNet?"

**Answer**: EDISCO cannot be applied to MatNet's coordinate-free ATSP formulation because:

1. **Architectural requirement**: E(2)-equivariant networks require coordinates for their operations
2. **Fundamental limitation**: Without coordinates, E(2)-equivariance is undefined
3. **No workaround**: Coordinate reconstruction from asymmetric cost matrices is ill-posed

This is **not a weakness of EDISCO**—it's a natural consequence of targeting different problem formulations:
- **MatNet**: Designed for coordinate-free problems with arbitrary costs
- **EDISCO**: Designed for geometric problems with coordinate-based structure

### 10.2 Broader Perspective

The neural combinatorial optimization field benefits from **architectural diversity**:

- E(2)-equivariant methods (EDISCO): Excel on geometric problems
- Matrix encoding methods (MatNet, GREAT): Excel on coordinate-free problems
- Modular diffusion frameworks (DIFUSCO): Flexible across problem types

**These are complementary tools**, not competing approaches. Practitioners should select methods based on:
1. Whether coordinates are available and meaningful
2. Whether geometric structure exists in the problem
3. What inductive biases match the problem characteristics

### 10.3 Our Contribution Remains Valid

EDISCO's contribution—demonstrating that **E(2)-equivariant continuous diffusion** achieves strong performance on Euclidean TSP—is orthogonal to MatNet's contribution of handling coordinate-free ATSP. Both advance the field by addressing different problem spaces.

We appreciate the reviewer's question as it helps clarify the scope and applicability of E(2)-equivariant methods in combinatorial optimization.

---

## References

[1] Y.-D. Kwon, J. Choo, I. Yoon, M. Park, D. Park, and Y. Gwon, "Matrix encoding networks for neural combinatorial optimization," in *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 34, 2021, pp. 5138–5149.

[2] Y.-D. Kwon, J. Choo, B. Kim, I. Yoon, S. Min, and Y. Gwon, "POMO: Policy optimization with multiple optima for reinforcement learning," in *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 33, 2020, pp. 21188–21198.

[3] W. Kool, H. van Hoof, and M. Welling, "Attention, learn to solve routing problems!" in *International Conference on Learning Representations (ICLR)*, 2019.

[4] Z. Li, Q. Yang, L. Chen, and S. Yan, "DIFUSCO: Graph-based diffusion solvers for combinatorial optimization," in *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 36, 2023.

[5] V. G. Satorras, E. Hoogeboom, and M. Welling, "E(n) equivariant graph neural networks," in *International Conference on Machine Learning (ICML)*, 2021, pp. 9323–9332.

[6] T. F. Cox and M. A. A. Cox, *Multidimensional Scaling*, 2nd ed. Boca Raton, FL: Chapman and Hall/CRC, 2000.

[7] T. Kuhn, J. Hintze, P. Ruckel, J. Kotary, B. Dilkina, and S. Tschiatschek, "A GREAT architecture for edge-based graph problems like TSP," *arXiv preprint arXiv:2408.16717*, 2024.

[8] S. Qiu, Z. Sun, and Y. Ye, "Time to evolve: Fast and accurate generative diffusion models for combinatorial optimization over graphs," *arXiv preprint arXiv:2403.04695*, 2024.

[9] O. Vinyals, M. Fortunato, and N. Jaitly, "Pointer networks," in *Advances in Neural Information Processing Systems (NIPS)*, vol. 28, 2015, pp. 2692–2700.

[10] I. Bello, H. Pham, Q. V. Le, M. Norouzi, and S. Bengio, "Neural combinatorial optimization with reinforcement learning," in *International Conference on Learning Representations (ICLR) Workshop*, 2017.

[11] C. K. Joshi, T. Laurent, and X. Bresson, "An efficient graph convolutional network technique for the travelling salesman problem," *arXiv preprint arXiv:1906.01227*, 2019.

[12] F. Zhou, W. Chen, H. Xiong, J. Song, and L. Itti, "Learning-guided local search for asymmetric traveling salesman problem," in *International Conference on Learning Representations (ICLR)*, 2025.

[13] D. Gaile, S. D'Angelo, and M. Keller-Ressel, "Unsupervised training for neural TSP solver," in *Asian Conference on Machine Learning (ACML)*, 2022, pp. 913–928.

---

## Appendix: Technical Deep Dive

### A. Why Coordinate Reconstruction Doesn't Work

**Problem**: Given asymmetric distance matrix D with d_ij ≠ d_ji, can we reconstruct coordinates?

**Classical MDS** [6]:
- Input: Symmetric distance matrix D (d_ij = d_ji)
- Output: Coordinates X ∈ ℝ^(n×2) such that ||x_i - x_j|| ≈ d_ij
- **Requirement**: Distances must be metric (triangle inequality + symmetry)

**For ATSP**:
- D is asymmetric: d_ij ≠ d_ji
- Could symmetrize: D_sym = (D + D^T)/2
- **Problem**: This loses the asymmetry that defines ATSP!
- Reconstructed coordinates would only reflect symmetric component

**Conclusion**: No principled way to extract coordinates from asymmetric costs while preserving asymmetry information.

### B. Mathematical Formulation of E(2)-Equivariance

**E(2) Group**: Euclidean transformations in 2D
- Rotations: R ∈ SO(2)
- Translations: t ∈ ℝ²
- Combined: (x → Rx + t)

**Equivariance Property**:
```
f(Rx + t, h) = (Rf(x, h), h')
```

where:
- f is the network function
- x are coordinates
- h are features
- Coordinates transform equivariantly
- Features transform invariantly

**Critical observation**: This definition **requires coordinates x** as input. Without coordinates, the left-hand side (Rx + t) is undefined.

### C. DIFUSCO's Architecture vs EDISCO's Architecture

**DIFUSCO** [4]:
```python
# Encoding (one-time)
node_features = encoder(coordinates)  # or encoder(graph)

# Diffusion process
for t in reverse(timesteps):
    # Standard GNN operates on features only
    node_features = anisotropic_gnn(node_features, edges)
    node_features = denoise_step(node_features, t)

# Decoder
solution = extract_tour(node_features)
```

**Key**: Coordinates used only for initial encoding, then discarded. GNN processes features.

**EDISCO**:
```python
# Initial encoding
node_features = initial_encoder(coordinates)
coords = coordinates

# Diffusion process
for t in reverse(timesteps):
    # EGNN updates BOTH features AND coordinates
    node_features, coords = egnn_layer(node_features, coords, edges)
    node_features, coords = denoise_step(node_features, coords, t)

# Decoder
solution = extract_tour(node_features, coords)
```

**Key**: Coordinates evolved throughout the network. Cannot work without them.

---

**End of Rebuttal**

*This document provides a focused response explaining why E(2)-equivariant methods like EDISCO cannot handle coordinate-free ATSP formulations, clarifying this as an architectural constraint rather than a limitation, and positioning EDISCO's contribution appropriately.*
