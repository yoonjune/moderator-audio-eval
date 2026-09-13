# GPT-Live 10-case pilot 결과

> 판정은 `EVAL_SETTING.md`를 기준으로 한 provisional 결과다. Semantic review는
> fresh blinded `gpt-5.6-luna`가 수행했고, `human_gold: false`다.

## 범위와 결론

- 평가 상태: `PROVISIONAL_COMPLETE`; primary unit: `removed_moderator_utterance`; primary N: **74**.
- 선택 case 수: 10; registered gap: 71; semantic adjudication: 1개.
- Baselines: unavailable / not run. 이 pilot 결과를 기존 baseline 대비 개선으로 해석하지 않는다.
- 이전 `c31843` source-preparation 시도와 그 recovery artifact는 source provenance 불일치로 제외했다.

## Provenance

- source checkout: `2b69adb7a376d834a6918184d53fa8d597d51724`; data last change: `e6b394c`; git ref: `renewal-30`.
- source blob verification: `PASS` (20/20 files); selected: L107, L033, L079, L075, L040, L073, L092, L034, L057, L002.
- plan SHA-256: `9a2c60b9ed2189bfbc9f455e62958b8c004e4b5747cc3c6951828c6756e38331`; scaffold SHA-256: `0a8622c2a63c5a89540975dc7d1edc56a1db1ee17e75207e799465f1d0c3dec5`; contract SHA-256: `015846b356f4120796c07dcd39579e799bb79a8d383c02161e79ba3a39e6578a`; anchor decisions SHA-256: `12b27c8de71951b7aca458ba815ee6d061d8f3f6e34c514ebf71e72f6cad7793`.
- inference profile: `gpt-live-gap-only-1000ms-cap2s-v1`; adapter: `skill://scripts/adapters/gpt_live_gap_only.py`; adapter SHA-256: `1fe417ae0869ee511fee4e014cb2b9dd3bbbb2e2eefb671cae4bc02be4441dd2`.
- inference provider/model: `gpt-live` / `gpt-live-1`; semantic reviewer: `gpt-5.6-luna`; `human_gold: false`.
- adjudication provenance: 1 fresh blinded Luna file(s): `/Users/june/Workspace/study/reports/gpt_live_gap_only_cap2s_10_20260913_latest_network_recovery/nontrigger_adjudication.json` (6 overridden ID(s)).

## Primary denominator

Primary N은 각 case의 `gaps[].mod_turn_ids` unique moderator utterance 수다. Gap 수나 고정 action 수를 denominator로 사용하지 않았다.

| case | primary N | registered gaps | timing | content mean | joint mean | non-trigger N |
| --- | --- | --- | --- | --- | --- | --- |
| L107 | 6 | 6 | MISSED=1, ON_TIME=3, PREMATURE=2 | 0.333 | 0.333 | 3 |
| L033 | 6 | 6 | LATE=1, MISSED=3, ON_TIME=1, PREMATURE=1 | 0.250 | 0.167 | 7 |
| L079 | 7 | 7 | LATE=2, MISSED=2, ON_TIME=3 | 0.429 | 0.286 | 3 |
| L075 | 7 | 7 | LATE=2, MISSED=3, ON_TIME=2 | 0.071 | 0.000 | 4 |
| L040 | 9 | 8 | LATE=5, MISSED=3, ON_TIME=1 | 0.444 | 0.111 | 3 |
| L073 | 9 | 9 | LATE=2, MISSED=2, ON_TIME=2, PREMATURE=3 | 0.500 | 0.222 | 4 |
| L092 | 9 | 9 | LATE=4, MISSED=2, ON_TIME=2, PREMATURE=1 | 0.333 | 0.111 | 6 |
| L034 | 7 | 6 | LATE=4, MISSED=1, ON_TIME=2 | 0.286 | 0.143 | 7 |
| L057 | 7 | 7 | LATE=2, MISSED=1, ON_TIME=1, PREMATURE=3 | 0.714 | 0.143 | 11 |
| L002 | 7 | 6 | LATE=2, MISSED=1, ON_TIME=1, PREMATURE=3 | 0.143 | 0.000 | 7 |

