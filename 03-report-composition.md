# 03. 리포트 생성 (Report Composition)

This document explains how to turn collected evidence into a useful diagnostic report. It is not a fixed dashboard, parser, metric schema, or product-specific template.

Default report language is Korean unless the user asks otherwise. Prefer a self-contained HTML report when possible, because the intended output includes visual summaries, compact interpretation, and inspectable detail.

## Core Principle

The report should answer two user questions:

1. `위키가 잘 구축되어 있는가?`
2. `내가 효율적으로 사용하고 있는가?`

Do not answer only with raw counters. Turn metrics into plain-language judgments, confidence labels, caveats, and next measurement suggestions.

The report may adapt sections, terms, visuals, and examples to the user's wiki substrate: Markdown, Obsidian, graph database, Graphify, vector index, tag-first workflow, MCP route, local LLM, Claude, Codex, Cursor, or another runtime.

## Report Inputs

Treat Stage 2 collection artifacts as the metric source of truth. The report may derive display-level summaries such as ratios, labels, grouping names, and chart-ready series from collected values, but it should not silently create new core metrics that collection did not measure, infer, or mark unavailable.

Before writing prose, scan the aggregate for user-important grouped metrics that are already available. A report should not drop collected evidence simply because it does not match an expected field name. For example, if the aggregate contains per-request-type cost, per-time-window cost, connection-health candidates, hot objects, source-scope counts, or measurement-confidence fields under any reasonable name, surface them in the relevant section as a chart, compact table, or interpreted card.

Use compact final Stage 2 artifacts first. Intermediate row dumps, edge lists, parser scratch files, sampled event summaries, and raw logs are supporting evidence only. If only intermediates exist, mark collection as incomplete and request or rerun Stage 2 reduction before presenting the report as a complete diagnostic.

Do not use previous diagnostic reports, old validation outputs, copied sample metrics, or temporary scratch artifacts as current-run metrics unless the user explicitly requested comparison. The report should be based on the current run's intake and collection artifacts. If old artifacts exist, treat them as reference-only or excluded derived output.

If a user-important value is missing, show it as a measurement gap instead of hiding it. For example, absent elapsed time, token deltas, request depth, selected rank, hit/fallback, or connection-source evidence should become a clear `측정 공백` item with the smallest next telemetry or parser improvement.

When report writing reveals a likely metric problem, stop treating it as a wording issue. Map it back to intake scope or Stage 2 collection, then rerun the affected stage before regenerating the report.

## Recommended Flow

Use this as a reading order, not as a rigid layout.

1. `한눈에 보기`: a compact front area with a few key values and trends.
2. `핵심 분석`: answers to the two top-level questions, each with at least 3 visible subquestions. Do not count the two broad questions themselves as subquestions.
3. `추천 방향`: normally three directions: structure/navigation, usage workflow, and telemetry/measurement.
4. `세부 지표와 시각화`: broader evidence, grouped charts, tables, examples, caveats, and raw aggregate views.
5. `측정 공백과 재생성 가이드`: what was not measurable and how to rerun after improving data collection.

The top interpretation area should usually fit within about two screenfuls. The lower detail area can be much richer. Compact front matter does not mean a shallow report.

Even when the layout adapts, the beginning of the HTML should make the reading order visible: `한눈에 보기`, `핵심 분석`, then `추천 방향` or clearly equivalent labels. Recommendations should not become the first major named section unless key metrics, trends, and the two question answers have already appeared under visible labels.

Unless the user renames them, keep the two main question headings recognizable near the top as headings, section labels, or prominent card titles, not only as phrases inside a paragraph:

- `위키가 잘 구축되어 있는가?`
- `내가 효율적으로 사용하고 있는가?`

In the detail area, keep the analysis families recognizable even when their exact headings change: operating components, request-level usage, time-window trend, size/reading burden, connection health, trust/schema, measurement gaps, and diagnostic run cost.

## Report Quality Questions

