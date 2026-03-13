# 🛡️ pentAGI Autonomous Security Assessor: A Comprehensive Case Study

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![AI Agent](https://img.shields.io/badge/Agent-pentAGI-e53e3e.svg)]()
[![LLM](https://img.shields.io/badge/LLM-Azure%20GPT--4.1--mini-success.svg)]()
[![Status](https://img.shields.io/badge/Status-Evaluation%20Complete-brightgreen.svg)]()

> **An empirical evaluation of the pentAGI autonomous penetration testing framework against modern web architectures (Node.js/Angular & Dockerized Python/Flask).**



## 📖 Overview

This repository contains the setup configurations, telemetry traces, cost analyses, and vulnerability reports from deploying **pentAGI** as an autonomous security researcher. The project aimed to evaluate the agent's ability to move beyond deterministic scanning and utilize LLM-driven logical reasoning to chain exploits.

**Targets Evaluated:**
1. **OWASP Juice Shop:** A deliberately vulnerable Node.js/SQLite application (Successful exploitation of Critical flaws).
2. **Knowledge Sharing & Slot Booking App:** A custom Python/Flask application (Highlighted critical white-box Docker environment limitations).

## 🚀 Key Findings & Inference Analysis

The agent utilized `azure/gpt-4.1-mini` with a `16,384` token context window, orchestrated via Langfuse observability.

### ✅ What the Agent Did Well
* **Complex Exploit Chaining:** Successfully navigated `/rest/products/search` to execute a SQLite Injection (`apple')) OR 1=1--`), identifying database structure dynamically based on 500 Internal Server Error stack traces.
* **JWT Forgery:** Discovered default admin credentials, analyzed the JWT structure, and forged a payload by exploiting a missing backend signature verification mechanism.
* **Tool Orchestration:** Successfully wrote syntax for, executed, and parsed outputs from integrated tools like `nmap` and `curl`.

### ❌ Where the Agent Failed (The Blind Spots)
* **Client-Side Blindness:** Missed critical DOM-based XSS vulnerabilities due to the lack of a headless browser integration (operating purely on HTTP requests).
* **The Environment Clash (Step 148):** During the Flask app audit, the agent failed to locate `.py` source files. It correctly identified that standard Docker container isolation prevented it from reading the target's source code without a specifically configured Bind Mount, safely halting the audit instead of hallucinating results.

## 📂 Repository Navigation

* 📁 **[`/setup`](./setup)** - Docker compose files and `.env.example` configurations to bypass Docker networking issues (`host.docker.internal`).
* 📁 **[`/traces`](./traces)** - Raw Langfuse telemetry JSONs mapping the LLM's step-by-step reasoning.
* 📁 **[`/findings`](./findings)** - Payload evidence, request/response captures, and CVSS scorings.
* 📁 **[`/cost-analysis`](./cost-analysis)** - Breakdown of Azure API inference costs and token utilization graphs.
* 📁 **[`/docs`](./docs)** - The final generated professional PDF assessment reports.
* 📁 **[`/improvements`](./improvements)** - Architectural blueprints for adding Playwright (headless browser) and OOB servers to pentAGI.

## ⚙️ Reproducing the Setup

To replicate this autonomous agent environment locally, ensure you have Docker and Docker Compose installed.

### 1. Clone & Configure
```bash
git clone [https://github.com/yourusername/pentagi-assessment.git](https://github.com/yourusername/pentagi-assessment.git)
cd pentagi-assessment
cp setup/.env.example .env
### 2. Configure the `.env` (Critical Step)
Autonomous agents require precise networking and API routing. Update your `.env` with your Azure/OpenAI credentials. **Note:** Ensure `MAX_CONTEXT_TOKENS` is balanced to prevent amnesia while avoiding `429 Too Many Requests` API limits.

```env
LLM_PROVIDER=azure
AZURE_OPENAI_API_KEY=your_key_here
AZURE_OPENAI_ENDPOINT=[https://your-resource.openai.azure.com/](https://your-resource.openai.azure.com/)
AZURE_OPENAI_API_VERSION=2023-05-15
TARGET_URL=[http://host.docker.internal:3000](http://host.docker.internal:3000)
```
### 3. Launch the Environment
The provided docker-compose.yml includes the necessary volume bind mounts to prevent the "Flask Environment Clash" experienced during the initial run.

```Bash
docker-compose -f setup/docker-compose.yml up -d
```
### 📈 Cost Analysis & Inference Performance
See the /cost-analysis directory for detailed graphs and visual data.

Deploying stateful agents is resource-intensive. By analyzing the Langfuse traces, we observed that maintaining the agent's semantic memory required feeding previous terminal outputs back into the context window.

Average Tokens per Execution Loop: ~8,500 tokens.

Total Audit Time (Juice Shop): ~45 minutes.

Efficiency vs. Cost: The agent autonomously parsed 500 Internal Server Error stack traces in milliseconds. While the token cost for a 45-minute autonomous run is higher than a standard DAST scan, it replaces significant manual human overhead in exploit verification.

## 🤝 Contributing & Future Improvements
We are actively exploring ways to enhance the pentAGI framework based on this assessment. Current focus areas in the /improvements directory include:

Multi-Session State Management: Enabling the agent to hold "Attacker" and "Victim" tokens simultaneously to test for IDOR.

Headless Browser Injection: Integrating Puppeteer or Playwright to catch XSS and DOM mutations.

Disclaimer: This repository is for educational and authorized security research purposes only. Do not use pentAGI against targets you do not explicitly own or have written permission to audit.
