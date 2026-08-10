# Reliability of RAG Evaluation Metrics in Financial Question Answering

This repository contains the artefact developed for an MSc Data Science dissertation at Coventry University. The project evaluates the reliability of three RAGAS metrics—**Faithfulness**, **Answer Relevancy**, and **Context Precision**—across three benchmark datasets:

- **RAGBench (FinQA):** baseline evaluation using pre-supplied financial evidence and existing annotations.
- **MultiHop-RAG:** retrieval and reasoning stress test using a FAISS-based retrieval pipeline.
- **FinanceBench:** independent financial benchmark used to check whether patterns observed in FinQA are reproduced.

The repository includes four Google Colab notebooks and the CSV outputs generated during evaluation.

## Research Aim

To evaluate the reliability of selected Retrieval-Augmented Generation evaluation metrics in financial question-answering tasks, identify their limitations, and provide recommendations for more effective domain-specific evaluation.

## Repository Structure

```text
rag-metric-reliability-finance/
├── notebooks/
│   ├── 01_ragbench_finqa_evaluation.ipynb
│   ├── 02_multihop_rag_evaluation.ipynb
│   ├── 03_financebench_evaluation.ipynb
│   └── 04_cross_dataset_analysis.ipynb
├── results/
│   ├── ragbench_finqa_final_results.csv
│   ├── multihop_rag_final_results.csv
│   ├── financebench_final_results.csv
│   └── all_datasets_combined_results.csv
├── figures/
│   ├── cross_dataset_metric_comparison.png
│   └── calculation_vs_lookup_faithfulness.png
├── requirements.txt
├── .gitignore
└── README.md
```

Rename the notebooks in this structure if their current filenames are different.

## Notebook Roles

### 1. RAGBench (FinQA) Evaluation

Loads the FinQA subset of RAGBench, prepares a reproducible sample, categorises questions as calculation or lookup, and recomputes the selected RAGAS metrics using pre-supplied evidence.

### 2. MultiHop-RAG Evaluation

Builds the most technically substantial part of the artefact:

1. Loads and prepares the MultiHop-RAG article corpus.
2. Generates embeddings using `sentence-transformers/all-MiniLM-L6-v2`.
3. Creates a FAISS vector index.
4. Retrieves the top three documents for each query.
5. Generates responses using Llama 3.1 through Groq.
6. Evaluates the responses using RAGAS.

### 3. FinanceBench Evaluation

Adapts the financial evaluation workflow to FinanceBench and uses its native reasoning labels to compare numerical reasoning with information-extraction questions.

### 4. Cross-Dataset Analysis

Combines the three result files, calculates summary statistics, compares metric behaviour across datasets and question types, and produces the final charts used in the dissertation.

## Evaluation Configuration

| Component | Implementation |
|---|---|
| Programming language | Python |
| Development environment | Google Colab |
| Evaluation framework | RAGAS |
| Judge LLM | `llama-3.1-8b-instant` through Groq |
| Embedding model | `sentence-transformers/all-MiniLM-L6-v2` |
| Retrieval index | FAISS |
| Metrics | Faithfulness, Answer Relevancy, Context Precision |

Context Precision was implemented using the variant most appropriate to each dataset:

- **RAGBench (FinQA):** reference-free
- **FinanceBench:** reference-free
- **MultiHop-RAG:** reference-based

## Sample Sizes

| Dataset | Evaluated sample |
|---|---:|
| RAGBench (FinQA) | 15 |
| MultiHop-RAG | 15 |
| FinanceBench | 16 |
| **Combined** | **46** |

The samples were limited by free-tier API constraints. Fixed random seeds were used where sampling was required to support reproducibility.

## Main Findings

The results indicate that metric reliability depends on the retrieval and reasoning demands of the task.

- MultiHop-RAG produced the lowest average scores across the three metrics, reflecting the additional difficulty of retrieval and cross-document reasoning.
- Faithfulness was lower for calculation or numerical-reasoning questions than for direct lookup or information-extraction questions in both financial datasets.
- Answer Relevancy sometimes penalised short but correct responses.
- High Faithfulness and Context Precision did not always indicate a correct final answer, demonstrating that groundedness and correctness are related but distinct concepts.

These findings are exploratory because the evaluated samples are small.

## Installation

The notebooks were designed for Google Colab. Install the required libraries at the beginning of a Colab session:

```bash
pip install -r requirements.txt
```

Package versions may need to remain pinned because compatibility issues were observed between RAGAS and newer LangChain releases.

## API Configuration

The evaluation uses the Groq API. Store the API key securely as an environment variable or Colab secret.

```python
import os

GROQ_API_KEY = os.environ["GROQ_API_KEY"]
```

Do **not** place API keys directly in notebooks or commit them to GitHub.

## Running the Project

Run the notebooks in this order:

1. `01_ragbench_finqa_evaluation.ipynb`
2. `02_multihop_rag_evaluation.ipynb`
3. `03_financebench_evaluation.ipynb`
4. `04_cross_dataset_analysis.ipynb`

The first three notebooks create dataset-specific CSV files. The final notebook combines these outputs and generates cross-dataset summaries and charts.

## Reproducibility

The artefact supports reproducibility through:

- fixed random seeds;
- saved intermediate and final CSV files;
- documented sample sizes;
- pinned package versions;
- consistent judge-LLM and embedding-model settings;
- separate notebooks for dataset evaluation and combined analysis.

API-based LLM evaluation may still produce minor variation between runs.

## Ethical and Legal Considerations

The project uses publicly available benchmark datasets and does not process personal or confidential data. Dataset licences and original research sources should be reviewed and acknowledged before redistribution. The repository should contain only code, derived results and files permitted by the relevant licences.

## Limitations

- Small sample sizes due to free-tier API restrictions.
- Use of a single judge-LLM family.
- Dataset-specific Context Precision variants limit direct comparison of absolute values.
- Rule-based question categorisation was used for FinQA.
- LLM-based evaluation may be sensitive to model and prompt choices.

## Citation

```bibtex
@mastersdissertation{shrestha2026ragmetrics,
  author  = {Ayusha Shrestha},
  title   = {Evaluating the Reliability of Retrieval-Augmented Generation Evaluation Metrics in Financial Question Answering},
  university  = {Coventry University},
  year    = {2026}
}
```

## Author

**Ayusha Shrestha (11494885)**  
MSc Data Science  
Coventry University