Before calling the report useful, ask:

- Can the user tell in the first two screenfuls whether the wiki is basically navigable, where it is becoming heavy, and what to improve first?
- Does each headline judgment have a nearby value, source family, confidence, and caveat?
- Are scale, trend, and request-level cost separated so totals do not hide per-request friction?
- Are connection-health labels framed as review candidates under the user's connection model, not universal defects?
- Are recommendations tied to measured evidence or named data gaps rather than generic advice?
- If the user challenges a number, does the report say which artifact or stage should be rerun?

## Research-Informed Report Questions

The report should turn common maintenance and observability questions into user-facing sections. These are recommended lenses, not fixed chart requirements.

- Inventory versus audit: can the user distinguish `what exists` from `what needs attention` without reading the raw file tree?
- Findability: can the user see whether important knowledge is reachable, isolated, a dead end, heavily depended on, missing, or hidden behind a weak route?
- Freshness and ROT: can the user see which areas look stale, redundant, trivial, duplicated, unsourced, or ready to archive/refresh?
- Search usefulness: if query evidence exists, can the report show zero-result/fallback/refinement/hit signals, not only search count?
- Request journey: if trace/session evidence exists, can the report show one-request cost as a journey of search/read/write/tool/time/token rather than only totals?
- Growth pressure: can scale and unit cost appear together, so the user can tell whether the wiki is merely larger or actually harder to use?
- Graph pressure: if graph evidence exists, can components, isolated neighborhoods, hub concentration, bridge nodes, and important disconnected nodes be shown as a small visual or ranked list?
- Confidence and gaps: can the user tell which values are measured, inferred, estimated, unreliable, unavailable, or blocked by missing telemetry?

Good visuals for these lenses include role-split cards, stacked coverage bars, small trend bars, request-type cost bars, p50/p90 paired bars, missing-target ranked bars, component-size histograms, hot-object tables with role badges, and measurement-gap checklists.

## Top Area

The top area should be friendly to a user who wants an immediate diagnosis.

Include, when evidence supports it:

- key scale: objects/documents/nodes, ordinary content objects, important entrypoints, logs/activity, sessions or telemetry windows
- key burden: typical object size, large/hot objects, index/guide reading burden, request-level work
- key trend: growth, activity, request volume, request cost, or measurement coverage over time
- key gap: what prevents exact time/token/depth/hit-rate analysis

If the broad object count includes guides, indexes, logs, daily notes, route surfaces, or operating records, do not show it as a single unexplained `문서 수`. Pair it with a narrower content count or a role split so the user can tell whether the wiki is large because of knowledge pages, logs, entrypoints, generated records, or runtime-adjacent material.

Prefer user-facing Korean labels for these denominators near the top, for example `전체 관리 대상`, `일반 지식 객체`, `운영 기록`, `진입점/인덱스`, and `LLM 관련 추정 객체`. Raw names such as `active_wiki_objects` or `content_objects` are acceptable in artifacts, but should not be the only labels in the HTML.

Trend visuals are more useful than trend names. If there is dated evidence, show whether the wiki or usage is becoming heavier with a small line, sparkline, bar, or paired volume-versus-unit-cost view. If dated evidence is absent, say so.

## Diagnostic Questions

Generate context-appropriate subquestions. The following defaults are broadly useful, but the agent may add, merge, or rename them when the local wiki calls for it.

In the HTML, keep these subquestions visible near the top inside `핵심 분석` or an equivalent interpretation block. They may appear as small cards, `h3` headings, a two-column question list, or concise question-answer rows, but they should not disappear into later chart titles such as `요청 유형 분포` or `운영 범위 분해`. The detailed charts can reuse the same evidence below; the top analysis should still show the user's diagnostic questions and short answers.

Default expectation: show at least three subquestions under each broad question. If the report merges, renames, or omits one of the default subquestions below, it should be obvious from the wording which local question replaced it, or the evaluation note should mark that as a report-composition caveat. A report that only shows the two broad questions plus two or three scattered subquestions is incomplete even if the lower charts contain the underlying metrics.

