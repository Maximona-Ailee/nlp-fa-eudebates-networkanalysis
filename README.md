# Project Title: Network Analysis of European Parliament Debates 🇪🇺

This project applies a full Natural Language Processing (NLP) pipeline, anchored by Large Language Models (LLMs), to parliamentary speeches. The goal is to transform raw, unstructured political text into a structured **Knowledge Graph** to uncover hidden patterns and power dynamics using Network Science metrics.

---

## 1. Dataset and Subset Rationale

| Detail | Description |
| :--- | :--- |
| **Source Dataset** | `RJuro/eu_debates` from Hugging Face |
| **Original Scale** | Over **30,000** EU Parliament speeches |
| **Subset Used** | A stratified **sample of 500 speeches** |
| **Rationale** | The full dataset is computationally prohibitive for a research project relying on resource-intensive, local Transformer models (RoBERTa, BART). A subset of 500 speeches was chosen to ensure **scalability** for the complex **extraction phase** and allow for **rigorous quality checking** during the **exploration phase**. The manageable size guarantees high success rate and reliability for the downstream graph construction. |

---

## 2. Methodology: Our 4-Step Pipeline



### Step 1: LLM Extraction (The Conversion Layer)

The core function is to convert unstructured text into structured, relational data (triples) using specialized local LLMs (Transformer models).

* **Sentiment Analysis:** Used the **RoBERTa model** for Positive/Neutral/Negative classification.
* **Topic Classification:** Used the **BART Zero-Shot Classifier** to categorize speech content against a predefined list of topics.
* **Party Detection:** Used a robust, **keyword-based classifier** to link speakers to their political group.
* **Output:** Each speech is converted into a structured JSON object containing **Speaker**, **Topics**, and **Sentiment**.

### Step 3: Network Construction (The Structure)

The structured JSON data is used to build a **Heterogeneous Knowledge Graph** using the `networkx` library.

* **Node Types (62 total):** Speakers, Political Parties (e.g., EPP, S&D), and Discussion Topics.
* **Relationship Types (169 total):** `member_of` (Speaker → Party) and `mentions` (Speaker → Topic).
* **Graph Type:** The resulting graph is a **bipartite network** between Parties and Topics.

### Step 4: Network Analysis (The Insight)

Advanced graph metrics are used to quantify influence and structural positioning.

* **Topic Centrality:** **Degree Centrality** was used to identify the most connected discussion topics.
* **Party Diversity:** **Degree Centrality** measured which parties engage with the widest range of issues, revealing **Generalists vs. Specialists**.
* **Influence Analysis:** **Betweenness Centrality** was used to pinpoint the topics that act as **"gatekeepers"** or **critical bridges** for communication between different political parties.

---

## 3. Key Findings and Limitations

### Main Findings

1.  **Core Pillars of Discourse:** The structural analysis confirms that the topics of **'Economy'** and **'Social Justice'** are the **gravitational centers** of EU political discourse, possessing the highest **Betweenness Centrality**.
2.  **Strategic Divide:** **Major political parties** (e.g., S&D, EPP) operate as **Generalists** (high topic diversity), while smaller, more specialized groups (e.g., Greens) maintain **focused influence**.
3.  **Scalability:** The pipeline demonstrates that accessible, local LLM architectures can successfully enable complex, scalable structural analysis of political data.

### Limitations and Future Work

| Limitation | Description | Future Work |
| :--- | :--- | :--- |
| **Topic Set** | Classification is limited to a small, predefined set of categories, potentially missing nuanced emerging topics. | Expand the topic taxonomy, possibly using unsupervised clustering (LDA). |
| **Party Detection** | The simple **keyword-based party detection** method is brittle. | Integrate Named Entity Recognition (NER) or leverage richer metadata for more robust mapping. |
| **Sentiment Nuance** | The simple Positive/Neutral/Negative sentiment model lacks the nuance needed for complex political language. | Implement advanced, fine-tuned political sentiment models. |

---

## 4. Environment Setup

To reproduce the analysis, install the required packages using the provided `requirements.txt` file.

```bash
pip install -r requirements.txt