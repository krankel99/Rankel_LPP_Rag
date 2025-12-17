# AI Bubble RAG Assistant

## Project Description

The **AI Bubble RAG Assistant** is a Retrieval-Augmented Generation (RAG) application that allows users to ask questions about the so-called *AI Bubble* and receive grounded, source-backed answers. The system combines a vector database of curated documents with an LLM-powered agent that intelligently decides when to retrieve external context before generating a response.

This repository is structured and documented as if presented to a potential employer, emphasizing clarity, modular design, and real-world applicability of modern AI systems.

---

## Domain Overview & Problem Statement

### Domain: The AI Bubble

The rapid rise of artificial intelligence has led to intense speculation, investment, and debate around whether the industry is experiencing a speculative bubble similar to the dot-com era. Information on this topic is fragmented across videos, articles, and research papers, making it difficult to form well-grounded opinions.

### Problem Statement

General-purpose LLMs may hallucinate or provide shallow answers when asked nuanced questions about the AI Bubble. This project addresses that problem by:

* Grounding responses in curated, domain-specific documents
* Allowing transparent inspection of retrieved sources
* Giving users control over retrieval depth and model behavior

---

## System Architecture & Pipeline

### High-Level Pipeline

1. **User Input**: The user submits a question via a Streamlit chat interface.
2. **Agent Reasoning**: A CrewAI agent evaluates whether external knowledge is required.
3. **Retrieval (RAG)**:

   * The agent calls a custom tool to query a DuckDB vector database.
   * Query text is embedded using a SentenceTransformer model.
   * Top-k semantically similar passages are retrieved.
4. **Generation**:

   * Retrieved passages are injected into the agent context.
   * The LLM generates a grounded response.
5. **UI Display**:

   * The answer is shown to the user.
   * Source passages and similarity scores are expandable and transparent.

### Key Components

* **Frontend**: Streamlit (`app.py`)
* **Agent Layer**: CrewAI (`backend/agent.py`)
* **Database Layer**: DuckDB + vector search (`backend/database.py`)
* **Configuration**: Centralized via `config.py`

---

## Document Collection Summary

The vector database contains curated documents related to the AI Bubble, including:

* **Video transcripts** discussing AI market dynamics
* **News and opinion articles** analyzing AI valuations and risks
* **Research papers** providing historical and economic context

These documents were selected to ensure:

* Diverse perspectives (bullish, bearish, analytical)
* Factual grounding for agent responses
* Coverage of both historical analogies and modern AI-specific factors

---

## Agent Configuration & Rationale

The RAG agent is implemented using CrewAI and configured as follows:

### Agent Role

**AI Bubble Content Assistant**

### Goal

Provide accurate, grounded answers to questions about the AI Bubble using retrieved database content when necessary.

### Backstory

The agent is framed as an expert with exclusive access to a specialized database on the AI Bubble, encouraging tool usage rather than speculative reasoning.

### Design Rationale

* **Tool-based retrieval** ensures the LLM only queries the database when useful
* **Max iteration limits** prevent excessive or runaway tool calls
* **Verbose execution** supports debugging and transparency

---

## Installation & Setup Instructions

### Prerequisites

* Python 3.10+
* OpenAI API key

### Clone the Repository

```bash
git clone https://github.com/your-username/ai-bubble-rag.git
cd ai-bubble-rag
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Configure the Project

Update `config.py` if needed:

* `DEFAULT_DB_PATH` → Path to your DuckDB vector database
* `EMBEDDING_MODEL_NAME` and `EMBEDDING_DIMENSION` → Must match your stored embeddings

### Run the Application

```bash
streamlit run app.py
```

Optional testing utilities:

* `streamlit run db_test.py` → Test database connection
* `streamlit run agent_test.py` → Test agent + database integration

---

## Streamlit Deployment

🔗 **Live App**: *(https://rankellpprag-2vsjckb7sfmdpekhy2edec.streamlit.app/)*

The deployed app allows users to:

* Enter their own OpenAI API key securely
* Ask free-form questions about the AI Bubble
* Inspect retrieved sources and similarity scores