For `위키가 잘 구축되어 있는가?`, normally answer:

- `운영 구성요소가 갖춰져 있는가?`  
  Are guide/startup memory, schema/rules, indexes/hubs, logs/activity records, retrieval routes, and telemetry/session sources identifiable?

- `인덱스와 시작 지점이 탐색을 돕는가?`  
  Are there useful entrypoints for active projects/topics, and are they sized and updated well enough to guide an LLM?

- `운영 기록과 신뢰 단서가 남는가?`  
  Are additions, edits, maintenance, archive/index updates, provenance, freshness, review, or source signals visible?

- `문서 단위와 연결은 문제가 없는가?`  
  Are important objects reachable, appropriately sized, and connected through the user's actual connection model?

For `내가 효율적으로 사용하고 있는가?`, normally answer:

- `요청당 비용은 어떤가?`  
  How many searches, reads, opened objects, tool steps, route changes, seconds, or tokens does one request tend to use?

- `시간이 지나며 무거워지는가?`  
  Are request volume, objects/request, time/request, tokens/request, or maintenance burden increasing?

- `무엇을 자주 쓰는가?`  
  Which objects are repeatedly read or selected, and are they content, guides, indexes, logs, generated files, or runtime artifacts?

- `어디서 비용이나 실패가 생기는가?`  
  Do request type, project/topic, route, search depth, selected rank, fallback, or missing telemetry explain friction?

Each subquestion answer should include a short plain-language conclusion, 1-3 supporting values or a data gap, and a confidence/source label such as `정적 파일`, `로그`, `세션 추정`, `실측 telemetry`, or `측정 불가`.

## Recommended Evidence Families

The report should usually preserve evidence from these families when available. If a family is important but unavailable, show it as a measurement gap rather than silently omitting it.

### 운영 구성요소

Explain what the agent found before interpreting quality:

- wiki substrate and counted scope
- guide/startup memory, schema/rules, indexes/hubs/dashboards
- logs/activity records
- retrieval/search/graph/vector/tool routes
- telemetry or session evidence
- exclusions such as generated reports, plugin files, caches, or unrelated runtime artifacts

Good visuals include component coverage cards, route/source-family bars, and guide/index/log size or freshness comparisons.

### 요청당 분석

Show whether usage can be understood per request, not only in totals.

Useful views include request type mix, searches/request, reads/request, objects/request, writes/request, route depth/rank/hops, elapsed time, token fields, hit/fallback/usefulness, and hot objects.

If query telemetry is absent, a temporary session parser may infer request windows from structured transcripts or runtime traces. If it cannot find reliable traces, explain that this is a parser/telemetry gap and recommend request logging.

When the report says request-level cost is unavailable, it should also say which session/telemetry source families were checked and why they were rejected, secondary, or insufficient. Otherwise the user cannot tell whether the cost is truly unmeasured or the collector simply failed to inspect the right history.

When session parsing succeeds but key fields remain approximate, explicitly recommend a future `query log`, `request log`, hook log, wrapper log, or equivalent telemetry. Use the term that fits the user's environment, but make the recommendation concrete enough that the user understands what to start recording next.

Show request classification quality. If a large share of windows is unknown, other, unrelated, or mixed, explain that the parser found activity but cannot yet confidently separate lookup, writing, maintenance, diagnostics, and unrelated work. Treat that as a metric caveat and a telemetry/parser recommendation.

If request type counts exist, avoid rendering an empty `요청 유형별 비용` section. Prefer grouped cost metrics by type when collected. This does not require a fixed schema name: look for any reasonable per-type map or rows such as `type_metrics`, `request_type_metrics`, `per_type_metrics`, or equivalent labels in the user's language. A visible type-cost view should include request count plus one or more cost signals such as tool calls, search/read/write counts, elapsed time, token fields, route depth, or confidence/gap. If grouped cost metrics are unavailable, show the request type distribution beside the global p50/p90 cost values and explicitly state that type-level cost grouping is a collection gap.

