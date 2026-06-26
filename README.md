# 📡 Telecom Support RAG — AI Customer Care Assistant

> A multi-source RAG chatbot that resolves telecom support queries using **FAQs**, **resolved tickets**, and a **technical guide PDF** — powered by **Qwen3-32B on Groq**.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-RAG-1C3C3C?style=flat&logo=langchain&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-Qwen3--32B-F55036?style=flat)
![Chroma](https://img.shields.io/badge/ChromaDB-Vector%20Store-6A0DAD?style=flat)
![Streamlit](https://img.shields.io/badge/Streamlit-UI-FF4B4B?style=flat&logo=streamlit&logoColor=white)

---

## 🚀 What It Does

Pulls context from **three independent knowledge sources** at once — FAQ, past resolved tickets, and a chunked PDF guide — merges them, and feeds a grounded context block to an LLM instructed to answer **only** from that context, falling back to "call 611 / use the app" otherwise.

---

## 🏗️ Architecture

```
                ┌──────────────┐
                │ User Question│
                └──────┬───────┘
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   FAQ Retriever  Ticket Retriever  Guide Retriever
   (k=3)          (k=3)             (k=3)
        │              │              │
        └──────────────┼──────────────┘
                       ▼
              Merged Context (9 docs)
                       ▼
              ┌─────────────────┐
              │  Prompt Template │
              └────────┬────────┘
                       ▼
              Qwen3-32B  (via Groq)
                       ▼
              Streamed Response 🔥
```

Two front-ends, one shared chain: **`main.py`** (CLI) and **`app.py`** (Streamlit, with chat history + sample questions).

---

## ⚙️ Tech Stack

LangChain (LCEL) · Groq (Qwen3-32B) · ChromaDB (3 collections) · `all-MiniLM-L6-v2` embeddings · Streamlit · CSV + SQLite + PDF data sources

---

## 📦 Setup

```bash
pip install langchain langchain-core langchain-chroma langchain-huggingface \
            langchain-groq langchain-community langchain-text-splitters \
            streamlit pandas python-dotenv pypdf

echo "GROQ_API_KEY=your_key_here" > .env

# data/faq.csv, data/tickets.db, data/telecom_guide.pdf

python ingest_faq.py
python ingest_tickets.py
python ingest_pdf.py

python main.py            # CLI
streamlit run app.py      # Web UI
```

---


<p align="center"><i>Built as part of a hands-on Agentic AI / RAG workshop 🛠️</i></p>
