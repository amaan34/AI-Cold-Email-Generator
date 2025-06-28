# AI Lawyer

![Legal AI](https://img.shields.io/badge/AI-Legal%20Assistant-blue?style=for-the-badge&logo=balance-scale)

## Introduction

The **AI Lawyer** is an intelligent legal document analysis system that leverages Retrieval-Augmented Generation (RAG) technology to provide accurate, context-aware legal insights. Built with Streamlit and advanced language models, this application allows users to upload legal documents and receive precise answers to their legal questions based on the document content.

## Features

- **Document Upload & Analysis**: Upload PDF legal documents for intelligent analysis
- **RAG-Powered Responses**: Context-aware answers based on document content using vector embeddings
- **Multiple LLM Support**: Compatible with Groq and Ollama models for flexible deployment
- **Real-Time Processing**: Instant document processing and query response
- **Source Verification**: All responses are grounded in the uploaded document content
- **User-Friendly Interface**: Clean Streamlit web interface for easy interaction
- **Vector Database Storage**: Efficient document storage and retrieval using FAISS

## Tech Stack

- **Python 3.11+**
- **Streamlit** – for the web application interface
- **LangChain** – for RAG pipeline orchestration and document processing
- **FAISS** – for efficient vector storage and similarity search
- **Ollama** – for local embedding generation (DeepSeek R1 model)
- **Groq** – for high-performance LLM inference
- **PDFPlumber** – for PDF document parsing and text extraction

## Architecture

The system implements a sophisticated RAG pipeline:

### Document Processing Pipeline
1. **PDF Upload**: Users upload legal documents through the Streamlit interface
2. **Text Extraction**: PDFPlumber extracts text content from uploaded documents
3. **Chunking**: RecursiveCharacterTextSplitter creates optimal text chunks for processing
4. **Embedding Generation**: Ollama embeddings convert text chunks to vector representations
5. **Vector Storage**: FAISS stores embeddings for efficient similarity search

### Query Processing Pipeline
1. **Query Input**: Users submit legal questions through the interface
2. **Query Embedding**: Question is converted to vector representation
3. **Similarity Search**: FAISS retrieves most relevant document chunks
4. **Context Assembly**: Retrieved chunks are combined into context
5. **LLM Response**: Groq LLM generates accurate answers based on context

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/AI-Lawyer.git
   cd AI-Lawyer
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up Ollama (for embeddings):**
   ```bash
   # Install Ollama from https://ollama.ai/
   ollama pull deepseek-r1:14b
   ```

5. **Set up environment variables:**
   ```bash
   export GROQ_API_KEY="your-groq-api-key"
   ```

## Usage

### Starting the Application

```bash
streamlit run main.py
```

### Using the AI Lawyer

1. **Upload Document**: Use the file uploader to select a PDF legal document
2. **Ask Questions**: Enter your legal question in the text area
3. **Get Answers**: Click "Ask AI Lawyer" to receive context-aware responses

### Example Queries

```
- "What are the key terms of this contract?"
- "What are my rights under this agreement?"
- "What are the penalties for breach of contract?"
- "What is the dispute resolution process?"
```

## Configuration

### Model Configuration

The system supports multiple LLM configurations:

```python
# Groq LLM (default)
llm_model = ChatGroq(model="deepseek-r1-distill-llama-70b")

# Ollama LLM (alternative)
llm_model = Ollama(model="deepseek-r1:14b")
```

### Embedding Configuration

```python
# Ollama embeddings (default)
embeddings = OllamaEmbeddings(model="deepseek-r1:14b")

# Alternative embedding models
embeddings = OllamaEmbeddings(model="nomic-embed-text")
```

### Chunking Configuration

```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,      # Size of each text chunk
    chunk_overlap=200,    # Overlap between chunks
    add_start_index=True  # Include position information
)
```

## API Reference

### Main Functions

#### `upload_pdf(file)`
- **Purpose**: Upload and save PDF file to local storage
- **Parameters**: `file` - Streamlit uploaded file object
- **Returns**: None

#### `load_pdf(file_path)`
- **Purpose**: Extract text content from PDF file
- **Parameters**: `file_path` - Path to PDF file
- **Returns**: List of document objects

#### `create_chunks(documents)`
- **Purpose**: Split documents into manageable chunks
- **Parameters**: `documents` - List of document objects
- **Returns**: List of text chunks

#### `create_vector_store(db_faiss_path, text_chunks, ollama_model_name)`
- **Purpose**: Create and save vector database
- **Parameters**: 
  - `db_faiss_path` - Path to save FAISS database
  - `text_chunks` - List of text chunks
  - `ollama_model_name` - Name of Ollama model
- **Returns**: FAISS database object

#### `retrieve_docs(faiss_db, query)`
- **Purpose**: Retrieve relevant documents for query
- **Parameters**: 
  - `faiss_db` - FAISS database object
  - `query` - User question
- **Returns**: List of relevant document chunks

#### `answer_query(documents, model, query)`
- **Purpose**: Generate answer based on retrieved documents
- **Parameters**: 
  - `documents` - Retrieved document chunks
  - `model` - LLM model object
  - `query` - User question
- **Returns**: Generated answer

## File Structure

```
AI-Lawyer/
├── main.py                 # Main Streamlit application
├── frontend.py            # Frontend components and UI
├── rag_pipeline.py        # RAG pipeline implementation
├── vector_database.py     # Vector database operations
├── requirements.txt       # Python dependencies
├── Pipfile               # Pipenv configuration
├── vectorstore/          # Vector database storage
│   └── db_faiss/        # FAISS database files
├── pdfs/                 # Uploaded PDF storage
└── README.md            # This file
```

## Customization

### Custom Prompt Template

Modify the prompt template in `main.py`:

```python
custom_prompt_template = """
You are a legal expert. Use the provided context to answer the user's question.
If you don't know the answer based on the context, say so.
Only provide information from the given context.

Question: {question} 
Context: {context} 
Answer:
"""
```

### Adding New Document Types

Extend document support by adding new loaders:

```python
from langchain_community.document_loaders import TextLoader, Docx2txtLoader

# Add support for different file types
if file.name.endswith('.txt'):
    loader = TextLoader(file_path)
elif file.name.endswith('.docx'):
    loader = Docx2txtLoader(file_path)
```

## Performance Optimization

### Vector Database Optimization

```python
# Increase search parameters for better results
retrieved_docs = faiss_db.similarity_search(
    query, 
    k=5,  # Number of chunks to retrieve
    score_threshold=0.7  # Minimum similarity score
)
```

### Chunking Optimization

```python
# Adjust chunk size based on document type
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1500,  # Larger chunks for legal documents
    chunk_overlap=300,  # More overlap for context preservation
    separators=["\n\n", "\n", ".", "!", "?", ",", " ", ""]
)
```

## Troubleshooting

### Common Issues

1. **Ollama Connection Error**
   - Ensure Ollama is running: `ollama serve`
   - Check model installation: `ollama list`

2. **Memory Issues**
   - Reduce chunk size in text splitter
   - Use smaller embedding models
   - Implement document cleanup

3. **Slow Response Times**
   - Optimize vector search parameters
   - Use faster LLM models
   - Implement caching for repeated queries

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This AI Lawyer is for educational and research purposes only. It should not be considered as legal advice. Always consult with qualified legal professionals for legal matters. The accuracy of responses depends on the quality of uploaded documents and should be verified independently.