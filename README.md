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

**[pdf-parsing-retrieval](https://github.com/andresjmnz92-jpg/pdf-parsing-retrieval)** · Does a PDF
parser that understands tables retrieve better than plain text extraction? Measured on FinanceBench:
368 filings, **53,901 pages**, 150 questions that ship with expert-verified answers and page-level
evidence. 282 of the indexed documents carry no question and are indexed anyway, because that is
what makes the task realistic.

**The answer is no, and it costs 11–41% more fragments to find out.** Plain extraction ranks first
on all six measurements; Docling's output is 11% larger as Markdown and 41% as HTML. **And not one
of those gaps survives a paired test** — McNemar's exact test, closest pair p = 0.071. The method
was committed before any result existed, which is the only reason I can say that instead of
reaching for a friendlier statistic afterwards.

The finding I was not looking for is the bigger one: **75 of 150 questions retrieve the correct
document and the wrong page inside it.** 85% find the filing, 35% find the page. That 50-point gap
is an order of magnitude wider than anything separating the three parsers.

**[rag-langgraph](https://github.com/andresjmnz92-jpg/rag-langgraph)** · The same RAG rebuilt in
Python — a LangGraph agent over pgvector, exposed through FastAPI and MCP. The first job was not to
improve it but to make it **identical**: 16/16 recall and MRR 0.938, matching the original to three
decimals, so that any later comparison measures the agent instead of the port.

Then the useful part. **`recall@10` was reporting 100% on a system delivering 81%.** It asks whether
the right *section* came back, and it did — while the chunk actually carrying the answer sat at rank
**63, 26 and 151**. A corpus version I had already measured and **rejected** turned out to fix two
of them. The rejection was a sound decision made on a metric that could not see the defect, which is
worth more than any score in the repo.

The cycle LangGraph exists for was built and measured too: same 19/20, **twice the time and 2.7× the
cost**. Fixing the data had made the architecture unnecessary. It stays in the repo with its table,
because a negative result only teaches if it is published.

**[rag-privado](https://github.com/andresjmnz92-jpg/rag-privado)** · A retrieval system over HIPAA
(45 CFR Parts 160, 162 and 164) running on a $9/month server with no GPU. Embeddings are computed
locally, so the document never leaves the machine — which is the entire pitch for a clinic.

I measured it three times with the same 20 hand-verified questions: **55% → 75% → 80%**, and the
README explains why that last jump proves nothing on its own. *(The sequel is in `rag-langgraph`
above: that 80% was judged by a metric that could not see three of the four failures.)* The corpus
was rebuilt mid-way when I found the real bottleneck wasn't my chunking strategy but the input
format — the regulation publishes as a structured API and I had been indexing a PDF of it.

Hybrid search was the top recommendation on my own list. Built, measured, **not adopted**: recall@10
went from 16/16 to 13/16 and MRR from 0.865 to 0.435.

It also contains something I have not seen in another repo: **an audit of my own citations**. Ten
claims across two research documents turned out to be attributed to pages that don't say them. They
are listed in tables with what each page actually says, rather than quietly deleted.

**[job-radar](https://github.com/andresjmnz92-jpg/job-radar)** · Four job boards read every hour,
deduplicated, filtered by a cheap regex, then scored 0–10 by an LLM against a profile. Only 6+
reaches my phone. When nothing is new it never pays for a single model call — the fastest run on
record is **0.4 s**.

Running hourly on my own server since 8 August. Across the **351 executions n8n still holds, one
scheduled run failed**: 18 August at 21:00, Telegram rejecting an unescaped entity. That is the same
class of bug the README already documents — the one where a cosmetic escaping error is also a silent
data-loss bug, because the alert it was carrying never arrived. It is on the list, and the number
stays here as 1 rather than 0.

**[servidor-n8n-autoalojado](https://github.com/andresjmnz92-jpg/servidor-n8n-autoalojado)** · The
server everything else runs on, including a client's ordering bot in production. Automatic patching,
daily backups off-site to R2, and a watchdog that runs on GitHub's machines rather than the one it
watches.

Both alarms were tested in the failing direction, and the backup was **restored onto a throwaway
volume to prove it opens** — because a backup nobody has restored is an assumption, not insurance.

**[subagent-startup-cost](https://github.com/andresjmnz92-jpg/subagent-startup-cost)** · Your rules
file is loaded into every subagent you start, including the rules that subagent cannot act on.
Measured across one day, moving procedure into files that load on demand: **33,662 → 17,408 tokens
to start one cold agent — −48%, with no rule deleted.** One machine, one day, numbers to reproduce
rather than trust.

*Also public: [cazador-oracle](https://github.com/andresjmnz92-jpg/cazador-oracle), switched off and
kept for one measured finding — GitHub's cron accepts "every 5 minutes" and fires every 1–2 hours,
so the fix was making each run last 50 minutes rather than asking more often.*

## How I work

I build n8n workflows from Claude Code through n8n's MCP server rather than by dragging nodes across
a canvas. `validate_workflow` runs before anything is activated, and I confirm the result by reading
execution data, not by watching it work once.

My node names are full sentences in Spanish. I am the one reading them at 3am when something
stopped.

**And the habit that shows up in every repo above:** *is this verified, or am I asserting it from
memory?* Asking that killed three expensive fixes I was about to build, and one of them would have
cost an afternoon reindexing a corpus that turned out to be fine.

I am not a software engineer. Six semesters of mechatronics, no degree, and currently working
through Henry's full-stack program. Certified C1 English. I can connect APIs, write the script a
workflow needs, and find why it stopped overnight.

## Right now

Three evaluations in a row have pointed at the same place, and none of them were designed to.

The HIPAA evaluation said the remaining failures were not retrieval failures — the right section was
already arriving at rank 1, 1, 2 and 1. Measuring the chunk instead of the section showed the
sentence that answers had never been in front of the model at all. And the FinanceBench study, built
to compare PDF parsers, found that **the retriever identifies the right filing 85% of the time and
the right page 35%**.

Parser, chunker, hybrid search, an agent loop: four things measured, and the ones that were supposed
to help did not. What keeps showing up instead is the distance between finding the document and
finding the sentence.

So that is the open question — 50 points that no amount of parser tuning moved. Two approaches are
written down and not yet built: scoring per chunk rather than per page, and retrieving the small
fragment while handing the model the section it came from. Whichever way it lands goes up, the same
as the ones that lost.

andresjmnz92@gmail.com · [LinkedIn](https://linkedin.com/in/aj1604)
