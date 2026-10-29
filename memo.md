# Recommendation to the Head of Strategy: rival pit-entry alert

**Decision it could support.** At the end of each lap, the alert flags a rival who may enter the pits within their next three laps. The pit wall could use it to choose which rivals to watch closely, e.g. to prepare a cover or undercut response. It does not say when our own car should stop.

**What we tested.** We trained on 2018–2021 races and compared six alert systems on the first eleven races of 2022. Every system had the same limit: it may raise an alert on at most 10% of rival lap-ends (945 of 9,451). We count a pit entry as "caught" if at least one alert fired in the three laps before it.

| System | Pit entries caught | Alerts raised | Alerts followed by an entry |
|---|---|---|---|
| Simple history rule | 94 of 370 (25%) | 931 | 250 (27%) |
| Original forest model | 140 of 370 (38%) | 945 | 340 (36%) |
| **Adjusted forest (larger leaves)** | **159 of 370 (43%)** | **938** | **376 (40%)** |

The adjusted model bases each prediction on bigger groups of similar past laps. It caught 19 more entries than the original with slightly fewer alerts, the best of the six.

**Where it fails.**
- *Old hard tyres.* 230 of its 274 alerts on hard tyres (84%) were not followed by an entry within three laps. In every one of those cases the rival did stop later, typically about 7 laps after the alert. These alerts are early rather than wrong about the rival, but they use almost a third of the alert budget.
- *Fresh tyres.* For 49 of the 370 entries the rival's tyres were at most five laps old (for example a second stop soon after the first). The alert caught none of them.
- *One circuit.* At circuit 15 (22 May race) it caught only 12 of 53 entries.

**Recommendation: check further before use.** We do not recommend relying on it during races yet. It still misses 211 of 370 entries, and 6 of 10 alerts are not followed by a stop within three laps. The 43% figure is also likely optimistic. We used these same 2022 races to set the alert level and to pick the winning model, so they are not an independent test. The model's score is a ranking, not a reliable probability. We have not measured any gain in positions or race time.

**Next check.** Freeze the adjusted model and its alert level exactly as they are. Then measure it once on later races it has never seen, and repeat the training with a few different random seeds. That will show whether the 19-entry gain is real or just chance. Separately, tag the 49 fresh-tyre entries by reason (weather, damage, safety car) to see whether any lap-end information could anticipate them.
