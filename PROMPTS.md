# Common AI recommendation audit

Respond in at most five short lines to the recommendation quoted in the brief.
Accept/reject, methodological reason, and a precise checkable cell/function/output in this repository:

Reject: choosing the forest setting and threshold on 2026 makes 2026 part of selection, so its score is optimistic, not independent.
Independence means the data drove no decision; distance from the 2021 training cut-off does not provide it.
Our threshold is chosen on dev only and reused unchanged: `evaluate_scores` → `choose_threshold(dev_scores, dev, budget=BUDGET)` in `lab03_support_v3.py`.
Recorded as `"threshold_selection": "dev only; reused unchanged on train"` in `outputs/run_manifest_v3.json`.
A real final test is a later period scored once with model and threshold frozen (section 4, "Bounded recommendation").

## Truthful use / no-use record

Tool and use, or “No AI used for this lab”:
Claude (Anthropic), used by Valentín Castro as a support tool. Valentín made the experimental decisions:
he chose the additional candidate (`forest_sqrt`, `min_samples_leaf` 30 → 100) among the three allowed options,
and set its expected effect (dev event recall rises slightly) before the first run. With Claude's help he recorded
that spec, ran the notebook, and drafted sections 1, 2 and 4 and the README environment steps; he reviewed the
drafts against the outputs and is responsible for the final content.
Camilo Bufadel used Claude Code (Anthropic, Opus 5.5). He first asked it to explain in detail how the lab is built so he could understand how to do it ("Explicame detalladamente como esta hecho este laboratorio, para poder entender como realizarlo"). He then asked it to review what he had and propose improvements if needed ("revisa lo que tengo y si es necesario, puedes proponer mejoras"). Camilo did the work himself: he ran the notebook in a fresh Python 3.12 environment, explored the frozen dev predictions, wrote the section 5 query cells and the three entries, wrote `memo.md` and this audit, and updated the README run notes. Claude only explained the lab structure and then reviewed his work and proposed improvements. Camilo decided which suggestions to apply and is responsible for the final content.

If used, one actual suggestion (quote or faithful summary):
Claude suggested explaining the candidate's gain through larger leaves averaging over bigger groups of training laps,
and adding a descriptive cell that counts leaves per tree in the fitted parent and candidate.
Camilo's part: Camilo chose the three section 5 patterns himself (HARD false alerts, fresh-tyre misses, circuit 15 misses); he first considered circuit 19 (38/42 entries missed) as the third pattern. In its review, Claude suggested justifying the choice of circuit 15 better (faithful summary; Camilo does not remember the exact wording).

What we accepted/rejected:
Accepted the diagnostic cell (descriptive only, no refit or threshold change). The "less overfitting" explanation is kept
only as a hypothesis in section 4, because the leaf count is observed but the effect on 2022 generalisation was not measured.
Camilo's part: accepted the suggestion to justify circuit 15 better; the justification is the following. Circuit 19 was not used as a separate pattern, because 21 of the 49 fresh-tyre entries are in that race and it would mostly repeat pattern 2. Circuit 15 shares only 1 missed entry with pattern 2. No circuit-specific threshold is proposed, since that would be a new selection on dev. The proposed causes are labelled as hypotheses.

Exact check performed and repository evidence:
The numbers in section 4 (159/370 vs 140/370 event recall, 376/938 vs 340/945 precision, thresholds 0.602 vs 0.608)
were checked against the `forest_sqrt` and `forest_sqrt__min_samples_leaf_100` dev rows in `comparison_table.md` /
`outputs/comparison_v3.csv`. The leaf counts (32.3 vs 40.6 per tree) come from the diagnostic cell in section 4.
Camilo's part: the first circuit 15 query used `CONTEXT_VALUE = 15` and returned 0 rows, because `circuit_id` is stored as text. The empty result was not treated as evidence; it was changed to `'15'` and rerun (159 positive windows, 140 FN). Every count in "Your three entries" and in `memo.md` was recomputed by the section 5 cells after Restart + Run All. The missed events cited were checked with `positive_windows_for_event` (3 positive windows, 0 alerts each).

One limitation relevant to this work:
The assistant's mechanism explanation is plausible but untested (single seed, dev also used for threshold selection).
Our own expectation that train recall would fall did not hold (0.520 → 0.522), and this is reported as such.
Camilo's part: the patterns come from one dev run with one seed, and the subgroups are small (274 alerts, 49 and 53 events). The fresh-tyre and circuit 15 explanations are not measured. The rerun on Python 3.12 changed only `forest_all` AP in the 6th decimal; alerts and error types were identical.

Do not invent an error or interaction. No minimum number of prompts is required.

## Contributions

- Contributor 1 (Valentín Castro), concrete action and related file/cell: decided the one-change tuple and its expectation (`experiment_spec.md`), configured and ran the candidate (cell under “3. Record your expectation…”), produced sections 1, 2 and 4 with Claude's assistance and verified their numbers against `comparison_table.md`, and ran the final Restart + Run All.
- Contributor 2 (Camilo Bufadel), concrete action and related file/cell: section 5, i.e. the query cells (HARD FP, `tyre_life <= 5`, circuit 15 FN, whole-event checks) and the "Your three entries" cell; `memo.md`; the common audit in this file; and the final Restart + Run All on Python 3.12 with README run notes. Claude Code only reviewed this work and proposed improvements (see record above).
