# Common AI recommendation audit

Respond in at most five short lines to the recommendation quoted in the brief.
Accept/reject, methodological reason, and a precise checkable cell/function/output in this repository:

[Camilo: completar]

## Truthful use / no-use record

Tool and use, or “No AI used for this lab”:
Claude (Anthropic), used by Valentín Castro as a support tool. Valentín made the experimental decisions:
he chose the additional candidate (`forest_sqrt`, `min_samples_leaf` 30 → 100) among the three allowed options,
and set its expected effect (dev event recall rises slightly) before the first run. With Claude's help he recorded
that spec, ran the notebook, and drafted sections 1, 2 and 4 and the README environment steps; he reviewed the
drafts against the outputs and is responsible for the final content.
[Camilo: añadir tu uso de IA o “No AI used” para tu parte]

If used, one actual suggestion (quote or faithful summary):
Claude suggested explaining the candidate's gain through larger leaves averaging over bigger groups of training laps,
and adding a descriptive cell that counts leaves per tree in the fitted parent and candidate.

What we accepted/rejected:
Accepted the diagnostic cell (descriptive only, no refit or threshold change). The "less overfitting" explanation is kept
only as a hypothesis in section 4, because the leaf count is observed but the effect on 2022 generalisation was not measured.

Exact check performed and repository evidence:
The numbers in section 4 (159/370 vs 140/370 event recall, 376/938 vs 340/945 precision, thresholds 0.602 vs 0.608)
were checked against the `forest_sqrt` and `forest_sqrt__min_samples_leaf_100` dev rows in `comparison_table.md` /
`outputs/comparison_v3.csv`. The leaf counts (32.3 vs 40.6 per tree) come from the diagnostic cell in section 4.

One limitation relevant to this work:
The assistant's mechanism explanation is plausible but untested (single seed, dev also used for threshold selection).
Our own expectation that train recall would fall did not hold (0.520 → 0.522), and this is reported as such.

Do not invent an error or interaction. No minimum number of prompts is required.

## Contributions

- Contributor 1 (Valentín Castro), concrete action and related file/cell: decided the one-change tuple and its expectation (`experiment_spec.md`), configured and ran the candidate (cell under “3. Record your expectation…”), produced sections 1, 2 and 4 with Claude's assistance and verified their numbers against `comparison_table.md`, and ran the final Restart + Run All.
- Contributor 2 (Camilo Bufadel), concrete action and related file/cell: [Camilo: completar, p. ej. sección 5 “Your three entries”, `memo.md`, auditoría común]
