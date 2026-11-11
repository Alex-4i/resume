# Alexey Vorobyev: Resume and Case Studies

**CEO of an AI agency.** I bring AI into industrial companies and build products around it: from a platform with an AI chat and automations to internal accounting systems.

I sell, design and build: I talk to the customer, map the process, write the spec and the commercial proposal, build a prototype and bring it to a working demo environment. I write code together with AI agents (Claude Code and others). It is my everyday tool, and I know where it speeds things up and where it needs tight control: tests, number verification, isolation, manual review of results.

- GitHub: [Alex-4i](https://github.com/Alex-4i)

> Customer names and commercial terms are deliberately not disclosed: some projects are under NDA. I describe the problem, the solution and the outcome.

---

## What I Do

| Area | Scope |
|---|---|
| AI for industry | assistants over ERP data, drawing and design-documentation analysis, labor-time estimation, template-based documents, computer vision for quality control |
| Product development | multi-tenant platforms, internal accounting systems, CRM/PM, lead-generation landing pages |
| Consulting | pre-project audit, payback estimates, AS-IS/TO-BE process maps, pilots with an honest risk assessment |
| Sales and marketing | cold B2B outreach, funnels, personal-brand content |

**Stack:** Python (Flask), TypeScript, Next.js 15, React, Prisma, PostgreSQL, SQLite, MongoDB, Docker, nginx, LibreChat, DeepSeek / Claude / local models, OData and REST integrations with ERP and CRM systems, chat-bot APIs, PaddleOCR-VL, python-pptx, Playwright / headless Chrome.

---

## Case Studies

### 1. Enterprise AI platform for industrial companies

**Business problem.** Companies running an ERP (including heavily customized legacy versions) want to work with their data and documents through AI, but cannot send data to a public chat and do not trust numbers the model "made up".

**What I built.** A platform on top of the LibreChat engine (used as a headless core): an AI chat with access to ERP data through a data-access gateway, a research mode, automations in natural language (the Flow module), ready-made "products" for specific customer tasks, analytical dashboards and reports (PDF and PPTX).

**Technical challenges:**
- **Calculation layer.** The model does not do the math itself: it composes a calculation plan, and the platform computes the numbers. Figures in the answer are checked against the results; unverified ones are sent back to the model for recalculation. Covered by automated tests (over 900 checks in the full run).
- **Request anonymization** before sending to external models: rules for requisites, personal names, legal entities and amounts, plus an LLM reviewer for data coming from internal sources.
- **A separate web gateway** that is the only service with internet access, with SSRF protection and page/PDF reading with OCR.
- **The answer lives on the server, not in the browser tab:** generation runs in a separate thread, so the tab can be closed and reopened.
- **Secure by default:** sandbox for untrusted HTML, outbound requests from automations denied by default, customer systems read-only, request-forgery protection, session lifetimes.
- **Modular core** with a module registry, per-module schema migrations, licensing, and token-usage accounting per employee, department and product, with limits.
- **Chat attachments:** background parsing, fragment search with page and sheet references, xlsx/csv tables as a source for calculations.
- **Load measurements:** 50 concurrent sessions on a laptop; the bottleneck is the model, not the platform. GPU memory sizing for local models.
- **Optimization:** a complex multi-part question was sped up from 35 to 20 minutes by separating "fast" and "deliberate" model phases and improving the calculation layer.

**Stack:** Python, Flask, SSE, SQLite (WAL), MongoDB, LibreChat, DeepSeek, Waitress, python-pptx.

---

### 2. Drawing version comparison and change notices (design documentation): equipment manufacturer

**Business problem.** When a drawing changes, the designer manually finds the differences, writes a change notice, distributes it and looks for other products that use the part. It is slow and leads to missed changes.

**What I did.**
- Read drawings through the COM interface of the KOMPAS-3D CAD system and compared two versions: dimensions, geometry, title block. Objects are matched by their internal identifier.
- Validated against a reference set: the customer made 11 edits of five types in copies of the drawings; all were found, there were no false positives, and a control "re-saved without changes" produced zero findings.
- Designed the TO-BE process: comparison, a draft change notice with editable fields, replacement of the drawing in the archive, distribution, where-used check.
- Built a demo of the "Design change" product inside the platform.
- Recorded honestly: on a real pair of revisions only 41-57% of objects matched by identifier and the geometry is noisy, so the risk is reduced but not removed, and the way forward is a paid pilot without promising results in advance.

**Outcome:** the customer received a functional specification, a commercial proposal and infrastructure requirements for their IT department. In parallel I worked on labor-time estimation from drawings (reference books from the 1970s-80s digitized with OCR) and on generating product manuals.

**Stack:** Python, pywin32 (COM), KOMPAS-3D, PaddleOCR-VL.

---

### 3. AI assistant for item-master matching and raw-material calculation: metal structures manufacturer

**Business problem.** The customer's item list has to be matched by hand to the internal item master in the ERP, and raw-material consumption calculated.

**What I did.** Ran a pre-project audit and two meetings with the customer, and designed a solution on the platform (case 1): item matching and raw-material calculation, with ERP data access via OData. This became the first customer of the platform.

---

### 4. Automating design-documentation output: electrical equipment manufacturer

**Business problem.** Drawings, specifications, DXF files and schematic albums for every questionnaire are assembled by hand in KOMPAS / AutoCAD / SolidWorks.

**What I did.** Held meetings, arranged the NDA and agreement, and prepared a pilot plan: a test of replacing an element in schematics as the minimum check of the hypothesis. Compared "classic ERP customization" with "AI" and stated honestly where each one ends.

---

### 5. Concrete dispatching and paperwork: concrete producer

**Business problem.** Issuing delivery notes and quality certificates with errors takes 5-10 minutes when it should take 30 seconds; orders get lost; there is no queueing for drivers.

**What I did.** In a day, before a meeting with management, I built a clickable demo as a single offline HTML file: a board and Gantt chart with plan suggestion, delivery calculation inside the order, documents in 30 seconds, a single orders window, a driver screen, and "live" parsing of an incoming order email. I added industry research, scenarios and synthetic data with a verifiable calculation core. Facts I was not sure about (for example, penalties under the electronic consignment note rules) are marked as unconfirmed.

---

### 6. Cold B2B outreach pipeline: database, letters, sending, analytics

**Business problem.** A steady flow of inbound inquiries from industrial companies is needed without a sales department.

**What I did.**
- An outreach panel (Next.js + Prisma/SQLite): clients, projects, segments (one segment is one letter hypothesis), a shared company registry with unique tax IDs, mailboxes with sent-mail sync, reply analytics.
- Letter generation from company databases: a prompt with gates by revenue and headcount, checks for banned words and for fit between the offer and the company size, website research for larger companies.
- Batch sending through a scheduler: state-level duplicate protection, a shared blocklist by domain and address, deduplication against all mailboxes (over 16,000 already contacted addresses in one snapshot).
- Solved typical pitfalls: numbering between batches, DNS blackholing over VPN, laptop sleep, an empty Sent folder.
- Ran a rebranding follow-up on the base: about 1.4 thousand and 1.7 thousand letters in two campaigns.

**Outcome:** the funnel produces live leads, including manufacturers with whom meetings and audits took place.

---

### 7. Internal finance platform (double-entry bookkeeping)

**Business problem.** The agency needs to see real profit per business line (projects, a subscription product, mailings), accounting for turnover tax, obligations to customers and shared expenses.

**What I did.**
- Full double-entry accounting: accounts, journal entries, an **independent Postgres trigger enforcing entry balance**, an append-only ledger (corrections only by reversal), periods closed forward only, multi-currency.
- Import of bank statements (CSV and ERP exchange formats) with recognition rules and reconciliation.
- P&L, Cashflow and Balance Sheet reports; profit in three levels: per source, shared expenses, net.
- A model specification based on real statements: a flat 6% turnover tax on the full amount received, customer balance top-ups treated as a liability rather than revenue.

**Stack:** Next.js 15, TypeScript, Prisma 6, PostgreSQL, JWT (refresh-token rotation), Tailwind, recharts.

---

### 8. AI gateway and usage billing

**Business problem.** Access to models must be sold to customers with usage accounting in rubles, limits and audit.

**What I did.** A multi-tenant control plane: organizations, workspaces, projects, products; policy-based routing of requests to models with fallback chains; billing in rubles at the central bank rate, a ledger, usage analytics, audit; platform-operator and customer-admin roles.

**Stack:** Next.js, TypeScript, Prisma.

---

### 9. Client Success Platform: CRM and implementation project management

**Business problem.** Sales, contracts, implementation projects and support live in different places, and company history gets lost.

**What I did.** Designed the architecture using PSA practices and open-source project-management systems: a "spine" around a company entity with a stable ID (like the partner in Odoo), membership as user x project x role, statuses from a configurable workflow, an immutable event feed. Implemented the foundation (authentication, companies, employees, access registry), CRM and project management. Laid the groundwork for a unified company history feed from several sources: CRM form, meeting transcripts, wiki.

---

### 10. Websites and lead generation

- Landing pages for an AI agency: Next.js 15, lead forms that write to the database and send chat notifications, case pages (an AI assistant for a paint and coatings producer, computer vision for PVC profile geometry control and for metalworking, an AI assistant for tank operations).
- A product website for a conversation-analytics product for e-commerce. Vite + React + Tailwind, five industry versions on subdomains, nginx routing by Host header, Docker, Let's Encrypt, SEO, cookie consent before the analytics script loads, legal documents aligned with Russian personal-data law, self-hosted fonts to avoid cross-border data transfer.

---

### 11. Corporate portal and filter selection (pre-project studies)

- A portal for a company of 92 employees: brief, letter to the customer, preparation for a commercial proposal.
- An assistant for selecting industrial filters: building the brief and the selection model.

---

### 12. Training and personal projects

- **A consultant course for industry** (web app): 30 lessons of 45 minutes, about 130 questions with alternating variants, spaced repetition, 17 SVG diagrams with an automatic text-overlap check, tasks with a reference solution visible only to the AI mentor, automated data and UI checks.
- **A company dossier system:** from a tax ID and any piece of information, AI tags industry, size and production type and files it into Markdown cards for cross-cuts.
- **Wake Up!:** a web prototype of an alarm-clock mobile app built from a Figma design (Vite + React).
- **A blog and content about AI at work:** a content pipeline from a messenger channel to Instagram, a funnel, lead magnets, a "keyword / views" metric.
- A local OCR on PaddleOCR-VL for scans and page photos.

---

## How I Work

- **Problem first, technology second.** An audit may end with an honest "this needs classic ERP customization, not AI".
- **I do not promise what has not been verified.** First a reference set and a measurable hypothesis, then a paid pilot.
- **Security from the start:** isolation, outbound requests denied by default, anonymization, secrets kept out of repositories.
- **Verifiability.** Numbers in AI answers are checked by code, the platform has automated tests, and in finance the database guarantees balance.
- **I document decisions:** why it was done this way and not another, what was postponed and what remains open.

---

*This resume is updated as new projects appear.*
