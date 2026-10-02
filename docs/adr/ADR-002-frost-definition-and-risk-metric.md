# ADR-002-frost-definition-and-risk-metric

## Status
Proposed

## Context
The report needs a frost-day definition and a canton risk metric. The
prototype (apiary-spring-creep) counted days with air minimum ≤ 0 °C in
April–June, summed them over stations and divided by colonies. UC-001
explored 13 MeteoSwiss stations (1981–2026; 6 long records since 1901),
MeteoSwiss phenology (cherry flowering, 11 matched stations) and Agroscope
colony counts (2022); see `notebooks/uc-001-explore-frost-risk.ipynb`,
section 11, revised after review. Key findings:

- Harm mechanisms differ: **forage loss** (frost on open blossom) and
  **brood chilling** (cold spells). The analysis covers forage loss only.
- Blossom damage depends on bud stage: 10 % kill at −2.2 °C applies only
  from about first bloom; earlier stages tolerate −2.8 to −9.4 °C (WSU
  EB0913/EB1128). Most low-elevation frost days fall in March, before bloom.
- The ETCCDI growing-season start is not a vegetation signal (r = 0.47 with
  observed cherry flowering, median 40 days earlier, 16.7-day error). A
  thermal sum (GDD > 5 °C reaching 136) predicts flowering within 5.7 days
  (leave-one-station-out).
- Frost on open blossom (≤ −2.2 °C after modelled flowering) is rare: 0–0.37
  events per spring, 0–17 % of springs per station.
- Frost days declined since 1901 (F1 −0.71, F2 −0.40 per decade). Frost after
  the ETCCDI start shows no trend since 1901; its rise since 1981 is partly
  definitional (the start moved earlier faster than the last frost). Against
  flowering there is no detectable change (gap +0.14 days per decade since
  1901, CI −0.41 to 0.71; share of springs 6.4 % → 10.2 %, p = 0.11; with
  observed flowering 8.8 % → 8.2 %, p = 0.87).
- Canton values are thin: 5 of 9 sample cantons rest on one station; AG, LU
  and FR (19 % of colonies) are missing; canton probability intervals
  overlap widely. Threshold-only definitions depend strongly on station
  elevation (GR 21.5 with Davos, 4.1 without); the flowering-based one much
  less.

## Alternatives
Frost definitions compared (March–June, 2 m air unless stated):

- F1 ≤ 0 °C (prototype): no damage link; dominated by pre-bloom frost.
- F2 ≤ −2.2 °C: bloom threshold applied to hardy pre-bloom buds; overcounts.
- F3 ground (5 cm) ≤ 0 °C: 2.6–5.3× more events than air; no documented
  threshold.
- F4/F5 ≤ 0 °C / ≤ −2.2 °C after the ETCCDI start: timing proxy fails
  validation; F5 mixes a bloom threshold with February timing.
- F6 ≤ −2.2 °C on or after thermal-sum cherry flowering: stage-consistent for
  forage loss; validated timing; rare events; conservative (50 % flowering
  is after first bloom); one species; possibly underestimated at altitude.

Metrics: M0 (events summed over stations ÷ colonies) scales with stations ÷
colonies, not with frost. M1 (mean events per station-year), M2 (share of
springs with an event) and M3 (M1 × colonies) are the remaining options.

## Decision
Pending the owner's decision. Options to decide:

1. **Mechanism:** forage loss only, or also brood chilling (needs its own
   cold-spell definition and exploration).
2. **Frost definition:** F6, optionally with stage-dependent thresholds
   before bloom; or F2 as a simpler calendar alternative.
3. **Metric:** hazard (M1, or M2 for rare events) with an uncertainty
   interval, and exposure (M3) reported separately; M0 dropped.
4. **Aggregation:** station weighting within cantons; elevation cap or not
   (assumption: most apiaries below 1000 m, unverified; trade-off: removes
   high-hazard areas); stations needed before publishing a canton ranking
   (more per canton, and AG, LU, FR).
5. **Reference period:** e.g. 1991–2020 (WMO climate normal) or all years
   with flowering data.

Accepted by: pending (owner).

## Consequences
- Depends on the options chosen; to be completed on acceptance.
- Data limits apply to every option: non-homogenised station series, one
  year of colony counts per canton without apiary locations, Agroscope
  figures © Agroscope (republishing derived figures to be confirmed).
- UC-002 implements the accepted definition and metric as domain rules
  (pure Python, test-first).

## Supersedes
none
