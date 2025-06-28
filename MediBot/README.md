# MediBot

![Medical AI](https://img.shields.io/badge/AI-Medical%20Assistant-blue?style=for-the-badge&logo=stethoscope)

## Introduction

The **MediBot** is an intelligent medical information assistant that leverages Retrieval-Augmented Generation (RAG) technology to provide accurate, context-aware medical insights. Built with Streamlit and advanced language models, this application offers a conversational interface for medical queries, drawing from comprehensive medical knowledge bases to deliver reliable health information.

## Features

- **Conversational Medical Interface**: Natural language chat interface for medical queries
- **RAG-Powered Responses**: Context-aware answers based on medical knowledge base
- **Real-Time Processing**: Instant query processing and response generation
- **Source Verification**: All responses are grounded in medical literature
- **Memory Management**: Persistent conversation history for context continuity
- **Vector Database Integration**: Efficient medical knowledge storage and retrieval using FAISS
- **HuggingFace Integration**: Support for various medical language models

## Tech Stack

- **Python 3.11+**
- **Streamlit** – for the web application interface
- **LangChain** – for RAG pipeline orchestration and conversation management
- **FAISS** – for efficient vector storage and similarity search
- **HuggingFace** – for medical language models and embeddings
- **Sentence Transformers** – for text embedding generation
- **Pipenv** – for dependency management and virtual environment

## Architecture

The system implements a sophisticated medical RAG pipeline:

### Knowledge Base Processing
1. **Medical Document Ingestion**: Comprehensive medical literature and guidelines
2. **Text Chunking**: Optimal segmentation of medical documents for processing
3. **Embedding Generation**: Conversion of medical text to vector representations
4. **Vector Storage**: FAISS database for efficient medical knowledge retrieval

### Query Processing Pipeline
1. **User Input**: Natural language medical questions through chat interface
2. **Query Embedding**: Medical question conversion to vector representation
3. **Similarity Search**: Retrieval of most relevant medical information
4. **Context Assembly**: Combination of retrieved medical knowledge
5. **LLM Response**: Generation of accurate medical responses based on context

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/MediBot.git
   cd MediBot
   ```

2. **Install Pipenv (if not already installed):**
   ```bash
   pip install pipenv
   ```

3. **Create and activate virtual environment:**
   ```bash
   pipenv install
   pipenv shell
   ```

4. **Set up environment variables:**
   ```bash
   export HF_TOKEN="your-huggingface-token"
   ```

5. **Initialize the vector database:**
   ```bash
   python create_memory_for_llm.py
   ```

## Usage

### Starting the Application

```bash
streamlit run medibot.py
```

### Using MediBot

1. **Start Chat**: The application opens with a chat interface
2. **Ask Questions**: Type medical questions in natural language
3. **Get Responses**: Receive context-aware medical information
4. **Continue Conversation**: Chat history is maintained for context

### Example Queries

```
- "What are the symptoms of diabetes?"
- "How is hypertension treated?"
- "What are the side effects of common medications?"
- "Explain the cardiovascular system"
- "What are the risk factors for heart disease?"
```

## Configuration

### Model Configuration

The system supports various HuggingFace models:

```python
# Default medical model
HUGGINGFACE_REPO_ID = "mistralai/Mistral-7B-Instruct-v0.3"

# Alternative medical models
HUGGINGFACE_REPO_ID = "microsoft/DialoGPT-medium"  # For conversation
HUGGINGFACE_REPO_ID = "gpt2"  # For general text generation
```

### Embedding Configuration

```python
# Default medical embeddings
embedding_model = HuggingFaceEmbeddings(
    model_name='sentence-transformers/all-MiniLM-L6-v2'
)

# Alternative medical embeddings
embedding_model = HuggingFaceEmbeddings(
    model_name='sentence-transformers/all-mpnet-base-v2'
)
```

### Prompt Configuration

```python
CUSTOM_PROMPT_TEMPLATE = """
You are a medical information assistant. Use the provided context to answer the user's question.
If you don't know the answer based on the context, say so.
Only provide information from the given context.
Always include appropriate medical disclaimers.

Context: {context}
Question: {question}

Start the answer directly. No small talk please.
"""
```

## API Reference

### Main Functions

#### `get_vectorstore()`
- **Purpose**: Load and cache the medical knowledge vector database
- **Returns**: FAISS vector store object
- **Cache**: Results are cached for performance

#### `set_custom_prompt(custom_prompt_template)`
- **Purpose**: Configure the medical response prompt template
- **Parameters**: `custom_prompt_template` - Prompt template string
- **Returns**: PromptTemplate object

#### `load_llm(huggingface_repo_id, HF_TOKEN)`
- **Purpose**: Initialize the medical language model
- **Parameters**: 
  - `huggingface_repo_id` - HuggingFace model repository ID
  - `HF_TOKEN` - HuggingFace API token
- **Returns**: HuggingFaceEndpoint LLM object

#### `main()`
- **Purpose**: Main Streamlit application entry point
- **Features**: Chat interface, session management, response generation

## File Structure

```
MediBot/
├── medibot.py                    # Main Streamlit application
├── create_memory_for_llm.py      # Vector database initialization
├── connect_memory_with_llm.py    # Memory-LLM connection utilities
├── Pipfile                       # Pipenv configuration
├── Pipfile.lock                  # Locked dependencies
├── data/                         # Medical knowledge base
│   └── The_GALE_ENCYCLOPEDIA_of_MEDICINE_SECOND.pdf
├── vectorstore/                  # Vector database storage
│   └── db_faiss/                # FAISS database files
├── medical-chatbot-ppt.pdf       # Project documentation
└── README.md                    # This file
```

## Customization

### Adding New Medical Knowledge

1. **Prepare Medical Documents**: Add new medical literature to the `data/` directory
2. **Update Vector Database**: Run the memory creation script
3. **Verify Integration**: Test with medical queries

```python
# Add new medical documents
from langchain_community.document_loaders import PDFPlumberLoader

loader = PDFPlumberLoader("data/new_medical_document.pdf")
documents = loader.load()
```

### Custom Medical Prompts

Modify the prompt template for specific medical domains:

```python
MEDICAL_PROMPT_TEMPLATE = """
You are a specialized {medical_domain} expert. Use the provided context to answer the user's question.
Always include relevant medical disclaimers and recommend consulting healthcare professionals.

Context: {context}
Question: {question}

Provide accurate, evidence-based information:
"""
```

### Model Fine-tuning

For domain-specific medical models:

```python
# Load fine-tuned medical model
llm = HuggingFaceEndpoint(
    repo_id="your-org/medical-model",
    temperature=0.3,  # Lower temperature for medical accuracy
    model_kwargs={
        "token": HF_TOKEN,
        "max_length": 512,
        "do_sample": True
    }
)
```

## Performance Optimization

### Vector Search Optimization

```python
# Optimize retrieval for medical queries
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=vectorstore.as_retriever(
        search_kwargs={
            'k': 5,  # Number of medical chunks to retrieve
            'score_threshold': 0.7  # Minimum relevance score
        }
    ),
    return_source_documents=True
)
```

### Caching Strategy

```python
# Implement caching for medical responses
@st.cache_data
def get_medical_response(query):
    # Cache medical responses for common queries
    return qa_chain.invoke({'query': query})
