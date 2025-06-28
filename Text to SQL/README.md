# Text to SQL

![SQL AI](https://img.shields.io/badge/AI-SQL%20Generator-blue?style=for-the-badge&logo=database)

## Introduction

The **Text to SQL** is an intelligent natural language to SQL query converter that transforms human-readable questions into executable SQL statements. Built with Streamlit and advanced language models, this application allows users to interact with databases using natural language, making data querying accessible to non-technical users while maintaining the power and flexibility of SQL.

## Features

- **Natural Language Processing**: Convert English questions to SQL queries using AI
- **Interactive Web Interface**: User-friendly Streamlit application for easy interaction
- **Real-Time Query Execution**: Instant SQL generation and database execution
- **Multiple Database Support**: Compatible with various SQL databases (SQLite, PostgreSQL, MySQL)
- **Error Handling**: Robust error handling and user feedback
- **Query Validation**: Automatic SQL syntax validation and optimization
- **Result Visualization**: Clean display of query results
- **Educational Tool**: Learn SQL through natural language interaction

## Tech Stack

- **Python 3.11+**
- **Streamlit** – for the web application interface
- **LangChain** – for LLM integration and prompt management
- **Groq** – for high-performance LLM inference (Llama 3.1 70B)
- **SQLite** – for database management and storage
- **Pandas** – for data manipulation and result processing
- **SQLAlchemy** – for database abstraction and connection management

## Architecture

The system implements a sophisticated NLP-to-SQL pipeline:

### Natural Language Processing
1. **User Input**: Natural language questions through Streamlit interface
2. **Query Analysis**: LLM analysis of question intent and database schema
3. **SQL Generation**: Conversion to valid SQL syntax
4. **Query Validation**: Syntax and semantic validation

### Database Interaction
1. **Connection Management**: Secure database connections
2. **Query Execution**: Safe SQL execution with error handling
3. **Result Processing**: Data formatting and presentation
4. **Response Display**: Clean result visualization

### Schema Understanding
- **Table Structure**: Automatic schema detection and understanding
- **Column Mapping**: Intelligent mapping of natural language to database columns
- **Relationship Recognition**: Understanding of table relationships and joins

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/amaan34/Text-to-SQL.git
   cd Text-to-SQL
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install streamlit langchain-groq sqlite3 pandas
   ```

4. **Set up environment variables:**
   ```bash
   export GROQ_API_KEY="your-groq-api-key"
   ```

5. **Initialize the database:**
   ```bash
   python database.py
   ```

## Usage

### Starting the Application

```bash
streamlit run app.py
```

### Using Text to SQL

1. **Enter Question**: Type your question in natural language
2. **Submit Query**: Click "Enter" to generate SQL
3. **View Results**: See the generated SQL and query results
4. **Learn**: Understand how natural language maps to SQL

### Example Queries

```
- "How many students are in the database?"
- "Show me all students studying Data Science"
- "What is the average marks for each course?"
- "Which students have marks above 80?"
- "Count students by section"
```

## Configuration

### Database Configuration

```python
# SQLite database configuration
DATABASE_PATH = "student.db"

# Database connection
def get_database_connection():
    return sqlite3.connect(DATABASE_PATH)
```

### LLM Configuration

```python
# Groq LLM configuration
llm = ChatGroq(
    model="llama3-8b-8192",
    groq_api_key=os.environ.get("GROQ_API_KEY"),
    temperature=0.1  # Low temperature for consistent SQL generation
)
```

### Prompt Template

```python
GROQ_SYS_PROMPT = """
You are an expert in converting English questions to SQL query!
The SQL database has the name STUDENT and has the following columns - NAME, COURSE, 
SECTION and MARKS.

Example conversions:
- "How many entries of records are present?" → SELECT COUNT(*) FROM STUDENT;
- "Tell me all the students studying in Data Science COURSE?" → SELECT * FROM STUDENT WHERE COURSE="Data Science";

Rules:
- No ``` in beginning or end
- No "sql" word in output
- Only valid SQL please
- No preamble, only SQL query

Convert: {user_query}
"""
```

## API Reference

### Main Functions

#### `get_sql_query(user_query)`
- **Purpose**: Convert natural language to SQL query
- **Parameters**: `user_query` - Natural language question
- **Returns**: Generated SQL query string
- **Process**: LLM-based conversion with prompt engineering

#### `return_sql_response(sql_query)`
- **Purpose**: Execute SQL query and return results
- **Parameters**: `sql_query` - SQL query string
- **Returns**: Query results as list of tuples
- **Features**: Error handling and result formatting

#### `main()`
- **Purpose**: Main Streamlit application entry point
- **Features**: User interface, query processing, result display

### Database Schema

```sql
-- Student table schema
CREATE TABLE STUDENT (
    NAME TEXT,
    COURSE TEXT,
    SECTION TEXT,
    MARKS INTEGER
);

-- Sample data
INSERT INTO STUDENT VALUES ('John Doe', 'Data Science', 'A', 85);
INSERT INTO STUDENT VALUES ('Jane Smith', 'Computer Science', 'B', 92);
```

## File Structure

```
Text-to-SQL/
├── app.py                 # Main Streamlit application
├── database.py            # Database initialization and management
├── student.db             # SQLite database file
├── requirements.txt       # Python dependencies
└── README.md             # This file
```

## Customization

### Adding New Database Tables

```python
# Add new table to schema
def create_new_table():
    conn = sqlite3.connect('student.db')
    cursor = conn.cursor()
    
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS COURSES (
            COURSE_ID INTEGER PRIMARY KEY,
            COURSE_NAME TEXT,
            INSTRUCTOR TEXT,
            CREDITS INTEGER
        )
    """)
    
    conn.commit()
    conn.close()
```

### Custom Prompt Templates

```python
# Domain-specific prompt template
CUSTOM_PROMPT = """
You are a {domain} database expert. Convert English questions to SQL for the {table_name} table.

Schema: {schema_description}

Examples:
{example_conversions}

Question: {user_query}
SQL Query:
"""
```

### Multiple Database Support

```python
# PostgreSQL support
import psycopg2

def postgres_connection():
    return psycopg2.connect(
        host="localhost",
        database="mydb",
        user="username",
        password="password"
    )

# MySQL support
import mysql.connector

def mysql_connection():
    return mysql.connector.connect(
        host="localhost",
        database="mydb",
        user="username",
        password="password"
    )
```

## Advanced Features

### Query Optimization

```python
# SQL query optimization
def optimize_query(sql_query):
    # Add query optimization logic
    optimized_query = sql_query.replace("SELECT *", "SELECT specific_columns")
    return optimized_query
```

### Result Visualization

```python
# Enhanced result display
def display_results(results):
    if results:
        df = pd.DataFrame(results, columns=['Name', 'Course', 'Section', 'Marks'])
        st.dataframe(df)
        st.bar_chart(df['Marks'])
```

### Query History

```python
# Track query history
@st.cache_data
def get_query_history():
    return []

def save_query(natural_query, sql_query, results):
    history = get_query_history()
    history.append({
        'natural_query': natural_query,
        'sql_query': sql_query,
        'timestamp': datetime.now(),
        'result_count': len(results)
    })
```

## Examples

### Basic Queries

```
Input: "How many students are there?"
Output: SELECT COUNT(*) FROM STUDENT;

Input: "Show me all Data Science students"
Output: SELECT * FROM STUDENT WHERE COURSE="Data Science";

Input: "What's the average marks?"
Output: SELECT AVG(MARKS) FROM STUDENT;
```

### Complex Queries

```
Input: "Which students have marks above the average?"
Output: SELECT * FROM STUDENT WHERE MARKS > (SELECT AVG(MARKS) FROM STUDENT);

Input: "Count students by course"
Output: SELECT COURSE, COUNT(*) FROM STUDENT GROUP BY COURSE;

Input: "Show top 3 students by marks"
Output: SELECT * FROM STUDENT ORDER BY MARKS DESC LIMIT 3;
```

## Performance Optimization

### Query Caching

```python
# Cache frequently used queries
@st.cache_data(ttl=3600)
def cached_sql_generation(user_query):
    return get_sql_query(user_query)
```

### Connection Pooling

```python
# Implement connection pooling
from contextlib import contextmanager

@contextmanager
def get_db_connection():
    conn = sqlite3.connect('student.db')
    try:
        yield conn
    finally:
        conn.close()
```

### Batch Processing

```python
# Process multiple queries efficiently
def batch_process_queries(queries):
    results = []
    for query in queries:
        sql = get_sql_query(query)
        result = return_sql_response(sql)
        results.append(result)
    return results
```

## Troubleshooting

### Common Issues

1. **SQL Generation Errors**
   - Check prompt template clarity
   - Verify database schema in prompt
   - Ensure proper example formatting

2. **Database Connection Issues**
   - Verify database file exists
   - Check file permissions
   - Ensure proper SQLite installation

3. **LLM API Errors**
   - Verify GROQ_API_KEY environment variable
   - Check API rate limits
   - Ensure proper model access

4. **Query Execution Errors**
   - Validate SQL syntax before execution
   - Check table and column names
   - Handle NULL values appropriately

### Debug Mode

```python
# Enable debug logging
import logging
logging.basicConfig(level=logging.DEBUG)

# Verbose SQL generation
def debug_sql_generation(user_query):
    print(f"Input query: {user_query}")
    sql = get_sql_query(user_query)
    print(f"Generated SQL: {sql}")
    return sql
```

## Security Considerations

### SQL Injection Prevention

```python
# Use parameterized queries
def safe_query_execution(sql, params=None):
    conn = sqlite3.connect('student.db')
    cursor = conn.cursor()
    
    if params:
        cursor.execute(sql, params)
    else:
        cursor.execute(sql)
    
    return cursor.fetchall()
```

### Input Validation

```python
# Validate user input
def validate_input(user_query):
    # Check for malicious patterns
    dangerous_patterns = ['DROP', 'DELETE', 'UPDATE', 'INSERT']
    
    for pattern in dangerous_patterns:
        if pattern.lower() in user_query.lower():
            raise ValueError(f"Potentially dangerous operation: {pattern}")
    
    return user_query
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/sql-feature`)
3. Commit your changes (`git commit -m 'Add SQL feature'`)
4. Push to the branch (`git push origin feature/sql-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This Text to SQL converter is for educational and research purposes. Generated SQL queries should be reviewed before execution in production environments. The accuracy of SQL generation depends on the quality of the language model and should be validated independently.
