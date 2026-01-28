# Structured Data Extraction with LangChain & Groq

## Overview

This project demonstrates **structured information extraction** from unstructured text using **LangChain**, **Groq LLMs**, and **Pydantic schemas**. It shows how to reliably extract entities such as names, countries, and emails from natural language inputs like reviews or comments.

The project uses **Groq's LLaMA 3.1 (8B Instant)** model with LangChain's `with_structured_output` feature to convert free-form text into validated Python objects.

This is a **practice-to-production-ready pattern** for building extraction pipelines in GenAI applications.

---

## Key Features

* 🔹 Environment-based API key management (`.env`)
* 🔹 Groq LLM integration via `langchain_groq`
* 🔹 Deterministic outputs (`temperature=0`)
* 🔹 Pydantic-based schema validation
* 🔹 Single-entity and multi-entity extraction
* 🔹 Robust handling of missing or unknown fields (`Optional` values)

---

## Tech Stack

* **Python 3.10+**
* **LangChain** (Core + Groq integration)
* **Groq LLM API**
* **Pydantic** (schema definition & validation)
* **python-dotenv** (environment variables)

---

## Project Flow (Architecture)

1. **Load Environment Variables**

   * Reads `GROQ_API_KEY` securely from `.env`

2. **Initialize LLM**

   * Uses Groq-hosted `llama-3.1-8b-instant`
   * Temperature set to `0` for consistent extraction

3. **Define Extraction Schema**

   * `Person` model defines extractable fields
   * Fields are optional to allow partial extraction

4. **Create Prompt Template**

   * System message enforces strict extraction rules
   * Human message injects raw text

5. **Structured Output Chain**

   * `prompt | llm.with_structured_output(schema=...)`
   * LLM output is parsed directly into Pydantic objects

6. **Invoke Chain**

   * Pass unstructured text
   * Receive validated structured data

---

## Folder Structure (Suggested)

```
project-root/
│── main.py
│── .env
│── .gitignore
│── requirements.txt
│── README.md
```

---

## Environment Setup

### 1. Create `.env` file

```
GROQ_API_KEY=your_groq_api_key_here
```

> ⚠️ Never commit `.env` files to version control.

---

## Installation

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### Required Dependencies

```txt
langchain
langchain-groq
langchain-core
pydantic
python-dotenv
```

---

## Usage Examples

### 1. Single Entity Extraction

Input:

```text
I absolutely love this product! ... - Riyadh, Bangladesh
```

Output:

```json
{
  "name": "Riyadh",
  "lastname": null,
  "country": "Bangladesh"
}
```

---

### 2. List of Entities Extraction

Input:

```text
I'm so impressed with this product! ... - Emily Clarke, Canada
```

Output:

```json
{
  "people": [
    {
      "name": "Emily",
      "lastname": "Clarke",
      "country": "Canada",
      "Email": null
    }
  ]
}
```

---

### 3. Multiple People in One Text

Input:

```text
Riyadh riyadhgenai@gmail.com from Bangladesh ... Bob Smith from the USA ...
```

Output:

```json
{
  "people": [
    {
      "name": "Riyadh",
      "lastname": null,
      "country": "Bangladesh",
      "Email": "riyadhgenai@gmail.com"
    },
    {
      "name": "Bob",
      "lastname": "Smith",
      "country": "USA",
      "Email": null
    }
  ]
}
```

---

## Why Optional Fields Matter

All fields are defined as `Optional` to:

* Prevent hallucinated values
* Allow partial extraction
* Improve robustness in real-world noisy text

If a value is not confidently found, the model returns `null`.

---

## Practice vs Production Notes

| Aspect         | Practice     | Production                     |
| -------------- | ------------ | ------------------------------ |
| Schema         | Simple       | Versioned & validated          |
| Prompt         | Inline       | Centralized & tested           |
| LLM Calls      | Direct       | Wrapped with retries & logging |
| Error Handling | Minimal      | Strict validation & fallbacks  |
| Scaling        | Single input | Batch / async processing       |

This project already follows **production-safe extraction patterns**.

---

## Common Use Cases

* Review analysis
* Resume / profile parsing
* CRM data ingestion
* Customer feedback mining
* Document intelligence pipelines

---

## Security Notes

* API keys must remain in environment variables
* Never log raw keys or sensitive text
* Validate outputs before downstream usage

---

## Author

**Riyadh GenAI**
GenAI & LLM Engineering



