# Andrés Jiménez

Barranquilla, Colombia.

I ran US healthcare revenue cycle operations for seven years: eligibility verification, DME
billing, prior authorization, ICD-10 cross-referencing. Now I build the systems that do that work
without a person in the middle. The BPO I founded reached $100K/month in revenue with a team of 10
and 20+ partner call centers, until US program shutdowns ended every contract in October 2025.

Since August 2026 I work as an AI Automation Engineer at Contractor Marketing Pros, a US marketing
agency for home service contractors. I also run [Sikorre](https://sikorre.com), where I build AI
agents and automation for businesses.

People who know revenue cycle don't build automation. People who build automation have never run a
business.

I ran that business myself: payroll, clients and partner call centers. I know how a business works
from the inside, so before I build anything I look for the bottleneck, the step where time or money
leaks. That is the one I automate first. In `rag-privado` below, the bottleneck was not how I split
the text. It was the input: the regulation is published as a structured API and I had been indexing
a PDF of it.

Everything below is public, and every number in it came out of execution data or a test I ran by
hand.

## Repositories

**[pdf-parsing-retrieval](https://github.com/andresjmnz92-jpg/pdf-parsing-retrieval)** · The retriever finds the right
filing 85% of the time and the right page only 35%. I built this to compare three PDF parsers on
FinanceBench: 368 filings, 53,901 pages, 150 questions with expert-verified answers. Plain text
extraction ranked first on all six measurements, and none of the gaps survived a paired test
(McNemar, closest pair p = 0.071).

**[rag-langgraph](https://github.com/andresjmnz92-jpg/rag-langgraph)** · `recall@10` reported 100% on a system that was
delivering 81%. The right section came back, but the chunk with the answer sat at rank 63, 26 and
151. This is the HIPAA RAG rebuilt in Python: a LangGraph agent over pgvector, exposed through
FastAPI and MCP. The agent loop scored the same 19/20 at twice the time and 2.7× the cost, and it
stays in the repo with its table.

**[rag-privado](https://github.com/andresjmnz92-jpg/rag-privado)** · A retrieval system over HIPAA (45 CFR Parts 160, 162
and 164) on a $9/month server with no GPU. Embeddings are computed locally, so the document never
leaves the machine. Measured three times with the same 20 hand-verified questions: 55% → 75% → 80%.
Hybrid search was built, measured and not adopted: recall@10 fell from 16/16 to 13/16.
`rag-langgraph` shows what that 80% could not see.

**[job-radar](https://github.com/andresjmnz92-jpg/job-radar)** · Reads four job boards every hour, filters with a cheap
regex, then an LLM scores each posting 0 to 10 against a profile. Only 6+ reaches my phone. When
nothing is new it makes no model call, and the fastest run on record is 0.4 s. It ran hourly on my
own server from 8 to 27 August 2026. Of the 351 executions n8n still held, one scheduled run failed.

**[servidor-n8n-autoalojado](https://github.com/andresjmnz92-jpg/servidor-n8n-autoalojado)** · The server everything else
runs on, including a client's ordering bot in production. Automatic patching, daily off-site backups
to R2, and a watchdog that runs on GitHub's machines instead of the one it watches. Both alarms were
tested by making them fail, and the backup was restored onto a throwaway volume.

**[subagent-startup-cost](https://github.com/andresjmnz92-jpg/subagent-startup-cost)** · Your rules file loads into every
subagent you start, including the rules it cannot act on. Moving procedure into files that load on
demand cut the cost of starting one cold agent from 33,662 to 17,408 tokens (−48%) with no rule
deleted. One machine, one day, numbers to reproduce.

**[finance-analyst-assistant](https://github.com/andresjmnz92-jpg/finance-analyst-assistant)** · Ask a question in plain
English and it queries the ledger, reads the policy, checks the budget and does the arithmetic.
Deterministic tools, a router instead of a free-running loop, and a trace of every run. Python 3.12,
standard library only.

*Also public: [cazador-oracle](https://github.com/andresjmnz92-jpg/cazador-oracle), switched off. Its one finding: GitHub's
cron accepts "every 5 minutes" and fires every 1 to 2 hours.*

## How I work

I build n8n workflows from Claude Code through n8n's MCP server instead of dragging nodes across a
canvas. `validate_workflow` runs before anything is activated, and I confirm the result by reading
execution data, not by watching it work once.

My node names are full sentences in Spanish. I am the one reading them at 3am when something stops.

Before I state a result I ask whether I verified it or I am repeating it from memory. That question
killed three expensive fixes before I built them. One would have cost an afternoon reindexing a
corpus that turned out to be fine.

I am not a software engineer. Six semesters of mechatronics, no degree, and I am working through
Henry's full-stack program. Certified C1 English. I can connect APIs, write the script a workflow
needs, and find why it stopped overnight.

## Right now

At work I build and repair automation for a marketing agency and its clients: CRM syncs, AI chat
agents, live client dashboards on Cloudflare Workers, and a self-hosted n8n server. That code is
private.

The question from the repos above is still open. The retriever finds the right document 85% of the
time and the right page 35%. Parser tuning, chunking, hybrid search and an agent loop did not move
that gap. Two approaches are written down and not built yet: scoring per chunk instead of per page,
and retrieving the small fragment while handing the model the section it came from.

andresjmnz92@gmail.com · [LinkedIn](https://linkedin.com/in/aj1604)
