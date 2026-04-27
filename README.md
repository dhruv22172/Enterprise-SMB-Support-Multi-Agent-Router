# Enterprise SMB Support Multi-Agent Router

An autonomous, deterministic multi-agent workflow system designed to handle Tier-1 customer support for SMB web hosting platforms and digital service providers. 

Built using LangGraph, NVIDIA NeMo LLMs, and Langfuse Tracing, this system autonomously classifies inbound customer requests and routes them to specialized AI agents for immediate, highly-contextual resolution.

---

## System Architecture

The core of this system is a stateful directed acyclic graph (DAG) managed by LangGraph. It utilizes a centralized state (`SMBSupportState`) to pass business context, live website metrics, and support history across agent boundaries without data loss.

### 1. The Intelligent Router (`SMB_Support_Router`)
Acts as the entry node. It performs zero-shot intent classification on incoming support tickets, outputting a strict JSON schema containing a `routing_decision`, `confidence_score`, and `urgency_level`. It dynamically routes the graph execution to one of three specialized nodes.

### 2. The Specialist Agents (ReACT Framework)
Each agent is configured with domain-specific system prompts and temperature settings to optimize for either deterministic troubleshooting or creative generation:

* **SEOStrategyAgent:** Analyzes Core Web Vitals, domain authority, and local search intent to generate 30-day actionable SEO roadmaps for SMBs. 
* **ContentGeneratorAgent:** Acts as a localized copywriter, generating conversion-focused, SEO-aware web content (Homepages, Service Pages, Blogs) tailored to the SMB's industry and target audience. (High Temperature)
* **TechSupportAgent:** A deterministic, low-temperature responder acting as a Tier-2 hosting engineer. It diagnoses server errors, plugin conflicts, and SSL certificate failures, outputting strict step-by-step resolution runbooks. (Low Temperature)

---

## Enterprise Observability (Langfuse)

To meet modern AI engineering standards, this system integrates Langfuse at the client layer for full enterprise observability. 

Every AsyncOpenAI call is wrapped and traced, allowing administrators to monitor:
* **P95 Latency & Time-to-First-Token** across the entire agentic loop.
* **Token Usage & Cost tracking** per routing category.
* **Prompt Versioning & Execution Traces** to debug logic errors and optimize ReACT workflows.

---

## Tech Stack

* **Orchestration:** LangGraph, LangChain Core
* **LLM Backend:** NVIDIA NeMo (`llama-3.1-nemotron-70b-instruct`) via AsyncOpenAI
* **Observability:** Langfuse
* **Environment:** Python 3.10+, Jupyter Notebook / Google Colab

---

## How to Run

This prototype is packaged as a Jupyter Notebook for seamless execution and demonstration.

1. **Open the Notebook:** Open the `.ipynb` file in your preferred environment (e.g., local Jupyter server or Google Colab).
2. **Configure API Keys:** Ensure the following environment variables or secrets are configured:
   * `NEMOTRON_4_340B_INSTRUCT_KEY` (NVIDIA API Key)
   * `LANGFUSE_PUBLIC_KEY`
   * `LANGFUSE_SECRET_KEY`
3. **Execute:** Run the notebook cells sequentially. 
4. **View Output:** The notebook will automatically compile the graph, render a Mermaid.js architecture diagram inline, and execute real-world SMB test cases, generating a batch execution summary table.