```

## Medical Disclaimer

**Important Medical Information:**

This MediBot is designed for educational and informational purposes only. It should not be used as a substitute for professional medical advice, diagnosis, or treatment. Always consult with qualified healthcare professionals for medical concerns.

- **Not a Medical Professional**: This AI system is not a licensed healthcare provider
- **Educational Purpose**: Information provided is for learning and reference only
- **Professional Consultation**: Always seek professional medical advice for health issues
- **Emergency Situations**: In medical emergencies, contact emergency services immediately

## Troubleshooting

### Common Issues

1. **HuggingFace Token Error**
   - Verify HF_TOKEN environment variable
   - Check token permissions and validity

2. **Vector Database Issues**
   - Reinitialize database: `python create_memory_for_llm.py`
   - Check FAISS file permissions

3. **Model Loading Errors**
   - Verify internet connection for model downloads
   - Check HuggingFace model availability

4. **Memory Issues**
   - Reduce chunk size in text processing
   - Implement conversation cleanup

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/medical-feature`)
3. Commit your changes (`git commit -m 'Add medical feature'`)
4. Push to the branch (`git push origin feature/medical-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This MediBot is for educational and research purposes only. It should not be considered as medical advice. Always consult with qualified healthcare professionals for medical matters. The accuracy of responses depends on the quality of the medical knowledge base and should be verified independently.



