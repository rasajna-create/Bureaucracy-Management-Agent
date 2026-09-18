# BUREAUCRACY MANAGEMENT AGENT

## 1. Team Details

**Team Name / ID:** Team Cybotic / 

**Team Lead:** Mohammed Rehan

**Team Members:**
<!--
One line per person, including the team lead. Role is optional.
Pick one, combine two, write your own, or leave it blank:
  Agent Whisperer (agents, prompts, LLMs)
  Backend Developer
  Frontend Developer
  UI/UX Designer
  Integrations Engineer (APIs, tools, connecting services)
  Data Engineer (data, databases, retrieval)
  Product & Pitch Lead (idea, presentation, demo)
  Cool Team Member (a bit of everything)
-->

- Mohammed Rehan 
- Sristi Rasajna 
- Vishwanath Sri Sai Tanmai 
- Mylavarapu Jesmitha 
- Parankusham Sai Samanvith 

**Repo Link (Optional):** N/A

**Demo Link (Optional):** N/A

---

## 2. Problem Statement

<!-- Paste the full problem statement exactly as it was given to you. Don't shorten, fix, or reword anything. No character limit here. -->

Build a production-ready agentic AI workflow that helps users navigate bureaucracy (passport, visas, loans, taxes, education abroad, travel, etc.) while maintaining bank-level security. The system must autonomously track documents, match requirements, send reminders, execute actions, and comply with data protection laws.

---

## 3. TL;DR

<!-- One line each. A judge should get your idea in 10 seconds. -->

**Problem:** Fragmented context, manual tracking, insecure document sharing, and passive workflows make bureaucracy slow and unsafe.

**Solution:** Our agent combines a document vault, RAG for bureaucracy guidelines, PII masking, and automated API execution.

**Who benefits:** Users gain fast, secure bureaucracy help based on personal document and context.

---

## 4. Scope of the Project

**What are you building?**

A secure AI agent that manages personal bureaucracy — visas, loans, GST, licenses, and more. It stores documents in an encrypted vault, remembers user context across cases, retrieves current requirements using RAG, matches documents to requirements, and executes approved actions with full auditability.

**How does it solve the problem statement?**

It replaces scattered documents and repetitive form-filling with one context-aware agent: it knows what documents the user has, retrieves applicable requirements, identifies what's missing, and guides or executes approved actions while protecting PII and logging every access.

**Key features you're building for this hackathon:**

Encrypted document vault with PII masking and per-access audit logging
RAG-powered requirements engine for visa, loan, GST, and other bureaucracy workflows
Agentic document-to-requirement matching with human-confirmed actions

**What are you deliberately NOT doing? (Optional)**

We are deliberately not storing the PII data on a cloud database to protect privacy.

---

## 5. Why an Agentic Approach?

**What does your agent decide or do on its own?**

It decides which bureaucracy domain applies, checks the user’s vault against applicable requirements, identifies exactly what’s missing, chooses when to retrieve data or call a tool, and determines whether an action can be automated or requires user confirmation first.

**Why wouldn't a fixed script, if-else rules, or a simple chatbot be enough?**

Bureaucracy involves changing rules and messy, conditional cases that hardcoded logic can’t handle well. A chatbot can generate answers but doesn’t maintain document state, query systems, or execute actions. An agent dynamically reasons, retrieves data, and uses tools to complete workflows.

**What does your agent decide or do on its own?**

It identifies which bureaucracy domain applies from your request, diffs your document vault against current requirements to find what's missing, picks whether to answer directly or call a tool (fetch a document, query live rules, submit a form), and judges when an action is safe to auto-run vs. needs your confirmation.

**Why wouldn't a fixed script, if-else rules, or a simple chatbot be enough?**

Bureaucracy rules change often (visa thresholds, tax slabs) — hardcoded if-else goes stale fast and can't be updated without a redeploy. A chatbot can't remember your documents across sessions or take action for you. Real cases also involve messy, conditional logic (duration, income, nationality) that doesn't reduce cleanly to static rules.

---

## 6. Who It's For & What Changes

**Who or what is this for?**

<!-- Doesn't have to be end users. It could be people, a team, a business, developers, or an internal system or process. -->

Everyday citizens, study-abroad students, job seekers, and travelers needing a secure, centralized life vault for high-stakes filings.

**The world today, without your solution:**

<!-- What happens right now? Who struggles, and what does it cost them in time, money, effort, errors, or missed opportunities? -->

Citizens manually dig up records, verify student visa funds, navigate work sponsorship proofs, and decipher tax rules from scratch. Re-uploading sensitive proofs across fragmented municipal, consular, and banking portals exposes unmasked IDs, while subtle document discrepancies trigger instant rejections.

**The world with your solution, fully built and scaled to production:**

<!-- Imagine your whole idea is built properly and used by everyone it's meant for. What's different? -->

A secure sovereign vault where an adversarial agent pre-audits applications to guarantee approval, recycles proofs across domains (e.g. academic transcripts reused for employment visas), and issues purpose-bound zero-knowledge attestations to portals without ever revealing raw documents.

