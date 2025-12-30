# AssistGen - AI Intelligent Customer Service System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.68+-green.svg)
![Vue](https://img.shields.io/badge/Vue-3.3+-brightgreen.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-5.2+-blue.svg)
![LangGraph](https://img.shields.io/badge/LangGraph-0.3.25-orange.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

**Enterprise-grade Intelligent Customer Service Solution Built on Large Language Models**

[Features](#features) • [Quick Start](#quick-start) • [Architecture](#architecture) • [API Docs](#api-documentation) • [Deployment](#deployment-guide)

</div>

---

## Introduction

AssistGen is a full-stack intelligent customer service system built with **FastAPI + Vue 3**. The system integrates multiple large language models (DeepSeek V3, Qwen2.5, Llama3, etc.) and covers mainstream Agent and RAG application scenarios in the intelligent customer service domain.

### Key Highlights

- 🤖 **Multi-Model Support** - Seamlessly switch between DeepSeek online API and Ollama local models
- 🧠 **LangGraph Multi-Agent** - State graph-based intelligent routing and task orchestration
- 📊 **GraphRAG Knowledge Graph** - Neo4j-powered structured knowledge retrieval
- 💬 **Streaming Response** - Real-time SSE push for enhanced user experience
- 🔐 **Enterprise Security** - JWT authentication, password encryption, semantic caching

---

## Features

### 1. General Q&A

| Feature | Description |
|---------|-------------|
| DeepSeek V3 | High-quality conversations via online API |
| Ollama Integration | Support for local models like Qwen2.5, Llama3 |
| Semantic Cache | Redis-powered, reduces costs and improves response speed |

### 2. Deep Thinking

| Feature | Description |
|---------|-------------|
| DeepSeek R1 | Dedicated model for complex reasoning tasks |
| Chain-of-Thought Display | Real-time display of model reasoning process |

### 3. Web Search

| Feature | Description |
|---------|-------------|
| SerpAPI Integration | Real-time retrieval of latest web information |
| Result Aggregation | Intelligent synthesis of search results for answers |

### 4. RAG Document Q&A

| Feature | Description |
|---------|-------------|
| Document Parsing | Support for PDF and Word document uploads |
| Vector Retrieval | FAISS + Sentence Transformers |
| Context Enhancement | Generate answers based on relevant document snippets |

### 5. GraphRAG Knowledge Graph

| Feature | Description |
|---------|-------------|
| Neo4j Integration | Graph database for entity and relationship storage |
| Auto Extraction | Extract entities, relationships, and communities from documents |
| Cypher Queries | Automatic generation of graph query statements |

### 6. E-commerce Features

| Feature | Description |
|---------|-------------|
| Product Search | Intelligent product retrieval based on knowledge graph |
| Order Management | Customer order information queries |
| Smart Recommendations | Relationship-based product recommendations |

### 7. Session Management

| Feature | Description |
|---------|-------------|
| Multi-turn Dialogue | Complete context preservation |
| History Records | MySQL persistent storage |
| Session Types | Normal / Deep Thinking / Web Search / RAG |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend (Vue 3)                          │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐            │
│  │  Views  │  │ Stores  │  │ Router  │  │Services │            │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘            │
│       └────────────┴────────────┴────────────┘                  │
│                          │ Axios                                 │
└──────────────────────────┼──────────────────────────────────────┘
                           │ HTTP/SSE
┌──────────────────────────┼──────────────────────────────────────┐
│                     Backend (FastAPI)                            │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    API Layer (/api)                      │    │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐           │    │
│  │  │  Chat  │ │ Reason │ │ Search │ │  Auth  │           │    │
│  │  └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘           │    │
│  └──────┼──────────┼──────────┼──────────┼─────────────────┘    │
│         └──────────┴──────────┴──────────┘                       │
│                          │                                       │
│  ┌───────────────────────┴───────────────────────────────────┐  │
│  │                   LangGraph Agent                          │  │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐      │  │
│  │  │ Router  │→│ Planner │→│Executor │→│Responder│      │  │
│  │  └─────────┘  └─────────┘  └─────────┘  └─────────┘      │  │
│  └───────────────────────────────────────────────────────────┘  │
│                          │                                       │
│  ┌───────────────────────┴───────────────────────────────────┐  │
│  │                   Service Layer                            │  │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │  │
│  │  │ LLM      │ │ Search   │ │ Index    │ │ Conver-  │     │  │
│  │  │ Factory  │ │ Service  │ │ Service  │ │ sation   │     │  │
│  │  └────┬─────┘ └──────────┘ └──────────┘ └──────────┘     │  │
│  │       │                                                    │  │
│  │  ┌────┴────────────────────────────────┐                  │  │
│  │  │  DeepSeek  │  Ollama  │  Embedding  │                  │  │
│  │  └─────────────────────────────────────┘                  │  │
│  └───────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
                           │
      ┌────────────────────┼────────────────────┐
      │                    │                    │
┌─────┴─────┐       ┌──────┴──────┐      ┌─────┴─────┐
│   MySQL   │       │    Neo4j    │      │   Redis   │
│  Sessions │       │ Knowledge   │      │ Semantic  │
│  Storage  │       │   Graph     │      │   Cache   │
└───────────┘       └─────────────┘      └───────────┘
```

---

## Tech Stack

### Backend

| Category | Technology | Version |
|----------|------------|---------|
| Web Framework | FastAPI | ≥0.68.0 |
| ASGI Server | Uvicorn | ≥0.15.0 |
| LLM Orchestration | LangGraph | 0.3.25 |
| LLM Integration | LangChain (DeepSeek/Ollama/Neo4j) | - |
| Knowledge Graph | GraphRAG | 2.1.0 |
| ORM | SQLAlchemy (async) | ≥2.0.0 |
| Relational DB | MySQL + aiomysql | - |
| Graph Database | Neo4j | - |
| Cache | Redis | ≥5.0.0 |
| Vector Search | FAISS + Sentence Transformers | - |
| Authentication | JWT (python-jose) + BCrypt | - |
| Logging | Loguru | 0.7.2 |

### Frontend

| Category | Technology | Version |
|----------|------------|---------|
| UI Framework | Vue 3 | ^3.3.11 |
| State Management | Pinia | ^2.3.1 |
| Routing | Vue Router | ^4.5.0 |
| Type Checking | TypeScript | ^5.2.2 |
| Build Tool | Vite | ^5.0.8 |
| HTTP Client | Axios | ^1.7.9 |
| Markdown Rendering | markdown-it | ^14.1.0 |

---

## Quick Start

### Prerequisites

- Python 3.10+
- Node.js 18+
- MySQL 8.0+
- Neo4j 5.0+ (optional)
- Redis 7.0+ (optional)
- Ollama (required for local models)

### 1. Clone the Repository

```bash
git clone https://github.com/your-repo/assistant-service-agent.git
cd assistant-service-agent
```

### 2. Backend Setup

```bash
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows
.venv\Scripts\activate
# Linux/Mac
source .venv/bin/activate

# Install dependencies
cd deepseek_agent
pip install -r requirements.txt
```

### 3. Environment Configuration

Create `deepseek_agent/llm_backend/.env` file:

```env
# ==================== LLM Service Config ====================
CHAT_SERVICE=DEEPSEEK          # Chat service: DEEPSEEK or OLLAMA
REASON_SERVICE=OLLAMA          # Reasoning service: DEEPSEEK or OLLAMA
AGENT_SERVICE=DEEPSEEK         # Agent service: DEEPSEEK or OLLAMA

# ==================== DeepSeek Config ====================
DEEPSEEK_API_KEY=your-deepseek-api-key
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
DEEPSEEK_MODEL=deepseek-chat

# ==================== Ollama Config ====================
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_CHAT_MODEL=qwen2.5:7b
OLLAMA_REASON_MODEL=deepseek-r1:7b
OLLAMA_EMBEDDING_MODEL=nomic-embed-text
OLLAMA_AGENT_MODEL=qwen2.5:7b

# ==================== MySQL Config ====================
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your-password
DB_NAME=assistgen

# ==================== Neo4j Config (Optional) ====================
NEO4J_URL=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your-password
NEO4J_DATABASE=neo4j

# ==================== Redis Config (Optional) ====================
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=
REDIS_CACHE_EXPIRE=3600

# ==================== Security Config ====================
SECRET_KEY=your-super-secret-key-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# ==================== Search Config (Optional) ====================
SERPAPI_KEY=your-serpapi-key
SEARCH_RESULT_COUNT=5
```

### 4. Start Backend Service

```bash
cd deepseek_agent/llm_backend
python run.py
```

Backend service will start at `http://localhost:8000`.

### 5. Frontend Setup

```bash
# Open a new terminal, navigate to frontend directory
cd DsAgentChat_web

# Install dependencies
npm install

# Start development server
npm run dev
```

Frontend development server will start at `http://localhost:3000`.

### 6. Access the System

- **Frontend UI**: http://localhost:3000
- **API Documentation**: http://localhost:8000/docs
- **ReDoc Documentation**: http://localhost:8000/redoc

---

## Project Structure

```
assistant-service-agent/
├── deepseek_agent/                 # Backend project
│   ├── llm_backend/
│   │   ├── app/
│   │   │   ├── api/                # API routing layer
│   │   │   │   └── auth.py         # Authentication endpoints
│   │   │   ├── core/               # Core configuration
│   │   │   │   ├── config.py       # Environment config
│   │   │   │   ├── database.py     # Database connection
│   │   │   │   ├── security.py     # Security & auth
│   │   │   │   └── logger.py       # Logging config
│   │   │   ├── models/             # Data models
│   │   │   │   ├── user.py         # User model
│   │   │   │   ├── conversation.py # Conversation model
│   │   │   │   └── message.py      # Message model
│   │   │   ├── services/           # Business service layer
│   │   │   │   ├── llm_factory.py  # LLM factory
│   │   │   │   ├── deepseek_service.py
│   │   │   │   ├── ollama_service.py
│   │   │   │   ├── search_service.py
│   │   │   │   └── conversation_service.py
│   │   │   ├── lg_agent/           # LangGraph Agent
│   │   │   │   ├── lg_builder.py   # Graph building core
│   │   │   │   ├── lg_states.py    # State definitions
│   │   │   │   └── lg_prompts.py   # Prompt templates
│   │   │   ├── graphrag/           # GraphRAG integration
│   │   │   └── tools/              # Tool definitions
│   │   ├── main.py                 # FastAPI app entry
│   │   ├── run.py                  # Startup script
│   │   └── requirements.txt        # Python dependencies
│   └── README.md
├── DsAgentChat_web/                # Frontend project
│   ├── src/
│   │   ├── views/                  # Page components
│   │   │   ├── Home.vue            # Main chat page
│   │   │   ├── Login.vue           # Login/register page
│   │   │   └── EcommerceService.vue # E-commerce service page
│   │   ├── components/             # Reusable components
│   │   ├── stores/                 # Pinia stores
│   │   ├── services/               # API services
│   │   ├── router/                 # Router config
│   │   └── types/                  # TypeScript types
│   ├── package.json
│   └── vite.config.ts
├── LICENSE
└── README.md
```

---

## API Documentation

### Authentication Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/register` | User registration |
| POST | `/api/token` | User login, get JWT |
| GET | `/api/users/me` | Get current user info |

### Conversation Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/chat` | General conversation (streaming) |
| POST | `/api/reason` | Deep thinking (streaming) |
| POST | `/api/search` | Web search (streaming) |
| POST | `/api/langgraph/query` | LangGraph Agent query |
| POST | `/api/langgraph/resume` | Resume interrupted conversation |

### Session Management

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/conversations` | Create session |
| GET | `/api/conversations/user/{user_id}` | Get all user sessions |
| GET | `/api/conversations/{id}/messages` | Get session messages |
| PUT | `/api/conversations/{id}` | Update session |
| DELETE | `/api/conversations/{id}` | Delete session |

### File Processing

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/upload` | Upload document (PDF/Word) |
| POST | `/api/upload/image` | Upload image |

---

## Neo4j E-commerce Knowledge Graph

### Data Model

```
(Product)─[:BELONGS_TO]─>(Category)
(Product)─[:SUPPLIED_BY]─>(Supplier)
(Order)─[:CONTAINS]─>(Product)
(Customer)─[:PLACED]─>(Order)
(Customer)─[:WROTE]─>(Review)
(Review)─[:ABOUT]─>(Product)
```

### Node Properties

| Node | Main Properties |
|------|-----------------|
| Product | ProductID, ProductName, UnitPrice, UnitsInStock |
| Category | CategoryID, CategoryName, Description |
| Supplier | SupplierID, CompanyName, ContactName |
| Customer | CustomerID, CompanyName, ContactName |
| Order | OrderID, OrderDate |
| Review | ReviewID, Rating, ReviewText, ReviewDate |

---

## Deployment Guide

### Production Configuration

1. **Backend Configuration**

```python
# run.py
uvicorn.run(
    "main:app",
    host="0.0.0.0",
    port=8000,
    reload=False,           # Disable hot reload
    log_level="info",       # Adjust log level
    access_log=True         # Enable access logs
)
```

2. **Environment Security**

```env
# Use strong secret key
SECRET_KEY=your-256-bit-secret-key

# Restrict CORS origins
CORS_ORIGINS=["https://your-domain.com"]

# Use HTTPS
SSL_KEYFILE=/path/to/key.pem
SSL_CERTFILE=/path/to/cert.pem
```

3. **Frontend Build**

```bash
cd DsAgentChat_web
npm run build
# Deploy dist/ directory to CDN or Nginx
```

### Docker Deployment (Recommended)

```yaml
# docker-compose.yml
version: '3.8'
services:
  backend:
    build: ./deepseek_agent
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=mysql
      - REDIS_HOST=redis
      - NEO4J_URL=bolt://neo4j:7687
    depends_on:
      - mysql
      - redis
      - neo4j

  frontend:
    build: ./DsAgentChat_web
    ports:
      - "80:80"
    depends_on:
      - backend

  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: your-password
      MYSQL_DATABASE: assistgen
    volumes:
      - mysql_data:/var/lib/mysql

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

  neo4j:
    image: neo4j:5
    environment:
      NEO4J_AUTH: neo4j/your-password
    volumes:
      - neo4j_data:/data

volumes:
  mysql_data:
  redis_data:
  neo4j_data:
```

---

## Development Guide

### Adding a New LLM Service

1. Create a new service class in `app/services/`
2. Implement the `generate_stream()` method
3. Register the service in `llm_factory.py`

### Adding a New Agent Tool

1. Define the tool function in `app/tools/`
2. Register with `@tool` decorator
3. Add to the tools list in `lg_builder.py`

### Running Tests

```bash
cd deepseek_agent/llm_backend
pytest app/test/ -v
```

---

## FAQ

<details>
<summary><b>Q: How do I switch between DeepSeek and Ollama?</b></summary>

Modify the `CHAT_SERVICE` and `REASON_SERVICE` variables in the `.env` file:
- `DEEPSEEK` - Use DeepSeek online API
- `OLLAMA` - Use local Ollama models

</details>

<details>
<summary><b>Q: How do I enable semantic caching?</b></summary>

1. Ensure Redis service is running
2. Configure Redis connection info in `.env`
3. Caching is automatically enabled; similar queries will return cached results

</details>

<details>
<summary><b>Q: Is Neo4j required?</b></summary>

Neo4j is optional and only needed when using GraphRAG knowledge graph or e-commerce features. Basic conversation functionality does not require Neo4j.

</details>

---

## Contributing

1. Fork this repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

- [FastAPI](https://fastapi.tiangolo.com/)
- [LangChain](https://www.langchain.com/)
- [LangGraph](https://langchain-ai.github.io/langgraph/)
- [DeepSeek](https://www.deepseek.com/)
- [Ollama](https://ollama.ai/)
- [Vue.js](https://vuejs.org/)

---

<div align="center">

**If this project helps you, please give it a ⭐ Star!**

</div>