Translate raw parser labels into user-facing language. Do not leave `unknown_or_chat`, `other`, or `unclassified` as the main explanation without a nearby plain phrase such as `미분류/대화성 요청` and a note about classification confidence.

For any session-derived request count, show the scope behind the number near the metric or in the source audit: candidate stores checked, candidate files found, target-matched files, parsed files, request-window rule, and confidence. If a previous or parallel run produced a very different request-window count, frame it as a parser-scope question until the denominator is reconciled.

When request/session metrics are prominent, include a visible `세션 분석 범위` or equivalent section in the HTML, not only in an evaluation note. It should summarize the primary trace source, candidate and parsed file counts, parsed record count when available, request-window rule, caps/recency filters, confidence, and the reason a dedicated query/request log would improve the next run.

### 시간대별 분석

Show whether scale or cost is changing.

Possible windows include day, week, month, session, release period, project phase, or user-defined cycle. Pair volume with unit cost when possible: for example, documents over time next to objects/request, or request count next to time/request.

If the aggregate includes per-window cost metrics, do not show only volume. Add at least one visible cost trend such as p50/p90 elapsed time, tool calls, read/write counts, output tokens, total token signal, objects/request, or confidence by window. The chart can be a compact bar group, small multiples, table with bars, or annotated line-style visual; the important point is that the user can tell whether use is getting heavier, lighter, or merely more frequent.

Do not hide trend evidence under a generic table title. If dated evidence exists, make the trend visible with a heading or label such as `시간대별 분석`, `월별 추세`, `최근 변화`, or a domain-specific equivalent, and include a nearby plain-language sentence explaining whether the wiki or usage appears to be getting heavier.

When the report includes dated activity or request-cost bars, keep the section label broad enough for the user's question. A heading like `최근 수정 활동` can be useful inside the section, but it should not replace the recognizable `시간대별 분석` or equivalent top-level trend section when trend evidence exists.

### 크기와 깊이

Translate size into reading burden.

Useful views include typical object size, long-tail objects, guide/index size, hot large objects, chunk/result-set size, graph hops, selected rank, route depth, and repeated startup reads.

Avoid opaque labels such as `size band` unless the report explains them in user-facing language.

### 연결성

Report whether important knowledge is reachable and whether it leads onward.

Connection surfaces may be links, backlinks, folders, tags, frontmatter relations, graph edges, semantic neighbors, search routes, query tools, MCP routes, logs, or session selection.

Useful concept families:

- `stub candidate`: too small, placeholder-like, or under-summarized to answer likely questions.
- `orphan candidate`: hard to reach from expected entrypoints or neighborhoods.
- `dead-end candidate`: reachable but weak in onward routes, related context, or source hints.
- `broken-reference candidate`: unresolved link, embed, relation, edge, citation, route, or target.
- `hot weak candidate`: frequently used but oversized, stale, unsourced, under-summarized, or weakly connected.

These are review candidates, not automatic defects. A deliberate daily note, archive page, generated log, or terminal project note may legitimately look weak under one model.

Do not make illustrative syntax look like a real broken link. If the collector sees candidate references inside code blocks, documentation examples, template placeholders, escaped/truncated route snippets, or generated sample data, the report should either exclude them from headline counts or label them as low-confidence/example references. Broken-reference counts are useful only when the user can tell which part is likely operational and which part may be parser noise.

When unresolved targets include likely examples, placeholders, syntax labels, template variables, or generic documentation words, do not mix them into the main operational `top missing target` list as if they were real missing wiki objects. Either filter them from the main list, show them under a separate low-confidence/example bucket, or make the table label say that it is an unfiltered rough candidate list. The user should be able to tell which broken-reference candidates are worth fixing first.

