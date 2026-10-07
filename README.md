# FinRAG Analyst — AI-Powered Fundamental Stock Analysis Using RAG

## Project Overview

**FinRAG Analyst** is a Retrieval-Augmented Generation (RAG) system designed for fundamental stock analysis.

The project uses **Microsoft Corporation (MSFT)** as the demonstration company.

Financial data is collected using Yahoo Finance, transformed into a structured financial knowledge base, converted into semantic embeddings using Sentence Transformers, and indexed using FAISS.

When a user asks a financial question, the system retrieves relevant financial evidence from the FAISS vector database and provides that evidence to Google Gemini. Gemini then generates a grounded financial analysis with references to the retrieved evidence.

The objective is to demonstrate how RAG can improve the grounding, traceability, and reliability of AI-generated financial analysis.

---

## Project Objective

The project implements an AI-powered financial analyst capable of:

- Collecting company and financial-statement data
- Calculating fundamental financial metrics
- Building a structured financial knowledge base
- Creating semantic embeddings
- Storing and retrieving embeddings using FAISS
- Answering natural-language financial questions
- Using Google Gemini for grounded financial analysis
- Providing evidence chunk citations
- Handling unsupported questions
- Evaluating retrieval and generation quality

---

## System Architecture

```text
Microsoft Corporation (MSFT)
            |
            v
      Yahoo Finance
            |
            v
+--------------------------------+
| Financial Data                 |
|                                |
| - Company Profile              |
| - Income Statement             |
| - Balance Sheet                |
| - Cash Flow Statement          |
| - Historical Stock Prices      |
+--------------------------------+
            |
            v
   Financial Data Processing
            |
            v
 Fundamental Metrics & Ratios
            |
            v
 Structured Financial
     Knowledge Base
            |
            v
    Metadata-Aware Chunking
            |
            v
 Sentence Transformer Embeddings
            |
            v
      FAISS Vector Index
            |
            |
            +-----------------------------+
                                          |
User Financial Question                   |
            |                             |
            v                             |
     Question Embedding                   |
            |                             |
            v                             |
      FAISS Semantic Search <-------------+
            |
            v
 Relevant Financial Evidence
            |
            v
     Grounded RAG Prompt
            |
            v
       Google Gemini
            |
            v
 Grounded Financial Analysis
 + Evidence Chunk Citations
```

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Yahoo Finance (`yfinance`)
- Sentence Transformers
- FAISS
- Google Gemini
- Google GenAI SDK
- Google Colab / Jupyter Notebook
- JSON and CSV for generated project artifacts

---

## Financial Data Collection

The project retrieves Microsoft financial information using Yahoo Finance.

The collected information includes:

- Company profile
- Income statement
- Balance sheet
- Cash flow statement
- Historical stock prices
- Market and valuation information

The raw financial information is then transformed into analytical datasets used throughout the project.

---

## Fundamental Financial Analysis

The project analyses financial areas including:

- Revenue
- Revenue growth
- Gross profit
- Operating income
- Net income
- Gross margin
- Operating margin
- Net profit margin
- Basic and diluted EPS
- EPS growth
- Total assets
- Total liabilities
- Stockholders' equity
- Cash
- Total debt
- Net cash position
- Current assets
- Current liabilities
- Debt-to-equity ratio
- Debt-to-assets ratio
- Current ratio
- Return on assets (ROA)
- Return on equity (ROE)
- Operating cash flow
- Free cash flow
- Operating cash flow margin
- Free cash flow margin
- Cash conversion
- Historical stock-price performance

---

## Historical Stock Performance

The project also evaluates Microsoft's historical stock-price behaviour.

Metrics include:

- Price return
- Annualized return
- Annualized volatility
- Maximum drawdown

Historical return in this project represents **price return** and should not be interpreted as dividend-reinvested total shareholder return.

---

## Financial Knowledge Base

Instead of providing large raw financial tables directly to the language model, the project converts financial information into structured financial documents.

Knowledge-base categories include:

- Company profile
- Revenue and growth
- Profitability
- Earnings
- Balance sheet
- Liquidity
- Cash flow
- Financial ratios
- Stock performance
- Latest fundamentals

These documents provide the foundation for semantic retrieval.

---

## Chunking and Metadata

Financial documents are divided into smaller chunks before embedding.

Each chunk contains metadata such as:

- Ticker
- Company
- Document title
- Financial category
- Document type
- Source
- Chunk ID
- Chunk index

Chunking allows the retrieval system to return focused pieces of financial evidence rather than entire financial documents.

---

## Embeddings

The project uses the Sentence Transformer model:

`sentence-transformers/all-MiniLM-L6-v2`

