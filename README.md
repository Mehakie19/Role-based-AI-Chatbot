# Role-Based AI Chatbot

A sophisticated retrieval-augmented generation (RAG) chatbot with role-based access control designed for **FinSolve Technologies**. This application allows employees to query company documents while respecting departmental access boundaries through intelligent semantic search and LLaMA3 powered natural language responses.

---

## 🎯 Project Overview

This chatbot enables employees across different departments to ask questions about company information while maintaining strict role-based access control. The system leverages vector embeddings to semantically search company documents and uses a local LLaMA3 model via Ollama to generate context-aware, conversational responses.

**Key Use Cases:**
- Employees quickly finding department-specific information
- HR staff accessing employee handbook and policies
- Finance team reviewing financial reports
- Engineering team consulting technical documentation
- Marketing team accessing market research and campaign data
- C-level executives viewing company-wide information

---

## ✨ Features

### 🔐 **Role-Based Access Control**
- 8 predefined user roles with different access levels:
  - **C-Level Executives**: Full access to all company documents
  - **Engineering**: Access to technical documentation
  - **Finance**: Access to financial reports and data
  - **Marketing**: Access to marketing reports and strategies
  - **HR**: Access to employee handbook and HR policies
  - **General**: Common company information accessible to all
  - **Employee**: Limited access to general information only
  
### 🧠 **Semantic Document Search**
- Documents embedded using `SentenceTransformer` (all-MiniLM-L6-v2 model)
- Vector database (Chroma) stores embeddings with role-based metadata
- Intelligent similarity search returns most relevant documents for each query

### 💬 **Natural Language Responses**
- LLaMA3 model integration via Ollama
- Context-aware prompt generation based on user role
- Conversational, human-friendly responses
- Temperature control for response variability

### 🎨 **User-Friendly Interface**
- Streamlit web application for intuitive interaction
- Real-time chat history with helpful/unhelpful feedback
- Session-based authentication with login panel
- Visual role explanation and access level indicators

### 📚 **Multi-Department Document Support**
- Automatic document loading from department-organized folders
- Support for Markdown (.md) and CSV files
- Intelligent text chunking with overlap for context preservation
- Automatic metadata tagging by department

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Streamlit Frontend                        │
│              (Web UI, Login, Chat Interface)                 │
└──────────────────────────┬──────────────────────────────────┘
                           │
                    HTTP Requests
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                    FastAPI Backend                           │
│              (Auth, Chat Logic, RAG Pipeline)                │
└──────────────────────────┬──────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   ┌─────────┐      ┌─────────┐      ┌──────────────┐
   │  Chroma │      │ Sentence│      │   Ollama     │
   │   VDB   │      │Transform│      │   LLaMA3     │
   │         │      │          │      │              │
   │Embeddings│      │ Embeddings  │  │ LLM Engine   │
   └─────────┘      └─────────┘      └──────────────┘
```

---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Backend** | FastAPI, Uvicorn | REST API, async request handling |
| **Frontend** | Streamlit | Interactive web interface |
| **Vector DB** | Chroma | Semantic search and embeddings storage |
| **Embeddings** | Sentence-Transformers | Generate vector representations of text |
| **LLM** | Ollama + LLaMA3 | Local language model for response generation |
| **Document Processing** | LangChain, Unstructured | Load and chunk documents |
| **Auth** | HTTP Basic Auth | Simple username/password authentication |
| **Python Version** | 3.10+ | Modern Python features |

---

## 📋 Prerequisites

### System Requirements
- **Python**: 3.10 or higher
- **RAM**: 8GB+ (for running Ollama + FastAPI + Streamlit simultaneously)
- **Disk Space**: 5GB+ (for LLaMA3 model and vector database)
- **OS**: Windows, macOS, or Linux

### External Services
- **Ollama**: Download and install from [ollama.ai](https://ollama.ai)
- **LLaMA3 Model**: `ollama pull llama3` (after Ollama installation)

---

## 📦 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/Mehakie19/Role-based-AI-Chatbot.git
cd role_based_aichatbot
```

### 2. Set Up Python Environment
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Install & Configure Ollama
```bash
# Download from ollama.ai and install
# Then pull the LLaMA3 model:
ollama pull llama3

# Start Ollama service (required for chatbot to work)
ollama serve
```
> 💡 **Tip**: Keep the Ollama service running in a separate terminal window while using the chatbot.

---

## 🚀 Getting Started

### Step 1: Generate Vector Embeddings
Before starting the chatbot, you need to embed the company documents:

```bash
# From the app directory
cd app
python embed_documents.py
```

**Output**: Creates/updates `chroma_db` directory with vector embeddings.

### Step 2: Start FastAPI Backend
```bash
# From the app directory
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

**Expected Output**:
```
INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete
```

### Step 3: Start Streamlit Frontend
```bash
# From the app directory (in another terminal)
streamlit run frontend.py
```

**Expected Output**:
```
  You can now view your Streamlit app in your browser.
  URL: http://localhost:8501
```

### Step 4: Access the Application
Open your browser and navigate to: `http://localhost:8501`

---

## 👥 Demo Users

The chatbot comes with 8 pre-configured demo users for testing:

| Username | Password | Role | Access Level |
|----------|----------|------|--------------|
| `Tony` | `password123` | Engineering | Engineering docs only |
| `Bruce` | `securepass` | Marketing | Marketing docs only |
| `Sam` | `financepass` | Finance | Finance docs only |
| `Natasha` | `hrpass123` | HR | HR docs only |
| `Peter` | `pete123` | Engineering | Engineering docs only |
| `Sid` | `sidpass123` | Marketing | Marketing docs only |
| `Alice` | `ceopass` | C-Level Executive | All documents |
| `Bob` | `employeepass` | Employee | General docs only |

### Testing Access Control
1. Login as `Bob` (Employee role) → Can only see general information
2. Login as `Sam` (Finance role) → Can only see financial documents
3. Login as `Alice` (C-Level) → Can see all documents

---

## 📁 Project Structure

```
role_based_aichatbot/
├── README.md                          # This file
├── pyproject.toml                     # Project metadata
├── requirements.txt                   # Python dependencies
│
├── app/
│   ├── main.py                        # FastAPI backend (auth, chat endpoints)
│   ├── frontend.py                    # Streamlit web UI
│   ├── embed_documents.py             # Document embedding pipeline
│   ├── __pycache__/                   # Python cache (auto-generated)
│   ├── chroma_db/                     # Vector database (generated at runtime)
│   └── chroma_store/                  # Additional vector storage
│
└── resources/
    └── data/
        ├── general/                   # General company information
        │   └── employee_handbook.md
        ├── engineering/               # Engineering department docs
        │   └── engineering_master_doc.md
        ├── finance/                   # Finance department docs
        │   ├── financial_summary.md
        │   └── quarterly_financial_report.md
        ├── hr/                        # HR department docs
        │   └── hr_data.csv
        └── marketing/                 # Marketing department docs
            ├── marketing_report_2024.md
            ├── marketing_report_q1_2024.md
            ├── marketing_report_q2_2024.md
            ├── marketing_report_q3_2024.md
            └── market_report_q4_2024.md
```

---