**What your hackathon build actually delivers today:**

<!-- Of everything you proposed, which part have you built, and which part of the problem does that piece solve right now? A small piece that truly works is a great answer. -->

Audits multi-domain readiness across visas, education admissions, loans, and tax setups while demonstrating purpose-bound PII redaction, adversarial rejection risk checks, and cross-domain vault reuse across municipal and consular workflows.

**Before vs. After**

<!--
2 to 4 rows. Pick things that change: time, cost, effort, accuracy, scale, reach, manual work, risk.
Max 80 characters per cell. Replace the example row with your own.
-->

| What Changes | Today | With Our Current Build | At Production Scale |
|--------------|-------|------------------------|---------------------|
| Document gap analysis |	2–5 hours of manual reading |	Instant automated checklist match |	Zero-click real-time compliance sync |
| Data privacy risk |	Plain files sent over unencrypted email |	Deterministic regex PII masking |	Hardware-level secure enclave processing |
| Application submission | Manual multi-page portal re-entry |	Single-trigger automated API payload |	End-to-end direct government API dispatch |

---

## 7. Architecture & Agents

<!--
All the examples in this section describe ONE made-up project, a college helpdesk agent,
so you can see how the parts fit together. Aim for this level of detail, no more.
You don't need to list every library or every function.
-->

**How is your system put together?**

<!--
Example:
Students ask questions in a web chat. A Triage Agent sorts each message, an Answer Agent
replies using college policy documents, and anything needing a human becomes a helpdesk ticket.
-->

Seven shared layers (auth, security, storage, rules engine, actions, state machine, orchestrator) sit under pluggable domain modules (visa, loan, GST, license). Every domain reuses the same vault, RAG rules engine, and action gateway — no domain-specific forks in the core.

### 7.1 Agents

<!--
One line per agent. For each one, say what its job is, which model it uses and why that model
fits the job, and what it talks to (other agents, APIs, databases, services).

Example:
- **Triage Agent:** Reads each message and decides if it's a policy question, a complaint, or needs a human. Uses Llama 3.1 8B locally, since sorting is simple and student data stays on our machine. Talks to the Answer Agent and Web Chat.
- **Answer Agent:** Answers policy questions from college documents and files a ticket when approval is needed. Uses Claude Sonnet because it handles long policy text and reasons well about exceptions. Talks to College Docs Store and Helpdesk Ticket API.
-->

- **Planner Agent:** Parses the user's request, identifies the bureaucracy domain, and breaks it into steps. Claude Sonnet — strong structured intent classification. Talks to the Requirements Diff Engine and Context Store.
- **Tool-Calling Agent:** Decides which tool to invoke — vault fetch, RAG rule lookup, or external API (bank, embassy, GST portal). Claude Sonnet with function-calling. Talks to Action Gateway and Vector DB.
- **Reasoning Agent:** Diffs what the user has against what's required, ranks missing items, and drafts the plain-language answer. Claude Sonnet. Talks to Context Store and Case State Machine.

### 7.2 Services, APIs, Databases & Memory

<!--
One line for everything that isn't an agent: databases, APIs, external services, tools,
and your interface (web app, bot, CLI). Say what it is, what it does, and who uses it.
Mention if it's mocked.

Example:
- **College Docs Store (Chroma vector database):** Holds fee, exam, and hostel policy PDFs. Used by the Answer Agent.
- **Helpdesk Ticket API (mocked):** Creates a ticket for the right college office. Used by the Answer Agent.
- **Web Chat (Streamlit):** Where students type questions and see answers. Talks to the Triage Agent.
-->

- **PostgreSQL (database):** Stores users, documents, cases, requirements. Used by every agent for structured lookups.
- **Pinecone/pgvector (vector DB):** ndexes bureaucracy rule text for RAG retrieval. Used by Tool-Calling Agent.
- **S3/MinIO (storage):** Holds encrypted document files; DB stores pointers only. Used by the vault layer.
- **Vault/KMS (secrets):** Holds encryption keys and third-party API credentials. Used by Security Middleware.
- **Temporal (workflow engine):** Runs long-running actions (visa submission, status polling) with retries. Used by Action Gateway.

**How does your system remember things (memory & state)?**

<!--
Example:
Each chat keeps its last 10 messages in session memory so follow-up questions make sense.
Tickets are saved in SQLite so students can check their status later.
-->

User facts and documents persist in Postgres across sessions — income entered for a loan case is reused for a GST case. Case status is tracked in an explicit state machine, not just chat history.

**Diagram Link (Optional):** N/A

### 7.3 Example Walkthrough

<!--
Take ONE realistic input and show how it moves through your system: which agent picks it up,
what gets passed on, which tools or databases are used, and what comes out at the end.
Up to 8 steps. If the flow branches, use 3a / 3b.

Example:
**Example input:** A student types "Can I pay my semester fee late? I'm waiting on my scholarship."

