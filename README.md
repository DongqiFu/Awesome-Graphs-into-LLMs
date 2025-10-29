# 🧠 Awesome Graphs into LLMs  
*A curated survey and resource list on bringing complex geometric information to Large Language Models (LLMs)*  

[![Paper](https://img.shields.io/badge/Paper-LOG%202025-deepgreen)](https://arxiv.org/abs/25XX.XXXXX)  [![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)  [![Survey](https://img.shields.io/badge/Survey-Graph%20Parametric%20Representation-purple)](#survey)  

---

## 🧩 Current Strategies for Language Models Using Graph Data as Input (i.e., Section 2)

| # | Strategy | Core Idea | Example Works |
|:--:|:--|:--|:--|
| 1️⃣ | **Topological Query** | Concatenate text of structurally nearby nodes (e.g., 1-hop neighbors) | GraphPrompter, GPT4Graph |
| 2️⃣ | **Semantic Query** | Retrieve nodes by semantic similarity instead of topology | FewshotRAG, AuGLM |
| 3️⃣ | **GNN Embedding** | Use GNN to generate latent embeddings as soft prompts for LLM | LlaGA, PromptGFM |
| 4️⃣ | **GNN Prediction** | Inject interpretable GNN outputs (logits → labels) into LLM | GraphICL, TEA-GLM |

| Method | Year | Backbone LLM | Fine-Tuning | Strategy 1️⃣ | Strategy 2️⃣ | Strategy 3️⃣ | Strategy 4️⃣ | Task Level | GitHub |
|---|---:|---|:--:|:--:|:--:|:--:|:--:|---|---|
| NLGraph |  2023 | GPT | No | ✓ |  |  |  | Node, Link, Graph | [Code](https://github.com/Arthur-Heng/NLGraph) |
| GPT4Graph |  2023 | GPT | No | ✓ |  |  |  | Node, Link, Graph | — |
| LLM4GT |  2023 | GPT | No | ✓ |  |  |  | Node, Link | — |
| GraphText |  2023 | GPT | No | ✓ |  |  |  | Node | [Code](https://github.com/AndyJZhao/GraphText) |
| DGTL |  2023 | Llama | Yes |  | ✓ |  |  | Node | — |
| InstructGLM |  2024 | T5, Llama | Yes |  | ✓ |  |  | Node | [Code](https://github.com/agiresearch/InstructGLM) |
| LLaGA |  2024 | Llama | Yes |  | ✓ |  |  | Node, Link | [Code](https://github.com/VITA-Group/LLaGA) |
| AuGLM |  2024 | T5 | Yes |  | ✓ | ✓ |  | Node | — |
| GraphPrompter |  2024 | Llama | Yes | ✓ |  |  |  | Node, Link | [Code](https://github.com/franciscoliu/graphprompter) |
| G-Recall |  2024 | GPT, Gemini | No |  |  |  | ✓ | Subgraph | — |
| TLG |  2024 | PaLM | No |  |  |  | ✓ | Node, Link, Subgraph | — |
| LLM4DyG |  2024 | GPT, Llama | No |  |  |  | ✓ | Temporal | [Code](https://github.com/wondergo2017/LLM4DyG) |
| SNS |  2024 | GPT | No |  | ✓ |  |  | Node | — |
| MuseGraph |  2024 | BART, T5, Llama | Yes |  | ✓ |  |  | Node, Graph | — |
| OFA |  2024 | Llama | No |  |  | ✓ |  | Node, Link, Graph | [Code](https://github.com/LechengKong/OneForAll) |
| GraphGPT |  2024 | Llama | Yes |  | ✓ |  |  | Node, Link | [Code](https://github.com/HKUDS/GraphGPT) |
| Graph-CoT |  2024 | GPT | No |  |  |  | ✓ | Node | [Code](https://github.com/PeterGriffinJin/Graph-CoT) |
| TEA-GLM |  2024 | Vicuna | Yes |  |  | ✓ |  | Node, Link | [Code](https://github.com/W-rudder/TEA-GLM) |
| GPEFT |  2024 | Llama | Yes |  |  | ✓ |  | Link | — |
| PromptGFM |  2025 | T5, Llama | Yes |  |  | ✓ |  | Node, Link | — |
| GraphICL |  2025 | Llama, GPT | No |  | ✓ |  | ✓ | Node, Link | — |
| UniGraph |  2025 | Llama | Yes |  | ✓ | ✓ |  | Node, Link, Graph | [Code](https://github.com/yf-he/UniGraph) |
| SKETCH |  2025 | Nomic, Llama | Yes |  | ✓ | ✓ |  | Node | — |
| TGTalker |  2025 | Qwen, Mistral, Llama | No |  |  |  | ✓ | Temporal | — |
| FewshotRAG | 2025 | Llama, Qwen, etc. | No |  | ✓ |  |  | Node | — |

---

## 🧮 Graph Parametric Representation Pipeline (i.e., Section 3)

1. **From Empirical Laws to Descriptors** – Extract graph-level statistics (α, diameter, clustering, spectral gap).  
2. **Language Grounding** – Serialize parameters into natural language templates.  
3. **Contextual Injection** – Feed as prompt prefix, retrieval snippet, or adapter input.  
4. **Reasoning Alignment** – LLM uses interpretable parameters for explainable graph reasoning.

### Example Prompt

```yaml
GraphSummary:
  name: "example_graph"
  nodes: 102,345
  edges: 1,234,567
  densification_exponent_alpha: 1.12
  effective_diameter_90pct: 6.3
  avg_clustering_coef: 0.21
  degree_distribution: "heavy-tailed (power-law, gamma = 2.8)"
  motif_counts: {"triangle": 12345, "4-cycle": 2345}
  spectral_gap (second smallest eigenvalue): 0.015
  temporal_window: "2016-01-01 -- 2020-12-31"

Task: >
  Using the GraphSummary above, estimate [node classification / link prediction / ...]
  or answer Q: ...
  Provide reasoning and cite which parameter(s) you used.
```

---

## 🚀 Summary of Graph Parametric Representations (i.e., Section 4)

| Input | Law | Parameter | Scope | Order | Temporality | Description |
|:--|:--|:--|:--|:--|:--|:--|
| **Graphs** | [Densification Law](https://dl.acm.org/doi/10.1145/1081870.1081893) | Density degree $\\alpha$ | Macro | Low | Dynamic | $e(t) \\propto n(t)^{\\alpha},\\; \\alpha\\in[1,2]$, where $e(t)$ is #edges at time $t$ |
|  | [Shrinking Diameter Law](https://dl.acm.org/doi/10.1145/1081870.1081893) | Effective diameter $d$ | Macro | Low | Dynamic | $d_{t+1} < d_t$, diameter decreases as network grows |
|  | [Motif Differing Law (1)](https://dl.acm.org/doi/10.1145/3018661.3018733) | Similar motif counts $n$ | Macro | High | Dynamic | $n_1 \\ne n_2$ for different domains |
|  | [Motif Differing Law (2)](https://dl.acm.org/doi/10.1145/3018661.3018733) | Motif timestamp $t$ | Macro | High | Dynamic | $t_1 \\ne t_2$ for different motif types |
|  | [Egonet Differing Law](https://doi.org/10.1073/pnas.1800683115) | Egonet features $X$ | Macro | High | Static | Egonet statistics $X_1\\ne X_2$ across domains |
|  | [Simplicial Closure Law](https://doi.org/10.1073/pnas.1800683115) | Closure probability $p$ | Macro | High | Static | $p$ increases with tie strength or additional relations |
|  | [Spectral Power Law](https://dl.acm.org/doi/10.1145/3184558.3191527) | Degree/SVD/Eigen distributions | Macro | High | Static | These spectral distributions often follow power law |
|  | [Edge Attachment Law](https://snap.stanford.edu/class/cs224w-readings/leskovec08patterns.pdf) | Node degree $d$ and edge creation $p_e(d)$ | Micro | Low | Dynamic | $p_e(d) \\propto d$ for nodes of degree $d$ |
|  | [Triangle Closure Law (1)](https://snap.stanford.edu/class/cs224w-readings/leskovec08patterns.pdf) | Edges $e_1,e_2,e_3$ | Micro | Low | Dynamic | Strong $e_3$ ⇒ $e_1,e_2$ less likely to weaken |
|  | [Triangle Closure Law (2)](https://snap.stanford.edu/class/cs224w-readings/leskovec08patterns.pdf) | Edges $e_1,e_2,e_3$ | Micro | Low | Dynamic | Strong $e_1,e_2$ ⇒ unlikely to weaken |
|  | [Local Closure Law](https://doi.org/10.1007/s41109-020-00283-0) | Local closure coef. $H(u)$ | Micro | Low | Static | $H(u)=\\frac{2T(u)}{W^g(u)}$, see Appendix A |
|  | [Spectral Density Law](https://epubs.siam.org/doi/10.1137/17M1114731) | Density of states $\\mu(\\lambda)$ | Macro | High | Static | $\\mu(\\lambda)=\\frac{1}{N}\\sum_i\\delta(\\lambda-\\lambda_i)$ |
| | [Motif Activity Law](https://arxiv.org/abs/2112.03498) | Motif type / reappear rate | Micro | High | Dynamic | Motifs retain type and reappear at rate $r$ |
| **Hypergraphs** | [Degree Distribution Law](https://doi.org/10.1007/978-3-030-47436-2_7) | Node degree, link prob. | Macro | High | Dynamic | High-degree nodes more likely to form new links |
|  | [SVD Distribution Law](https://doi.org/10.1007/978-3-030-47436-2_7) | Singular value distribution | Macro | High | Static | Singular value spectra often heavy-tailed |
|  | [Diminishing Overlaps](https://dl.acm.org/doi/10.1145/3394486.3403338) | Overlap density DoI($H(t)$) | Macro | High | Dynamic | Overall hyperedge overlap decreases with $t$ |
|  | [Densification Law (Hypergraph)](https://dl.acm.org/doi/10.1145/3394486.3403338) | Density degree $\\alpha$ | Macro | High | Dynamic | $e(t) \\propto n(t)^{\\alpha},\\; \\alpha\\ge1$ |
|  | [Shrinking Law (Hypergraph)](https://dl.acm.org/doi/10.1145/3394486.3403338) | Effective diameter $d$ | Macro | High | Dynamic | $d_{t+1}<d_t$ as hypergraph grows |
|  | [Edge Interacting Law](https://doi.org/10.1016/j.physa.2018.12.030) | Interaction rate | Micro | High | Dynamic | Temporally adjacent edges have similar activity |
| **Heterographs** | [Densification Law (Hetero)](https://ieeexplore.ieee.org/document/7934374) | $\\alpha$, meta-path count | Macro | Low | Dynamic | Some meta-paths follow $e(t)\\propto n(t)^\\alpha$ |
|  | [Non-densification Law](https://ieeexplore.ieee.org/document/7934374) | $\\alpha$, meta-path count | Macro | Low | Dynamic | Some meta-paths do *not* follow densification law |

---

### Notation

- $e(t)$ — #edges at time $t$  
- $n(t)$ — #nodes at time $t$  
- $H(u)$ — local closure coefficient  
- $T(u)$ — number of triangles including node $u$  
- $W^g(u)$ — #potential triads around $u$  

---

**Citations:** Leskovec et al. 2005, 2008; Benson et al. 2018; Yin et al. 2019; Do et al. 2020; Eikmeier & Gleich 2017; Comrie & Kleinberg 2021; Shi et al. 2017; Massaro et al. 2020.


---

## 🌐 Citation  

If you find this repository useful, please consider citing our paper:

```bibtex
@inproceedings{fu2025graphlaw,
  title={Bring Complex Geometric Information to LLMs: A Positional Survey of Graph Parametric Representation},
  author={Dongqi Fu and Liri Fang and Zihao Li and Zhe Xu and Hanghang Tong and Vetle I. Torvik and Jingrui He},
  booktitle={Proceedings of the Fourth Learning on Graphs Conference (LoG 2025)},
  year={2025}
}
