# research-agent

Agents and skills for research that survives someone checking it.

The failure mode of assisted research is not fabrication so much as
confident approximation: a citation that exists but does not say what it was
cited for, a figure carried through three summaries from a primary source nobody
opened, a claim attributed to a paper that argues something adjacent.

This repository holds the agents and skills for research that holds up when
somebody follows the footnotes. Sources are read rather than summarised, claims
are checked against primary text with page-level quotes, and what could not be
verified is stated as unverified rather than quietly rounded into the argument.

## What this is not

It is not a search wrapper. Finding sources is the easy half.

It is not fast. The checking pass costs more than the writing pass, and a skill
here that promises otherwise is lying.

It does not settle contested questions. It makes clear what is contested and on
what grounds.

## Layout

```
agents/
  <name>.md           one specialist, its brief and its boundaries
skills/
  <name>/
    SKILL.md          the instruction, with triggering description frontmatter
    scripts/          only where deterministic code beats instruction
examples/
  <name>/             worked example on real input, with the output committed
```

## Roadmap

See [BACKLOG.md](BACKLOG.md). The first unchecked item is the one being built.

## Planned contents

Nothing here is built yet. This table is the intended shape, and the daily loop
fills it in one item at a time.

| # | Skill | What it does |
| --- | --- | --- |
| 001 | [two-pass-source-check](skills/two-pass-source-check) | The verification method: pass one asks whether each work exists as cited, pass two asks whether it says what the document claims, with page-level quotes. |
| 002 | [literature-scan](skills/literature-scan) | Map a field: what the main positions are, who holds them, what each rests on, and where the genuine disagreements sit rather than the terminological ones. |
| 003 | [evidence-table](skills/evidence-table) | Build the table that carries a claim, its sources, the strength of each, and the strongest counter-evidence, so a reader can weigh rather than trust. |
| 004 | [primary-source-retrieval](skills/primary-source-retrieval) | Get to the original rather than the summary, and record what to do when the original is paywalled, withdrawn or in another language. |
| 005 | [numeric-provenance](skills/numeric-provenance) | The rule for figures: primary source, retrieval date, data vintage. |
| 006 | [adversarial-review](skills/adversarial-review) | Read your own research as a hostile referee would and write down what they would find, before they do. |
| 007 | [research-brief](skills/research-brief) | The output format: what was asked, what was found, what remains unverified, and how confident each part is. |
| 008 | [stale-claim-sweep](skills/stale-claim-sweep) | Re-check an existing document against current sources and report what has aged, since research decays quietly. |

## Licence

MIT. See [LICENSE](LICENSE).
