# dimensionality-reduction-tSNE-UMAP-PCA

Here’s a concise breakdown of the three dimensionality-reduction techniques—PCA, t-SNE, and UMAP—covering what they do, how they work at a high level, and when you might choose each.

---

## Principal Component Analysis (PCA)

**What it is**
A **linear** technique that finds the directions (principal components) of maximum variance in your data and projects onto the top k of those.

**How it works**

1. Center your data (subtract the mean).
2. Compute the covariance matrix.
3. Solve for its eigenvalues λ₁ ≥ λ₂ ≥ … and eigenvectors.
4. Project onto the eigenvectors corresponding to the largest eigenvalues.

**Pros**

* **Fast & deterministic**: closed-form solution via eigendecomposition or SVD.
* **Global structure**: preserves overall variance (largest-scale patterns).
* **Interpretable**: principal components are linear combinations of original features.

**Cons**

* **Linear only**: can’t unravel complex nonlinear manifolds.
* **Can miss fine clusters**: if small groups don’t contribute much variance.

**Typical use**

* Quick exploratory plots of high-dimensional data
* Preprocessing/denoising before clustering or classification
* Feature reduction for linear models

---

## t-Distributed Stochastic Neighbor Embedding (t-SNE)

**What it is**
A **nonlinear**, probabilistic method that emphasizes preservation of **local** structure (“neighbors”) when mapping high-D → low-D.

**How it works (high-level)**

1. Build a probability distribution over pairwise distances in the original space (neighbors get higher probability).
2. Define a similar distribution in low-D, but use a Student-t kernel to allow far-apart points to stay apart.
3. Minimize the Kullback–Leibler divergence between these two distributions via gradient descent.

**Pros**

* **Local fidelity**: excels at revealing tight clusters and fine structure.
* **Visual clarity**: clusters often well separated in 2D/3D plots.

**Cons**

* **Slow/scales poorly**: O(N²) distance computations (though approximate variants exist).
* **Non-deterministic**: different random seeds can yield different layouts.
* **Global distortions**: distances between clusters aren’t always meaningful.

**Typical use**

* Visualizing high-dimensional embeddings (e.g. word vectors, neural activations)
* Exploratory cluster discovery when local grouping matters most

---

## Uniform Manifold Approximation and Projection (UMAP)

**What it is**
A **nonlinear** method grounded in manifold theory, aiming to preserve both **local** and some **global** structure, often faster than t-SNE.

**How it works (high-level)**

1. Construct a fuzzy simplicial complex (graph) in high-D based on k-nearest neighbors.
2. Optimize a low-D embedding by matching that fuzzy topological structure, using a cross-entropy loss.

**Pros**

* **Speed & scalability**: often much faster than t-SNE on large datasets.
* **Balances structure**: preserves local neighborhoods while giving more faithful global layout.
* **Deterministic-ish**: more consistent between runs (though still has randomness).

**Cons**

* **Hyperparameters matter**: choices like `n_neighbors` and `min_dist` strongly affect results.
* **Less interpretable**: embedding axes aren’t linear combinations of inputs.

**Typical use**

* Large-scale visualization (datasets with tens or hundreds of thousands of points)
* When you need a faster alternative to t-SNE with better global context

---

## When to pick which?

* **PCA**:

  * You need speed, reproducibility, and an interpretable reduction.
  * You suspect your data lie roughly on a linear subspace.

* **t-SNE**:

  * Your main goal is to explore or present tight clusters in a small-to-medium dataset.
  * Exact global distances aren’t critical.

* **UMAP**:

  * You want many of t-SNE’s local-clustering benefits but on larger data, faster.
  * You’d like some preservation of large-scale geometry as well.

All three tools are invaluable in different scenarios. Often you’ll start with PCA to get a quick sense, then move to UMAP or t-SNE for deeper cluster insights.