If the collector did not assess example/template/code-block contexts, say so as a caveat near the connection-health chart. This is better than presenting every unresolved reference candidate as equally likely.

Separate units clearly: broken-reference events, source objects with broken references, unique missing targets, candidate flags, and de-duplicated objects needing review.

If the collector measured or inferred connection-health families, show the family names in user-facing language. Do not collapse everything into a generic `연결 문제` bucket. If `stub`, `orphan`, `dead-end`, `broken-reference`, or `hot weak` is unavailable because the local connection model differs, say that explicitly and explain the alternative connection evidence.

In the HTML detail area, include a visible count or visual row for each measured nonzero connection family before showing examples. Broken references alone are not enough when stub, orphan, dead-end, or hot-weak candidates were also collected. If a family was collected but intentionally omitted from the visual, name the reason as a measurement/display caveat.

Do not expose raw metric keys as primary labels. Convert `dead_end` to `dead-end 후보`, `broken_reference_source_objects` to `broken-reference 소스 객체`, and parser-specific buckets to plain Korean or well-explained concept labels. Raw keys can appear in a method note or appendix, not as the main visual text.

### 신뢰와 스키마

Show whether the wiki can be filtered, maintained, and trusted.

Useful signals include source/provenance, citation, review status, updated dates, owner/status fields, tag consistency, frontmatter consistency, relation consistency, stale important objects, duplicate aliases, and naming drift.

Frontmatter is only one possible schema surface. Do not imply it is mandatory.

### 측정 공백

Show what the report cannot know yet.

Common gaps include request IDs, start/end time, token deltas, selected objects, selected rank, route depth, graph hops, hit/fallback, user feedback, and session scope.

For each important gap, recommend the smallest next field or wrapper that would make the next report more precise.

For request-level gaps, the smallest next step is often a query/request log with request ID, start/end/elapsed time, request type, retrieved/read/written objects, selected rank or route depth, token fields, and hit/fallback or feedback. Present it as a telemetry recommendation, not as a requirement for every wiki.

If many request windows are unclassified, recommend a request type field or a parser-vocabulary update. Otherwise the report may overstate usage efficiency because lookup, writing, maintenance, and unrelated sessions are mixed together.

### 진단 실행 비용

When available, include the cost of the diagnostic run itself: elapsed time, scanned files/objects/traces, artifact sizes, token availability, and skipped scope. If exact cost is unavailable, say so plainly.

## User-Facing Terms

Explain terms before using them as judgments.

- `위키 객체`: the unit being diagnosed. It may be a Markdown document, section, graph node, vector chunk, database record, search result, or custom knowledge object.
- `진입점`: where an agent starts navigation: guide, README, CLAUDE/AGENTS, index, dashboard, hub, tag view, graph route, query template, search tool, or MCP tool.
- `문서 읽기 부담`: the cost of repeatedly loading an object, including length, chunks, payload, token size, or repeated startup/index reads.
- `연결 건강`: whether important knowledge can be reached and can lead to useful next context.
- `요청당 비용`: the work needed for one user request: search, read, write, tool calls, opened objects, route changes, elapsed time, tokens, depth/rank, or fallback attempts.
- `토큰 사용량`: measured token fields when the runtime records them; otherwise a clearly labeled estimate based on text, payload, or trace size. Do not mix measured and estimated tokens in one chart without labeling them.
- `검색 품질`: whether returned or selected objects helped answer the request, preferably visible through hit/fallback, selected rank, reuse, citation, feedback, or fewer repeated failed routes.
- `운영 기록`: evidence that the wiki is maintained: logs, daily notes, changelogs, index refreshes, lint/fix records, moves, archives, reviews.
- `데이터 공백`: a value the user may care about but the current evidence cannot measure.

Prefer terms that explain the user's question. For example, `인덱스 페이지들이 최신이고 탐색에 도움이 되는가?` is clearer than `entrypoint health`.

## Visualization Guidance

