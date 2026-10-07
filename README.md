# FinRAG Analyst — AI-Powered Fundamental Stock Analysis

## Project Overview

FinRAG Analyst is a Retrieval-Augmented Generation (RAG) system for
fundamental stock analysis.

The project uses Microsoft Corporation (MSFT) as the demonstration company.
Financial data is collected from Yahoo Finance, transformed into a structured
financial knowledge base, embedded using Sentence Transformers, indexed using
FAISS, and retrieved as evidence for Google Gemini.

The objective is to generate financial analysis that is grounded in retrieved
financial evidence rather than relying only on the language model's internal
knowledge.

## Architecture

```text
Microsoft (MSFT)
       |
       v
Yahoo Finance
       |
       v
Financial Statements + Historical Prices
       |
       v
Financial Metrics & Ratios
       |
       v
Structured Financial Knowledge Base
       |
       v
Chunking + Metadata
       |
       v
Sentence Transformer Embeddings
       |
       v
FAISS Vector Index
       |
       +--------------------------+
                                  |
User Question                    |
       |                          |
       v                          |
Question Embedding               |
       |                          |
       v                          |
FAISS Semantic Search <----------+
       |
       v
Relevant Financial Evidence
       |
       v
Google Gemini
       |
       v
Grounded Financial Answer
+ Evidence Chunk Citations
Technologies
- Python
- Pandas
- NumPy
- Yahoo Finance / yfinance
- Sentence Transformers
- FAISS
- Google Gemini
- Google GenAI SDK
- Jupyter Notebook / Google Colab
Financial Analysis
The project analyses areas including:
- Revenue and revenue growth
- Gross profit
- Operating income
- Net income
- Operating margin
- Net profit margin
- Earnings per share (EPS)
- Assets and liabilities
- Cash and debt
- Debt-to-equity ratio
- Current ratio
- Return on assets (ROA)
- Return on equity (ROE)
- Operating cash flow
- Free cash flow
- Historical stock-price performance
- Financial strengths and risks
How RAG Works
Instead of sending a financial question directly to the language model,
FinRAG first searches the financial knowledge base.
The workflow is:
1. The user asks a financial question.
2. The question is converted into an embedding.
3. FAISS compares the question embedding with stored financial embeddings.
4. The most relevant financial chunks are retrieved.
5. Retrieved evidence is added to the Gemini prompt.
6. Gemini analyses the supplied evidence.
7. The answer includes references to the retrieved financial chunks.
This approach helps reduce unsupported financial claims and improves
traceability.
Embeddings and Vector Search
The project uses:
sentence-transformers/all-MiniLM-L6-v2
The model generates 384-dimensional embeddings.
The FAISS index uses normalized vectors with inner-product similarity,
providing cosine-similarity-style ranking for semantic retrieval.
Similarity scores are ranking signals and should not be interpreted as
probabilities or model confidence scores.
Grounding and Hallucination Guardrails
The generation system instructs Gemini to:
- Use only supplied financial evidence
- Avoid inventing financial figures
- Distinguish evidence from interpretation
- Cite retrieved chunk IDs
- Report insufficient evidence when required
- Avoid unsupported predictions
- Avoid unsupported buy/sell/hold recommendations
Evaluation Results
The system was evaluated using a defined project evaluation set.
Retrieval Evaluation
Metric	Result
Hit@3	100%
Top-1 Category Accuracy	73.33%
Mean Reciprocal Rank	86.67%


Hit@3 indicates that the expected financial category appeared within the first
three retrieved results for all questions in the defined retrieval test set.
Top-1 accuracy is lower because financial concepts frequently overlap across
categories such as profitability, cash flow, liquidity and financial ratios.
Generation & Grounding Evaluation
Metric	Result
Answer Generation Rate	100%
Evidence Return Rate	100%
Citation Presence Rate	100%
Citation Precision	100%
Unsupported Question Guardrail Rate	100%


These results apply to the project's defined evaluation set and should not be
interpreted as universal model accuracy.
Citation precision verifies that cited chunk IDs were among the retrieved
evidence. It does not by itself prove complete claim-level entailment.
Example Questions
FinRAG can answer questions such as:
- How has Microsoft's revenue changed?
- How profitable is Microsoft?
- How has Microsoft's EPS changed?
- Evaluate Microsoft's debt and cash position.
- How strong is Microsoft's free cash flow?
- What do Microsoft's financial ratios indicate?
- How has Microsoft's stock price performed historically?
- What are Microsoft's main financial strengths?
- What financial risks are visible in the available data?
The system can also be tested with unsupported questions such as:
What will Microsoft's exact stock price be on December 31, 2030?

The expected behaviour is to report insufficient evidence rather than invent
an exact future stock price.
Repository Structure
finrag-stock-analysis/
├── Microsoft_FinRAG_Analyst_FINAL.ipynb
├── README.md
├── requirements.txt
├── data/
│   ├── msft_company_profile.csv
│   ├── msft_income_statement.csv
│   ├── msft_balance_sheet.csv
│   ├── msft_cash_flow.csv
│   ├── msft_price_history.csv
│   ├── msft_financial_summary.csv
│   ├── msft_financial_summary_readable.csv
│   ├── msft_fundamental_trends.csv
│   ├── msft_price_analysis.csv
│   ├── msft_data_quality_report.csv
│   ├── msft_financial_knowledge_base.json
│   └── msft_financial_chunks.json
├── vector_store/
│   ├── msft_financial_index.faiss
│   └── msft_vector_mapping.json
└── outputs/
    ├── msft_retrieval_evaluation.csv
    ├── msft_generation_evaluation.csv
    ├── msft_guardrail_evaluation.csv
    ├── msft_rag_evaluation_scorecard.csv
    ├── msft_rag_sample_outputs.json
    └── msft_final_financial_analysis.json

Installation
Install the required Python packages:
pip install -r requirements.txt

Gemini API Configuration
The Gemini API key is intentionally not stored in this repository.
When running the notebook in Google Colab, create a Colab Secret named:
Microsoftfinrag
The notebook accesses the secret using:
from google.colab import userdataGEMINI_API_KEY = userdata.get("Microsoftfinrag")


Never commit API keys or secret values to a public repository.
Running the Project
1. Download or clone the repository.
2. Open Microsoft_FinRAG_Analyst_FINAL.ipynb in Google Colab.
3. Configure the Microsoftfinrag Colab Secret with a valid Gemini API key.
4. Install the required dependencies.
5. Run the notebook cells sequentially.
6. Review retrieval tests, RAG answers, evaluation results and final analysis.
Limitations
- Financial data depends on Yahoo Finance availability and field consistency.
- The evaluation dataset is relatively small and manually constructed.
- Retrieval thresholds are heuristic.
- Financial concepts may overlap across knowledge-base categories.
- Citation validation confirms retrieval identity rather than full
  claim-level entailment.
- The system does not incorporate every real-time market, news or
  macroeconomic factor.
- Historical stock performance represents price return rather than
  dividend-reinvested total shareholder return.
- RAG reduces hallucination risk but does not eliminate it.
Future Improvements
Potential improvements include:
- Multi-company analysis
- Metadata-filtered retrieval
- Hybrid keyword and semantic retrieval
- Reranking
- Query decomposition
- Larger automated evaluation datasets
- Claim-level grounding verification
- Additional financial and market data sources
Disclaimer
This project is for educational and analytical purposes only.
It does not constitute financial or investment advice.
Author
Sombir Singh
Generative AI Capstone Project
AI-Powered Fundamental Stock Analysis using RAG
