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

## How to use this

These are skills for Claude, not a command-line tool. There is nothing to
install and nothing to import — you describe the work and the matching skill
fires on its own.

**In Claude Code or Cowork**, once the skills are on your machine:

```bash
bash ~/Desktop/agent/_setup/sync-skills.sh
```

That clones every agent repository and links its `skills/` into `~/.claude/skills`,
so they are available in every session and every folder. Re-run it whenever the
daily loop ships something new — it pulls rather than re-clones.

Then simply ask. Each skill's `description` frontmatter is written to match how
the request actually gets phrased, in English or Turkish, so you do not name the
skill and generally should not have to think about which one applies.

**If nothing fires**, that is a defect in the skill rather than in how you
asked. The description was written for the wrong phrasing. Say what you asked
and what you expected, and it gets fixed — that feedback is more valuable than
working around it.

**What is actually built** is listed under Contents below and in the Done
section of [BACKLOG.md](BACKLOG.md). Everything under Queue is planned and does
not exist yet.

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

## Contents

| Skill | What it does |
| --- | --- |
| [deep-research](skills/deep-research) | Run a multi-source investigation with triangulation and an adversarial pass, when getting it wrong is expensive. |
| [source-check](skills/source-check) | Verify citations in two passes: does the work exist, and does it say what it was cited for. |

| Agent | What it does |
| --- | --- |
| [arastirmaci](agents/arastirmaci.md) | Runs sourced web research on a question. |

These arrived already written and in daily use, rather than being built against the queue below — which is why most carry no item number. Some have Turkish bodies: they were written in the language they are used in, and translating them is a queue item rather than a blocker.

Everything still under Queue in [BACKLOG.md](BACKLOG.md) does not exist
yet. The daily loop builds one item a day.
## Licence

MIT. See [LICENSE](LICENSE).
