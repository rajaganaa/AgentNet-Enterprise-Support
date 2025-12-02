# AgentNet: Self-Correcting Enterprise Support System

![Multi-Agent System](https://img.shields.io/badge/Architecture-Multi--Agent%20System-blue)
![RAG](https://img.shields.io/badge/Technology-RAG-green)
![Kaggle Capstone](https://img.shields.io/badge/Project-Kaggle%20Capstone-orange)

## 🚀 Overview

**AgentNet** is an **Autonomous Multi-Agent Orchestrator** that uses **"LLM-as-a-Judge"** to evaluate and self-correct its own customer support responses. This enterprise-grade system ensures high-quality, accurate responses through intelligent self-reflection and multi-layered verification.

## ✨ Key Features

- **Self-Reflection**: QA Auditor Agent scores responses (1-5) before delivery, ensuring quality control at every step.

- **Resiliency**: Implements a Strategy Pattern for Auth (Vertex AI fallback), providing robust authentication handling.

- **Observability**: Custom TraceLogger for tracking agent latency and decision paths, enabling deep system insights.

- **Nuclear Silencer**: Custom Context Manager to suppress C++ system logs, maintaining clean execution environments.

## 📖 Usage

This project follows a **"Notebook-First Architecture"** where running the notebook generates the `enterprise_agents` package dynamically. This approach allows for rapid prototyping while maintaining production-ready code generation.

To get started:

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Run the orchestrator notebook:
   ```bash
   jupyter notebook notebooks/agentnet_orchestrator.ipynb
   ```

3. The notebook will dynamically generate and execute the multi-agent system.

## 🏗️ Architecture

The system leverages multiple specialized agents that work in concert:
- **Support Agent**: Primary response generator
- **QA Auditor Agent**: Response evaluator and quality scorer
- **Orchestrator**: Coordinates agent interactions and manages decision flows

## 📝 License

This is a Kaggle Capstone Project for educational and portfolio purposes.
