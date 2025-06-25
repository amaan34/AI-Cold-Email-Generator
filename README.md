# AI Cold Email Generator

## Introduction

The **AI Cold Email Generator** is an intelligent, automated tool designed to streamline and personalize the process of reaching out to potential clients. By leveraging advanced web scraping, natural language processing, and portfolio matching, this application crafts compelling cold emails tailored to job postings and business opportunities. It empowers consulting firms and freelancers to efficiently showcase relevant expertise, increasing the likelihood of successful client engagement.

## Features

- **Automated Opportunity Discovery**: Scrapes job postings and business opportunities from provided URLs.
- **Intelligent Information Extraction**: Uses LLMs to extract key job requirements and relevant details from unstructured text.
- **Portfolio Matching**: Matches your portfolio projects to the skills and requirements of each opportunity using a vector database.
- **Personalized Email Generation**: Crafts persuasive, context-aware cold emails that highlight your most relevant projects.
- **Streamlit Web Interface**: User-friendly interface for inputting URLs and viewing generated emails.
- **Scalable and Extensible**: Easily add new portfolio projects or adapt to different consulting domains.

## Tech Stack

- **Python 3.11+**
- **Streamlit** – for the web application interface
- **LangChain** (including `langchain_community` and `langchain_groq`) – for LLM orchestration and prompt management
- **ChromaDB** – for vector-based portfolio search and matching
- **Pandas, CSV** – for portfolio data handling
- **Regex** – for text cleaning and preprocessing

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/AI-Cold-Email-Generator.git
   cd AI-Cold-Email-Generator
   ```

2. **Create and activate a virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install streamlit langchain langchain-community langchain-groq chromadb pandas
   ```

   > *Note: You may need to install additional dependencies if your LLM provider requires them.*

4. **(Optional) Add your portfolio projects:**
   - Edit `sample_portfolio.csv` to include your own technical skills and project links.

## Usage

1. **Start the Streamlit app:**
   ```bash
   streamlit run main.py
   ```

2. **In the web interface:**
   - Enter the URL of a job posting or opportunity.
   - Click "Submit".
   - View the generated cold email, tailored to the opportunity and your portfolio.

3. **Customize:**
   - Update `sample_portfolio.csv` with your latest projects.
   - Modify prompt templates in `chains.py` for your firm's branding or tone.

## License

This project is licensed under the [MIT License](LICENSE).

