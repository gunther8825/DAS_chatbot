# DAS_chatbot
## Data Availability Statement (DAS) Extraction using RAG

This project uses a **retrieval-augmented generation (RAG)** strategy with a large language model (LLM) to automatically extract **Data Availability Statements (DAS)** from journal articles and categorize them.  

---

## Overview

Journal articles often include a **Data Availability Statement (DAS)** describing how and where the underlying research data can be accessed. This repository contains Python code that:  

1. **Extracts DAS sections** from journal articles using a RAG pipeline.  
2. **Classifies the DAS** into predefined categories (e.g., "Data in main text," "Data in repository," etc.).  
3. **Outputs results** into a structured CSV file for further analysis.  

---

## Methodology

A **RAG (retrieval-augmented generation) pipeline** was implemented:  

1. Articles are split into **chunks**, embedded, and vectorized.  
2. The LLM is queried with prompts specifically designed to:  
   - Locate and extract the DAS.  
   - Report "no DAS" if none is found.  
3. Extracted DAS are saved to a CSV along with their corresponding DOI file names.  

**Pipeline Illustration:**  
_Add diagram here showing article → chunking → embeddings → retrieval → LLM → extracted DAS → CSV._  

---

### Accuracy

- Tested with **2021 journal articles**.  
- Achieved **98.6% accuracy (206/209)** in retrieving DAS after manual checking.  
- Failures mainly occurred when a DAS spanned across multiple pages (chunking issue). Even in failures, partial DAS was usually retrieved.
- Achieved **95.7% accuracy (200/209)** in retrieving DAS after manual checking.  If taking the fraction of categories are correct then the **accuracy is 97.5% (203.87/209).** 

---

### DAS Categories

Simplified extracted DAS statements are classified into the following categories:  

1. Data is included in the main text.  
2. Data is available upon request.  
3. Data is shared in a repository.  
4. Data available in supplementary materials.  
5. No data availability statement.  
6. Data sharing is not applicable.  
7. Previously published data was used.  

The RAG technique was reapplied:  
- Input = Extracted DAS.  
- Prompt = "Based on the DAS, assign one or more of the following categories."  
- Output = Category numbers (e.g., `2,4`).  

---

## Output

The program produces a CSV file with the following structure:  

| DOI File Name    | Extracted DAS                                                                 | Category |
|------------------|-------------------------------------------------------------------------------|----------|
| 10.1234.example1.pdf | "All data supporting the findings of this study are available in the main text"| 1        |
| 10.5678.example2.pdf | "Data are available upon reasonable request to the authors."                  | 2        |
| 10.9101.example3.pdf | "Data can be found in repository X under accession number Y."                 | 3        |

---

## Requirements

- Python 3.8+  
- Libraries:  
  - `pandas`  
  - `langchain`  
  - `openai` (or other LLM API wrapper)  
  - `faiss` (or another vector store backend)  

Install dependencies:  

```bash
pip install -r requirements.txt
