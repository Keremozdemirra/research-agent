---
name: "deep-research"
description: "Run a disciplined, multi-source research investigation for a high-stakes question or decision: fan-out web search, source triangulation (each claim backed by 3+ independent sources), an adversarial review pass, and every source saved to its own file with verbatim quotes. Use when a low-quality answer is expensive: strategy work, comparing N products/methods/markets, validating a hypothesis with external data, or mapping how a field works. Türkçe isteklerde de tetiklen: \"şu konuyu derinlemesine araştır\", \"kaynaklarıyla birlikte çıkar\", \"şu ikisini karşılaştır\", \"literatür taraması yap\", \"bu iddia doğru mu, araştır\". NOT for quick fact-checks."
---

# Deep Research: Disciplined Meta-Research

Turn "research this topic" into an auditable, reusable investigation. The output is a folder you can return to in a month: every claim traces to a specific source file, the plan documents *why* each choice was made, and a refresh protocol lets you update it later without re-running everything.

**This is the heavy, methodical end of research.** It is the workflow to reach for when getting the answer *wrong* costs more than the tokens spent getting it right.

## When to use

- A low-quality answer is expensive: strategy, business plan, report, or article groundwork.
- Comparing N institutions, products, methodologies, or markets and you need defensible reasoning.
- Validating a hypothesis or a decision against external data.
- Meta-research: "understand how X works," "map the landscape of Y," answering a connected series of questions.

Do **not** use it for quick fact-checks (answer directly) or fast topic overviews where decision risk is low.

## The pipeline (10 phases)

Depth scales with the task. `shallow` runs the core phases inline; `medium` and `deep` add capability discovery, verification, and refresh targets. Phase 0 runs at every depth.

| # | Phase | What it does |
|---|-------|--------------|
| 0 | **Existing work** | Before any search, run the long-term-memory search and grep the prior research folders (example: `grep -ril "<key term>" <root>/*/plan.md`). Start `plan.md` with one line: the command run, the paths grepped, and the prior work found or `none found` |
| 1 | **Reframe** | Rewrite the question, fix the underlying decision, state 2 to 4 *falsifiable* hypotheses, log any supplied prior as a check target |
| 2 | **Genre & blocks** | Pick the report genre (qa / explainer / decision / landscape / validation / custom) and its building blocks |
| 3 | **Plan** | Write `plan.md`: scope, structure, sourcing strategy, opposition queries, risk register, stop-criteria |
| 3.5 | **Capability discovery** | Audit available API keys/channels in the environment; map subtopics to sources; fall back to HTML where needed; write each access limitation with its tool, endpoint and date |
| 4 | **Search** (loop) | Dispatch sources → launch sub-agents in parallel → fetch & dedup → save each to `sources/NN.md`; re-evaluate between rounds |
| 5 | **Score & triangulate** | Rate every source on Credibility / Recency / Bias; require ≥3 independent, differently-typed sources per thesis |
| 6 | **Synthesize + adversarial** | Assemble the report from blocks, run 4 self-critique questions, add steel-manned counter-arguments |
| 6.5 | **Verify** | Lightweight citation check before closing |
| 7 | **Refresh targets** | Extract entities / numbers / hypotheses into `refresh_targets.md`, the entry point for future updates |

## Core mechanisms

These are what separate a documented investigation from a confident guess:

