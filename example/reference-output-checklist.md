# Reference Output Checklist

Use this checklist when validating whether the three Markdown files are enough for a fresh agent to produce a similar result.

## Required Shape

- The run starts with structure discovery before metric collection.
- The collector is generated or adapted to the local wiki shape.
- If temporary code is generated, it is small enough to inspect and is saved before execution; one large collector/renderer script is not required and should not block stage artifacts.
- If the runtime transcript contains generated code, patch diffs, or long command bodies, the run summary or evaluation note names this as a run-log quality caveat and points back to saved artifacts as the review surface.
- Core-only validation works without reading the example folder.
- Example-assisted validation may use the example folder for style, terminology, and report shape, but not for fictional metric values or fixed section requirements.
- The user-facing report is self-contained HTML when local file generation is available; Markdown is only a companion or fallback.
- The report is Korean-first unless the user asks otherwise.
- The top of the report answers:
  - `위키가 잘 구축되어 있는가?`
  - `내가 효율적으로 사용하고 있는가?`
- The top-level answers are supported by smaller diagnostic questions, not just two broad paragraphs.
- Each top-level question has 3-4 clear subquestions unless the evidence family is unavailable and shown as a data gap.
- Counts name their denominators: scanned files, active objects, content objects, entrypoints, log sources, activity events, session windows, or another explicit scope.
- The first two screenfuls contain interpreted metrics, trends, and recommendations.
- Detailed charts, timelines, bars, distributions, and raw aggregates appear below the summary when the data supports them.
- Trends, distributions, rankings, category splits, and connection-health signals are shown with actual visual encodings when HTML can be generated, not only as long tables.
- The HTML report uses polished but restrained presentation: compact cards, clear hierarchy, confidence chips, mini charts, useful captions, and raw tables below the interpretation layer.
- Tables are used for exact raw values and audit detail, while charts carry the first interpretation.
- The lower sections preserve enough grouped raw metrics for the user to challenge or refine the report, rather than only showing a few headline counters.
- Detailed charts/tables include a short plain-language explanation and interpretation, not just raw labels.
- Chart captions name the denominator, source/confidence, plain-language interpretation, and caveat.
- Request-level and time-window groupings are preferred over totals when usage data exists.
- Session analysis separates broad activity from target-related activity when possible, so unrelated chat, other projects, or diagnostic work do not dominate the wiki usage story.
- Request-cost views compare useful request groups such as lookup/research, write/maintenance, mixed lookup-write, and chat/planning when the parser can infer them.
- If measured query/hook telemetry is absent but relevant wiki-use sessions exist, a temporary session parser is generated or adapted and its inferred metrics are clearly labeled.
- The diagnostic run's own stage-level time/token availability is recorded separately when possible.
- The diagnostic run records elapsed time and, when observable, whether code/diff noise appeared in the execution log.
- Missing session, token, time, or trend data is shown as a visible data gap instead of a fabricated chart.
- If sessions exist, the report attempts inferred request-level metrics and labels the parsing caveats.
- Elapsed-time charts state whether values are request-log measurements or transcript timestamp estimates, and call out idle gaps/session resumes when those can inflate durations.
- Token charts state whether values are provider usage fields, request-log deltas, cache-inclusive signals, or rough text-size estimates; mixed token sources are not shown as one clean cost line.
- Static wiki, logs/activity, sessions, and telemetry are not collapsed into one confidence level.
- Measured, inferred, and unavailable values are visibly distinguished.
- Suspicious values trigger collector repair and regeneration, not manual report editing.

## Expected Metric Families

The exact metric names should vary by environment, but the report should try to answer these questions:

- How many documents, nodes, chunks, or knowledge objects exist?
- How much reading burden do ordinary documents and entrypoints create?
- What connection model is being used, and are hard-to-reach objects visible under that model?
- Are stub/orphan/dead-end/broken-reference/oversized-hot candidates visible as review candidates, with local calculation logic explained?
- Are additions, edits, or maintenance actions recorded over time?
- For lookup-like requests, how many retrieval steps happen per request?
- For each request, can the report infer or measure tool-call count, search count, read-object count, write count, depth/rank, elapsed time, and token fields?
- Can the report distinguish all parsed requests from target-related requests, and show whether the target-related subset is heavier?
- How many objects are inspected per request?
- How much time and token budget does a lookup-like request consume?
- Are the cost signals getting heavier over time?
- Which important values are unavailable until hook/query telemetry is added?
- Does the report distinguish the preferred measured query/hook log path from the interim session-parser path?
- Which common wiki concepts were considered: scale, roles, reading burden, entrypoints, connection health, growth, maintenance, freshness, trust, consistency, hot objects, request cost, retrieval quality, route mix, data gaps, and diagnostic run cost?

## Good Signs

- The report uses labels like `문서 수`, `보통 문서 길이`, `요청당 검색/조회 단계`, `검색성 요청 시간`, and `토큰 부담`.
- It explains terms like `진입점`, `문서 읽기 부담`, `연결 건강`, `요청당 비용`, `검색 품질`, and `데이터 공백` before relying on them.
- It explains `문서 읽기 부담` in plain Korean instead of exposing unexplained labels like `size band`.
- It shows connection health as a user-facing signal such as `연결 점검 필요 문서`, with categories and caveats.
- It uses common visualization concepts where useful: denominator bars, role composition bars, reading-burden distributions, activity timelines, request-cost trends, route mix charts, connection-health bars, hot-object views, trust/freshness coverage, data-gap cards, and diagnostic run-cost timelines.
- It avoids unexplained headings like `p90`, `orientation`, `friction`, or hidden composite scores.
- It treats stub/orphan/dead-end/frontmatter metrics as configurable heuristics, not universal truth.
- It separates live content from archives, generated diagnostic files, templates, and examples when prioritizing connection-health candidates.
- It adapts pages/links/indexes to graph nodes, vector chunks, tags, relationship fields, MCP routes, or custom retrieval layers when needed.
- It separates guide/startup reads from content reads when session traces are available.
- It avoids treating arbitrary prompt fragments as hot documents.
- It explains obvious count changes across runs by denominator/scope differences before treating them as real wiki changes.
- It does not confuse diagnostic-run cost with normal user lookup/request cost.

## Privacy Check

- No raw prompts.
- No raw assistant answers.
- No full note bodies.
- No absolute local paths.
- No personally identifying file names.
- No dependency folders or generated implementation dumps.
