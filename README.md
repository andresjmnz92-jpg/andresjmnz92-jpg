# Andrés Jiménez

Barranquilla, Colombia.

I ran US healthcare revenue cycle operations for seven years: eligibility verification, DME
billing, prior authorization, ICD-10 cross-referencing. Now I build the systems that do that work
without a person in the middle. The BPO I founded reached $100K/month in revenue with a team of 10
and 20+ partner call centers, until US program shutdowns ended every contract in October 2025.

People who know revenue cycle don't build automation. People who build automation have never had
to check whether an L-Code is billable.

Everything below is public, and every number in it came out of execution data or a test I ran by
hand.

## Repositories

**[rag-privado](https://github.com/andresjmnz92-jpg/rag-privado)** · A retrieval system over HIPAA
(45 CFR Parts 160, 162 and 164) running on a $9/month server with no GPU. Embeddings are computed
locally, so the document never leaves the machine — which is the entire pitch for a clinic.

I measured it three times with the same 20 hand-verified questions: **55% → 75% → 80%**, and the
README explains why that last jump proves nothing on its own. The corpus was rebuilt mid-way when
I found the real bottleneck wasn't my chunking strategy but the input format — the regulation
publishes as a structured API and I had been indexing a PDF of it.

It also contains something I have not seen in another repo: **an audit of my own citations**. Ten
of twenty-two claims in my research turned out to be attributed to pages that don't say them. They
are listed in a table with what each page actually says, rather than quietly deleted.

**[job-radar](https://github.com/andresjmnz92-jpg/job-radar)** · Four job boards read every hour,
deduplicated, filtered by a cheap regex, then scored 0–10 by an LLM against a profile. Only 6+
reaches my phone. **26 scheduled runs, 0 failures, median 18 s** — and the fastest run is **0.4 s**,
because when nothing is new it never pays for a single model call.

The README shows a screenshot of real alerts and points at the two bugs visible in it, including
one where a cosmetic escaping error is also a silent data-loss bug.

**[servidor-n8n-autoalojado](https://github.com/andresjmnz92-jpg/servidor-n8n-autoalojado)** · The
server everything else runs on, including a client's ordering bot in production. Automatic
patching, daily backups off-site to R2, and a watchdog that runs on GitHub's machines rather than
the one it watches.

Both alarms were tested in the failing direction, and the backup was **restored onto a throwaway
volume to prove it opens** — because a backup nobody has restored is an assumption, not insurance.

**[cazador-oracle](https://github.com/andresjmnz92-jpg/cazador-oracle)** · Switched off, and the
README explains why — which is the useful part. It also documents something measured: GitHub's
cron asks for every 5 minutes and fires every 1–2 hours, so the fix was making each run last 50
minutes rather than asking more often.

## How I work

I build n8n workflows from Claude Code through n8n's MCP server rather than by dragging nodes
across a canvas. `validate_workflow` runs before anything is activated, and I confirm the result by
reading execution data, not by watching it work once.

My node names are full sentences in Spanish. I am the one reading them at 3am when something
stopped.

**And the habit that shows up in every repo above:** *is this verified, or am I asserting it from
memory?* Asking that killed three expensive fixes I was about to build, and one of them would have
cost an afternoon reindexing a corpus that turned out to be fine.

I am not a software engineer. Six semesters of mechatronics, no degree, and currently working
through Henry's full-stack program. I can connect APIs, write the script a workflow needs, and find
why it stopped overnight. Architecting your microservices is somebody else's job.

## Right now

Adding reranking and hybrid search to the RAG — the two fixes its own evaluation points at — and
measuring whether they move the number or just the latency. Both are in that repo's "what's next",
written before building them.

andresjmnz92@gmail.com · [LinkedIn](https://linkedin.com/in/andres-jimenez-112915148)