The model converts financial text into **384-dimensional numerical embeddings**.

The same embedding model is used for:

1. Financial knowledge-base chunks
2. User financial questions

This allows semantic similarity between questions and financial evidence to be calculated.

---

## FAISS Vector Search

The project uses FAISS as the vector database.

The FAISS index stores the financial embeddings and enables semantic retrieval.

Normalized vectors are used with:

`IndexFlatIP`

With normalized vectors, inner-product ranking provides cosine-similarity-style semantic ranking.

Similarity scores are retrieval-ranking signals and should **not** be interpreted as probabilities or model confidence scores.

---

## How the RAG Pipeline Works

The complete Retrieval-Augmented Generation workflow is:

1. A user asks a financial question.
2. The question is converted into an embedding.
3. FAISS compares the question embedding with stored financial embeddings.
4. The most relevant financial chunks are retrieved.
5. Retrieval results are filtered using the configured threshold.
6. Retrieved chunks are converted into structured evidence context.
7. The evidence and user question are included in the RAG prompt.
8. Google Gemini receives the grounded prompt.
9. Gemini generates the financial analysis.
10. The response includes references to retrieved financial chunk IDs.

This separates the system into two major components:

**Retrieval:** Sentence Transformers + FAISS

**Generation:** Google Gemini

---

## Grounding and Hallucination Guardrails

The system includes instructions designed to reduce unsupported financial claims.

Gemini is instructed to:

- Use supplied financial evidence
- Avoid inventing financial figures
- Avoid unsupported estimates
- Distinguish evidence from interpretation
- Cite retrieved financial chunk IDs
- Report insufficient evidence when necessary
- Avoid unsupported future predictions
- Avoid unsupported buy/sell/hold recommendations

RAG helps reduce hallucination risk, but it does not guarantee that hallucinations can never occur.

---

## Example Questions

Example supported questions include:

- How has Microsoft's revenue changed?
- How profitable is Microsoft?
- How has Microsoft's EPS changed?
- Evaluate Microsoft's debt and cash position.
- How strong is Microsoft's operating cash flow?
- How strong is Microsoft's free cash flow?
- What do Microsoft's financial ratios indicate?
- How has Microsoft's stock price performed historically?
- What are Microsoft's main financial strengths?
- What financial risks are visible in the available data?

---

## Unsupported Question Testing

The system was also tested with questions for which the financial knowledge base does not contain sufficient evidence.

For example:

> What will Microsoft's exact stock price be on December 31, 2030?

The intended behaviour is to report insufficient evidence rather than fabricate an exact future stock price.

Other unsupported-question tests include requests for exact future revenue, employee counts, advertising expenses, and electricity expenses when those values are not available in the retrieved evidence.

---

## Evaluation Methodology

The project evaluates both retrieval and generated responses.

### Retrieval Evaluation

Retrieval was evaluated using a manually constructed financial question set covering multiple financial categories.

The primary metrics were:

- Hit@3
- Top-1 Category Accuracy
- Mean Reciprocal Rank (MRR)

### Retrieval Results

| Metric | Result |
|---|---:|
| Hit@3 | 100.00% |
| Top-1 Category Accuracy | 73.33% |
| Mean Reciprocal Rank | 86.67% |

### Interpretation

**Hit@3 = 100%**

The expected financial category appeared within the first three retrieved results for every question in the defined retrieval evaluation set.

**Top-1 Category Accuracy = 73.33%**

The expected category appeared as the first result for approximately 73% of the evaluation questions.

Financial concepts frequently overlap across categories such as profitability, liquidity, cash flow, balance-sheet analysis and financial ratios. Therefore, a relevant result may still be retrieved even when the expected category is not ranked first.

**Mean Reciprocal Rank = 86.67%**

Relevant evidence generally appeared near the top of the retrieval ranking.

---

## Generation and Grounding Evaluation

The project also evaluates whether the RAG pipeline successfully generates answers, returns evidence and produces valid chunk citations.

| Metric | Result |
|---|---:|
| Answer Generation Rate | 100.00% |
| Evidence Return Rate | 100.00% |
| Citation Presence Rate | 100.00% |
| Citation Precision | 100.00% |
| Unsupported Question Guardrail Rate | 100.00% |

These percentages apply specifically to the **defined project evaluation set** and should not be interpreted as universal model accuracy.

Citation precision verifies that cited chunk IDs were present among the retrieved evidence. It does not independently prove complete claim-level entailment.

The unsupported-question guardrail metric is based on the project's defined unsupported-question tests and detection methodology.

---

## Repository Structure

