# AI Finance Advisor

![Finance Analysis](https://img.shields.io/badge/AI-Finance%20Analysis-blue?style=for-the-badge&logo=chart-line)

## Introduction

The **AI Finance Advisor** is an intelligent financial analysis system that leverages multi-agent AI technology to provide comprehensive stock market insights, real-time financial data, and expert recommendations. Built with the Phi framework, this application combines the power of specialized financial analysis agents with web research capabilities to deliver actionable investment intelligence.

## Features

- **Multi-Agent Financial Analysis**: Collaborative AI agents specializing in different aspects of financial analysis
- **Real-Time Stock Data**: Live stock prices, fundamentals, and analyst recommendations via Yahoo Finance integration
- **Web Research Integration**: Comprehensive market research using DuckDuckGo search capabilities
- **Intelligent Data Presentation**: Automated table generation for easy comparison and analysis
- **Source Attribution**: All information includes proper source citations for transparency
- **Team-Based Analysis**: Coordinated analysis from multiple specialized agents

## Tech Stack

- **Python 3.11+**
- **Phi Framework** – for multi-agent orchestration and management
- **OpenAI GPT-4o-mini** – for intelligent financial analysis and reasoning
- **Yahoo Finance Tools** – for real-time stock market data and fundamentals
- **DuckDuckGo Search** – for comprehensive web research and market intelligence
- **Markdown Support** – for rich text formatting and data presentation

## Architecture

The system employs a sophisticated multi-agent architecture:

### Financial Analyst Agent
- **Role**: Specialized financial data analysis and interpretation
- **Capabilities**: Stock price analysis, fundamentals review, analyst recommendations
- **Tools**: Yahoo Finance integration for real-time market data
- **Output**: Structured financial analysis with comparison tables

### Web Researcher Agent
- **Role**: Market intelligence and external research
- **Capabilities**: Web search, news analysis, market sentiment gathering
- **Tools**: DuckDuckGo search for comprehensive information gathering
- **Output**: Sourced market intelligence with proper attribution

### Agents Team
- **Role**: Coordinated analysis and synthesis
- **Capabilities**: Combines insights from both specialized agents
- **Features**: Debug mode for transparency, source tracking, table generation
- **Output**: Comprehensive financial reports with multiple perspectives

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/AI-Finance-Advisor.git
   cd AI-Finance-Advisor
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install phi-agent phi-model-openai phi-tools-duckduckgo phi-tools-yfinance
   ```

4. **Set up environment variables:**
   ```bash
   export OPENAI_API_KEY="your-openai-api-key"
   ```

## Usage

### Basic Financial Analysis

```python
from financial_advisors import agents_team

# Get comprehensive analysis of a stock
response = agents_team.print_response("Analyze Tesla's current market position and future outlook")
```

### Individual Agent Usage

```python
from financial_advisors import financial_agent, web_researcher

# Get stock fundamentals
financial_agent.print_response("What are the key fundamentals for Apple stock?")

# Research market trends
web_researcher.print_response("What are the latest trends in renewable energy stocks?")
```

### Advanced Analysis

```python
# Compare multiple stocks
agents_team.print_response("Compare the analyst recommendations for NVIDIA, AMD, and Intel")
```

## Configuration

### Model Selection
The system supports multiple LLM providers:

```python
# OpenAI (default)
model=OpenAIChat(id="gpt-4o-mini")

# Groq (alternative)
model=Groq(id="llama-3.3-70b-versatile")
```

### Tool Configuration
Customize the available tools for each agent:

```python
financial_agent = Agent(
    tools=[YFinanceTools(
        stock_price=True,
        analyst_recommendations=True,
        stock_fundamentals=True,
        # Add more tools as needed
    )]
)
```

## API Reference

### Financial Analyst Agent
- **Purpose**: Specialized financial data analysis
- **Input**: Natural language queries about stocks, markets, or financial data
- **Output**: Structured financial analysis with tables and comparisons

### Web Researcher Agent
- **Purpose**: Market intelligence and external research
- **Input**: Research queries about market trends, news, or company information
- **Output**: Sourced information with proper attribution

### Agents Team
- **Purpose**: Coordinated multi-agent analysis
- **Input**: Complex financial queries requiring multiple perspectives
- **Output**: Comprehensive reports combining financial data and market intelligence

## Examples

### Stock Analysis
```
Query: "Analyze NVIDIA's current market position"
Output: 
- Current stock price and fundamentals
- Analyst recommendations summary
- Recent market news and sentiment
- Comparison tables with competitors
```

### Market Research
```
Query: "What are the emerging trends in AI stocks?"
Output:
- Web research findings
- Market sentiment analysis
- Key players and developments
- Sourced information with citations
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This AI Finance Advisor is for educational and research purposes only. It should not be considered as financial advice. Always consult with qualified financial professionals before making investment decisions. The accuracy of financial data and analysis depends on various market factors and should be verified independently.