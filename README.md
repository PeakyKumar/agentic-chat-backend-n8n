# Agentic Stateful AI Chat Backend (n8n)

![status](https://img.shields.io/badge/status-prototype-green)
![n8n](https://img.shields.io/badge/built%20with-n8n-orange)
![license](https://img.shields.io/badge/license-MIT-lightgrey)

A webhook-based, stateful AI chat backend built with **n8n**.  
The system supports **multi-turn conversations**, **explicit session memory**, and an **API-first design** suitable for real-world automation workflows.

---

## Overview

This project implements a conversational AI backend using:
- HTTP webhooks as the primary interface
- Explicit session-based memory
- LLM orchestration via n8n AI nodes

The design is intentionally **channel-agnostic**, allowing the same backend to be reused across web apps, chat platforms, or internal tools.

---

## Architecture

High-level flow:

Client
➡️
Webhook (POST)
➡️
Input Normalization
➡️
Session Memory(load + append user message)
➡️
LLM Agent
➡️
Session Memory (append assistant reply)
➡️
HTTP Response


---

## Quick Start (Local)

### Prerequisites
- Docker
- n8n
- LLM provider API key (e.g. OpenAI)

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```
## Accessing the n8n Workflow

This project uses an **n8n workflow** as the core execution engine.  
The workflow is exported and stored in this repository for transparency and reproducibility.

### Workflow File

The main workflow is located at:

workflows/contentbuddy-chat.json


---

### Importing the Workflow into n8n

1. Open your n8n instance in the browser:
http://localhost:5678


2. In the n8n dashboard, click **Import** (top-right).

3. Select the file:
workflows/contentbuddy-chat.json


4. After import, open the workflow and review the nodes.

---

### Activating the Workflow

1. Ensure required environment variables (LLM API keys) are configured.
2. Click **Activate** in the top-right corner of the workflow.
3. Copy the generated **Webhook URL** from the Webhook node.

---

### Triggering the Workflow

The workflow is triggered via an HTTP **POST** request to the webhook URL.

Example request:

```json
{
"session_id": "user1",
"message": "Write a LinkedIn post about AI automation"
}
```
### Repository Structure

```text
.

├── workflows/
├── memory/
├── api/
├── deployment/
├── security/
└── README.md
```

⚠️ Current Limitations

Volatile Memory
Session history is stored in n8n’s workflowStaticData. This data is cleared if the workflow is deactivated, re-activated, or updated. It is not suitable for long-term production storage.

No Context Window Management
The JavaScript logic appends the full conversation history to each prompt. Long conversations may exceed the LLM provider’s token limit (context window), resulting in API errors.

Concurrency Issues
The staticData global object is not thread-safe. High-volume concurrent requests may lead to race conditions where session history is overwritten or lost.

### License
MIT
