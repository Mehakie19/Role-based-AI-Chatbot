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

## 🔧 Configuration

### Adding New Users
Edit [app/main.py](app/main.py) (lines ~30):
```python
users_db: Dict[str, Dict[str, str]] = {
    "NewUser": {"password": "newpassword", "role": "engineering"},
    # ... other users
}
```

### Adding New Documents
1. Create a folder in `resources/data/` with the department name (e.g., `legal/`)
2. Add Markdown (.md) or CSV files to the folder
3. Run `embed_documents.py` to regenerate embeddings

### Modifying Embedding Model
In [app/embed_documents.py](app/embed_documents.py) and [app/main.py](app/main.py):
```python
embedding_function = SentenceTransformerEmbeddings(
    model_name="all-MiniLM-L6-v2"  # Change this to another model
)
```
Available models: `all-mpnet-base-v2`, `all-roberta-large-v1`, etc.

### Adjusting LLM Temperature
In [app/main.py](app/main.py) (line ~110):
```python
ollama_payload = {
    "temperature": 0.7  # Lower = more deterministic, Higher = more creative
}
```

---

## 🧪 Testing

### Test Backend API Directly
```bash
# Login
curl -X GET http://127.0.0.1:8000/login \
  --user "Alice:ceopass"

# Chat
curl -X POST http://127.0.0.1:8000/chat \
  -H "Content-Type: application/json" \
  -d '{
    "user": {"username": "Alice", "role": "c-levelexecutives"},
    "message": "What are our quarterly financial results?"
  }'
```

### Test Web Interface
1. Login with different user roles
2. Verify access restrictions (e.g., Bob only sees general docs)
3. Test semantic search accuracy
4. Check response quality and timeout handling

---

## 🐛 Troubleshooting

### ❌ Error: "Connection refused" on port 8000
- Ensure FastAPI backend is running: `python -m uvicorn main:app --reload`
- Check if port 8000 is already in use

### ❌ Error: "Ollama LLM error: 404" or "Connection refused"
- Ensure Ollama is running: `ollama serve` in separate terminal
- Verify LLaMA3 is installed: `ollama list`
- Check Ollama is accessible at `http://localhost:11434`

### ❌ Error: "No module named 'langchain_community'"
- Reinstall dependencies: `pip install -r requirements.txt`
- Ensure Python version is 3.10+: `python --version`

### ❌ Vector database is empty or outdated
- Regenerate embeddings: `python embed_documents.py`
- Delete `chroma_db/` directory and regenerate

### ❌ Slow response times
- Reduce chunk size in `embed_documents.py` (default: 500)
- Reduce `k` (number of retrieved docs) in `main.py` (default: 3-5)
- Ensure Ollama has sufficient hardware resources

### 🔍 Performance Tips
- Run Ollama and FastAPI on the same machine
- Use SSD storage for vector database
- Monitor system RAM usage (at least 8GB available)
- Limit concurrent connections to prevent bottlenecks

---

## 🚀 Advanced Usage

### Batch Embedding Documents
```python
# Modify embed_documents.py to process documents from a different source
loader = UnstructuredFileLoader("path/to/document.pdf")
docs = loader.load()
```

### Custom Role-Based Filtering
Modify the chat endpoint in [app/main.py](app/main.py) to implement custom access logic:
```python
if user_role == "custom_role":
    docs = vectordb.similarity_search(message, k=5, filter={"category": "specific"})
```

### Integrate with External LLM
Replace Ollama with OpenAI, Azure OpenAI, or other providers:
```python
from langchain_openai import OpenAI
llm = OpenAI(api_key="your-key")
```

---

## 📈 Future Enhancements

- [ ] User management dashboard
- [ ] Document upload interface
- [ ] Multi-turn conversation memory
- [ ] Query logging and analytics
- [ ] Support for PDF and image documents
- [ ] Conversation export functionality
- [ ] Integration with external LLM APIs (OpenAI, Azure)
- [ ] Docker containerization
- [ ] Database-backed user management (replacing in-memory dict)
- [ ] Refined role hierarchy and granular permissions
- [ ] Response citation with document sources
- [ ] Feedback-based model fine-tuning

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Create Pull Request

---

## 📝 License

This project is proprietary to **FinSolve Technologies**. Unauthorized copying or distribution is prohibited.

---

## 📞 Support & Contact

For issues, questions, or suggestions, please contact the development team or open an issue in the repository.

---

## 🎓 Learning Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [LangChain Documentation](https://langchain.readthedocs.io/)
- [Chroma Vector Database](https://docs.trychroma.com/)
- [Ollama Guide](https://github.com/ollama/ollama)
- [Sentence Transformers](https://www.sbert.net/)

---

**Last Updated**: February 27, 2026  
**Version**: 0.1.0  
**Status**: Active Development