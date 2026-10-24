# SecurePR AI - Threat, Risk & Vulnerability Review Agent

> Evidence-driven AI security review toolkit and conversational agent for GitHub pull requests with empirical precision/recall benchmarking and zero-leakage redaction.

[![Live Interactive App](https://img.shields.io/badge/Live%20Demo-SecurePR%20AI%20Console-4f46e5?style=for-the-badge&logo=google-chrome&logoColor=white)](https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app)
[![Empirical Precision](https://img.shields.io/badge/Precision-100%25-059669?style=for-the-badge)](https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app)
[![Zero Leakage](https://img.shields.io/badge/Secret%20Leakage-0.00%25-0284c7?style=for-the-badge)](https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app)

**Live Web Console & Benchmark Preview**: [https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app](https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app)

---

## Overview

### Problem Statement & Intended Users

- **Problem**: Most AI-powered code reviewers act as generic LLM wrappers that hallucinate false positives, lack code context, flag benign patterns (such as standard authorization headers), and inadvertently leak sensitive API keys or credentials into model prompts. Furthermore, conventional static analysis tools emit noisy, unprioritized lists of warnings without verifying whether proposed fixes actually mitigate the vulnerability or merely cosmetically rename variables.
- **Intended Users**: Software engineers, application security (AppSec) teams, DevOps / DevSecOps engineers, and open-source maintainers who require automated CI/CD security gatekeeping combined with interactive, evidence-grounded remediation assistance on pull requests.

---

## Key Features

- **Feature 1 — Empirical Evaluation & Quantitative Benchmarking**: Measures real AppSec performance with an automated benchmark suite reporting **Precision (100%)**, **Recall (100%)**, **False-Positive Rate (0.0%)**, **F1 Score**, **Fix-Verification Accuracy (100%)**, **Evidence Correctness (100%)**, and **Secret Leakage Rate (0.00%)**, complete with a $2 \times 2$ confusion matrix across vulnerability categories.
- **Feature 2 — Multi-Turn Conversational Review & Fix Verification**: Interactive conversational agent allowing developers to query threat paths, request unified diff patches, and run iterative verification to prove whether code fixes legitimately eliminate vulnerabilities before merging.
- **Feature 3 — Deterministic Detection with Zero-Leakage Scrubber**: High-fidelity detection for JWT flaws (`verify=False`, `alg: none`), high-entropy API secrets (AWS, Stripe, DB credentials), API BOLA mutations, permissive CORS with credentials, and CI workflow privilege escalation. All credentials are deterministically redacted before logging or sending to models.
- **Feature 4 — STRIDE Attack Path Correlation & Context-Aware Risk**: Maps isolated static findings into multi-step exploitation chains (Spoofing, Tampering, Elevation of Privilege) and weights risk dynamically based on asset exposure (`PUBLIC`, `AUTHENTICATED`, `INTERNAL`).
- **Feature 5 — Dual Ingestion & Automated CI/CD Gating**: Reviews code directly via live GitHub Pull Request URLs (fetching diffs and posting formatted PR summary comments) or local repository files and diff uploads.

---

## Technical Stack

- **Language & Runtime**: Python 3.10+ (CLI & agent package) / Node.js 20+ & TypeScript (Full-stack web console & benchmark server)
- **Agent Framework / Core Library**: Native Python multi-turn conversational agent with function calling / Google GenAI SDK (`@google/genai`)
- **Models**: Gemini 3.8 Flash (`gemini-3.8-flash`) with safe timeout fallback to deterministic AppSec rule engine
- **External APIs / Tools**: GitHub REST API (`/repos/{owner}/{repo}/pulls`, `/issues/{number}/comments`), Tailwind CSS v4, Lucide Icons, Express

---

**Data Flow Execution:**
1. **Input Ingestion**: The agent receives a GitHub PR URL or local file diff.
2. **Deterministic Scrubbing**: Credentials and tokens are scrubbed and replaced with safe hashes (`[REDACTED_AWS_KEY_...]`).
3. **Static & Threat Analysis**: Analyzers extract AST evidence, trace cross-file data flows, map STRIDE threats, and calculate exposure-weighted risk scores.
4. **CI/CD Gating**: Verifies whether any findings violate the configured threshold (`CRITICAL`, `HIGH`, `MEDIUM`).
5. **Interactive Remediation**: Developers converse with the agent to examine attack paths, generate patches, and run fix verification.

---

## Prerequisites

Before running this project, ensure you have:

- **Python**: Python 3.10+ (Standard library `argparse` and `urllib` are supported with zero mandatory dependencies; optional: `pydantic`, `typer`, `rich`, `requests`).
- **Node.js**: Node.js 18+ and `npm` (for the interactive web console and benchmark dashboard).
- **API Keys**:
  - `GEMINI_API_KEY`: Required for full AI-assisted conversational reasoning (agent falls back safely to deterministic AppSec rules if omitted).
  - `GITHUB_TOKEN`: (Optional) Required only when fetching private GitHub repositories or posting review comments to PRs.

---

## Installation & Setup

### 1. Python CLI & Review Agent Package
## 🚀 Installation & Setup

### 1. Python CLI & Review Agent Package

Navigate to the agent directory:

```bash
cd agents/securepr-agent
```

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate the virtual environment:

**Linux/macOS:**

```bash
source .venv/bin/activate
```

**Windows (PowerShell):**

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

> **Note:** If the CLI uses only Python's standard library, installing additional dependencies may be optional. Follow the dependencies declared in `requirements.txt`.

Configure environment variables:

```bash
cp .env.example .env
```

**Windows (PowerShell):**

```powershell
Copy-Item .env.example .env
```

> **Security:** Add your API keys to `.env` locally. Never commit real API keys, tokens, or credentials to GitHub.

---

### 2. Interactive Web Dashboard & Benchmark Console

Navigate to the directory containing the web console's `package.json`:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Open the local URL displayed in your terminal.

If your application runs on port `3000`, visit:

**Local URL:** http://localhost:3000

You can also try the **[SecurePR AI Live Demo](https://ais-pre-jfcqgavphygpd27fzgez7f-822512156195.asia-southeast1.run.app)**.

---

### 3. Configuration & Environment Variables

Configure the following variables in your `.env` file, if supported by your implementation.

| Variable | Required | Description |
|---|---|---|
| `GEMINI_API_KEY` | Optional | Enables AI-assisted security reviews and remediation |
| `GITHUB_TOKEN` | Optional | Enables authenticated GitHub API access |
| `PORT` | Optional | Specifies the web server port |
| `NODE_ENV` | Optional | Defines the Node.js environment |
| `APP_URL` | Optional | Specifies the application URL |

**Example `.env.example`:**

```dotenv
GEMINI_API_KEY=
GITHUB_TOKEN=
PORT=3000
NODE_ENV=development
APP_URL=http://localhost:3000
```

> **Important:** Use placeholder values in `.env.example`. Keep actual secrets in your local `.env` file and exclude it from version control.

---

### 4. Run the Empirical Evaluation Benchmark

Run the benchmark suite from the repository root:

```bash
python3 agents/securepr-agent/src/securepr_agent/cli.py benchmark
```

The benchmark evaluates the security review system using metrics supported by the implementation, such as:

- **Precision**
- **Recall**
- **F1 Score**
- **False-Positive Rate**
- **Fix-Verification Accuracy**
- **Secret Leakage Rate**

> **Note:** Report benchmark percentages only after reproducing them with an actual run.

---

### 5. Run an Automated Security Scan

Scan a local directory with a high-severity failure threshold:

```bash
python3 agents/securepr-agent/src/securepr_agent/cli.py scan --path . --fail-on HIGH --exposure PUBLIC
```

**Generate Markdown and JSON reports:**

```bash
python3 agents/securepr-agent/src/securepr_agent/cli.py scan --path . --md-out pr-summary.md --json-out pr-report.json
```

The scan identifies supported security issues and evaluates findings against the configured severity threshold.

---

### 6. Scan a GitHub Pull Request

Analyze a GitHub pull request:

```bash
python3 agents/securepr-agent/src/securepr_agent/cli.py scan-pr \
  --pr-url https://github.com/owner/repo/pull/42 \
  --token "$GITHUB_TOKEN" \
  --post-comment \
  --md-out pr-review.md
```

I also runs with standard library):
pip install -r requirements.txt