```text
finrag-stock-analysis/
│
├── Microsoft_FinRAG_Analyst.ipynb
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
│
├── data/
│   ├── msft_balance_sheet.csv
│   ├── msft_cash_flow.csv
│   ├── msft_company_profile.csv
│   ├── msft_data_quality_report.csv
│   ├── msft_financial_chunks.json
│   ├── msft_financial_knowledge_base.json
│   ├── msft_financial_summary.csv
│   ├── msft_financial_summary_readable.csv
│   ├── msft_fundamental_trends.csv
│   ├── msft_income_statement.csv
│   ├── msft_price_analysis.csv
│   └── msft_price_history.csv
│
├── vector_store/
│   ├── msft_financial_index.faiss
│   └── msft_vector_mapping.json
│
└── outputs/
    ├── msft_generation_evaluation.csv
    ├── msft_guardrail_evaluation.csv
    ├── msft_retrieval_evaluation.csv
    ├── msft_rag_evaluation_scorecard.csv
    ├── msft_rag_sample_outputs.json
    └── msft_final_financial_analysis.json
```

---

## Installation

Install the required Python packages using:

```bash
pip install -r requirements.txt
```

The project uses packages including:

- `yfinance`
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `faiss-cpu`
- `sentence-transformers`
- `google-genai`
- `python-dotenv`

---

## Gemini API Configuration

The Gemini API key is intentionally **not stored in this repository**.

When running the project in Google Colab, create a Colab Secret named:

`Microsoftfinrag`

The notebook retrieves the secret using:

```python
from google.colab import userdata

GEMINI_API_KEY = userdata.get("Microsoftfinrag")
```

A valid Gemini API key must be stored as the value of that Colab Secret.

**Never commit API keys, `.env` files, passwords or other secrets to a public GitHub repository.**

---

## How to Run the Project

1. Download or clone this repository.
2. Open `Microsoft_FinRAG_Analyst.ipynb` in Google Colab.
3. Create a Colab Secret named `Microsoftfinrag`.
4. Store your own valid Gemini API key in that secret.
5. Install the required dependencies.
6. Run the notebook cells sequentially.
7. Review the generated financial datasets.
8. Review the financial knowledge base and chunks.
9. Review FAISS retrieval examples.
10. Run the RAG financial analyst questions.
11. Review retrieval, generation and guardrail evaluation results.
12. Review the final strengths, risks and comprehensive financial analysis.

---

## Key Project Files

### `Microsoft_FinRAG_Analyst.ipynb`

Main project notebook containing the complete financial-data, RAG, evaluation and analysis workflow.

### `data/`

Contains processed financial statements, calculated financial metrics, historical price analysis, financial knowledge-base documents and chunks.

### `vector_store/`

Contains the FAISS financial vector index and the mapping between vectors and financial chunks.

### `outputs/`

Contains retrieval evaluation, generation evaluation, guardrail testing, sample RAG outputs and final financial analysis.

### `requirements.txt`

Contains the Python dependencies required to reproduce the project environment.

---

## Limitations

The project has several important limitations:

- Financial information depends on Yahoo Finance availability and field consistency.
- Yahoo Finance fields may occasionally be missing or use different labels.
- The evaluation dataset is relatively small and manually constructed.
- Retrieval thresholds are heuristic.
- Financial concepts can overlap across knowledge-base categories.
- Citation validation confirms citation identity rather than complete claim-level entailment.
- Unsupported-question detection uses project-defined heuristics.
- The project does not incorporate every real-time news, market or macroeconomic factor.
- Historical stock performance represents price return rather than dividend-reinvested total shareholder return.
- Broad multi-topic questions can be more challenging for single-vector retrieval.
- RAG reduces hallucination risk but does not eliminate it.

---

## Future Improvements

Potential improvements include:

- Multi-company stock analysis
- Company comparison functionality
- Metadata-filtered retrieval
- Hybrid keyword and semantic search
- Retrieval reranking
- Query decomposition for complex financial questions
- Larger automated evaluation datasets
- Claim-level grounding verification
- Additional financial-data sources
- Real-time news integration
- Interactive financial-analysis interface

---

## Project Outcome

FinRAG Analyst demonstrates an end-to-end RAG architecture for financial analysis:

**Financial Data → Financial Metrics → Knowledge Base → Chunking → Embeddings → FAISS → Evidence Retrieval → Gemini → Grounded Financial Analysis**

The project combines traditional financial-data analysis with semantic retrieval and generative AI while incorporating evidence citations, unsupported-question handling and formal retrieval/generation evaluation.

---

## Disclaimer

This project is intended for **educational and analytical purposes only**.

It does not constitute financial, investment, trading or professional advice.

---

## Author

**Sombir Singh**  
Generative AI Capstone Project  
**AI-Powered Fundamental Stock Analysis Using RAG**
