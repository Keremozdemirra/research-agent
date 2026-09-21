---
name: source-check
description: Verifies citations in two passes: does each work exist as cited, and does it say what the document claims, against primary text with page-level quotes. Writes REFERENCES.md and SOURCE-CHECK.md, failed claims first. Covers numeric factors and thresholds (primary source, retrieval date, vintage). Use before publishing anything that cites, when adding a published factor to code, or on "kaynaklari dogrula", "atiflari kontrol et". Not for finding sources; use deep-research.
---

# Source check

Most citation checking stops at "the work exists". That catches typos in a page
range and nothing else. The errors that damage an argument are of a different
kind: the work exists, is cited correctly, and does not say what it was cited
for. Those survive every reference manager and every reviewer who recognises
the name.

So this skill runs **two passes that stay separate**, and writes them to two
files, because collapsing them lets the weaker pass borrow the authority of the
stronger one.

| Pass | Question | Output |
|---|---|---|
| 1: existence | Does the work exist, as cited? | `REFERENCES.md` |
| 2: substance | Does it say what we claim it says? | `SOURCE-CHECK.md` |

A bibliography that has been through pass 1 only is a *verified bibliography*.
A checked one has been through pass 2 as well. The file must say which of the
two it is, in those words. Overclaiming here is the one failure mode that makes
the whole exercise worthless.

## The rules that make this worth doing

**Never confirm a citation you have not opened.** That rules out "this is the
standard reference for X", a page number reconstructed from memory, and a
characterisation of an author's argument based on how they are usually
summarised. Secondhand citation is precisely what produces the errors this skill
exists to catch: an argument described as logical when the author is explicit
that it is physical, a theorem described as proving randomness when it proves a
martingale property.

**Open the block around every search hit.** A pattern search tells you where
the tool matched. Before stating what a file says, read the enclosing block
around the match: two lines adjacent in the search output can belong to
different constants in the file, and one audit lifted a regulation citation
from the constant below the one it described. Before stating that something is
absent, search for the concept under every spelling it could take (a check date
can live in a constant, a comment and a rendered string under three names), and
say how you looked. A claim of absence is the weakest claim available: ask what
the strongest evidence would look like if the thing were present, and whether
your search would have found that.

**Quote verbatim, with a page number.** A pass-2 finding is only checkable by
the next reader if it carries the words. Paraphrase is where the original error
crept in; repeating the paraphrase does not test it. Quote whole sentences or
none: an internal ellipsis is an edit, and the clause that looks redundant is
usually the one carrying the reading. If length is a constraint, quote a
shorter continuous span and put the omitted material in a bracketed gloss that
is visibly yours. Have the person who retrieved the text check the
transcription; the assembler can no longer see the clause they cut.

