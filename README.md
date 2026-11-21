# Automated CI/CD Pipeline & TypeScript Service

A production-structured Node.js, Express, and TypeScript service demonstrating automated Continuous Integration (CI) quality gates, DevSecOps least-privilege permissions, deterministic dependency management, and continuous deployment to Render.

## Key Engineering & DevOps Highlights

This repository demonstrates core CI/CD and operational stability practices implemented via GitHub Actions (`.github/workflows/ci.yml`):

* **Shift-Left Quality Gates:** Automated verification runs on every `push` and `pull_request` targeting `main`, preventing broken builds or type regressions from reaching production.
* **DevSecOps & Least-Privilege Security:** Explicitly restricts `GITHUB_TOKEN` scope to `permissions: contents: read`, minimizing attack surface within the runner environment.
* **Deterministic Builds:** Enforces `npm ci` against a locked `package-lock.json` rather than `npm install`, guaranteeing identical dependency trees across local, CI, and production environments.
* **Pipeline Performance & Governance:** Implements npm dependency caching (`cache: npm`) to reduce runner execution times and enforces a strict `timeout-minutes: 10` guardrail to prevent hung processes and wasted compute.
* **Operational Observability:** Exposes a dedicated `/api/health` readiness/liveness endpoint alongside `APP_VERSION` metadata injection for zero-downtime deployment verification.

---

## CI Pipeline Architecture

```text
  [Push / Pull Request to main]
                │
                ▼
  ┌───────────────────────────┐
  │   GitHub Actions Runner   │
  │  (ubuntu-latest, Node 24) │
  └─────────────┬─────────────┘
                │  1. Least-Privilege Checkout (contents: read)
                │  2. Deterministic Install (npm ci + cache)
                ▼
  ┌───────────────────────────┐
  │  Quality Gate (verify)    │
  │  • TypeScript Typecheck   │
  │  • Automated API Tests    │
  │  • Production Build (dist)│
  └─────────────┬─────────────┘
                │  (Pass)
                ▼
  ┌───────────────────────────┐
  │   Deployment (Render)     │
  │  • Health Check Verification│
  └───────────────────────────┘
```

## Tech StackRuntime & Language: 
* Node.js 24, TypeScript, ExpressCI/CD & Automation: 
* GitHub Actions (actions/checkout, actions/setup-node)
* Testing & Quality: Automated integration testing (self-contained server lifecycle), static type checking (tsc)
* Cloud Hosting: Render (PaaS)

## Local Development & Verification
Prerequisites
* Node.js 24+ and npm
* Git
### Setup
```text
Bash
git clone [https://github.com/fawadirfan/cicd-lab.git](https://github.com/fawadirfan/cicd-lab.git)
cd cicd-lab
npm ci
```

