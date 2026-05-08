# Organisational Knowledge Graph

Mapping internal communication networks from email data using graph theory, NLP, and interactive visualisation.

---

## What This Project Demonstrates

- Graph construction and analysis using NetworkX (PageRank, betweenness centrality, eigenvector centrality)
- Unsupervised community detection via greedy modularity optimisation
- NLP topic modelling with Latent Dirichlet Allocation (LDA)
- End-to-end data pipeline from raw email parsing to interactive visualisation
- Static and interactive network graph rendering with Matplotlib and PyVis

---

## Overview

This project builds a knowledge graph from internal organisational email data to reveal hidden communication structures, influence hierarchies, and topic-based communities — things that never appear on an org chart.

Using the Enron email dataset as a realistic, large-scale example, the notebook walks through the full pipeline from raw email parsing to an interactive, explorable network graph.

---

## Analysis Layers

| Analysis | What it answers |
|---|---|
| Network graph | Who talks to whom, and how often? |
| PageRank | Who holds real influence — not just seniority? |
| Betweenness centrality | Who are the gatekeepers and information brokers? |
| Community detection | What informal groups form outside the org chart? |
| LDA topic modelling | What does each community actually talk about? |
| Community × Topic heatmap | Which teams own which subjects? |

---

## Project Structure

```
├── org_knowledge_graph.ipynb   # Main notebook — run this
├── emails.csv                  # Enron dataset (download separately)
├── requirements.txt            # Python dependencies
├── overview_charts.png         # Output: bar charts (auto-generated)
├── topic_heatmap.png           # Output: community × topic matrix (auto-generated)
├── network_static.png          # Output: static network graph (auto-generated)
└── knowledge_graph.html        # Output: interactive browser graph (auto-generated)
```

---

## Getting Started

### 1. Clone the repo

```bash
git clone <your-repo-url>
cd <repo-folder>
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Download the dataset

The project uses the [Enron Email Dataset](https://www.kaggle.com/datasets/wcukierski/enron-email-dataset) from Kaggle.

```bash
# Option A — Kaggle CLI
kaggle datasets download -d wcukierski/enron-email-dataset
unzip enron-email-dataset.zip   # produces emails.csv

# Option B — Manual
# Go to https://www.kaggle.com/datasets/wcukierski/enron-email-dataset
# Download and unzip emails.csv into the project folder
```

### 4. Run the notebook

```bash
jupyter notebook org_knowledge_graph.ipynb
```

Run all cells top to bottom. Outputs are saved automatically.

---

## Configuration

All tunable parameters are in the **Configuration cell** near the top of the notebook:

| Parameter | Default | Description |
|---|---|---|
| `CSV_PATH` | `"emails.csv"` | Path to the dataset |
| `MAX_ROWS` | `20000` | Emails to load — more = richer graph, slower |
| `N_TOPICS` | `8` | Number of LDA topics to discover |
| `GRAPH_TOP_N` | `120` | Nodes shown in the interactive graph |
| `STATIC_TOP_N` | `80` | Nodes shown in the static plot |
| `MIN_EMAILS_NODE` | `5` | Minimum emails to include a person (noise filter) |

---

## Outputs

**Overview Charts** (`overview_charts.png`) — Four bar charts: top senders, top by PageRank, top brokers by betweenness centrality, and community size distribution.

**Topic Heatmap** (`topic_heatmap.png`) — A matrix showing how strongly each detected community leans toward each LDA topic — the core view for understanding what each group talks about.

**Static Network** (`network_static.png`) — Spring-layout graph of the top 80 people. Node size = influence, colour = community, edge thickness = email volume.

**Interactive Graph** (`knowledge_graph.html`) — Open in any browser. Hover nodes for full metrics, drag to rearrange, scroll to zoom. Each node tooltip shows PageRank, betweenness, community, and topic labels.

---

## Methods

- **Graph construction** — NetworkX directed weighted graph; edge weight = email count between two people
- **Centrality** — PageRank, betweenness, and eigenvector centrality via NetworkX
- **Community detection** — Greedy modularity optimisation (`networkx.community`)
- **Topic modelling** — LDA via scikit-learn; bigram features, custom email stopword list
- **Visualisation** — Matplotlib for static plots, PyVis (vis.js) for the interactive graph

---

## Requirements

```
pandas
numpy
networkx
matplotlib
scikit-learn
gensim
pyvis
```

---

## Data Source

**Enron Email Dataset** — collected during the FERC investigation into Enron Corporation. Contains ~500,000 emails from ~150 senior employees, making it the largest publicly available organisational email corpus.

- Source: [Kaggle — wcukierski/enron-email-dataset](https://www.kaggle.com/datasets/wcukierski/enron-email-dataset)
- Originally prepared by William Cohen at Carnegie Mellon University

> This dataset is used purely for demonstrative and analytical purposes.

---

## Adapting to Your Own Data

To use this with real internal data, replace the data loading step with your own email export. The minimum required fields per email are:

- `sender` — email address string
- `recipients` — list of email address strings
- `body` / `subject` — text content for topic modelling

Change the `DOMAIN` variable in the config cell to match your organisation's email domain.
