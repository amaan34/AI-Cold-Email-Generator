# NewsNinja

![News AI](https://img.shields.io/badge/AI-News%20Summarizer-blue?style=for-the-badge&logo=radio)

## Introduction

The **NewsNinja** is an intelligent news and Reddit content summarizer that transforms complex information into concise, engaging audio summaries. Built with a modern microservices architecture, this application combines web scraping, natural language processing, and text-to-speech technology to deliver personalized news consumption experiences.

## Features

- **Multi-Source Content Aggregation**: Scrape news from multiple sources and Reddit communities
- **Intelligent Content Summarization**: AI-powered summarization of news articles and Reddit posts
- **Audio Generation**: Convert summaries to natural-sounding speech using TTS technology
- **Topic-Based Filtering**: Focus on specific topics of interest
- **Real-Time Processing**: Live content scraping and processing
- **User-Friendly Interface**: Clean Streamlit frontend with intuitive controls
- **Downloadable Audio**: Save audio summaries for offline listening
- **Backend API**: RESTful API for programmatic access

## Tech Stack

### Frontend
- **Streamlit** – for the web application interface
- **Requests** – for API communication with backend

### Backend
- **FastAPI** – for high-performance REST API
- **Uvicorn** – for ASGI server
- **Pydantic** – for data validation and serialization

### Content Processing
- **BeautifulSoup** – for web scraping and HTML parsing
- **PRAW (Python Reddit API Wrapper)** – for Reddit content extraction
- **LangChain** – for AI-powered content summarization
- **Groq** – for high-performance LLM inference

### Audio Generation
- **gTTS (Google Text-to-Speech)** – for natural speech synthesis
- **Audio Processing** – for audio format conversion and optimization

### Data Management
- **SQLite** – for local data storage
- **Pandas** – for data manipulation and analysis

## Architecture

The system follows a modern microservices architecture:

### Frontend Service (Streamlit)
- **User Interface**: Topic management and audio generation controls
- **API Integration**: Communication with backend services
- **Audio Playback**: Real-time audio streaming and download functionality

### Backend Service (FastAPI)
- **Content Scraping**: News and Reddit content extraction
- **AI Summarization**: Intelligent content processing and summarization
- **Audio Synthesis**: Text-to-speech conversion
- **Data Management**: Content storage and retrieval

### Content Sources
- **News APIs**: Multiple news source integration
- **Reddit API**: Subreddit content extraction
- **Web Scraping**: Direct website content extraction

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/NewsNinja.git
   cd NewsNinja
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
   export GROQ_API_KEY="your-groq-api-key"
   export REDDIT_CLIENT_ID="your-reddit-client-id"
   export REDDIT_CLIENT_SECRET="your-reddit-client-secret"
   export REDDIT_USER_AGENT="your-reddit-user-agent"
   ```

5. **Start the backend server:**
   ```bash
   python backend.py
   ```

6. **Start the frontend application:**
   ```bash
   streamlit run frontend.py
   ```

## Usage

### Basic Operation

1. **Add Topics**: Enter topics of interest in the topic management section
2. **Select Sources**: Choose between news, Reddit, or both sources
3. **Generate Summary**: Click "Generate Summary" to create audio content
4. **Listen/Download**: Play the audio or download for offline use

### Advanced Features

#### Topic Management
```python
# Add multiple topics
topics = ["Artificial Intelligence", "Climate Change", "Space Exploration"]

# Remove topics
if remove_button_clicked:
    del topics[index]
```

#### Source Selection
```python
# Choose data sources
source_type = "both"  # Options: "news", "reddit", "both"
```

#### Audio Generation
```python
# Generate audio summary
response = requests.post(
    f"{BACKEND_URL}/generate-news-audio",
    json={
        "topics": topics,
        "source_type": source_type
    }
)
```

## Configuration

### Backend Configuration

```python
# FastAPI server configuration
app = FastAPI(
    title="NewsNinja API",
    description="AI-powered news and Reddit summarizer",
    version="1.0.0"
)

# CORS configuration
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"]
)
```

### Content Scraping Configuration

```python
# News scraping settings
NEWS_SOURCES = [
    "https://news.ycombinator.com",
    "https://techcrunch.com",
    "https://arstechnica.com"
]

# Reddit scraping settings
REDDIT_SUBREDDITS = [
    "technology",
    "science",
    "worldnews"
]
```

### AI Model Configuration

```python
# Groq LLM configuration
llm = ChatGroq(
    model="groq/llama-3.1-70b-versatile",
    temperature=0.7,
    groq_api_key=os.getenv("GROQ_API_KEY")
)

