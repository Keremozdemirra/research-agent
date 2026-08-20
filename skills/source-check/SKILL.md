---
name: source-check
description: Verifies citations in two separate passes, writing REFERENCES.md and SOURCE-CHECK.md. Pass one - does each work exist as cited, against publisher or DOI records. Pass two - does each source actually say what the document claims, checked against primary text with page-level quotes. Reports failed claims first, flags which are load-bearing, and states how many works were checked and which were not. Also covers numeric figures (emission factors, regulatory thresholds, GWP values), where the rule is a primary source plus retrieval date and data vintage. Use whenever the user wants citations, references, a bibliography or sources checked or fact-checked; asks "does this source actually say that" or "is this citation right"; says "kaynakları doğrula" or "atıfları kontrol et"; before publishing a paper, note or report that cites anything; when adding a published factor or threshold to code; and whenever a claim rests on a work reached through a summary or abstract rather than primary text.
---

# Source check

Most citation checking stops at "the work exists". That catches typos in a page
range and nothing else. The errors that actually damage an argument are of a
different kind: the work exists, is cited correctly, and does not say what it
was cited for. Those survive every reference manager and every reviewer who
recognises the name.

So this skill runs **two passes that are deliberately not merged**, and writes
them to two files, because collapsing them lets the weaker pass borrow the
authority of the stronger one.

| Pass | Question | Output |
|---|---|---|
| 1 — existence | Does the work exist, as cited? | `REFERENCES.md` |
| 2 — substance | Does it say what we claim it says? | `SOURCE-CHECK.md` |

A bibliography that has been through pass 1 only is a *verified bibliography*,
not a checked one, and the file must say so in those words. Overclaiming here is
the one failure mode that makes the whole exercise worthless.

## The rules that make this worth doing

**Never confirm a citation you have not opened.** Not "this is the standard
reference for X", not a page number reconstructed from memory, not a
characterisation of an author's argument based on how they are usually
summarised. Secondhand citation is precisely what produces the errors this skill
exists to catch — an argument described as logical when the author is explicit
that it is physical, a theorem described as proving randomness when it proves a
martingale property.

**Quote verbatim, with a page number.** A pass-2 finding is only checkable by
the next reader if it carries the words. Paraphrase is where the original error
crept in; repeating the paraphrase does not test it.

**Say what you did not check.** The credibility of the output comes from the
scope statement — "six of the twenty works were checked at primary-text level;
§4 lists the fourteen that were not" — far more than from the checks themselves.
An honest gap is an asset. A hidden one is what sinks the document later, and it
sinks the checked claims along with it.

**Mark how each source was reached.** Full text, a scanned page image, an
abstract, a secondary source quoting it, a publisher landing page. These are not
equivalent and the entry should not present them as if they were.

**Flag load-bearing failures.** A failed claim in a passing aside and a failed
claim the argument rests on are different events. Say which one you are looking
at, in the summary line, before the reader has to work it out.

## Workflow

### 1. Extract every claim that rests on a source

Read the document and list, separately:

- **Citations** — every work referenced, with what it is being used to support.
- **Claims about what a source says** — the sentences that characterise an
  author's argument, result or position. These are pass-2 material and they are
  easy to miss, because they often appear without a citation directly attached.
- **Numbers with provenance** — factors, thresholds, rates, prices, GWP values.
  These need a source, a retrieval date and a data vintage, and they follow the
  same two passes: does the source exist, and does it give this number for this
  scope and this year.

### 2. Pass 1 — existence

For each work, confirm author, year, title, venue, volume, issue and page range
against a publisher record, a DOI landing page, or the publisher's own catalogue.
Where the publisher page is paywalled, RePEc / EconPapers / PhilPapers /
PubMed / the national library catalogue are acceptable and should be named.

Use `deep-research` for the fan-out when the list is long — it saves each source
to its own file with verbatim quotes, which is exactly the raw material pass 2
needs. Do not let it substitute for reading, though; its summaries are a map,
not the territory.

Write `REFERENCES.md` (see `references/templates.md` for the full structure).
State the date of the pass and, in plain words, what "verified" does and does not
mean here — specifically that it does **not** verify that any work says what the
document claims.

### 3. Pass 2 — substance

Take the claims from step 1 and check each against primary text. For each:

- Locate the passage. If you cannot reach the primary text, that claim is
  **unchecked**, not "probably fine" — record it in the unchecked list and move
  on. Resisting the urge to settle it from a summary is the whole skill.
- Quote the relevant lines verbatim with a page reference.
- Decide: does the claim hold, hold with qualification, or fail?
- If it fails, write the **fix** — the sentence the document should say instead,
  and why the corrected version is or is not weaker for the argument. Often it
  is stronger; say so when it is.

Write `SOURCE-CHECK.md`. Order matters: **failures first**, then qualified
holds, then clean holds, then the explicit unchecked list. A reader who stops
after the first screen should have seen the bad news.

### 4. Feed the fixes back

A check that does not change the document was a waste of a pass. Apply each fix
to the source document, and where a fix removes a support the argument was
leaning on, say so in that document's own limitations section rather than
quietly patching the sentence.

Then update the summary line at the top of `SOURCE-CHECK.md` — how many claims
checked, how many held, how many failed, how many of the failures were
load-bearing — so the state is legible without reading the file.

## Numeric figures

Same two passes, different evidence. For an emission factor, a regulatory
threshold, a carbon price or a GWP value:

- **Primary source only.** The implementing act, the standard, the agency's own
  dataset — not a consultancy summary, not a blog, not a previous project.
- **Retrieval date and data vintage are different things** and both are needed.
  "Retrieved 12 August 2026" says when you looked; "2024 vintage" says which
  edition of the number you took. A figure without the second cannot be defended
  when the source restates it.
- **Scope has to match.** GWP values differ by IPCC assessment report; emission
  factors differ by boundary, by region and by whether they are location- or
  market-based. A correct number for the wrong scope is a wrong number.
- **When sources disagree, say so** and pick with a stated reason, rather than
  silently taking the first one.

If the figure is going into code, it belongs in an explicit argument with the
citation next to it, not in a hardcoded default — a default is a number nobody
will re-check.

## Reference

`references/templates.md` — the exact structure of `REFERENCES.md` and
`SOURCE-CHECK.md`, with the summary-line format and the scope-statement wording.
Read it before writing either file.