**Say what you did not check.** The credibility of the output comes from the
scope statement ("six of the twenty works were checked at primary-text level;
§4 lists the fourteen that were not"), far more than from the checks themselves.
An honest gap is an asset. A hidden one is what sinks the document later, and it
sinks the checked claims along with it. The counts in the scope statement are
claims too: derive every one from a command whose output you saw, and state the
command. A listing eyeballed at 42 files counted 85 when piped to `wc -l`. If
duplication inflates a count, say so in the same sentence.

The same rule covers your own verification. "Grepped committed history for X"
names a method; a scope statement adds the set and what a negative result
excludes: "grepped committed history for string X; does not exclude other names
or other identifying content". A negative result is a fact about the query.
Reporting it without its coverage turns a narrow check into a broad assurance,
and the assurance stops the next person running the query that would have
found it.

**Mark how each source was reached.** Full text, a scanned page image, an
abstract, a secondary source quoting it, a publisher landing page. These are not
equivalent and the entry should not present them as if they were. Record the
route that produced the bytes, with URL, date and byte count.

**Flag load-bearing failures.** A failed claim in a passing aside and a failed
claim the argument rests on are different events. Say which one you are looking
at, in the summary line, before the reader has to work it out.

## Workflow

### 1. Extract every claim that rests on a source

Read the document and list, separately:

- **Citations**: every work referenced, with what it is being used to support.
- **Claims about what a source says**: the sentences that characterise an
  author's argument, result or position. These are pass-2 material and they are
  easy to miss, because they often appear without a citation directly attached.
- **Numbers with provenance**: factors, thresholds, rates, prices, GWP values.
  These need a source, a retrieval date and a data vintage, and they follow the
  same two passes: does the source exist, and does it give this number for this
  scope and this year.

### 2. Pass 1: existence

For each work, confirm author, year, title, venue, volume, issue and page range
against a publisher record, a DOI landing page, or the publisher's own catalogue.
Where the publisher page is paywalled, RePEc / EconPapers / PhilPapers /
PubMed / the national library catalogue are acceptable and should be named.

Use `deep-research` for the fan-out when the list is long. It saves each source
to its own file with verbatim quotes, which is exactly the raw material pass 2
needs. Its summaries are a map of the territory; the reading is still yours.

Write `REFERENCES.md` (see `references/templates.md` for the full structure).
State the date of the pass and, in plain words, what "verified" does and does not
mean here: specifically that it does **not** verify that any work says what the
document claims.

### 3. Pass 2: substance

Take the claims from step 1 and check each against primary text. For each:

- Locate the passage. If you cannot reach the primary text, that claim is
  **unchecked**. Record it in the unchecked list and move on; settling it from a
  summary is the one move the skill exists to prevent.
- A fetch succeeds when the body holds the text. EUR-Lex answered every URL
  form with HTTP 200 and a 13.7 KB navigation shell that carried the act number
  in its menu and none of the regulation text. Count a fetch as successful only
  when the body contains the row or sentence you are checking. Record the URL,
  the date and the byte count; a body that is small or the same size on every
  request is a failure whatever the status says. If two routes exist, record
  the one that produced the bytes. For EU acts the scripted route is the CELLAR
  resource `http://publications.europa.eu/resource/celex/<CELEX>` with
  `Accept: application/xhtml+xml`; OJ tables use a decimal comma (3,251), so
  parse them as such.
- A tool that cannot parse a PDF may still have saved it. Before recording a
  claim as unchecked, look for the local path in the tool's output and run a
  local text extractor against the file; one call rendered a statutory
  information sheet the fetch tool had refused, version date included. A source
  counts as unreachable only after the local extraction also fails, and the
  entry then names the methods tried.
- Quote the relevant lines verbatim with a page reference.
- Decide: does the claim hold, hold with qualification, or fail?
- If it fails, write the **fix**: the sentence the document should say instead,
  and why the corrected version is or is not weaker for the argument. Often it
  is stronger; say so when it is.

Write `SOURCE-CHECK.md`. Order matters: **failures first**, then qualified
holds, then clean holds, then the explicit unchecked list. A reader who stops
after the first screen should have seen the bad news.

### 4. Feed the fixes back

A check that does not change the document was a waste of a pass. Apply each fix
to the source document, and where a fix removes a support the argument was
leaning on, say so in that document's own limitations section; a quiet patch to
the sentence hides the loss.

Then update the summary line at the top of `SOURCE-CHECK.md` (how many claims
checked, how many held, how many failed, how many of the failures were
load-bearing) so the state is legible without reading the file.

## Numeric figures

Same two passes, different evidence. For an emission factor, a regulatory
threshold, a carbon price or a GWP value:

- **Primary source only.** The implementing act, the standard, the agency's own
  dataset. A consultancy summary, a blog or a previous project is a pointer;
  follow it to the act, the standard or the dataset and cite that.
- **Retrieval date and data vintage are different things** and both are needed.
  "Retrieved 12 August 2026" says when you looked; "2024 vintage" says which
  edition of the number you took. A figure without the second cannot be defended
  when the source restates it.
- **Scope has to match.** GWP values differ by IPCC assessment report; emission
  factors differ by boundary, by region and by whether they are location- or
  market-based. A correct number for the wrong scope is a wrong number.
- **When sources disagree, say so** and pick with a stated reason. Taking the
  first one silently is the failure.
- **Every arithmetic step owes its own check.** A derived figure inherits the
  source and date of its inputs only when each input carries the unit the
  arithmetic needs. "$499 per project plus DAU-based rates" has no billing
  period, so multiplying it by 12 invents one; "$6,500 a year" was written from
  it anyway. Before any per-month or per-year arithmetic, re-read the source
  page for the period. If it publishes none, say the figure cannot be derived
  and anchor on a source that does publish its period.

If the figure is going into code, it belongs in an explicit argument with the
citation next to it. A hardcoded default is a number nobody will re-check.

### Carrying provenance through a tool

A citation kept next to the number in the source file is lost the moment the
number is exported. Provenance survives only as a field of the artefact, so
make it one:

- Name the factor set with a version string that encodes vintage and status,
  such as `2026.07-indicative`.
- Stamp that string on every result object and every exported artefact: the
  workbook's summary sheet, the tool's factor page, and each line of the audit
  trail next to the per-figure source string.
- Repeat the per-figure source on the line that uses it, so a reader holding
  only the export can tell where each number came from and whether it is
  fileable.

Two designs at the ends of the range are defensible: ship no regulatory
constants and require them as arguments, documenting why, or ship a named,
versioned, explicitly marked set. The failure case sits between them: unmarked
defaults that look authoritative after they go stale.

## Reference

`references/templates.md` has the exact structure of `REFERENCES.md` and
`SOURCE-CHECK.md`, with the summary-line format and the scope-statement wording.
Read it before writing either file.
