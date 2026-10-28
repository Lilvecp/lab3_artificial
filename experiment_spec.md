# Expectation for one additional candidate

The five base results are supplied. This expectation concerns YOUR additional run.

- Base model: `forest_sqrt` (RandomForestClassifier, 150 trees, max_features='sqrt', max_depth=6, class_weight='balanced_subsample', seed 414).
- Parameter and old → new value (one allowed tuple): `min_samples_leaf` 30 → 100.
- Conditions held fixed: same CSV (`data/learning_v2.csv`, unchanged), train 2018–2021 / dev first eleven rounds of 2022, the nine permitted inputs in `FEATURES`, preprocessing fitted on train only, seed 414, all other forest settings (n_estimators=150, max_depth=6, max_features='sqrt', class_weight), and the same dev threshold policy (≤10% of dev opportunities alert; maximise distinct eligible entries caught; precision then threshold as tie-breakers). The threshold is reselected on dev by that policy and may differ numerically from the parent's 0.608283.
- Have you already run this additional candidate? No. This expectation was written before the first run of the candidate. The five base systems were only read from the supplied outputs.
- Expected effect and mechanism, written before the first additional run if it has not happened: We expect dev event recall to rise slightly above the parent's 140/370 = 0.378. Mechanism: requiring at least 100 training windows per leaf stops trees from isolating small groups of laps, so scores become smoother averages over larger groups. This should reduce in-sample fit (train event recall below the parent's 0.520, smaller train/dev gap) and may generalise a little better to 2022, which would let the same 10% budget reach a few more distinct entries. We expect any change to be small, and it could be offset by a different selected threshold.
- Other trials already made, if any, and how the final candidate was chosen: None. Valentín chose this tuple among the three allowed options because the parent forest_sqrt was the strongest base system on dev and its train/dev gap (event recall 0.520 → 0.378) suggested in-sample overfitting worth testing with larger leaves. No other configuration was run.

Keep this record after running, even if wrong. Explain actual results in “4. Explain your additional result”.
Do not rewrite an expectation as if it predicted a result already seen.
