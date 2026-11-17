---
name: scientific-report
description: Write a physics fitting report — a model fitted to measurements across a sample series (e.g. Co/Ru/Co MOKE + MR + FMR + spin-diode vs Ru thickness), usually produced by /autoresearch. Use when the user asks to write up, report, or update the results of a fit or compare hypotheses.
---

# Scientific report (physics fitting)

## When to use

Results of a model fit must be presented: a physical model, one or more measurement types, a sample series (thickness, temperature, angle…), competing hypotheses. Typical source: an `/autoresearch` run. Not for literature notes (/note) or running fits (/autoresearch).

Reference case: Pt/Co/Ru/Co, Ru wedge 0.05–1.48 nm, coupled macrospins (J1, J2, Ku, easy axis) fitted to polar MOKE, MR, FMR dispersion and VDC lineshapes; DMI vs no-DMI hypotheses.

## Inputs

Read everything before writing:
- Run dirs `autoresearch/*/`: `*-results.tsv`, `handoff.json`, `evals-summary.md`, fit scripts, `output/*.png`.
- Raw data and the experimentalist's notes (file conventions, what was measured where, bad runs).
- Previous reports of this session — needed for model deviations (§2) and comparisons.

Every number traces to a file or script output. Not in a file → not in the report.

## One report per model

- **Significant model change** (new physical term, new coupling form, different measurement model) → **new report** (new doc tab / new file).
- **Same model**, different parameters / constraints / hypotheses → **same report**, one Findings subsection per variant (§3).
- Claude Docs connector available → a doc tab per report. Otherwise `REPORT-<short-name>.md` next to the results, figures beside it.
- Amending a report (clarification, user answer to §6) → edit in place, add `**Update (<date>):** <what changed>` where it changed, @-mention the user.

## Structure (fixed — all six sections, this order)

```
# <System>: <what this experiment tests>
<date> · <author>
<libraries,simulation-packages used>

## 1. Hypotheses
## 2. Model
## 3. Findings
### 3.1 <H1 label>: <one-line description>
### 3.2 <H2 label>: ...
## 4. Comparison
## 5. Limitations and open questions
## 6. Assumptions and possible contradictions
```

### 1. Hypotheses

The key ask of the experiment: what specific variation is tackled. Full, not a one-liner.

| Label | Hypothesis | What changes vs baseline | How it is tested (data, observable, success criterion) |
|---|---|---|---|
| H0 | baseline, e.g. no DMI | — | joint fit of MOKE, MR, FMR, VDC; per-dataset error |
| H1 | interlayer DMI along z | + D·(m1 × m2) per thickness | same data; gain vs H0 at equal budget; D smooth in t_Ru |

Labels H0, H1… are used verbatim in every later section.

### 2. Model

Only what this experiment adds or changes — not the whole standard model.
- One line naming the base model (e.g. "two coupled macrospins, as in <previous report>").
- The tested term(s) in detail: equation (display LaTeX), every symbol defined with unit and bounds, which layer it acts on, how it enters energy/field/torque/signal. E.g. for DMI: how it was implemented, not how the macrospin was.
- **Implementation quirks**: non-standard library code, custom implementations, workarounds, bugs found and fixed (name the file/function/commit).
- **Deviations from earlier experiments in this session**: what differs and why.

### 3. Findings

One subsection per hypothesis / parameter tweak / model variant, each mapped to a label from §1. **Every subsection uses the identical layout** so subsections compare at a glance:

```
### 3.k Hk: <description>
**Tests:** Hk from §1.
**Changed vs H0:** <exactly what differs — term added, constraint, bound, data subset>.
**Assumed / fixed:** <parameters held fixed with values + units, shared parameters + form>.

<Result in 1–2 sentences with numbers.>

<Figures: complete fitting series — every measurement type, every sample, data vs fit.
 Same panel layout, axes and sample order in every subsection.>

| Sample (t_Ru, nm) | J1 (mJ/m²) | J2 (mJ/m²) | Ku1 | Ku2 | θ1 | φ1 | θ2 | φ2 | <tested param> | ... |
<Same columns in every subsection; tested parameter column shows "—" where not in this hypothesis.>

<Parameters vs control variable figure, same axes in every subsection.>

**Fit quality:** | Dataset | Mean error | Typical (→ score 1) |
```

Rules:
- **Repeat** graphs and parameter tables for every variant. Never "same as above" or "only the changed panel".
- No measurement at a sample (or nearest > tolerance, e.g. 0.05 nm) → blank panel with a note. Never pair data and simulation from different samples silently.
- Fitted values with odd signs or sizes are flagged in the text.

### 4. Comparison