## 전체 및 code별 결과

- Timing aggregate: LATE=24, MISSED=19, ON_TIME=18, PREMATURE=13.
- Content mean (known): `0.358`; joint mean (known): `0.149`.
- `joint_score`는 timing이 `ON_TIME`인 경우에만 content score를 반영한다.

| code | N | timing | content mean | joint mean | half-credit N | misread N | predicted-action mapping |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A1 | 3 | LATE=2, PREMATURE=1 | 0.667 | 0.000 | 0 | 1 | UNKNOWN=3 |
| A2-1 | 6 | LATE=4, PREMATURE=2 | 0.417 | 0.000 | 1 | 4 | UNKNOWN=6 |
| A2-2 | 14 | LATE=4, MISSED=2, ON_TIME=4, PREMATURE=4 | 0.500 | 0.143 | 0 | 5 | UNKNOWN=12 |
| A3-1 | 10 | LATE=2, MISSED=1, ON_TIME=4, PREMATURE=3 | 0.450 | 0.300 | 1 | 5 | UNKNOWN=9 |
| A3-2 | 10 | LATE=4, MISSED=2, ON_TIME=3, PREMATURE=1 | 0.200 | 0.100 | 0 | 6 | UNKNOWN=8 |
| A4 | 15 | MISSED=12, ON_TIME=1, PREMATURE=2 | 0.200 | 0.067 | 0 | 0 | UNKNOWN=3 |
| A4xf | 2 | LATE=1, MISSED=1 | 0.000 | 0.000 | 0 | 1 | UNKNOWN=1 |
| A5 | 4 | LATE=3, MISSED=1 | 0.250 | 0.000 | 0 | 2 | UNKNOWN=3 |
| B1 | 4 | LATE=2, ON_TIME=2 | 0.500 | 0.500 | 0 | 2 | UNKNOWN=4 |
| B2 | 6 | LATE=2, ON_TIME=4 | 0.417 | 0.333 | 1 | 4 | UNKNOWN=6 |

### Half-credit

| code | gt_id | utterance | met | missing |
| --- | --- | --- | --- | --- |
| A2-1 | L033:mod:4 | L033:utt:2 | stops the speaker for time | hands the floor to the other side (name, side, or 'next') |
| A3-1 | L073:mod:6 | L073:utt:2 | announces that the debate moves on to the next round (any wording) | states the length (two and a half minutes) |
| B2 | L075:mod:20 | L075:utt:7 | asks the speaker to reconcile it | points out that the speaker contradicted themselves |

### Misread

`misread`는 judge가 line이 실제로 한 일을 설명한 free text이며, predicted action으로 자동 변환하지 않았다.

| code | N | misread |
| --- | --- | --- |
| A1 | 1 | It transitioned to crossfire without explicitly stopping the over-time speaker. |
| A2-1 | 1 | It issued only a mistimed warning. |
| A2-1 | 1 | It misread the over-time condition as a ten-second warning. |
| A2-1 | 1 | It stopped Larry but omitted the required handoff. |
| A2-1 | 1 | It treated the over-time opening as a warning point rather than stopping and handing off. |
| A2-2 | 1 | It acknowledged both speakers without making the required handoff. |
| A2-2 | 1 | It acknowledged the speaker but omitted the handoff. |
| A2-2 | 1 | It gave a time cue instead of calling the next speaker. |
| A2-2 | 1 | It issued a time cue rather than the required handoff. |
| A2-2 | 1 | It issued a time warning rather than calling Rod or the other side. |
| A3-1 | 1 | It begins an incomplete time transition without naming the next round or its length. |
| A3-1 | 1 | It begins the crossfire announcement but omits its duration. |
| A3-1 | 1 | It repeats a ten-second warning after the openings instead of opening crossfire. |
| A3-1 | 1 | It starts an incomplete transition after saying time. |
| A3-1 | 1 | It stops the speaker but omits the required crossfire announcement and duration. |
| A3-2 | 1 | It gave a time warning at the round transition instead of opening closings. |
| A3-2 | 2 | It gave a time warning instead of opening closings. |
| A3-2 | 1 | It repeats an incomplete time cue instead of opening closings. |
| A3-2 | 2 | It stopped without opening the closing round. |
| A4xf | 1 | It acknowledged the speaker instead of giving the required warning. |
| A5 | 1 | It acknowledged the interruption instead of restraining it. |
| A5 | 1 | It announced closings rather than restraining the interrupter. |
| B1 | 1 | It gave a timing cue instead of a motion redirect. |
| B1 | 1 | It treated the off-motion speech as a timing event. |
| B2 | 1 | It asks for reconciliation but fails to clearly identify the contradiction. |
| B2 | 1 | It gave a time warning instead of addressing the contradiction. |
| B2 | 1 | It moved to closings instead of addressing the contradiction. |
| B2 | 1 | It treated the contradiction as a timing event. |

