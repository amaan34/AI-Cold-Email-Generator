# RAGChatbot

A multi-phase chatbot project demonstrating UI setup, LLM integration, and Retrieval Augmented Generation (RAG) using Streamlit, LangChain, Groq, and vector embeddings.

---

## Project Overview

RAGChatbot is a stepwise project to build a modern chatbot with:
- **Phase 1:** Streamlit-based chat UI
- **Phase 2:** Integration with a Large Language Model (LLM) via Groq API
- **Phase 3:** Retrieval Augmented Generation (RAG) with document upload, vector embeddings, and context-aware answers

---

## Features
- Clean, interactive chat interface (Streamlit)
- LLM-powered responses (Groq + LangChain)
- Upload PDF documents for context-aware Q&A (RAG)
- Uses HuggingFace embeddings for vector search

---

## Project Structure

```
phase_1.py         # Streamlit UI only
phase_2.py         # Adds LLM (Groq) integration
phase_3.py         # Adds RAG: document upload, embeddings, retrieval
projectlayout.txt  # Project phase descriptions
README.md          # This file
venv/              # (optional) Python virtual environment
```

---

## Setup Instructions

### 1. Clone the Repository
```bash
git clone <repo-url>
cd RAGChatbot
```

### 2. Create and Activate a Virtual Environment (Recommended)
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
Create a `requirements.txt` file with the following content:

```
streamlit
langchain
langchain-groq
langchain-core
langchain-community
langchain-huggingface
huggingface-hub
pypdf
```

Then install:
```bash
pip install -r requirements.txt
```

> **Note:**
> - You may need additional dependencies for PDF and embeddings support (e.g., `pypdf`, `sentence-transformers`).
> - If you encounter errors, try installing `sentence-transformers` and `torch` as well:
>   ```bash
>   pip install sentence-transformers torch
>   ```

### 4. Set Up Groq API Key
Obtain a Groq API key from [Groq](https://console.groq.com/keys) and set it as an environment variable:

- **Windows (PowerShell):**
  ```powershell
  $env:GROQ_API_KEY="your-groq-api-key"
  ```
- **Mac/Linux:**
  ```bash
  export GROQ_API_KEY="your-groq-api-key"
  ```

---

## Running the Project

### Phase 1: Streamlit UI Only
```bash
streamlit run phase_1.py
```

### Phase 2: LLM Integration
```bash
streamlit run phase_2.py
```

### Phase 3: RAG (Document Q&A)
1. Place your PDF document in the project root as `reflexion.pdf` (or update the filename in `phase_3.py`).
2. Run:
   ```bash
   streamlit run phase_3.py
   ```

---

## Troubleshooting
- **Missing package errors:** Ensure all dependencies are installed in your active environment.
- **Groq API errors:** Double-check your API key and environment variable setup.
- **PDF not found:** Make sure your PDF is named correctly and in the project root.
- **Embeddings/model errors:** Try installing `sentence-transformers` and `torch`.

---

## Credits
- [Streamlit](https://streamlit.io/)
- [LangChain](https://python.langchain.com/)
- [Groq](https://groq.com/)
- [HuggingFace](https://huggingface.co/)

---

## License
MIT License
