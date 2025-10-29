# 🧠 Awesome Graphs into LLMs  
*A curated survey and resource list on bringing complex geometric information to Large Language Models (LLMs)*  

[![Paper](https://img.shields.io/badge/Paper-LOG%202025-deepgreen)](https://arxiv.org/abs/25XX.XXXXX)  
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)  
[![Survey](https://img.shields.io/badge/Survey-Graph%20Parametric%20Representation-purple)](#survey)  

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
|---|---|---|---|---|---|---|
| Graphs | Densification Law [34] | Density degree α | Macro | Low | Dynamic | e(t) ∝ n(t)^α, α ∈ [1, 2], e(t) is number of edges |
| Graphs | Shrinking Law [34] | Effective diameter d | Macro | Low | Dynamic | dt+1 < dt, diameter decreases as network grows |
| Graphs | Motif Differing Law(1) [45] | Number of motifs n | Macro | High | Dynamic | Different domains have different motif counts |
| Graphs | Egonet Differing Law [4] | Egonet features X | Macro | High | Static | Features differ across domains |
| Graphs | Simplicial Closure Law [4] | Closure prob. p | Macro | High | Static | p increases with tie strength |
| Graphs | Spectral Power Law [14] | Degree/eigen/SVD dist. | Macro | High | Static | Distributions often follow power-law |
| Graphs | Edge Attachment Law [33] | Node degree d | Micro | Low | Dynamic | pe(d) ∝ d |
| Graphs | Triangle Closure Law [25] | Triangular edges | Micro | Low | Dynamic | Stronger ties → less weakening |
| Hypergraphs | Densification Law [31] | α | Macro | High | Dynamic | e(t) ∝ n(t)^α, α ≥ 1 |
| Hypergraphs | Shrinking Law [31] | Diameter d | Macro | High | Dynamic | Diameter decreases as graph grows |
| Heterographs | Densification Law [58] | α, meta-path count | Macro | Low | Dynamic | Some meta-paths densify |

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