## Non-trigger verdicts

| case | N | verdict counts |
| --- | --- | --- |
| L002 | 7 | acceptable=5, violation=2 |
| L033 | 7 | acceptable=2, awkward=1, violation=4 |
| L034 | 7 | acceptable=1, awkward=1, violation=5 |
| L040 | 3 | awkward=1, backchannel=1, violation=1 |
| L057 | 11 | acceptable=4, violation=7 |
| L073 | 4 | acceptable=1, violation=3 |
| L075 | 4 | awkward=1, violation=3 |
| L079 | 3 | acceptable=1, violation=2 |
| L092 | 6 | acceptable=2, violation=4 |
| L107 | 3 | violation=3 |

## Barge-in diagnostic (7 disjoint groups)

| group | N |
| --- | --- |
| on_time_required_and_correct | 1 |
| late_required_and_correct | 2 |
| premature_but_content_correct | 5 |
| other_matched_barge_in | 18 |
| stale_required_action | 0 |
| other_contextually_acceptable_barge_in | 13 |
| other_awkward_or_violating_barge_in | 37 |

- Reconciliation: waveform barge-in total `76`; seven named disjoint groups 합계 `76`; ungrouped `0`.
- Semantic/mechanical backchannel disagreement: `1`; adjudicated semantic `backchannel` rows are retained in the mechanical seven-group denominator as `other_contextually_acceptable_barge_in`.
- Disagreement IDs: `L040:utt:6`.

## Mechanical checks and reconciliation

| case | status | failed checks | logged tail (s) |
| --- | --- | --- | --- |
| L107 | PASS | — | 15.12 |
| L033 | PASS | — | 15.12 |
| L079 | PASS | — | 14.96 |
| L075 | PASS | — | 15.12 |
| L040 | FAIL | tail_at_least_plan | 14.48 |
| L073 | PASS | — | 15.12 |
| L092 | PASS | — | 15.12 |
| L034 | PASS | — | 15.12 |
| L057 | PASS | — | 15.12 |
| L002 | PASS | — | 15.12 |

- Mechanical failure detail: `L040` failed tail_at_least_plan (logged tail 14.48s; plan tail 15s).
- Denominator reconciliation: case primary N 합계 `74` = reported primary N `74`; timing row 합계 `74` = primary N `74`.
- Review reconciliation: packet `119`, primary reviews `119`, resolved `119`, missing `0`.
- Semantic UNKNOWN: content/joint `0` / `0`; predicted-action mapping은 모든 primary row에서 `UNKNOWN` (`55`)이며 별도 post-mapping을 수행하지 않았다.

## 해석상 제한

이 결과는 10-case pilot이며 fresh Luna semantic review와 1개 fresh Luna adjudication에 기반한 model-only provisional 평가다. Mechanical failure가 있어 transport contract를 완전히 통과한 단일 aggregate로 보고하지 않으며, baseline·human gold가 없는 상태에서 correctness 개선이나 release gate 통과로 해석하지 않는다.
