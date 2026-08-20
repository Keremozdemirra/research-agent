# File templates

Two files, two jobs. Keep them separate even when both passes happen in the same
sitting — a reader needs to be able to tell which guarantee they are getting.

---

## REFERENCES.md — the existence pass

```markdown
# Verified bibliography — <document name>

**Verification pass completed <date>.**

Every reference in [<SOURCE DOC>](<link>) was checked against a publisher
record, DOI landing page, or the publisher's own catalogue entry.
**<N corrections were needed, touching M of the K entries> / <Nothing needed
correcting>**; those are listed in §2.

**What "verified" means here, precisely.** Author, title, year, venue, volume,
issue and page range were confirmed against publisher or DOI-level records, and
against <fallback databases used> where the publisher page was paywalled. It
does **not** mean the page images were inspected, and it does not verify that
any work says what <SOURCE DOC> claims it says. That is a separate reading pass;
see [SOURCE-CHECK.md](SOURCE-CHECK.md) for its current state.

---

## 1. Verified references

### <Thematic group>

- **Author, A. (Year).** *Title.* Venue, vol(issue), pp–pp. DOI/URL.
  <Edition or translation notes. How the record was confirmed, if not obvious.>

## 2. Corrections made

### 2.1 <Short label>
Cited as: <the wrong version>
Actually: <the right version>
Source of the correction: <publisher record / DOI / catalogue>

## 3. Entries that could not be confirmed

<Works where no authoritative record was reachable. Say what was tried. Do not
silently drop them — an unconfirmable citation is information.>
```

The paragraph beginning "What 'verified' means here" is not boilerplate. It is
the sentence that stops a reader treating a pass-1 file as a pass-2 result, and
it should be rewritten to match what was actually done rather than copied.

---

## SOURCE-CHECK.md — the substantive pass

```markdown
# Substantive source check — <document name>

**Pass completed <date>.** Companion to [REFERENCES.md](REFERENCES.md), which
established that the works exist as cited. This pass asks the harder question:
**does each source actually say what [<SOURCE DOC>](<link>) claims it says?**

<N> claims were checked against primary text. **<X> hold. <Y> do not, and <Z> of
those <Y> is/are load-bearing.** Page-level citations for everything checked are
in §3.

**Scope, stated honestly.** <N> of the <M> works in REFERENCES.md were checked
at the level of primary text: <list them>. <The rest> have **not** been checked
substantively; §4 lists them. Where a work was reached only through a secondary
source or an abstract rather than the full text, this is said in the entry.

---

## 1. Claims that do not survive the check

### 1.1 <§ref> — <one-line statement of what is wrong>

The document says: "<verbatim quote of the claim>"

<What the source actually says, with a block quote and a page reference:>

> "<verbatim quote from the primary text>"
> — Author (Year, part/section, p. N)

<Why the difference matters — one or two sentences, not a lecture.>

**Fix:** <the sentence the document should say instead.> <Whether the corrected
version is weaker or stronger for the argument, and why.>

## 2. Claims that hold with qualification

### 2.1 <§ref> — <what holds and what does not>

<Same shape. These are the entries that stop the file reading as a purge.>

## 3. Claims that hold

### 3.1 <§ref>
> "<verbatim>" — Author (Year, p. N)
Claim stands as written.

## 4. Not checked

Works in REFERENCES.md that have not been through this pass:

- **Author (Year)** — <why not: paywalled, physical copy only, not yet needed,
  ran out of time.> <Which claims depend on it, so the reader knows what is
  exposed.>
```

---

## The summary line

The bolded sentence near the top — *"<N> claims were checked. <X> hold. <Y> do
not, and <Z> of those is load-bearing"* — is the file's most-read line. Keep it
numeric and keep it current when fixes land. "Several issues were found" tells a
reader nothing and lets the number drift.

## What "load-bearing" means

A failed claim is load-bearing when removing it changes what the document can
conclude. A claim in a supporting aside can fail without the argument moving; a
claim the central argument rests on cannot. Deciding which one you have is a
judgment, so make it explicitly and in one sentence, rather than leaving the
reader to infer it from where the failure sits in the document.

## Common failure shapes to look for in pass 2

These recur often enough to be worth checking for by name:

- **Title taken as theorem.** A paper called "Proof That Properly Anticipated
  Prices Fluctuate Randomly" proves a martingale property — a restriction on
  the conditional mean, implying nothing about independence or variance. Titles
  overstate; the result is in the text.
- **Kind of impossibility swapped.** Physical impossibility described as logical
  undecidability, empirical limit described as a proof, and vice versa. This one
  survives review because both versions sound authoritative.
- **Scope silently widened.** The source shows it for one market, one period or
  one boundary; the citing document states it generally.
- **Hedge dropped.** The author writes "it might also be shown, I believe"; the
  citation reports it as shown. Check footnotes specifically — hedges live there.
- **Independence claimed between entangled arguments.** "This argument is
  independent of X" where the source's mechanism depends on X.
- **Secondary attribution.** A characterisation that traces back to a later
  summariser rather than the original, and has drifted in transit.