- One table, all hypotheses as rows: score per dataset, number of free parameters, physical plausibility (smooth trend? sign flips? matches literature?).
- Which fits best and why. Best score ≠ best answer: a fit with lower error but unphysical parameters is not recommended — say so.
- Extra parameters are compared at equal optimiser budget. Small gain + scattered values = noise absorbed, not evidence.
- Caveats on the recommendation.

### 5. Limitations and open questions

- Conditions under which each hypothesis holds: model approximations (macrospin, uniform states), calibrations taken as given, effective vs physical parameters, weakly constrained parameters, features not reproduced.
- What the measurements cannot see (e.g. intralayer DMI in uniform-state data) → "no evidence" is not evidence of absence; name the measurement that could tell.
- Literature comparison of key fitted quantities, sources tagged *opened* / *snippet* / *from memory*.
- Questions for the user / experimentalist, each with why the answer matters.

### 6. Assumptions and possible contradictions

Bullets. Critical assumptions and places where the work may not match what was asked:
- "You asked for X; I took it as X′ and implemented <how>. If you meant X, <what changes>."
- Interpretation choices made without asking (geometry, angles, which data counted, tolerances).
- Anything from the prompt not done, done differently, or contradicted by the data.

When the user answers, amend the report (Update line) and rerun affected fits if needed.

## Evidence rules (non-negotiable)

### NEVER

- **Never make up data.** No invented measurements, parameter values, literature numbers, error bars or figure points. Missing → say "not measured" / "not available" / blank panel.
- **Never extrapolate or interpolate** beyond the measured samples or field/frequency range. No "J1 at 0.70 nm would be…" from neighbouring points, no fit curve drawn past the data, no trend extended to unmeasured thickness. Model predictions outside the data are labelled **prediction, untested** and kept out of Findings.
- **Never pair data and simulation from different samples** without saying so (tolerance, e.g. 0.05 nm, stated).
- **Never present a snippet or memory value as checked**, and never cite a source not actually opened as if read.
- **Never hide a bad result.** Worst-fitting samples, failed hypotheses, crashed or discarded runs stay in the report.
- **Never claim evidence from a gain that is within noise** or from parameters that flip sign between neighbouring samples.
- **Never silently change the ask.** If the data, model or time cannot deliver what the user asked, do not substitute something close and present it as the answer.

### ALWAYS

- **Hard evidence only.** Every number traces to a data file, fit output or script run in this session; every claim points to the figure or table that shows it. No file → not in the report.
- **Separate measured / fitted / derived / predicted / interpreted.** Say which one each statement is. Speculation is marked as speculation, in §5, never in §3.
- **Physical plausibility beats best score.** Lowest error with unphysical parameters → reported as the lower bound, not recommended.
- **Compare key fitted quantities to literature**, with the mismatch factor and ranked explanations (calibration, artefact, model form, new physics last).
- **State what the measurement cannot see.** Then "no evidence for X" is expected, not evidence of absence.
- **Shared physics is shared.** Parameters of an interface identical across samples are shared; per-sample freedom is flagged as absorbing model error.
- **Fixed / shared / free is explicit** for every parameter, with units and bounds.

### If the user asks for something the evidence cannot support

E.g. a fit at a thickness with no data, a conclusion the data does not reach, a "better" fit that needs unphysical parameters, a figure for a measurement that does not exist:
1. Say so plainly in chat and in §6: what was asked, why the evidence does not support it.
2. Offer what *can* be done (which measurement, which data, which test would decide it).
3. If a model prediction is still useful, give it clearly labelled **prediction, not data**, outside Findings.
4. Do not quietly fill the gap to satisfy the request.

## Writing style

- Lead sentence of each section = the result with its number.
- Specific numbers with units; sentences < 25 words; no unexplained jargon; every equation symbol defined.
- Same names for parameters and hypotheses everywhere (text, tables, legends).
- Figures: labelled axes with units, data as points, model as lines, caption = takeaway. Follow /dataviz.
- End with `Code:` naming the scripts that produce every table and figure.

## Self-check before handing over

- [ ] All six sections present, in order.
- [ ] Every §3 subsection maps to a §1 label and states *Changed vs H0* and *Assumed / fixed*.
- [ ] Every §3 subsection has the full figure set and parameter table, same layout and columns.
- [ ] Significant model change → this is a separate report.
- [ ] §4 recommends the physically sound fit, not only the lowest score, with caveats.
- [ ] §6 lists every interpretation of the user's ask.
- [ ] Every number traces to a file or script output; nothing made up.
- [ ] No extrapolation or interpolation beyond measured samples/ranges; predictions labelled as such, outside §3.
- [ ] Requests the evidence cannot support are flagged in §6, not quietly filled.