1. [Web Chat] Sends the message and the student's ID to the Triage Agent.
2. [Triage Agent] Classifies it as a fee-policy question and passes it to the Answer Agent.
3. [Answer Agent] Finds the late-fee policy (uses: College Docs Store) and sees scholarship cases need approval.
4. [Answer Agent] Explains the policy and files an approval request (uses: Helpdesk Ticket API).
5. [Web Chat] Shows the student the answer and their ticket number.

**Final output:** A clear answer quoting the late-fee policy, plus a ticket raised with the accounts office.
-->

**Example input:** ["I'm planning a 3-week Schengen trip in December — help with visa, insurance, accommodation."]

1. [PostgreSQL (database)] [stores users, documents, cases, and requirements] (uses: queried by every agent)
2. [Pinecone/pgvector (vector DB)] [indexes bureaucracy rule text, passes retrieved chunks to the Tool-Calling Agent]
3. [S3/MinIO (storage)] [holds encrypted document files while Postgres keeps only the pointers] (uses: vault layer)
4. [Vault/KMS (secrets)] [holds encryption keys and API credentials, passes decrypted keys to Security Middleware]
5. [Vault/KMS (secrets)] [holds encryption keys and API credentials, passes decrypted keys to Security Middleware]

**Final output:** [Plain-language checklist showing what's ready, what's missing, and one-click next steps — all logged.]

**Anything special about how your workflow runs? (Optional)**

<!--
An algorithm you use, how agents decide what to do next, routing logic, loops, agents working
in parallel, scoring, self-checks. Anything you want us to notice.

Example:
The Triage Agent gives a confidence score with every decision. Below 0.7, the message skips the
Answer Agent and goes straight to a human, so students never get a confident wrong answer.
-->

Actions split into auto-execute (read-only lookups) vs. staged (anything that submits/pays/books) — staged actions always pause for explicit user confirmation before the Action Gateway fires the real API call.

---

## 8. Tech Stack

<!-- Write N/A for any row that doesn't apply. Models are already listed per agent in 7.1. Max 60 characters per cell. -->

| Layer | Technology |
|-------|------------|
| Frontend / Interface | [React + Tailwind] |
| Backend | [FastAPI (Python)] |
| Agent Framework | [LangGraph + Claude tool-calling] |
| Database / Storage | [PostgreSQL, Pinecone, S3/MinIO] |
| Hosting | [AWS (ECS/S3)] |
| Other | [Temporal (workflows), Vault (secrets), OPA] |

---

## 9. What to Expect From Our Current Build

<!--
Be honest. Unfinished, faked, or hard-coded parts are completely normal at a hackathon.
Telling us means we judge what you actually built, and that works in your favour.
Max 120 characters per bullet.
-->

**Working:**

- Document upload, encryption, and storage in the vault
- Chat interface with Claude for parsing bureaucracy requests
- PostgreSQL schema for users, documents, cases, requirements
- Basic audit logging on document access

**Partly working, mocked, or hard-coded:**

- RAG rule lookup using a small hand-curated set of visa/GST rules instead of live scraped sources
- Bank/embassy API calls are mocked with sample sandbox responses
- Requirements diff engine covers Schengen visa and home loan only; other domains stubbed

**Not working or not built yet:**

- Real third-party API integrations (actual embassy, bank, GST portal submission)
- Temporal-based long-running workflow orchestration
- Role-based access control and multi-user permissions
- PII masking via Presidio (currently manual regex-based masking)

**What we'd most like to be judged on:**

[The requirements diff engine — how it checks a user's stored documents against live rule data and identifies exactly what's missing, then hands off cleanly to the agent's answer and staged-action flow.]

---

## 10. Future Scope

<!-- 2 or 3 things you're NOT building yet but plan to. If you clear the checkpoint, you may be asked to build one of them, so keep them concrete and doable. -->

### Idea 1

**Name:** Multimodal OCR Document Ingestion

**What it is:** Automatically extracts validity dates, names, and numbers from uploaded PDF or image scans into vault metadata.

**Why it matters:** Removes the need for users to manually enter document expiration dates and types.

**How we'd build it:** Integrate Tesseract OCR and layout-aware Vision models to populate Pydantic vault schemas.

**Done when:** Uploading a raw passport image successfully registers its expiry date and nationality in SQLite.


### Idea 2

**Name:** Proactive Expiry & Renewal Watchdog

**What it is:** A scheduled worker that scans the vault weekly and notifies users before essential documents expire.

**Why it matters:** Prevents last-minute emergency renewals when urgent travel or loan needs arise.

**How we'd build it:**  Celery / Cron worker checking expiry dates against current timestamp, sending alerts.

**Done when:** Simulating a passport expiring in 30 days generates a proactive renewal notification card.

### Idea 3 (Optional)

**Name:** [Short title, or N/A (max 50 characters)]

**What it is:** [max 200 characters]

**Why it matters:** [max 150 characters]

**How we'd build it:** [max 200 characters]

**Done when:** [How we could show it works (max 150 characters)]

---

## 11. Additional Notes (Optional)

<!-- Anything else you'd like us to know. -->

[Your notes, or N/A (max 500 characters)]