Use visuals to make burden, trend, and concentration obvious. Do not force every metric into a chart.

A complete HTML report should not rely on tables alone for the main story. When evidence exists, include at least one clear visual for structure/navigation and one clear visual for usage/time/cost. CSS bars, sparklines, compact timelines, and small distribution panels are enough if they are labeled and interpreted; heavyweight chart libraries are optional.

Good default patterns:

- compact metric cards for scale, burden, trend, and gaps
- bars for category splits, component coverage, request type mix, and candidate families
- timelines or sparklines for growth, activity, and request-cost trends
- histograms or percentile summaries for object size and request cost
- scatter or ranked bars for hot objects versus reading burden
- stacked bars for measured/inferred/unavailable coverage
- small network/flow views only when they remain readable

Every visual should answer a sentence-level question. If the user cannot tell what a chart means from its title, axis labels, and nearby interpretation, rewrite it.

Tables are useful for exact values and audit trails. Put them below or beside visual summaries, especially for trend, burden, request-level cost, and connection-health evidence.

## Interpretation Rules

- Missing telemetry is not evidence of low cost.
- A high document count is not automatically bad; it matters when navigation, search cost, or maintenance burden rises.
- A large guide or index is not automatically bad; it matters when it is repeatedly read, stale, or too broad for routing.
- Stub/orphan/dead-end labels are review prompts, not defects.
- Session-derived request metrics are approximate unless backed by structured logs or telemetry.
- Totals show scale; per-request and time-window metrics show whether usage is becoming harder.
- Do not mix totals, per-request values, and trend values without naming the unit.
- Do not report overlapping candidate flags as unique document counts.

## Recommendation Style

Keep one clear `추천 방향` section near the top. Avoid scattering unrelated recommendation boxes through every detail section.

Default recommendation slots:

- `구조/탐색`: entrypoints, indexes/hubs, routes, object size, connection repair, source/trust conventions.
- `사용 흐름`: request routing, repeated reads, search/write mix, costly request types, workflow changes.
- `측정/텔레메트리`: query/request log, request IDs, elapsed time, token fields, selected objects, depth/rank, hit/fallback, feedback, or session-parser scope.

Each recommendation should state:

- what to inspect or change
- why it matters
- which metric or data gap supports it
- confidence/source

If evidence is insufficient, keep the recommendation as `확인 필요` and name the missing signal.

## Output Contract

A complete run should normally leave inspectable artifacts such as:

- intake/profile summary
- aggregate metrics or concept measurements
- session/telemetry probe result when attempted
- HTML report
- evaluation or self-check note
- a short run summary when the execution log is noisy or tool-specific

The main deliverable should normally be a self-contained HTML report. A Markdown companion is optional and may be concise, but the HTML report should carry the interpreted top area, visual summaries, detailed evidence, and caveats.

The report itself should mention:

- counted scope and exclusions
- source family and confidence for major claims
- method caveats for inferred values
- measurement gaps and next telemetry
- regeneration guidance

Use compact aggregate metrics as the report's primary input. Supporting static summaries, session probes, source audits, row dumps, and long examples may be cited or sampled, but the report should not require loading the full content of every supporting artifact to understand the current diagnostic. Write and parse-check this aggregate artifact before composing long HTML or prose, so a stalled report stage can be recovered without rerunning collection. If the aggregate metrics file mostly duplicates static/session supporting artifacts, treat that as a Stage 2 reduction issue.

If the aggregate includes long maps or long-tail lists, use only the top-N, percentiles, and confidence notes needed for the report. Exact long tails can be linked as supporting evidence rather than expanded into the main report.

If top lists are present, show them as concise display subsets with a clear label such as `상위 후보`, `자주 등장한 항목`, or `검토 우선순위`. Avoid expanding every collected item into the HTML unless the report is explicitly an audit appendix.

Do not store raw prompts, raw answers, or full note bodies by default. Prefer counts, paths, hashes, examples, source labels, and summarized evidence.