- **Triangulation.** Every thesis must be backed by ≥3 independent sources of *different types* (primary / academic / industry / discussion). A claim with fewer is flagged "insufficient evidence" and stays out of the conclusions.
- **Commensurable figures.** A ranked or compared list asserts that its rows measure the same thing. When a set mixes audited fiscal-year revenue with company-announced ARR or run-rate, tag each row with its metric type and as-of date, and say in the report that the two differ. Example: Canva reported $4B ARR and $3.5B recognised revenue for the same period. When the gap would change the order, place the ARR figure beside the ranking, outside the ordered rows.
- **Source-grounding.** Each source becomes its own `sources/NN_slug.md` with metadata, verbatim quotes, and scores. No dangling claim: every assertion links back to a specific file. An empty fetch produces an empty claim.
- **Retrieval ladder.** A failed fetch is a property of the tool. Climb three rungs before downgrading the source: (1) the fetch tool; (2) when the response reports a corrupted or binary document and names a saved file, run `pdftotext -layout <path>` and grep the text; (3) when the host returns 403, retry with `curl -sL -A "<name> <email>"`, the descriptive User-Agent that SEC EDGAR requires and many IR hosts accept. Substitute an aggregator for a primary source only after all three rungs fail, and record the failure in the source file.
- **Negative results are narrow.** A 404 or "not found" binds only the exact host or query. Before "not found" becomes "does not exist", widen along every dimension the target could occupy. For a name or product occupancy check that means three probes: the apex domain, the product subdomains (app, www, my, get, go), and the registration record's last-updated date. Record all three.
- **Adversarial pass.** Phase 6 always runs the strongest available reasoning: 4 self-critique questions plus an active search for counter-arguments and disconfirming evidence.
- **Falsifiable hypotheses.** Phase 1 commits to 2 to 4 hypotheses; Phases 5 and 6 explicitly confirm or refute each against the evidence, or mark it under-determined.
- **Labelled priors.** At intake, ask the requester for the figures they already believe and where each came from; record them in `plan.md` as "to check against", outside the evidence. When issuing a request, supply your own prior the same way: value, provenance, and a note that it is a check target. In the report, state for each figure whether the path to it was independent of any supplied prior; a confirmation counts as evidence only when the path was independent.
- **Scope before parameter.** When the question asks for a conversion factor, unit, parameter or coefficient, first confirm from the governing text that the quantity sits inside the boundary the parameter serves. Record the scope provision next to the parameter. Example: a gas absent from a regime's scope annex needs no GWP factor, because it never enters the calculation.
- **Parallel sub-agents.** Phase 4 launches search sub-agents concurrently: cheap models for broad web sweeps, stronger ones for reasoning-heavy subtopics.
- **Refresh protocol.** Phase 7 emits `refresh_targets.md`; an `update <slug>` run produces a delta (new entrants, entity changes, refreshed numbers, adversarial triggers) and leaves the rest of the investigation in place. Write every access limitation with its scope: which tool, which endpoint, on what date. On each `update` run, re-test every recorded limitation before inheriting it. A caveat that no longer holds is itself a finding: the primary text the earlier pass worked around is now readable, so read it.
- **Atomic findings.** Reusable theses in `findings/FN.md` plus a `sources.csv` index, so research compounds across questions.

## Output structure

```
<root>/<slug>/
├── plan.md                  # existing-work line, scope, sourcing strategy, risk register, changelog
├── sources.csv              # index of every source with scores
├── sources/
│   ├── 01_<slug>.md         # one file = one source (metadata + verbatim quotes)
│   └── ...
├── findings/                # atomic, reusable theses (larger investigations)
│   └── F1_<short>.md
├── refresh_targets.md       # what to watch on update (medium/deep)
├── diffs/
│   └── YYYY-MM-DD_delta.md  # delta from an `update <slug>` run
└── YYYY-MM-DD_<genre>.md    # final report
```

## Anti-patterns

- **Don't skip Phase 0.** The existing-work line in `plan.md` is the proof it ran. A plan without that line means search started blind; one such run repeated a 20-candidate naming study that had been on disk for three days.
- **Don't skip reframing**, even when the request "seems clear." The decision behind the question usually changes the search.
- **Don't output to chat only.** Always persist sources and the report to files; the reuse value lives in the folder.
- **Don't fabricate citations.** If a fetch returns nothing, the claim is empty. Bind every claim to a saved verbatim quote.
- **Don't build conclusions on a thin corpus.** Too few sources, or sources that all share one type, means triangulation hasn't happened. Say so in the report and lower the stated confidence.
- **Don't skip the adversarial pass** on medium/deep investigations. Confirmation-only research is the failure mode this skill exists to prevent.
- **Don't run sub-agents sequentially.** Fan-out in parallel; serial search wastes the wall-clock advantage.
- **Don't collapse `sources/` into one file.** Per-source files are what make findings searchable and reusable across investigations.
- **Don't pick the heaviest model for everything.** Match model to subtask: cheap for broad sweeps, strong for synthesis and the adversarial pass.

---
*Source: github.com/alirezarezvani/claude-skills (MIT)*