# Summarization prompt
SUMMARIZATION_PROMPT = """
Summarize the following content in a concise, engaging way:
{content}

Focus on key points and maintain natural flow for audio narration.
"""
```

## API Reference

### Endpoints

#### `POST /generate-news-audio`
- **Purpose**: Generate audio summary from news and Reddit content
- **Parameters**:
  - `topics` (List[str]): Topics to search for
  - `source_type` (str): "news", "reddit", or "both"
- **Returns**: Audio file (MP3 format)

#### `GET /health`
- **Purpose**: Health check endpoint
- **Returns**: Status information

### Data Models

```python
class AudioRequest(BaseModel):
    topics: List[str]
    source_type: Literal["news", "reddit", "both"]

class AudioResponse(BaseModel):
    audio_data: bytes
    filename: str
    duration: float
```

## File Structure

```
NewsNinja/
├── frontend.py              # Streamlit frontend application
├── backend.py               # FastAPI backend server
├── news_scraper.py          # News content scraping utilities
├── reddit_scraper.py        # Reddit content extraction
├── utils.py                 # Utility functions and helpers
├── models.py                # Data models and schemas
├── Pipfile                  # Pipenv configuration
├── Pipfile.lock             # Locked dependencies
├── audio/                   # Generated audio files
├── ai-journalist.pdf        # Project documentation
└── README.md               # This file
```

## Customization

### Adding New News Sources

```python
# Add new news source
def scrape_custom_news(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    
    # Extract articles
    articles = soup.find_all('article')
    
    # Process articles
    for article in articles:
        title = article.find('h2').text
        content = article.find('p').text
        # Process and store content
```

### Custom Reddit Communities

```python
# Add new subreddits
REDDIT_SUBREDDITS.extend([
    "machinelearning",
    "datascience",
    "programming"
])
```

### Audio Customization

```python
# Custom TTS settings
def generate_audio(text, filename):
    tts = gTTS(
        text=text,
        lang='en',
        slow=False,
        lang_check=True
    )
    tts.save(filename)
```

### Summarization Styles

```python
# Different summarization prompts
SUMMARIZATION_STYLES = {
    "concise": "Summarize in 2-3 sentences",
    "detailed": "Provide comprehensive summary with key points",
    "casual": "Summarize in conversational tone",
    "professional": "Summarize in formal business style"
}
```

## Performance Optimization

### Caching Strategy

```python
# Cache news content
@st.cache_data(ttl=3600)  # Cache for 1 hour
def get_news_content(topic):
    return scrape_news(topic)

# Cache Reddit content
@st.cache_data(ttl=1800)  # Cache for 30 minutes
def get_reddit_content(subreddit):
    return scrape_reddit(subreddit)
```

### Async Processing

```python
# Async content scraping
async def scrape_multiple_sources(topics):
    tasks = []
    for topic in topics:
        tasks.append(scrape_news_async(topic))
        tasks.append(scrape_reddit_async(topic))
    
    results = await asyncio.gather(*tasks)
    return results
```

### Audio Optimization

```python
# Optimize audio file size
def optimize_audio(input_file, output_file):
    audio = AudioSegment.from_mp3(input_file)
    # Compress audio
    audio.export(output_file, format="mp3", bitrate="64k")
```

## Examples

### News Summary Generation
```
Input: Topics=["AI", "Climate Change"]
Output: 
- Scraped 15 news articles
- Generated 3-minute audio summary
- Covered key developments in AI and climate science
- Included expert opinions and future implications
```

### Reddit Content Summary
```
Input: Subreddits=["technology", "science"]
Output:
- Extracted top posts from last 24 hours
- Generated 2-minute audio summary
- Highlighted trending discussions and breakthroughs
- Included community insights and expert comments
```

## Troubleshooting

### Common Issues

1. **Backend Connection Error**
   - Verify backend server is running on correct port
   - Check CORS configuration
   - Ensure proper API endpoint URLs

2. **Content Scraping Issues**
   - Verify internet connection
   - Check website accessibility
   - Update scraping selectors if websites change

3. **Audio Generation Problems**
   - Check gTTS internet connection
   - Verify text content is not empty
   - Ensure proper audio file permissions

4. **Reddit API Errors**
   - Verify Reddit API credentials
   - Check rate limiting compliance
   - Ensure proper user agent string

### Debug Mode

```python
# Enable debug logging
import logging
logging.basicConfig(level=logging.DEBUG)

# Verbose API responses
response = requests.post(url, json=data, verbose=True)
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/news-feature`)
3. Commit your changes (`git commit -m 'Add news feature'`)
4. Push to the branch (`git push origin feature/news-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This NewsNinja is for educational and research purposes. Generated summaries should be verified against original sources. The accuracy of content depends on the quality of source materials and should be validated independently.