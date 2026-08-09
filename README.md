# Hi, I'm Vel 👋

Quality Engineering and Test Automation professional focused on building scalable automation solutions, improving software quality, and accelerating software delivery.

## 🔧 Technical Focus

- **Test Automation:** Playwright, TypeScript, JavaScript, Selenium, REST Assured
- **API & Integration Testing:** REST APIs, Contract Testing, Postman
- **CI/CD & DevOps:** GitHub Actions, Jenkins, Docker, Kubernetes
- **Quality Engineering:** Test Strategy, Shift-Left Testing, Risk-Based Testing, Release Quality
- **AI-Assisted Testing:** GenAI for test design, test generation, synthetic data, and automation
- **Domain:** Financial Services & Capital Markets

## 🚀 Featured Projects

### 🎭 Playwright Enterprise Automation Framework

Enterprise-style Playwright + TypeScript automation framework demonstrating modern quality engineering practices.

**Highlights:** UI & API automation • Page Object Model • Cross-browser testing • Parallel execution • Failure diagnostics • GitHub Actions CI/CD

➡️ [View Project](https://github.com/murugavelanbalagan-byte/playwright-enterprise-framework)

# AI Quality Engineering Lab

A provider-agnostic **AI-assisted Quality Engineering framework** built with JavaScript and Node.js. The project demonstrates how Generative AI can support test design and failure analysis while keeping deterministic validation and human review at the center of the quality process.

The framework supports both **Ollama for local LLM execution** and **OpenAI for hosted AI models**, allowing the AI provider to be changed without modifying the Quality Engineering workflow.

## 🎯 What This Project Demonstrates

* AI-assisted requirement analysis and test design
* Risk-based test scenario generation
* AI-assisted test failure analysis
* Structured LLM outputs using JSON Schema
* Runtime response validation using Zod
* Provider-agnostic LLM architecture
* Local LLM execution with Ollama
* Optional OpenAI API integration
* Human-in-the-loop AI governance
* Deterministic CI with GitHub Actions
* Secure handling of API credentials and test data

## 🏗️ Architecture

```text
                 Quality Engineering Workflow
                           |
                           v
                  structuredResponse()
                           |
                  AI Provider Interface
                           |
                 +---------+---------+
                 |                   |
                 v                   v
              Ollama              OpenAI
            Local Model          Hosted API
                 |                   |
                 +---------+---------+
                           |
                           v
                   Structured JSON
                           |
                           v
                   Schema Validation
                           |
                           v
                     Human Review
```

This separation allows the Quality Engineering workflow to remain independent of the underlying LLM provider.

## 🧪 Use Case 1: Requirement → Test Analysis

The framework reads a user story and acceptance criteria and uses an LLM to identify:

* Requirement assumptions
* Product and quality risks
* Positive scenarios
* Negative scenarios
* Boundary conditions
* Security scenarios
* Integration scenarios
* Accessibility considerations
* Test priority (P0–P3)
* Automation candidates
* Coverage gaps

Example input:

```text
As a registered customer,
I want to sign in with my username and password
so that I can access my account.

Acceptance Criteria:

1. Valid credentials allow sign-in.
2. Invalid credentials display an authentication error.
3. Username is required.
4. Password is required.
5. Locked accounts cannot sign in.
6. Five consecutive failed attempts temporarily lock the account.
```

The AI response is constrained using **JSON Schema** and validated locally with **Zod** before being accepted by the application.

Conceptually:

```text
User Story
    |
    v
LLM Risk Analysis
    |
    v
Structured Test Scenarios
    |
    v
JSON Schema Validation
    |
    v
Zod Runtime Validation
    |
    v
Human Review
```

AI-generated scenarios are treated as **test proposals**, not automatically approved test requirements.

## 🔍 Use Case 2: AI-Assisted Failure Analysis

The framework can also analyze sanitized test execution evidence.

Example:

```text
Test: standard user can log in

Error:
Timeout waiting for inventory page

Recent change:
Application deployed 20 minutes before execution

Retry:
Passed on first retry

Trace:
Login returned HTTP 200.
Inventory request took 28.4 seconds.
```

The model classifies the failure as one of:

```text
application_defect
automation_defect
environment_issue
test_data_issue
potential_flaky_test
unknown
```

It also provides:

* Confidence level
* Supporting evidence
* Recommended investigation steps
* Quarantine recommendation
* Human-review requirement

A retry pass alone is deliberately **not treated as proof that a test is flaky**.

The AI provides an investigation hypothesis; it does not make the final root-cause or release decision.

## 🔌 Provider-Agnostic AI Architecture

The QE workflow communicates through a common provider interface:

```text
                    QE Workflow
                         |
                         v
              structuredResponse()
                         |
                +--------+--------+
                |                 |
                v                 v
             Ollama            OpenAI
             Local              Cloud
                |                 |
                +--------+--------+
                         |
                         v
                  Structured JSON
```

This makes it possible to evaluate:

* Model quality
* Privacy requirements
* Cost
* Latency
* Local vs. hosted execution

without redesigning the testing workflow.

## 🦙 Running Locally with Ollama

Ollama is the default provider and allows the project to run locally without API usage charges.

Install Ollama and verify:

```bash
ollama --version
```

Download the configured model:

```bash
ollama pull gemma3
```

Test the model:

```bash
ollama run gemma3
```

## ⚙️ Project Setup

Install dependencies:

```bash
npm install
```

Create the local environment file:

```bash
cp .env.example .env
```

For Ollama:

```text
AI_PROVIDER=ollama

OLLAMA_BASE_URL=http://localhost:11434/api
OLLAMA_MODEL=gemma3
```

No OpenAI API key is required when using Ollama.

## ▶️ Run Requirement Analysis

```bash
npm run analyze
```

The framework reads:

```text
requirements/login-story.md
```

and generates:

```text
generated/requirement-analysis.json
```

## ▶️ Run Failure Analysis

```bash
npm run failure
```

The framework reads:

```text
examples/failure-sample.txt
```

and generates:

```text
generated/failure-analysis.json
```

## ☁️ Switching to OpenAI

Change the provider configuration in `.env`:

```text
AI_PROVIDER=openai

OPENAI_API_KEY=your_api_key
OPENAI_MODEL=your_model
```

No application code changes are required.

> Never commit API keys or `.env` files to source control.

## 🧪 Run Unit Tests

```bash
npm test
```

Unit tests validate deterministic framework behavior such as schema enforcement.

AI model calls are intentionally excluded from normal CI execution to avoid:

* API costs
* Secret exposure
* Model-response variability
* Dependency on external AI services

## 🔐 Responsible AI Controls

The project intentionally demonstrates several controls that can be applied when introducing GenAI into enterprise Quality Engineering:

1. **Structured output** — LLM responses must conform to predefined JSON schemas.
2. **Runtime validation** — responses are validated locally using Zod.
3. **Human review** — generated tests are proposals rather than authoritative requirements.
4. **Data protection** — confidential data, credentials, PII, and production information should never be included in prompts.
5. **No automatic quarantine** — AI cannot hide failing tests by automatically classifying them as flaky.
6. **No automated release decisions** — release and root-cause decisions remain with engineers.
7. **Provider abstraction** — QE workflows are independent of a specific AI vendor.
8. **Deterministic CI** — standard CI validation does not depend on LLM availability.

## 📁 Project Structure

```text
ai-quality-engineering-lab/

├── .github/
│   └── workflows/
│       └── quality.yml
│
├── examples/
│   └── failure-sample.txt
│
├── requirements/
│   └── login-story.md
│
├── src/
│   ├── providers/
│   │   ├── index.js
│   │   ├── ollamaProvider.js
│   │   └── openaiProvider.js
│   │
│   ├── analyzeFailure.js
│   ├── analyzeRequirement.js
│   ├── config.js
│   ├── jsonSchemas.js
│   └── schemas.js
│
├── tests/
│   └── schema.test.js
│
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

## 🚀 Planned Enhancements

* Synthetic test-data generation
* Playwright test skeleton generation
* Requirement-to-test traceability
* Golden-dataset evaluation for AI-generated tests
* Duplicate scenario detection
* Risk-based regression selection
* Prompt and model version tracking
* Model quality comparison
* Cost and latency telemetry
* Prompt-injection and adversarial testing

## 💡 Engineering Principles

This project treats Generative AI as an **engineering accelerator rather than a source of truth**.

The goal is to combine LLM reasoning with deterministic validation, established test automation practices, human oversight, and measurable quality controls.

## Disclaimer

This is an independent portfolio project built using synthetic and public examples. It contains no proprietary employer code, data, architecture, prompts, credentials, or internal implementation details.


## 🧪 What I'm Exploring

- AI-assisted software testing
- Agentic test automation
- Playwright + AI integrations
- Intelligent test generation
- AI-enabled quality engineering

## 📫 Connect

[LinkedIn] https://www.linkedin.com/in/murugavel18/
