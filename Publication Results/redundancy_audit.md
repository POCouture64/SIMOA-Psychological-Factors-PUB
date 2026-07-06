# Streamlining Audit — Thesis_Streamlined.qmd

All line numbers refer to `Publication Results/Thesis_Streamlined.qmd` as it currently stands (Diagnostics/#8 already removed). Nothing has been edited yet — this is the full catalog from three parallel audits, consolidated and organized by confidence.

Legend:
- 🟢 **Tier 1 — Clear-cut, safe to delete.** Dead/commented code, exact duplicates, or scratch snippets that would error if run.
- 🟡 **Tier 2 — Consolidate.** Genuinely re-checks something already established; safe to merge into one occurrence.
- 🟠 **Tier 3 — Needs your confirmation.** Might feed a manuscript claim or downstream file; don't want to guess.
- 🔴 **Tier 4 — Flagged bug, not a redundancy.** Needs a decision, not a deletion.
- ⚪ **Explicit KEEP** — flagged only so you know it was considered and ruled load-bearing.

---

## Load Data
- 🟢 L17 `#View(SIMOA)` — dead commented-out line. Delete.

## Eligibility
- 🟡 L74–98 — "SAMPLE INCLUSION/EXCLUSION SUMMARY" and "PARTICIPANT FLOW" print the same 5 filtering counts twice in different formats. Consolidate into one (recommend keeping the CONSORT-flow version).
- 🟢 L100–106 — `pct_excluded_total` warning, console-only, nothing downstream reads it. Drop or fold into the summary above.
- 🟢 L140–142 — closing "filtering complete" banner, pure decoration. Drop.

## Personality Missingness
- 🟡 L420–474 (STEP 5 "Overall Summary") — re-synthesizes Steps 2–4 with no new computation. Consolidate to a short block.
- 🟡 L174–205 (STEP 0 "Validating dropout proxy") — diagnostic-only, conclusion already asserted in header comment L155–157. Shorten to a one-line assertion.
- ⚪ KEEP L372–382 — `tryCatch` fallback in dropout model, active control flow.
- Structural note: this whole section's output (`dropout_before_personality` etc.) is never used again downstream — it's a self-contained side-report, not pipeline-critical. Not a defect, just worth knowing.

## Variable Coalescing, Preparation, Selection
- 🟢 L819–835 ("POST-OVERWRITE DIAGNOSTIC") — fully duplicates prints already done in the loop right above it (L799–817). Nothing downstream reads it. Delete.
- 🟢 L525–541 ("PRE-FLIGHT CHECK") — recomputes counts redone later at L761–781; superseded by the real gate at L784–794. Delete.
- 🟡 L1742–1748 — second identical v2-leak check, duplicates L838–845. Keep the later one (checks the saved analysis set), drop this one.
- 🟡 L1842–1859 ("COLUMN INDEX") — same info as `var_inventory` print at L1830–1837, just reformatted. Consolidate.
- 🟡 L1926–1953 ("KEY DECISIONS RECORDED") — restates the header comment block (L562–597). Trim substantially.
- 🟡 L761–781 (pre-overwrite diagnostic print loop) — doesn't feed the actual gate (`v2_stopper_data_ok`, computed independently). Drop the table, keep the gate.
- ⚪ KEEP L784–794 — `stop()` gate on missing v2 stopper data. Active control flow.
- 🟡 L996–1008, L1351–1387 — pure descriptive sanity-check prints, nothing branches on them. Lower priority trims.

## Pre-Imputation
- 🟡 L3374–3467 ("PART H: FINAL SUMMARY") — re-prints m/maxit/method breakdown already shown earlier in the *same* chunk (L2802–2809, L3006–3018, L3110–3117, L3251–3269). Trim to a short recap.
- 🟡 L2039–2076 and L2621–2633 — `stop_wdl_exp_combined`-is-unordered gets checked 3 times total across the doc (creation in Variable Coalescing + these two). Consolidate to one.
- 🟡 L2324–2330 vs L2429–2434 — duplicate VIF explanatory paragraph. Consolidate.
- 🟡 L2528–2557 vs L2802–2809 — duplicate method-tally report. Consolidate.
- ⚪ KEEP L2835–3000 ("PART D5... PROBE 1/2") — despite the "PROBE" label, this **mutates `pred`/`method`** (zeroes predictors, switches polr→pmm), and those mutated objects feed the real imputation. Not scratch — load-bearing.
- ⚪ KEEP L3020–3134 (Von Hippel `m` derivation) and L3136–3269 (Gelman-Rubin `maxit` experiment) — real pilot `mice()` runs whose outputs are saved and consumed downstream.
- ⚪ KEEP L2036, L2095–2098 — file/data `stop()` gates.

## Imputation
- 🟡 L4445–4586 ("PART H: FINAL SUMMARY") — re-prints imputation parameters (dup of L3581–3603) and D1–D7 pass/fail status already printed inline. Trim.
- 🟡 L3564–3571 vs L3581–3603 — duplicate parameter print, ~15 lines apart. Consolidate.
- 🟢 L3699–3701 — restates prior section's expectation, adds nothing. Trim.
- ⚪ KEEP L3703–3773 — Gelman-Rubin PSRF on the *final* m=24 run. Looks like a copy of the Pre-Imputation pilot check but verifies a different (final) object — legitimate. (Could still be refactored into a shared helper function rather than left duplicated as code — your call.)
- ⚪ KEEP L3864–4292 (D1–D7 QA suite) — each check is distinct, feeds its own CSV referenced in manuscript reporting, and verifies the real final artifact (Pre-Imputation only ever checked pilot runs).
- ⚪ KEEP L3543–3546 — file-existence gate.

## Post-Imputation
- 🟡 L4712–4789 (early carry-through/polr-polyreg verification) — re-verified more rigorously later in the same section at L5467–5533 (Part D7) and L5562–5606 (Part D8). Consolidate into D7/D8, drop the early copy.
- 🟢 L5636–5736 ("FINAL SUMMARY", ~100 lines) — re-prints subscale/composite counts, outcome breakdown, and reliability overview all already printed earlier in this chunk. Trim to a one-line completion banner.
- 🟡 L5535–5559 — duplicate `stopped_bzra` consistency check, identical to Part B3 (L5209–5220). Consolidate.
- 🟢 L5675–5691 — restates the composite list already shown at L5116–5129. Drop.
- ⚪ KEEP L4688–4710 (`stop()` gate on reversed-item presence), L5310–5465 (Part D1–D6, the only cross-**all**-m validation), L4878–4932/L5230–5298 (`psych::alpha` reliability — substantive result).

## Descriptive Stats
- 🟢 L6121–6166 — tautological checks against a hardcoded vector defined a few lines above; can only fail if someone typos the literal list. Remove.
- 🟢 L6828–6835 ("CHANGES FROM PREVIOUS VERSION") — verbatim restatement of the header comment (L5749–5764). Remove.
- 🟡 L5846–5878 — re-verifies + reprints `stopped_bzra`/`freq_dich` distribution already shown in Post-Imputation. Keep the `stop()` gates (L5849–5853), drop the reprint.
- 🟠 **Needs your confirmation:** L6022–6043 (sleep-aid sparsity diagnostic) — reads exploratory, but a manuscript text snippet elsewhere (L6799–6802/L8800–8802) cites "sparsity was assessed prior to analysis." If that decision is final and won't change, this can go; otherwise it's the only audit trail for that claim. **Is the sparsity/collapse decision final?**

## PERS VSURF
- 🟢 L6955–7051 (`recode_dataset()`) — computes variables never used in this chunk (only `personality_vars` feeds VSURF). Also contains **stale/buggy encodings** for `stop_wdl_exp_combined` and `fall2` (old level scheme, superseded elsewhere). Currently inert only because unused — **delete outright, do not merge/promote when consolidating with the correct version** in FULL VSURF/Descriptive Stats.
- 🟢 L7079–7088 — third occurrence of the deterministic `stopped_bzra` consistency check. Remove.
- 🟡 L7266–7271 (`count_selections()`) — exact duplicate of FULL VSURF L8158–8163. Hoist to one shared helper.
- Structural: PERS VSURF and FULL VSURF are near-identical in banner/reporting structure — candidate for a shared `run_vsurf_mi()` helper (bigger refactor, flagging for awareness not immediate action).
- ⚪ KEEP `stop()` gates (L6943–6947, L7128–7136), EPV check (L7151–7169), the VSURF loop/majority-vote itself.

## FULL VSURF
- 🟡 L7722–7836 (`recode_dataset()`, the *correct* version) — triplicated with Descriptive Stats' version (L5895–6013) and the stale PERS VSURF copy above. This is the "real" one but has an extra Likert-coercion block (L7804–7819) not present in Descriptive Stats' copy — reconcile differences, then define once and reuse.
- 🟡 L8317–8358 vs L8534–8580 — near-identical "top 20" vs "all variables" importance plots, differ only in `slice_head(n=20)` and text size. Consolidate into one parameterized plotting function.
- 🟡 L8158–8163 — see PERS VSURF duplicate above.
- 🔴 **Flagged bug:** L7688–7692 — `if (m != 24) warning(...)` hardcodes 24 even though the adjacent comment (L7665–7667, L7688–7690) explicitly says this was fixed to **not** hardcode it. Self-contradicting. Needs a decision (drop the check, or intentionally keep the literal 24), not a mechanical dedup.
- ⚪ KEEP pre-loop NA `stop()` gate (L8009–8036), both EPV checks (different variable sets), 11A-comparison block.

## LR
- 🟢 L8836–8846 — standardization spot-check print, eyeballs a 2-line deterministic formula. Remove.
- 🟠 **Needs your confirmation:** L8953–9002 (raw-vs-standardized coefficient comparison tables, boilerplate "note" column) — written out to `regression_comparison_A/B_raw_vs_std.csv`. **Do you cite these CSVs as a supplementary table anywhere?** If not, safe to drop.
- ⚪ KEEP Part I (VIF + Cook's distance, L9164–9200), DeLong's test (L9078–9154), AUC+bootstrap CI (L9017–9066), EPV check (L8758–8776).

## Additional tests
- 🟡 L9838–9899 vs L9993–10019 — same R²/HL/AUC/threshold/sens/spec/PPV/NPV numbers printed twice in different formats. Consolidate.
- 🟡 L9947–9956 vs L10027–10036 — identical reference list, once to saved metadata (useful) once to console (redundant). Drop console copy.
- 🟢 L10021–10025 ("FILES CREATED") — pure console output. Trim/merge into final summary.
- ⚪ KEEP Parts C–G (R², HL, confusion matrix, Box-Tidwell, imbalance) — reported statistical results.

## Cross-Validation
- 🟡 L10454–10475 vs L10529–10570 — apparent/CV/optimism numbers printed twice. Consolidate.
- 🟢 L10557–10566 ("NOTES") — restates methodology already in the header comment (L10046–10076). Trim.
- ⚪ KEEP the core CV loop, optimism calc, fold-stability table.

## Influential Case
- 🟢 **L10578–10584** — leftover interactive debug snippet (`case_350 <- SIMOA[SIMOA$record_id == 350, ]` + clonazepam column grep), sitting before the chunk's own header comment even starts. Confirmed `SIMOA` isn't reloaded in this chunk (it loads `.rds` files instead) — **this would error on a clean run** and nothing downstream uses its output. Highest-confidence single deletion in the whole document.
- 🟡 `pool_glm_manual()` defined at L10840–10891, then **redefined with a different signature** in Subgroup Check (L12457–12516). Consolidate into one shared helper.
- 🟢 L11876–11938 (closing "CHUNK 14 COMPLETE") — restates Part J's summary_table (L11807–11826). Trim.
- 🟠 **Needs your judgment:** L11553–11605 ("Extreme case profiling," cases >10× threshold) — has real diagnostic value (flags possible data-entry errors) but is verbose/borderline exploratory. Your call whether to keep, trim, or cut.
- ⚪ KEEP Cook's D computation/threshold flagging, the 4-group overlap partition, Part G removal-sensitivity robustness classification, Part I overlap sensitivity checks — these gate actual reported conclusions.

## Subgroup Check
- 🟢 **Part E "Synthesis" (L12696–12744)** — pure `cat()` narrative with hardcoded outcome-rate percentages and predictor names not pulled from any computed object (`decomp_A`, `pmpp_A`, etc.). Reads like a pre-written expectation, not a result. High-confidence candidate to remove or rewrite to compute from the actual objects.
- 🟢 L12447–12454 (BACKGROUND) and L12662–12686 (D3 "Reporting recommendation") — D3 literally prints a "SUGGESTED REPORTING LANGUAGE" manuscript paragraph as script output; BACKGROUND hardcodes OR figures that Influential Case's `removal_comparison` already computed (will go stale on rerun). Rewrite to pull from the live objects, or relocate this prose out of the executable pipeline.
- 🟡 D1 (L12520–12600) and D2 (L12602–12659) do real new computation (saved to CSV) but repeat the same "remove subgroup, refit, compare OR" pattern already used in Influential Case Part I. Not a pure duplicate (different terms) — candidate to refactor into one shared removal-sensitivity helper, not delete.
- Note: L12778–12784 metadata comment ("Chunk 14b corrected to align with Chunk 14 actual results...") is evidence this section already went through an ad hoc correction pass — supports rewriting Part D/E to compute dynamically rather than trusting more hardcoded text.
- ⚪ KEEP Part B leverage/residual decomposition and Part C PMPP (L11991–12413) — the actual novel statistics this section exists to produce.

## Complete Case Analysis
- **Explicit assessment on Parts B–F (re-deriving subscales/composites/outcome):** this is **necessary, not redundant** — it operates on a genuinely different, non-imputed dataset (`SIMOA_analysis.rds`) than everywhere else in the document. Verdict: keep, with two carve-outs below.
- 🟢 L13166–13173 (`pharmacistC`, `prescriberC`) — computed but never referenced again anywhere in this file (confirmed via grep; not in Chunk 16's composite list, not in `cc_data`). Dead within this range. **Do you use these elsewhere / intend parity with the earlier pipeline?** If not, remove.
- 🟡 L13259–13270 vs L13279–13287 — same "all model vars present in raw_data" check run twice back-to-back (print loop, then `stop()` gate). Consolidate into one check that both prints and gates.
- 🟢 L13695–13715 ("SUGGESTED MANUSCRIPT TEXT") — pure author-facing prose, not analysis, doesn't feed any saved object. Candidate to relocate out of the pipeline (not necessarily delete the content).
- 🟢 L13770–13823 (closing "CHUNK 15 COMPLETE") — repeats numbers already printed in Parts J/K/N. Consolidate.
- ⚪ KEEP Part B reverse-coding *presence* check (L12969–12995, cheap `stop()` gate — different from re-verifying coding correctness, which lives in Variable Coalescing), Part G model fits, Part H coefficients, Part I AUC, Parts J/K/N CC-vs-MI comparison — the actual sensitivity-analysis conclusion.

## Correlation Matrix
- 🟢 **L13980–13996** — `"phq2_score"` is listed twice in `health_single`, with the *original author's own comment* admitting it: `# already listed above but harmless duplicate`, followed by `unique()` to paper over it. Zero-risk removal — delete the second entry and its comment.
- 🔴 **Flagged bug, not redundancy:** L13990 — `"bzra_daily_diazepam_eq"` in `health_single`. Complete Case Analysis's own documentation (L13746, L13814) says this variable was **renamed** to `bzra_daily_dose_eq` back in Variable Coalescing, and Complete Case's models use the new name. If the old column no longer exists in `raw_data`, it silently gets dropped as "absent" (L14056–14065) rather than erroring — meaning this correlation matrix may be silently missing an intended variable. **Needs your check, not a deletion.**
- 🟡 L14579–14634 (closing summary) — reprints variable-type/method-usage counts already shown in Parts D/F. Consolidate.
- ⚪ KEEP the environment-existence guard (L13931–13933) and the variable-type classifier/association dispatcher (Parts D–F) — load-bearing for the chunk's sole output.

---

## Cross-cutting, document-wide
- The `═══...═══` `cat()` banner scaffolding repeats verbatim hundreds of times across every section. Not a redundant *check* per se, but a single `banner(title)` helper could replace all of it with zero behavior change — pure line-count reduction, low risk, done last as a style pass if you want it.
- Three near-identical copies of `recode_dataset()` exist (Descriptive Stats — correct; PERS VSURF — stale/buggy/unused; FULL VSURF — correct + extra Likert block). Recommend: define once (based on FULL VSURF's version, reconciled with Descriptive Stats'), delete the stale PERS VSURF copy outright, reuse everywhere else.
- Two versions of `pool_glm_manual()` (Influential Case vs Subgroup Check) — consolidate to one.
- `count_selections()` duplicated between PERS VSURF and FULL VSURF — consolidate to one.

## Everything flagged 🔴 (bugs, need your decision, not part of "batch delete")
1. FULL VSURF L7688–7692 — hardcoded `m != 24` contradicts its own "no longer hardcoded" comment.
2. Correlation Matrix L13990 — possibly-stale variable name (`bzra_daily_diazepam_eq`) that may silently drop out of the matrix.
3. PERS VSURF's stale `recode_dataset()` — must not be the version that gets promoted when consolidating duplicates.

## Everything flagged 🟠 (needs a factual answer from you before deleting)
1. Descriptive Stats L6022–6043 — is the sleep-aid sparsity/collapse decision final?
2. LR L8953–9002 — are the raw-vs-standardized comparison CSVs used/cited anywhere?
3. Complete Case L13166–13173 — is `pharmacistC`/`prescriberC` intentionally kept for parity, or dead?
4. Influential Case L11553–11605 — keep, trim, or cut the extreme-case profiling narrative?
