# Power-grid-analysis-through-the-lens-of-graph-theory
## Overview
This report analyzes the Western United States power grid through the framework of **complex network theory**, identifying structural properties, vulnerabilities, and community structures.

The network consists of **4,941 nodes** (transformers and power relay points) and **6,594 edges** (power lines), and is undirected, unweighted, and planar.

---

## Methodology

### Structural Metrics
The following metrics were computed to characterize the network:
- **Density** — proportion of existing edges relative to all possible edges
- **Clustering Coefficient & Transitivity** — local and global measures of clustering
- **Average Path Length & Diameter** — measures of network spread
- **Degree Assortativity** — tendency of nodes to connect to similarly-connected nodes
- **Bipartivity** — how close the network is to being bipartite

### Centrality Measures
Seven centrality measures were computed and compared:
- Degree, Closeness, Betweenness, Eigenvector, Katz, PageRank, and Subgraph Centrality

### Degree Distribution Fitting
The degree distribution was fitted to five statistical models using Maximum Likelihood Estimation (via `scipy`):
- Power-law, Exponential, Power-law + Exponential, Mielke, and Poisson

### Network Model Comparisons
The real network was compared against three theoretical models, each simulated 10 times:
- **Erdős–Rényi** (random graph)
- **Barabási–Albert** (scale-free)
- **Watts–Strogatz** (small-world)

### Community Detection
Two algorithms were used to detect communities:
- **Louvain Method** (run 100 times to account for randomness)
- **Clauset-Newman-Moore Greedy Modularity Optimization**

Communities were evaluated using modularity, performance, and coverage metrics.

---

## Key Results

### Structural Properties
| Metric | Value |
|---|---|
| Nodes | 4,941 |
| Edges | 6,594 |
| Average Degree ⟨k⟩ | 2.67 |
| Density ρ | 5.40 × 10⁻⁴ |
| Clustering Coefficient C̄ | 0.080 |
| Transitivity C | 0.103 |
| Average Path Length ℓG | 18.99 |
| Diameter | 46 |
| Assortativity r | 0.003 |
| Bipartivity bs | 0.726 |

### Centrality Analysis
- **Closeness and Betweenness** centralities share 17 top nodes with each other but none with other metrics — they prioritize *inter-cluster connectors*.
- **Eigenvector, Katz, and Subgraph** centralities are nearly identical — they prioritize *cluster cores*.
- **Degree and PageRank** centralities share 17 top nodes, as both depend heavily on node degree.
- Betweenness centrality is highlighted as the most relevant for identifying **network vulnerabilities** and susceptibility to cascading failures.

### Degree Distribution
- Neither **power-law** nor **Poisson** distributions fit the data well.
- The best fits for the tail are the **Mielke** and **Exponential** distributions.
- The **Power-law + Exponential** achieves the highest log-likelihood (67.19) but uses more parameters.
- The **Mielke** fit (log-likelihood: 62.11) is considered the best overall, balancing tail accuracy and statistical robustness.
- The absence of a heavy tail is attributed to the **economic and planar constraints** of the power grid.

### Model Comparisons
No theoretical model successfully replicates all metrics of the real network:
- **Erdős–Rényi** — closest assortativity, but greatly underestimates clustering and path length
- **Barabási–Albert** — shortest path lengths, fails to capture clustering or diameter
- **Watts–Strogatz** — best approximation of clustering, but still overestimates it and underestimates path length
- A **random geometric graph** (planar by construction) is suggested as a more appropriate model for future work.

### Community Detection
| Algorithm | Communities | Modularity | Performance | Coverage |
|---|---|---|---|---|
| Louvain | 40.34 ± 0.18 | 0.9358 ± 0.0001 | 0.9657 ± 0.0001 | 0.9713 ± 0.0001 |
| Greedy Modularity | 43 | 0.9346 | 0.9671 | 0.9687 |

Both algorithms reveal a **well-defined modular structure** with high modularity and performance, consistent with the geographically clustered nature of the power grid.

---

## Conclusions
- The power grid is a **sparse, planar network** with low clustering and long path lengths, shaped by economic and geographic constraints.
- **Betweenness centrality** is the most informative measure for identifying vulnerable nodes — those acting as bridges between regional clusters.
- The degree distribution does not follow standard statistical models, with the **Mielke distribution** providing the best overall fit.
- No theoretical network model (ER, BA, WS) fully replicates the observed metrics, underlining the unique nature of infrastructure networks.
- Community detection reveals **40+ well-defined communities**, with high modularity values indicating strong internal cohesion.
- Nodes with high betweenness centrality represent the primary **vulnerabilities** of the network, as their failure could trigger cascading outages.
