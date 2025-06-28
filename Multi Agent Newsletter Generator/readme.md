# Multi Agent Newsletter Generator

![Newsletter AI](https://img.shields.io/badge/AI-Newsletter%20Generator-blue?style=for-the-badge&logo=newspaper)

## Introduction

The **Multi Agent Newsletter Generator** is an intelligent content creation system that leverages CrewAI's multi-agent framework to automatically generate comprehensive, well-researched newsletters on any topic. This system employs specialized AI agents working collaboratively to research, write, and refine content, delivering publication-ready newsletters with proper sourcing and professional quality.

## Features

- **Multi-Agent Collaboration**: Specialized agents for research, writing, and proofreading
- **Intelligent Research**: Technology intelligence specialist with global information monitoring
- **Professional Writing**: Expert storyteller with progressive depth technique
- **Quality Assurance**: Principal proofreader ensuring accuracy and readability
- **Source Attribution**: Proper citation and three additional sources for each topic
- **Topic Flexibility**: Generate newsletters on any subject with expert-level analysis
- **Real-Time Intelligence**: Track emerging breakthroughs and technological trends
- **Publication Ready**: Polished content ready for immediate publication

## Tech Stack

- **Python 3.11+**
- **CrewAI** – for multi-agent orchestration and collaboration
- **LangChain** – for LLM integration and prompt management
- **Groq** – for high-performance LLM inference (Llama 3.1 70B)
- **Google Search Tools** – for comprehensive web research
- **Memory Management** – for agent context and learning
- **Markdown Output** – for formatted newsletter generation

## Architecture

The system employs a sophisticated three-agent architecture:

### Technology Intelligence Specialist & Innovation Scout
- **Role**: Primary researcher and trend analyst
- **Capabilities**: 
  - Track emerging breakthroughs across academia, industry, and startups
  - Identify patterns and connections between developments
  - Predict technological inflection points
  - Assess real-world impact potential
  - Validate findings through multiple sources
- **Background**: Former quantum computing researcher with global intelligence network
- **Methodology**: Multi-Source Intelligence combining academic papers, patents, startup activities, and social signals

### Technology Storyteller & Innovation Chronicler
- **Role**: Content creation and narrative development
- **Capabilities**:
  - Transform complex concepts into compelling narratives
  - Bridge technical complexity and public understanding
  - Weave historical context with current developments
  - Challenge misconceptions while maintaining accuracy
  - Create memorable analogies and examples
- **Background**: Former quantum physicist turned science communicator
- **Technique**: Progressive Depth (Surface, Middle, Deep levels of understanding)

### Principal Proofreader
- **Role**: Quality assurance and content refinement
- **Capabilities**:
  - Ensure polished, accurate, and readable content
  - Proper grammar and sentence refinement
  - Source citation and verification
  - Provide three additional sources for further reading
  - Maintain highest standards of credibility
- **Focus**: Accuracy, clarity, and professional presentation

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/Multi-Agent-Newsletter-Generator.git
   cd Multi-Agent-Newsletter-Generator
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install crewai langchain-groq google-search-python
   ```

4. **Set up environment variables:**
   ```bash
   export GROQ_API_KEY="your-groq-api-key"
   export GOOGLE_API_KEY="your-google-api-key"
   export GOOGLE_CSE_ID="your-custom-search-engine-id"
   ```

## Usage

### Basic Newsletter Generation

```python
from crew import crew
from tasks import create_newsletter_task

# Generate newsletter on a specific topic
task = create_newsletter_task("Artificial Intelligence in Healthcare")
result = crew.kickoff()
```

### Advanced Configuration

```python
from agents import researcher, writer, proof_reader
from crewai import Crew

# Create custom crew with specific agents
crew = Crew(
    agents=[researcher, writer, proof_reader],
    tasks=[task],
    verbose=True,
    memory=True
)

# Generate newsletter
result = crew.kickoff()
```

### Running the Complete Pipeline

```bash
python crew.py
```

## Configuration

### Agent Configuration

Each agent can be customized for specific domains:

```python
# Technology Intelligence Specialist
researcher = Agent(
    role="Technology Intelligence Specialist & Innovation Scout",
    goal="""
    1. Track emerging breakthroughs in {topic} across academia, industry, and startups
    2. Identify patterns and connections between seemingly unrelated developments
    3. Predict future technological inflection points by analyzing current signals
    4. Assess real-world impact potential and adoption barriers
    5. Validate findings through multiple authoritative sources""",
    backstory="""Former quantum computing researcher turned tech intelligence expert...""",
    memory=True,
    verbose=True,
    llm=llm,
    tools=[google_search_tool],
    allow_delegation=True
)
```

### Model Configuration

The system supports multiple LLM providers:

```python
# Groq (default)
llm = ChatGroq(
    model="groq/llama-3.1-70b-versatile",
    verbose=True,
    temperature=0,
    groq_api_key=os.getenv('GROQ_API_KEY')
)

# OpenAI (alternative)
llm = ChatOpenAI(
    model="gpt-4",
    temperature=0.7,
    openai_api_key=os.getenv('OPENAI_API_KEY')
)
```

### Task Configuration

Customize newsletter tasks for specific requirements:

```python
def create_newsletter_task(topic):
    return Task(
        description=f"""
        Create a comprehensive newsletter on {topic} that includes:
        1. Current developments and breakthroughs
        2. Historical context and future implications
        3. Real-world applications and impact
        4. Expert analysis and predictions
        5. Three additional sources for further reading
        """,
        expected_output="A well-structured newsletter in markdown format",
        agent=researcher
    )
```

## API Reference

### Agent Classes

#### Technology Intelligence Specialist
- **Purpose**: Primary research and trend analysis
- **Input**: Topic specification and research requirements
- **Output**: Comprehensive research findings with source validation
- **Tools**: Google Search, web research capabilities

#### Technology Storyteller
- **Purpose**: Content creation and narrative development
- **Input**: Research findings and topic requirements
- **Output**: Engaging, well-structured content with progressive depth
- **Features**: Three-level content approach (Surface, Middle, Deep)

#### Principal Proofreader
- **Purpose**: Quality assurance and content refinement
- **Input**: Draft content from writer
- **Output**: Polished, accurate content with proper citations
- **Features**: Source verification and additional resource provision

### Crew Management

#### `Crew.kickoff()`
- **Purpose**: Execute the complete newsletter generation pipeline
- **Returns**: Final newsletter content
- **Features**: Agent collaboration, memory management, verbose logging

#### `Agent.delegate()`
- **Purpose**: Allow agents to delegate tasks to other agents
- **Parameters**: Task description and target agent
- **Returns**: Delegated task results

## File Structure

```
Multi-Agent-Newsletter-Generator/
├── agents.py              # Agent definitions and configurations
├── tasks.py               # Task definitions and workflows
├── tools.py               # Tool definitions (Google Search, etc.)
├── crew.py                # Main crew orchestration
├── newsletter.md          # Generated newsletter example
├── readme.md             # This file
└── requirements.txt      # Python dependencies
```

## Customization

### Domain-Specific Agents

Create specialized agents for different domains:

```python
# Medical Research Agent
medical_researcher = Agent(
    role="Medical Research Specialist",
    goal="Track medical breakthroughs and clinical developments",
    backstory="Former medical researcher with expertise in clinical trials",
    tools=[medical_search_tool, clinical_database_tool]
)

# Financial Analyst Agent
financial_analyst = Agent(
    role="Financial Market Analyst",
    goal="Analyze market trends and investment opportunities",
    backstory="Experienced financial analyst with global market expertise",
    tools=[financial_data_tool, market_news_tool]
)
```

### Custom Tools

Extend agent capabilities with custom tools:

```python
from langchain.tools import BaseTool

class CustomResearchTool(BaseTool):
    name = "custom_research"
    description = "Perform specialized research in specific domain"
    
    def _run(self, query: str) -> str:
        # Implement custom research logic
        return research_results
```

### Newsletter Templates

Customize newsletter output format:

```python
NEWSLETTER_TEMPLATE = """
# {topic} Newsletter

## Executive Summary
{summary}

## Key Developments
{developments}

## Expert Analysis
{analysis}

## Future Outlook
{outlook}

## Additional Resources
{sources}

---
*Generated by Multi-Agent Newsletter Generator*
"""
```

## Performance Optimization

### Agent Memory Management

```python
# Enable memory for better context retention
agent = Agent(
    memory=True,
    memory_config={
        "max_tokens": 1000,
        "return_messages": True
    }
)
```

### Parallel Processing

```python
# Enable parallel task execution
crew = Crew(
    agents=agents,
    tasks=tasks,
    verbose=True,
    memory=True,
    process=Process.sequential  # or Process.hierarchical
)
```

### Caching Strategy

```python
# Implement caching for research results
@st.cache_data
def get_research_results(topic):
    return researcher.run(f"Research {topic}")
```

## Examples

### Technology Newsletter
```
Topic: "Quantum Computing Breakthroughs"
Output:
- Current quantum supremacy achievements
- Comparison of different quantum approaches
- Real-world applications and limitations
- Expert predictions for next 5 years
- Three additional research sources
```

### Healthcare Newsletter
```
Topic: "AI in Medical Diagnosis"
Output:
- Latest AI diagnostic tools and accuracy rates
- Clinical trial results and FDA approvals
- Integration challenges in healthcare systems
- Future implications for medical practice
- Three additional medical research sources
```

## Troubleshooting

### Common Issues

1. **API Key Errors**
   - Verify GROQ_API_KEY environment variable
   - Check Google API credentials
   - Ensure proper API permissions

2. **Agent Communication Issues**
   - Enable verbose mode for debugging
   - Check agent delegation permissions
   - Verify tool availability

3. **Memory Issues**
   - Reduce memory token limits
   - Implement memory cleanup
   - Use hierarchical processing

4. **Content Quality Issues**
   - Adjust agent goals and backstories
   - Modify task descriptions
   - Fine-tune LLM parameters

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/newsletter-feature`)
3. Commit your changes (`git commit -m 'Add newsletter feature'`)
4. Push to the branch (`git push origin feature/newsletter-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This Multi-Agent Newsletter Generator is for educational and research purposes. Generated content should be reviewed and verified before publication. The accuracy of information depends on the quality of research sources and should be validated independently.