The report stage should cite the stage artifacts it used. If the final interpretation is challenged, map the issue back to `intake`, `collection`, or `report` so the next run can revise the right stage instead of rewriting the HTML by hand.

If the runtime transcript contains long command bodies, generated code, or file diffs, do not make that transcript the user's primary review surface. Leave a concise run summary or evaluation note with artifact paths, key counts, validation status, major caveats, and what to rerun next.

If the transcript or log was materially noisy, say so directly in the evaluation or run summary. A noisy run can still be valid when compact artifacts and HTML were produced, but the user should know that saved artifacts, not the runtime transcript, are the authoritative review surface.

When an execution log or transcript artifact is readable, include a small run-log quality line such as path, byte size, whether generated code/diffs were present, and whether the saved artifacts supersede the transcript. If the executing agent cannot observe its own transcript, record `runtime log not self-measured` rather than omitting the log-quality question entirely.

The evaluation artifact should be honest about quality. Do not mark a run as fully passing when:

- required artifacts exist but major evidence families are missing or internally inconsistent
- current-run reports depend on old validation outputs or previous diagnostic artifacts without an explicit comparison request
- the top report answers only the two broad questions but omits visible subquestions or turns them into unrelated chart titles without direct answers
- fewer than three visible subquestion answers appear for either broad question, unless the report clearly explains an intentional merge or local replacement
- a source family discovered during intake disappears from the report without a caveat
- counts for the same denominator disagree across artifacts
- a trend is reported as unavailable or zero even though matching dated source files were discovered
- per-window cost metrics exist, but the trend section shows only request/document counts and gives no cost trend or caveat
- grouped user-important metrics exist in the aggregate, such as request-type cost or connection-health candidates, but the HTML only shows totals or leaves the corresponding section empty
- the report is mostly a short summary and lacks enough lower evidence for the user to challenge the analysis
- collection produced terminal/debug output but no usable stage artifacts
- a collector or parser script was generated but failed before producing compact aggregate metrics or an HTML report
- the run consumed most of its effort on raw trace/debug output instead of reducing evidence into inspectable artifacts
- terminal/debug logs expose raw prompt, answer, preview, body, tool-result, or transcript values without user opt-in

In those cases, use `partial`, `needs rerun`, or an equivalent status, then name the intake, collector, or report step that needs repair.

If collection cannot fully complete, still write a partial report and evaluation when enough aggregate evidence exists. The user can improve the next run only if the current run leaves inspectable artifacts, gaps, and failure notes.

## Regeneration Dialogue

After the first report, the user may challenge values, ask for different visuals, or reveal missing context. Treat that as a collection/report refinement loop.

When a value looks wrong:

1. Identify whether the issue is intake scope, parser coverage, metric definition, denominator, visualization, or wording.
2. Adjust the relevant Markdown contract, collector assumptions, telemetry scope, or temporary parser.
3. Rerun collection before regenerating the report when the metric itself changed.
4. Regenerate only the report when the underlying metric is sound but the explanation or visual was unclear.
5. State what changed and what remains uncertain.

Common user challenges include undercounted distributed logs, omitted session sources, false session candidates, cumulative token fields mistaken for per-request deltas, idle time folded into elapsed time, connection assumptions that ignore the real routing model, and unclear terms.

## Self-Check

Before calling a report successful, check:

- Does the top area answer both main questions in plain Korean?
- Are key trends shown visually when dated evidence exists?
- Are request-level metrics attempted through telemetry or session inference when relevant?
- Are missing time/token/depth/hit fields treated as data gaps, not as zeros?
- Are operating components, logs, schema, indexes, and guide files represented before document-level repair lists?
- Are connection concepts explained and measured or marked unavailable?
- Is the lower detail area rich enough to inspect the evidence?
- Are artifact counts, denominators, and trend sources internally consistent?
- Can the user tell what to improve next without reading the collector code?
