# AgentNet: Enterprise Support System with Multi-Agent Orchestration

![Multi-Agent System](https://img.shields.io/badge/Architecture-Multi--Agent%20Orchestration-blue)
![RAG](https://img.shields.io/badge/Technology-RAG%20%2B%20Vector%20Search-green)
![LLM-as-a-Judge](https://img.shields.io/badge/Innovation-LLM--as--a--Judge-orange)
![Python](https://img.shields.io/badge/Language-Python-yellow)

## 🚀 Overview

**AgentNet** is an autonomous, self-correcting enterprise customer support system built on a **Multi-Agent Orchestration** architecture. The system leverages **LLM-as-a-Judge** methodology to evaluate and critique its own responses before delivery, ensuring enterprise-grade quality control through intelligent self-reflection and multi-layered verification.

Unlike traditional single-prompt chatbots that are prone to hallucinations and generic responses, AgentNet mimics a real human support team structure by distributing specialized responsibilities across multiple intelligent agents that work in concert to deliver accurate, contextual, and high-quality customer support.

---

## ✨ Key Features

### 🔍 **Self-Reflection & Quality Assurance**
- **QA Auditor Agent** scores every response (1-5) using LLM-as-a-Judge methodology
- Automatic quality gates prevent low-quality responses from reaching customers
- Self-correction loop ensures continuous improvement

### 🧠 **Retrieval Augmented Generation (RAG)**
- Vector-based knowledge retrieval using `SentenceTransers` embeddings
- Cosine similarity search against real customer support ticket datasets
- Prevents hallucinations by grounding responses in verified knowledge

### 🔄 **Fault-Tolerant Architecture**
- **Strategy Pattern** for authentication with automatic fallback
- Primary: Gemini API Key (SaaS)
- Fallback: Vertex AI Cloud authentication
- Ensures system resilience against service disruptions

### 📊 **Enterprise Observability**
- Custom `TraceLogger` for tracking agent interactions and decision paths
- Structured JSON logging for integration with monitoring tools (Datadog, Splunk)
- Latency metrics and performance tracking

### 🔇 **Nuclear Silencer**
- Custom context manager for OS-level log suppression
- Clean execution environment free from C++/TensorFlow/gRPC noise
- Production-ready logging output

---

## 🏗️ Architecture

AgentNet implements a **Sequential Orchestrator Pattern** with four specialized components:

```
┌─────────────────────────────────────────────────────────────┐
│                    User Query Input                          │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  🚦 TRIAGE AGENT                                            │
│  • Analyzes user query                                      │
│  • Extracts structured metadata (Category, Priority)        │
│  • Output: JSON classification                              │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  🧠 KNOWLEDGE TOOL (RAG Engine)                             │
│  • Embeds query using SentenceTransformers                  │
│  • Vector search via Cosine Similarity                      │
│  • Retrieves most relevant support policies/tickets         │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  💬 SUPPORT AGENT                                           │
│  • Receives: Query + Triage Metadata + Retrieved Knowledge  │
│  • Synthesizes professional response                        │
│  • Applies company policies and context                     │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  ⚖️ QA AUDITOR AGENT (LLM-as-a-Judge)                       │
│  • Reviews entire interaction thread                        │
│  • Assigns Quality Score (1-5)                              │
│  • Writes critique and improvement suggestions              │
│  • Flags low-quality responses (Score < 3)                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
                  ✅ Response
                  Delivered to User
```

### Agent Responsibilities

| Agent | Role | Input | Output |
|-------|------|-------|--------|
| **Triage Agent** | Classifier | Raw user query | JSON metadata (category, priority) |
| **Knowledge Tool** | RAG Retrieval | Query + Embeddings | Relevant support documents |
| **Support Agent** | Response Generator | Query + Context + Knowledge | Draft support response |
| **QA Auditor** | Quality Judge | Full conversation | Quality score (1-5) + Critique |

---

## 💻 Tech Stack

### **Core Technologies**
- **Python 3.x** - Primary programming language
- **Jupyter Notebook** - Development environment with notebook-first architecture
- **Google Gemini 2.5 Flash** - High-speed LLM for agent reasoning

### **AI/ML Frameworks**
- **Google Vertex AI** - Cloud-based LLM infrastructure (with fallback support)
- **Google Generative AI SDK** - Direct API integration for Gemini models
- **SentenceTransformers** - Local embedding model (`all-MiniLM-L6-v2`)
- **scikit-learn** - Cosine similarity computation for vector search

### **Infrastructure**
- **Custom TraceLogger** - Structured observability and telemetry
- **MCPRegistry** - Decorator-based tool registration pattern
- **MemoryBank** - In-memory vector database for RAG

---

## 📦 Installation

### Prerequisites
- Python 3.8+
- Jupyter Notebook or JupyterLab
- Google Cloud Project (for Vertex AI) or Google API Key

### Setup

1. **Clone the repository**:
   ```bash
   git clone git@github.com:rajaganaa/AgentNet-Enterprise-Support.git
   cd AgentNet-Enterprise-Support
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure authentication** (Choose one):
   
   **Option A: Using Google API Key** (Recommended for quick start)
   ```bash
   export GOOGLE_API_KEY="your-api-key-here"
   ```
   
   **Option B: Using Vertex AI** (Enterprise)
   - Set up Google Cloud Project
   - Update `PROJECT_ID` in the notebook configuration cell
   - Authenticate via `gcloud auth application-default login`

---

## 🎯 Usage

### Running the Multi-Agent System

The project follows a **"Notebook-First Architecture"** where the Jupyter notebook dynamically generates the `enterprise_agents` Python package at runtime.

1. **Launch Jupyter**:
   ```bash
   jupyter notebook notebooks/agentnet_orchestrator.ipynb
   ```

2. **Execute All Cells**:
   - The notebook will automatically:
     - Install dependencies silently
     - Generate the `enterprise_agents` package structure
     - Initialize the LLM connection (API Key → Vertex AI fallback)
     - Load the vector embedding model
     - Register agent tools via MCP decorator pattern
     - Execute sample support queries

3. **Review Results**:
   - Observe the sequential agent interactions
   - Check QA scores and critiques
   - Review structured JSON logs for observability

### Example Workflow

```python
# Input: User submits a support ticket
user_query = "I'm getting Error 503 when trying to access the dashboard"

# Agent Flow:
# 1. Triage Agent → {"category": "Technical", "priority": "High"}
# 2. Knowledge Tool → Retrieves maintenance policy docs
# 3. Support Agent → Drafts response with troubleshooting steps
# 4. QA Auditor → Score: 4/5, Critique: "Accurate and helpful"

# Output: High-quality, contextual support response
```

---

## 📊 Project Structure

```
AgentNet-Enterprise-Support/
│
├── notebooks/
│   └── agentnet_orchestrator.ipynb   # Main orchestration notebook
│
├── requirements.txt                   # Python dependencies
└── README.md                          # This file

# Generated at Runtime (Not in Git):
enterprise_agents/                     # Auto-generated package
├── config.py                          # LLM and project configuration
├── core/
│   ├── llm.py                        # Fault-tolerant LLM abstraction
│   └── observability.py              # TraceLogger for telemetry
├── services/
│   ├── memory.py                     # RAG vector search engine
│   └── tools.py                      # MCP tool registry
└── impl/
    ├── agents.py                     # BaseAgent, TriageAgent, EvaluatorAgent
    └── orchestrator.py               # Sequential workflow coordinator
```

---

## 🧪 Technical Highlights

### 1. **LLM-as-a-Judge Implementation**
The QA Auditor Agent uses a specialized prompt to evaluate responses across multiple dimensions:
- **Accuracy**: Is the information correct?
- **Completeness**: Are all aspects addressed?
- **Professionalism**: Is the tone appropriate?
- **Actionability**: Can the user resolve their issue?

### 2. **Vector Retrieval (RAG)**
Prevents hallucinations by:
1. Embedding all support tickets using `SentenceTransformers`
2. Converting user queries to vectors in the same embedding space
3. Retrieving top-k most similar tickets via cosine similarity
4. Injecting retrieved context into the Support Agent's prompt

### 3. **Fault-Tolerant Authentication**
```python
# Strategy Pattern: Try API Key first, fallback to Vertex AI
if api_key_available:
    model = genai.GenerativeModel()  # Fast SaaS connection
else:
    vertexai.init(project_id)        # Cloud fallback
    model = GenerativeModel()
```

### 4. **Nuclear Silencer Context Manager**
Suppresses C++/TensorFlow logs at the OS level using file descriptor redirection:
```python
os.environ["TF_CPP_MIN_LOG_LEVEL"] = "3"
os.environ["GRPC_VERBOSITY"] = "NONE"
```

---

## 🎓 Learning Outcomes

This project demonstrates:
- ✅ **Multi-Agent Systems (MAS)** design patterns
- ✅ **Retrieval Augmented Generation (RAG)** implementation
- ✅ **LLM-as-a-Judge** for automated quality assurance
- ✅ **Fault-tolerant architecture** with fallback strategies
- ✅ **Enterprise observability** with structured logging
- ✅ **Notebook-to-Production** code generation patterns

---

## 📝 License

This project is developed as a **Kaggle Capstone Project** for educational and portfolio demonstration purposes.

---

## 👤 Author

**Rajaganapathy M**  
GitHub: [@rajaganaa](https://github.com/rajaganaa)  
Email: rajaganaa@gmail.com

---

## 🙏 Acknowledgments

- **Dataset**: Customer Support Ticket Dataset (Kaggle)
- **Models**: Google Gemini 2.5 Flash, SentenceTransformers
- **Inspiration**: Enterprise-grade customer support automation challenges

---

**Built with ❤️ for Enterprise AI and Multi-Agent Systems**
