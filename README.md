# 🤖 AgentNet: Self-Correcting Enterprise Support Agents

### The Problem
Modern customer support teams face a "Volume vs. Quality" dilemma. Human agents cannot manually triage thousands of tickets efficiently, but standard chatbots often "hallucinate" incorrect answers or provide generic, unhelpful advice. Auditing these automated interactions at scale is impossible for human supervisors.

### Why Agents? (The Solution)
Standard automation scripts cannot reason or critique themselves. **AgentNet** solves this by using a **Multi-Agent System (MAS)** to mimic a human support team structure. By splitting responsibilities—Triage, Research, Response, and Quality Assurance—across specialized agents, the system achieves a level of reliability that a single LLM prompt cannot match.

### System Architecture
The system utilizes a **Sequential Orchestrator Pattern**:
1.  **🚦 Triage Agent:** Analyzes raw user text to output structured JSON metadata (Category & Priority).
2.  **🧠 Knowledge Tool (RAG):** A custom Vector Memory Bank searches a real-world Kaggle dataset (embedded via `SentenceTransformers`) to retrieve relevant policies.
3.  **💬 Support Agent:** Synthesizes the User Query + Triage Metadata + Retrieved Knowledge to draft a professional response.
4.  **⚖️ QA Auditor (The Innovation):** An "LLM-as-a-Judge" agent that reads the interaction, assigns a **Quality Score (1-5)**, and writes a critique. If the score is low, it flags the interaction.

### Project Journey & Technical Challenges
Building an enterprise-grade system in a notebook environment presented unique challenges:
*   **The "Nuclear" Logs:** The underlying C++ libraries (TensorFlow/gRPC) generated excessive noise. I engineered a custom **"Nuclear Silencer" Context Manager** that performs OS-level file descriptor redirection (`os.dup2`) to route these logs to `/dev/null`, ensuring clean observability.
*   **Fault Tolerance:** To ensure reliability, I implemented a **Strategy Pattern** that attempts to connect via Google Vertex AI (Cloud) and automatically falls back to the Gemini API Key if cloud authentication fails.

### Key Results
Tested against the **Customer Support Ticket Dataset**:
*   **Success Case:** The system correctly identified "Error 503" as a High-Priority Technical issue, retrieved the maintenance policy, and earned a **QA Score of 4/5**.
*   **Self-Correction:** When asked for a refund without sufficient context, the Support Agent gave a generic reply. The QA Auditor correctly identified this as unhelpful and awarded a **Score of 2/5**, proving the system's ability to quality-check itself.

### Technologies Used
*   **Model:** Google Gemini 2.5 Flash Lite / Pro
*   **Frameworks:** Google Vertex AI, Google Generative AI SDK
*   **Vector Search:** SentenceTransformers, Scikit-Learn
*   **Observability:** Custom JSON TraceLogger
