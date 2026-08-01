# Assignment 2: From Idea to Launch-Ready Product Plan

**Program:** AI SEEKHO  
**Deliverable:** `assignment2.md`  
**Status:** Complete  

---

## Part 1: Problem Discovery and Validation

### Problem Statement
Solo developers and small engineering teams struggle to maintain code quality and catch security bugs before merging Pull Requests (PRs). Traditional static analysis tools (like SonarQube) are notoriously heavy, difficult to configure, and lack contextual code explanations. Enterprise code review tools are cost-prohibitive for indie developers.

### Demand Signal Methodology & Findings

1. **Negative Review Mining & Reddit Community Dwelling (r/reactjs, r/Python, r/WebDev)**
   * **Source:** [r/solodev - Thread on PR Reviews](https://reddit.com/r/webdev)
   * **Finding:** Solo founders and agency devs frequently skip PR reviews due to lack of teammates or time constraints, leading to production bugs.
   * **Paraphrased Quote:** *"As a solo dev, I am my own code reviewer. I often merge subtle bugs in async state management because I don't have a second pair of eyes to spot edge cases."*

2. **Directory Surveillance & Capital Flow Mapping (Product Hunt / BetaList)**
   * **Source:** Product Hunt Developer Tools Category
   * **Finding:** AI-assisted developer tools (e.g., CodeRabbit, CodiumAI) consistently rank in the top 3 Product of the Day spots and are securing early-stage seed funding, indicating high market demand and active capital flow toward AI dev tools.

### Problem Classification: Painkiller vs. Vitamin
* **Classification:** **Painkiller**
* **Reasoning:** Merging broken code into production causes immediate revenue loss, server downtime, and wasted engineering hours. A tool that acts as an automated "second reviewer" directly removes the anxiety and financial pain of breaking production deployments.

---

## Part 2: Product Definition and Tier Classification

### Product One-Pager Definition
**PR-Guard AI** is a lightweight, automated GitHub Action bot designed for solo developers and micro-teams. When a developer opens a Pull Request, PR-Guard scans the diff using LLM reasoning, posts inline suggestions directly on specific code lines, flags potential security vulnerabilities, and generates a concise PR summary. With the recent drop in LLM API inference costs and the release of high-context open/proprietary models, instant, high-quality code review is now economically viable at low price points for individual builders.

### Tier Classification Framework
* **Selected Tier:** **Micro Tier**
* **Build Time:** 10 to 14 days
* **Pricing Model:** Freemium ($0/month for 5 PRs/month; $12/month for unlimited PRs)
* **Revenue Gate Target:** $300 MRR (25 paying subscribers) within 30 days of public launch.
* **Justification:** The core mechanism requires only a single core loop: catching a GitHub webhook event $\rightarrow$ parsing the `git diff` $\rightarrow$ sending a structured prompt to an LLM $\rightarrow$ writing line comments back to GitHub via the GitHub REST API.

---

## Part 3: Tech Stack Justification

| Layer | Technology | Justification |
| :--- | :--- | :--- |
| **Backend Framework** | Python (FastAPI) | Ultra-fast development speed, native async support for handling webhook traffic, and deep ecosystem integration with AI/LLM SDKs. |
| **Hosting & Infrastructure** | Render / Vercel Serverless | Zero operational overhead. Free/low-cost tiers easily handle early traffic with automatic deployments on git push. |
| **Database** | Supabase (PostgreSQL) | Instant REST/GraphQL APIs, built-in Auth, and a free tier supporting up to 500MB—more than enough for early user tables and usage logs. |
| **LLM Engine** | OpenAI API (`gpt-4o-mini`) / Claude API | Low latency, highly accurate structured JSON parsing, and extremely low cost per PR review. |
| **Payments & Billing** | Stripe Checkout + Lemon Squeezy | Merchant of Record handling sales tax/VAT automatically. Eliminates the need to write custom billing infrastructure. |

### Architectural Omissions (What We Are NOT Building)
* **No Custom Auth System:** Delegated entirely to GitHub OAuth via Supabase.
* **No Custom Billing Engine:** Relying strictly on Hosted Stripe/Lemon Squeezy Checkout links.
* **No Complex Dashboard UI:** The core product surface area lives directly inside GitHub PR interfaces via API comments.

> **Core Principle:** Distribution and user experience win over complex custom infrastructure.

---

## Part 4: Mobile App vs. Web App Decision

### Framework Analysis

1. **Distribution Channel:** Web / Developer Integrations win. Developers discover tools on GitHub Marketplace, Twitter/X, and tech forums on their desktops.
2. **Hardware/OS Access Needs:** Zero mobile sensor needs (no camera, GPS, or Bluetooth required).
3. **Usage Pattern:** Desk-bound professional workflow. PR reviews occur while sitting at a computer inside a code editor or browser tab.
4. **Iteration Speed:** Instant backend redeployments without waiting for App Store / Play Store 24-48 hour approval cycles.
5. **Monetization:** 100% direct revenue via Stripe/Lemon Squeezy vs. giving up 15–30% in-app purchase fees to Apple/Google.

### Decision
**Final Choice: Web Application / GitHub App Integration**  
Building a mobile app for developer code reviews creates unnecessary friction. Meeting developers where they already work (GitHub desktop interface) ensures higher adoption and lower churn.

---

## Part 5: SDLC Approach

### SDLC Model Selection: Agile / Iterative (3-Phase Blueprint)

Waterfall is strictly rejected because it commits resources to static assumptions without validating demand, leaving no agility for user-driven pivots.
