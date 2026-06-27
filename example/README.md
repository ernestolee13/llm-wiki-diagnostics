# Example

This folder exists only to show the shape of useful stage outputs. It is not an implementation, dashboard template, parser, benchmark, or live metric source.

The root `README.md` and the three stage Markdown files are the product core. A fresh agent should be able to run a useful diagnosis from those files alone. Use `example/` only after that core-only run is acceptable, when the next run needs a clearer sense of artifact shape, terminology, caveats, and restrained report polish.

## Files

- `report-preview.svg`: README용 정적 미리보기 이미지. 실제 실행 결과나 기준값이 아니라 리포트의 정보 배치와 시각화 밀도를 보여주는 sanitized preview입니다.
- `sample-intake-profile.json`: Stage 1 structure-analysis artifact. It shows how to summarize detected guides, entrypoints, logs, retrieval surfaces, session sources, assumptions, and user questions without copying raw content.
- `sample-aggregate-metrics.json`: Stage 2 metric-collection artifact. It shows how to group measured, inferred, and unavailable values around diagnostic questions.
- `sample-report-outline.md`: Stage 3 report-composition reference. It shows Korean-first interpretation style, visible subquestions under the two main questions, caveat language, and lightweight HTML presentation cues.
- `reference-output-checklist.md`: validation checklist for comparing core-only and example-assisted runs.

## Learn From This

- Stage boundaries: structure discovery first, metric collection second, report composition last.
- Confidence handling: measured, inferred, unavailable, and partial signals should stay visibly separate.
- Question framing: the report should answer `위키가 잘 구축되어 있는가?` and `내가 효율적으로 사용하고 있는가?`, with smaller subquestions near the top.
- Artifact granularity: store aggregate facts, derived metrics, caveats, and repair hints; avoid raw transcripts or note bodies.
- Report feel: concise cards and small charts first, detailed raw views lower down.
- Execution shape: when temporary code is useful, keep it stage-local and checkpointed. A small intake probe, a small collector, and a small renderer are usually easier to verify than one large script.
- Runtime hygiene: if the execution environment prints file-creation diffs, command bodies, or generated code into the transcript, the saved artifacts should still be the review surface. The run summary or evaluation note must call out the noisy transcript and say whether it affected artifact quality.

## Do Not Learn This

- Do not copy the fictional values, paths, counts, thresholds, dates, or labels.
- Do not force every sample section into every user's report.
- Do not treat Markdown, wikilinks, Obsidian, Claude Code, or the shown folder layout as universal.
- Do not skip local discovery because the examples mention guides, indexes, logs, or sessions.
- Do not hardcode the sample dashboard. Build the report from collected metrics.
- Do not spend the run generating a large all-in-one implementation before saving inspectable artifacts.
- Do not treat a long runtime transcript as the report. The user should be able to open the HTML and read the run summary without scrolling through generated code.

## Validation Rule

An example-assisted run succeeds only if it adapts the sample structure to the user's actual wiki. If the final report copies example values, treats example paths as live evidence, omits source/confidence caveats, or generates a fixed template instead of a local diagnosis, treat the run as failed and repair the stage instructions or collector.

If the run stalls while designing a collector, shrink the next cycle. Save the current intake profile first, collect only the highest-value static and session aggregates, render a partial but honest HTML report, and mark missing evidence as a data gap instead of waiting for a perfect parser.

If the run completes but the transcript is noisy, do not rerun just to make the transcript pretty. First check whether the HTML, aggregate metrics, run summary, and evaluation note are complete. Then improve the next cycle by using smaller stage-local scripts, suppressing generated-code output where the runtime allows it, and recording a `run-log quality` caveat.
