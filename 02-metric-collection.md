# 02. 지표 수집 (Metric Collection)

This document defines what a diagnostic run should try to measure. It is a concept contract, not a fixed parser, schema, formula, or provider integration.

The executing agent should adapt the collector to the user's wiki substrate: Markdown, Obsidian, graph database, Graphify, vector index, tag-first workflow, MCP route, local LLM, Claude, Codex, Cursor, or another runtime.

## Collection Execution Contract

Stage 2 should turn intake evidence into compact, parseable artifacts before report writing. It should not rely on terminal output as the metric store.

Start small and write files in this order when possible:

1. `collection checkpoint`: scope, caps, source families, intended parser approach, and known gaps.
2. `source/static summaries`: object counts, roles, sizes, activity/log signals, connection candidates, and trust/schema signals.
3. `session/telemetry probe`: request windows, tool calls, reads/writes, elapsed time, token fields or token estimates, and parser caveats.
4. `aggregate metrics`: a report-ready dataset with confidence labels and measurement gaps.

The names are descriptive, not mandatory. The invariant is that each expensive or uncertain step leaves a compact artifact before the next step depends on it. A command that prints long path lists, JSONL summaries, link inventories, or per-file diagnostics to stdout has not reduced the evidence enough.

If Stage 1 did not leave both a run manifest and a source inventory, create or repair those artifacts before writing a larger collector. Then write the collection checkpoint as Stage 2's first durable output. Stage 2 should not start with a complex parser or collector design as its first durable output.

Generated collector scripts and renderer scripts are also artifacts. Save them, run them, and report their path plus validation result; do not make the execution log the place where their full source code or generated file diffs are inspected.

After the collection checkpoint, the next durable output should prove execution, not just intent. Write one of these before doing long design work: a runnable tiny collector/probe script, a capped probe JSON, a static summary for one source family, or a session-shape probe for one target-matched trace family. Do not keep a large collector plan only in the model scratchpad while no new artifact appears.

## Core Principle

Collect metrics that answer two questions:

1. `위키가 잘 구축되어 있는가?`
2. `내가 효율적으로 사용하고 있는가?`

Prefer question-shaped metric families over isolated counters. A useful metric should have:

- a user-facing meaning
- a denominator or scope
- a source family: static wiki, logs/activity, sessions, query telemetry, runtime hook, graph/search/vector output, or user hint
- a confidence label: measured, inferred, unreliable, or unavailable
- enough caveat to prevent over-interpretation

Do not turn missing evidence into a positive signal. If a value cannot be observed, mark it as a measurement gap and explain the smallest telemetry or parser improvement that would measure it.

## Metric Quality Questions

Before treating a metric as report-ready, ask:

- Which user question does this metric answer, and what action could it change?
- What is the denominator: all scanned files, active wiki objects, content objects, entrypoints, logs, requests, target-related requests, or another local unit?
- Is the value measured, inferred from session structure, estimated from payload/text size, unreliable, or unavailable?
- Would the conclusion change if grouped by request, time window, source family, object role, route, or confidence?
- Could a missing parser field, skipped source family, cumulative token counter, or idle timestamp gap be mistaken for low cost or good health?
- Can Stage 3 explain this metric without inventing new definitions or re-reading raw evidence?

## Research-Informed Metric Questions

These questions are deliberately substrate-neutral. They come from common wiki maintenance, content audit, search analytics, graph analysis, and observability patterns.

- Inventory and role split: can ordinary knowledge objects, entrypoints, guides, logs, generated output, archives, and telemetry evidence be counted separately?
- Quality and lifecycle: can the collector identify stale, redundant, trivial, duplicate, unsourced, unreviewed, or low-confidence objects without treating every short note as a defect?
- Link and route health: can it measure or infer orphan/lonely candidates, dead-end candidates, missing/wanted targets, broken references, long pages, old pages, and highly linked hubs under the local connection model?
- Search usefulness: if search/query evidence exists, can it capture zero-result, fallback, query refinement, selected result, selected rank/depth, hit/usefulness, and result-set size?
- Request cost: can one request be grouped with searches, reads, writes, opened objects, route/hop/depth, elapsed time, token fields, child operations, and status?
- Trend pressure: can scale and unit cost be shown together by day/week/month/session/project, so growth is not confused with heavier individual requests?
- Graph shape: if a graph substrate exists, can connected components, isolated neighborhoods, hub concentration, bridge nodes, and disconnected important nodes be summarized without forcing markdown-link assumptions?
- Measurement coverage: for every unavailable high-value metric, can the collector say whether the issue is missing telemetry, skipped source family, parser weakness, privacy cap, or incompatible substrate?

## Collection Priority

When evidence exists, collect in this order:

1. `Operating components`: guide/startup memory, schema/rules, indexes/subindexes/hubs, logs/activity records, retrieval routes, quality rules, telemetry/session sources.
2. `Request/session usage`: request windows, request type, searches, reads, writes, tool calls, opened objects, route/depth/rank, elapsed time, token fields, hit/fallback, hot objects.
3. `Activity and growth`: additions, edits, moves, archives, index refreshes, log entries, dated notes, request volume, and whether unit cost changes over time.
4. `Static structure support`: object count, role mix, entrypoint burden, reading burden, connection-health candidates, large/hot objects.
5. `Trust and consistency`: source/provenance, freshness, review/citation, feedback, tag/frontmatter/relation/schema consistency.
6. `Coverage gaps`: missing fields, weak parser scope, unavailable telemetry, and next-run measurement recommendations.
7. `Diagnostic run cost`: elapsed time, files/objects/session traces processed, artifact sizes, and token availability for the diagnostic itself.

If the report must emphasize only a few things, prioritize operating components and request/session usage. Put document-level repair lists and raw examples in the lower detail area.

Before collecting, check whether Stage 1 inspected the target wiki and target session/telemetry candidates. If the intake is only a generic contract-derived profile, do not build confident metrics from it. Either rerun intake with target evidence access or mark collection as partial and make the missing target scan the first measurement gap.

## Stage Responsibility

Stage 2 is the source of truth for diagnostic values. The report stage should not need to invent whether document counts, activity counts, connection candidates, request windows, token signals, elapsed time, or parser gaps exist. This stage should either collect them, infer them with caveats, or mark them unavailable with the next smallest measurement step.

Keep this responsibility conceptual rather than product-specific:

- Static/wiki pass: count the local knowledge objects, operating components, logs/activity records, size burden, entrypoint burden, schema/trust signals, and connection-health candidates that can be measured from the discovered substrate.
- Usage/session pass: when measured query telemetry is absent, make a bounded attempt to generate or adapt a temporary parser for discovered session/runtime traces and produce request-level metrics when the trace shape allows it.
- Gap pass: when a value the user likely expects cannot be measured, preserve it as a named measurement gap with source family, reason, and recommended telemetry or parser repair.

The stage output should be rich enough that Stage 3 can focus on interpretation, visual design, and user-facing explanation.

Keep denominator splits report-ready. If `active_wiki_objects` includes guides, indexes, logs, daily notes, dashboards, generated operating records, or other non-content material, also preserve a narrower `content_objects` or equivalent role split. A user should not have to guess whether a headline document count means ordinary knowledge pages, operational records, route surfaces, or all Markdown-like objects after exclusions.

Do not treat Stage 2 as complete because it produced a top-level file count and a telemetry inventory. A useful final artifact should include the important families as measured, inferred, partial, or unavailable:

- operating components and source-family decisions
- activity/log/growth evidence
- static size and reading-burden evidence
- connection-health candidate evidence
- request/session usage evidence
- token/time/depth availability and caveats
- measurement gaps and next telemetry or parser repair

## Execution Shape

For a first run, do not spend the whole stage reading raw evidence into the conversation. Build a small local collector or parser plan, run bounded aggregate passes, and write artifacts early. Quiet artifact mode is part of the collection contract: terminal output should be a short progress and validation channel, not the metric store.

The agent should usually execute collection through a local aggregate collector, script, query, or parser rather than by pasting source evidence into the LLM context. The collector can be temporary and environment-specific, but its job is stable: read target sources, classify events/objects, aggregate metrics, and write compact artifacts. Standard output should show progress, counts, artifact paths, and validation errors only, not raw rows, full JSON records, long file-path lists, full Markdown log entries, or previous report contents.

The same stdout rule applies to generated collector code. A run is healthier when the user sees a short status line and an inspectable script path than when the log contains the whole script body or every patch diff. If an artifact is long enough that its creation would dominate the transcript, write it quietly as a file and make the saved artifact, parse check, and path the review surface. Avoid long inline shell heredocs for collectors or renderers when the execution environment logs command bodies; save long code as a script artifact first, then run a short command against it.

Some agent runtimes log file-creation diffs or command bodies even when the final user-facing output is short. If that cannot be avoided, keep the transcript out of the metric evidence, flag it as a run-log quality caveat, and make the saved artifacts plus validation checks the authoritative output. Do not compensate for noisy logs by treating terminal text as the collector output.

Choose collector tooling that matches the data shape. Shell commands and one-liners are useful for file discovery, counts, and simple filters. Complex link graphs, Unicode paths, multiline Markdown, JSONL sessions, token counters, timestamps, or nested tool-call traces usually need a structured parser or scripting environment available on the user's machine. Avoid making the diagnostic depend on shell-only associative arrays, locale-sensitive parsing, or fragile quoting when the evidence contains non-ASCII titles, long paths, nested records, or multiline values.

Do not postpone all file writes until the very end. After intake and source selection, create an initial checkpoint artifact before writing a complex collector or doing a long scan. It should record scope, chosen source families, caps, intended collector shape, and known gaps. Then update or replace it with final aggregate artifacts. A run that produces only terminal output and no stage artifact is incomplete even if the printed analysis looks plausible.

A practical Stage 2 loop is:

1. Load the intake profile and choose live-content, activity, and session/telemetry source families.
2. Write or update a Stage 2 checkpoint with scope, caps, source families, and intended parser approach.
3. Run a tiny sample/probe that validates the collector can parse the chosen evidence shape.
4. Run a static collector for counts, roles, size, schema/trust, activity, and connection candidates.
5. Run a bounded session/telemetry probe for request windows and per-request cost signals.
6. Write parseable JSON artifacts plus a short checkpoint.
7. Validate the artifacts and only then continue to reporting.

Prefer staged artifacts over one large all-in-one collector. A first implementation can be several small collectors or queries that each write one compact result: intake checkpoint, source inventory, static summary, connection summary, activity summary, session/telemetry probe, aggregate metrics, and report-ready dataset. Merge only after the pieces validate. If the agent starts designing a large collector that will produce every artifact at once, first write a smaller checkpoint and a tiny probe result so failure is inspectable.

A useful sequence is `checkpoint -> tiny runnable probe -> static summary -> session probe -> aggregate metrics`. The exact filenames are not important; the important part is that the user can see execution progress as valid artifacts, not only as a promise that a collector will be written later.

Probe output should be file-first. A probe may print `probe-ok`, counts, and artifact paths, but field inventories, per-file summaries, example paths, and parser caveats should be written to a compact probe artifact. Do not use stdout as the only place where source-shape results exist.

Treat long command output as failed reduction. If a discovery or collection command would print a long path list, JSONL sample, per-file summary, link list, or trace inventory, capture it into a bounded artifact and print only the artifact path plus a short count summary. The user and report stage need inspectable files, not a transcript scrollback full of intermediate evidence.

If a full parser would be expensive, create a capped probe first. A small sample that exposes request-window fields, token/time availability, and parser gaps is better than a long-running collector with no artifact. Record the cap, sample scope, and confidence so the user can rerun with a wider scope later.

For large user-level session stores, write an initial collection checkpoint before deep parsing. The first checkpoint should say which stores were considered, which one appears target-matched, what cap or recency/project filter will be used, and which values will remain partial if the cap is hit. Do not leave the user with no Stage 2 artifacts while recursively scanning a large history directory.

If previous diagnostic artifacts are present, keep them out of the collector input unless comparison is the explicit task. Do not mine old validation folders, old aggregate JSON, or previous HTML reports as if they were current wiki/session evidence. At most, record them as excluded derived artifacts or use sanitized examples for report style when the user supplied `example/`.

Parser debugging must not turn into transcript dumping. When testing a parser, use synthetic examples, field skeletons, counts, hashes, path examples, or short redacted snippets. Do not print raw prompts, raw answers, tool-result bodies, preview values, or full JSONL records into logs just to debug extraction. If a parser cannot be fixed quickly, write a partial probe with the failing source family, observed field names, error class, and next parser repair instead of continuing indefinitely.

Do not debug full scans with `set -x`, `zsh -x`, equivalent trace modes, or verbose per-record logging. Debug against a tiny synthetic case or capped sample, then rerun the full collector quietly. If a collector fails, patch or replace the collector and rerun in quiet mode; do not solve the failure by dumping every scanned path, link, trace line, or intermediate assignment into stdout.

The same rule applies before parser construction. Source-shape inspection should not use raw trace or activity-log lines as its default. Prefer commands or scripts that summarize keys, types, counts, event names, dated-entry buckets, timestamp availability, usage-field availability, and redacted preview lengths while omitting prompt, answer, body, preview, activity-entry text, and tool-result fields. Verbose stdout is mainly a problem when it leaks raw sensitive values, prevents completion, or replaces compact artifacts. If the final JSON and HTML are usable but the run was noisy, flag it as a collection-quality issue and improve the collector; if it prevents artifact generation, treat it as a Stage 2 failure.

## Recommended Analysis Blocks

These are default analysis blocks, not a fixed dashboard schema. Rename or merge them when the local wiki calls for it.

### 운영 구성요소 분석

Ask whether the wiki has the pieces an LLM needs to operate it:

- guide/startup memory
- schema, metadata, tag, relation, graph, or naming rules
- indexes, hubs, dashboards, graph routes, vector/search routes, or custom entrypoints
- logs, daily notes, changelogs, or activity records
- telemetry/session sources
- component gaps and confidence

Useful outputs: component inventory, role split, guide/index/log reading burden, freshness, and missing component notes.

### 요청당 분석

Ask how much work one user request costs and whether the request is lookup, write/maintenance, mixed, or unrelated.

Try to collect:

- request windows and request type
- request classification coverage, including unknown, unrelated, mixed, or low-confidence buckets
- grouped per-request metrics by request type when classification exists
- searches/retrieval steps per request
- reads/opened objects per request, separated by role when useful
- writes/edits/moves/log appends/index refreshes per maintenance request
- tool-call count, route changes, graph hops, selected rank, or equivalent depth signal
- elapsed time and token fields when available
- hit/fallback/usefulness signal when available

If request windows exist but cost fields are not recognized, label this as parser coverage gap. It is not evidence of efficient usage.

If many request windows remain `unknown`, `other`, `unclassified`, or `mixed`, preserve that as a classification-coverage metric. A parser that finds windows but cannot tell lookup from write/maintenance should not support a confident usage-efficiency judgment without caveats.

If request types are collected, also try to aggregate the main cost signals by type: tool calls, searches, reads, writes, object references, elapsed time, token fields, and sample count. If the trace cannot support grouped costs, write an explicit `type_metric_gap` or equivalent rather than leaving the report stage with an empty request-type cost table.

Keep raw parser labels internally, but also provide report-facing labels or descriptions. For example, `unknown_or_chat`, `other`, or `unclassified` should become a user-facing bucket such as `미분류/대화성 요청` with an explanation of why classification confidence is limited.

### 시간대별 분석

Ask whether the wiki or its use is getting heavier over time:

- document/object growth
- log/activity trend
- lookup/write/mixed request trend
- searches/request, objects/request, elapsed/request, tokens/request
- index/entrypoint burden over time
- connection-review or broken-reference trend when historical data exists

Use day/week/month windows that fit the available data. Do not compare totals to per-request values without naming the unit.

### 크기와 깊이 분석

Ask whether retrieval units are sized well:

- typical document/object reading burden
- large objects that are repeatedly used
- tiny or placeholder-like objects that may not answer questions
- entrypoint/index loading burden
- result-set size, chunk count, graph hops, selected rank, or route depth

Large or many objects are not automatically bad. They matter when they increase search steps, time, token cost, or missed results.

### 연결성 분석

Ask whether important knowledge is reachable and gives onward context under the user's actual connection model.

Connection surfaces may include links, backlinks, indexes, folders, tags, frontmatter relations, graph edges, semantic neighbors, search routes, query tools, MCP routes, logs, or session selection.

Useful candidate families:

- `stub candidate`: too small, placeholder-like, or under-summarized to answer likely questions.
- `orphan candidate`: hard to reach from expected entrypoints or neighborhoods under the accepted connection surfaces.
- `dead-end candidate`: reachable but weak in onward routes, related objects, source context, or route hints.
- `broken-reference candidate`: unresolved links, embeds, relation targets, graph edges, citations, tag routes, query outputs, or route targets.
- `hot weak candidate`: frequently read/selected object that is also oversized, stale, unsourced, under-summarized, or weakly connected.
- `connection model review`: route disagreement or insufficient evidence about the local connection model.

These are review candidates, not automatic defects. A short daily note, deliberate chunk, archive page, generated log, or terminal project note may legitimately look like a stub, orphan, or dead-end.

Connection parsing should distinguish operational references from illustrative syntax. Links, tags, route names, graph edges, or citations that appear inside code blocks, examples, templates, documentation about link syntax, escaped placeholders, truncated display snippets, or generated sample data should be excluded from headline broken-reference counts or kept in a low-confidence/example bucket. The collector does not need a universal parser for every Markdown dialect, but it should avoid turning `[[example]]`-style instructions, placeholder route names, or generic documentation targets into confident defects.

When top missing targets include generic words or syntax examples, either filter them out, place them under an `example/template/placeholder` bucket, or add a visible caveat that the broken-reference count is rough. Do not let illustrative targets dominate the recommendation list unless the source context shows they are actually operational references.

Preserve a report-ready caveat for this parsing choice. Useful fields include `operational_reference_count`, `example_or_template_reference_count`, `excluded_reference_contexts`, or `example_reference_filter_not_assessed`. Exact names do not matter; the report should be able to tell whether broken-reference candidates were filtered for examples/templates/code blocks or merely counted as rough candidates.

Keep units separate:

- broken reference events
- source objects with broken references
- unique missing targets
- object candidates by family
- de-duplicated objects needing review
- overlapping candidate flags

If candidate families overlap, either de-duplicate before calling a value `문서 수`, or label the total as `후보 플래그 수(중복 가능)`.

### 신뢰/스키마 분석

Ask whether results can be trusted, filtered, and maintained:

- source/provenance/citation coverage
- review/status/updated/owner/validation fields
- tag/frontmatter/relation/schema consistency
- stale important objects
- duplicate aliases, conflicting titles, or naming drift

Frontmatter is useful only when the workflow uses it. Do not imply it is mandatory for every wiki.

### 측정 공백 분석

Ask what cannot be measured yet and how to measure it next:

- missing request IDs
- missing elapsed time or token deltas
- missing route/depth/rank/hit/fallback/feedback
- unclear session scope
- weak object recognition
- unavailable graph/search/vector outputs

Measurement gaps should be actionable. Recommend the smallest next telemetry field or parser-scope repair.

When request-level precision is weak, name the preferred future mechanism plainly: a lightweight `query log`, `request log`, hook event, wrapper log, or equivalent local telemetry. The exact implementation is environment-specific, but the recommendation should make clear that future reports become more reliable when each user-visible request records its timing, retrieval, object selection, tool use, and token fields.

When request classification is weak, recommend either richer query/request logging or a small parser-vocabulary repair based on the user's actual commands, skills, tools, entrypoints, and object roles.

## Telemetry And Session-Parser Strategy

There are two practical paths for request-level metrics.

`Preferred measured path`: recommend lightweight local telemetry for future runs. This may be called a query log, request log, hook log, wrapper log, retrieval trace, or agent telemetry depending on the user's environment. Useful fields include:

- request ID and request type
- start/end/elapsed time
- retrieval route or layer
- returned and selected objects
- objects read or written
- selected rank, route depth, graph hops, or equivalent
- tool-call count
- token fields
- hit/fallback/usefulness or lightweight feedback

The implementation may be a Codex hook, Claude hook, MCP wrapper, Obsidian command wrapper, local LLM callback, graph/search client wrapper, or another local mechanism. This document only defines the concepts.

`Interim inferred path`: when measured telemetry is absent, generate or adapt a temporary parser for existing wiki-use sessions if they appear parseable. Do not assume those sessions live inside the wiki root. They may be stored in project-local runtime folders, user-level agent stores, editor/plugin databases, hook logs, or local LLM/chat history directories discovered during intake. The parser should try to infer:

- request windows
- request type
- search/read/write/tool steps
- wiki objects, nodes, chunks, or records touched
- route/depth/rank where visible
- elapsed time and token signals where present
- which values are measured, inferred, unreliable, or unavailable

If the user gave an agent, project, or session hint, try that source family early. It is often the best bridge when no query log exists. Still report whether the values are measured, inferred from structured events, or estimated from text or payload size.

Session parsing is a bridge for the first useful report, not the preferred long-term measurement system. A session parser should prefer structured transcripts or runtime traces with roles, tool calls, timestamps, token fields, or event types. Ordinary documents that merely mention prompts or sessions should not become request windows unless they contain usable runtime structure.

Separate trace roles before parsing:

- `request trace`: message/turn records with roles, timestamps, tool calls, request IDs, usage fields, or object references. Use these first for per-request metrics.
- `session summary`: project/session records with modes, agents, start/end, or completion status but no per-turn events. Use these for workload context, not as request windows.
- `usage context`: memories, skill docs, guides, and routing rules. Use these to classify requests and object roles, not as measured usage events.

For example, an OMC-like project/session log may be excellent context for what kind of work happened, but it should not be used as the primary source for searches/request, reads/request, elapsed/request, or tokens/request unless it includes actual per-turn events. A Claude/Codex/local-LLM style transcript, JSONL trace, editor chat database, or hook log with request IDs, timestamps, tool calls, and usage fields is a stronger temporary source for those values.

Classify low-resolution runtime logs carefully. Turn-complete summaries with timestamps and previews may support coarse request volume or elapsed estimates, but they do not measure search/read/write/tool cost unless tool interactions are present. Agent replay, hook status, session start/stop, and lifecycle records are normally workload context unless they expose user-visible request boundaries and object/tool interactions.

Select the primary request trace by target relevance. If a user-level agent store contains a project or session history whose path, working directory, project name, skill name, or object references match the target wiki, make a bounded probe of that source before falling back to easier but less relevant traces. If it is too large, inaccessible, malformed, or not parseable within the run, write that as a parser/access gap rather than silently replacing it with a generic current-session trace.

For session-derived request metrics, record the denominator and selection rule explicitly: candidate stores checked, candidate files seen, target-matched files, parsed files, records read, request-window rule, excluded trace families, caps, and recency filters. Large differences between two runs often mean the parser scope changed, not that the user's actual usage changed.

Make this denominator report-ready. A compact `session_scope_summary` or equivalent should be easy for the report stage to display without reading the whole session probe: checked source families, primary source, candidate files, target-matched files, parsed files, parsed records, request-window definition, cap/recency rule, and confidence.

Token fields should be treated by source quality. If explicit usage/token fields exist in request traces, label them measured. If they do not, optional token-like estimates may be derived from character/byte/message size or tool payload size, but must be labeled as estimated and kept separate from measured token usage.

Be careful with cumulative token counters. A repeated `total_tokens` or session-level usage field is not automatically a per-request cost. Use request-level deltas only when request windows and reset/monotonic behavior are clear; otherwise report token availability, session-level trend, or `unreliable for per-request cost` with the next telemetry needed.

When request traces contain several identifiers, choose the identifier that represents the user-visible request or provider turn before falling back to event UUIDs. Event UUIDs, attachment IDs, tool-result IDs, and message fragments can badly overcount requests. If usage fields repeat across multiple records for the same request, de-duplicate or use a documented max/last strategy instead of blindly summing. Ambiguous tools such as generic shell execution should be kept as their own bucket unless the parser safely understands the command purpose.

Before building the temporary parser, use intake usage-context evidence to understand the local workflow. Memories, skills, startup guides, routing tables, command names, and plugin state can reveal whether a trace line refers to a lookup route, an index refresh, an ingest/write flow, a lint/repair pass, a calendar/tool side quest, or unrelated development. These context sources usually define parser vocabulary and classification rules; they should not be mistaken for measured request events by themselves.

Useful parser-context extraction includes:

- local skill or command names for query, ingest, lint, index, read, write, move, append, graph/search, and MCP routes
- known entrypoint roles such as startup guide, root index, project hub, daily log, project log, content object, archive, generated report, or runtime scratchpad
- workflow phrases that identify lookup-like, write/maintenance, reporting, refactoring, or unrelated requests
- memory fields that expose session IDs, turn counts, token totals, last activity, or active skills
- caveats about compacted summaries, subagents, retries, generated diagnostics, and mixed-purpose sessions

If no relevant session traces are found after checking the discovered in-wiki, project-local, and user-level candidates, say so plainly and recommend query/hook telemetry. If traces are too large, sample deliberately and label the result.

If relevant session traces are found and parsed, still recommend measured query/request logging for future runs when elapsed time, token deltas, selected rank, route depth, hit/fallback, or user feedback had to be inferred, approximated, or omitted. Session parsing is useful for the first report; telemetry is the durable path.

When intake provides session or telemetry candidates, the collector should not stop at source names alone. Make a bounded attempt to use the intake `source_inventory`:

- If a candidate has recognizable structure, generate or adapt a temporary parser for aggregate request/session metrics.
- If a candidate only has filenames or counts, add a parser-plan gap explaining what structure must be sampled next.
- If a parser fails, leave a partial probe artifact with the failure reason, source family, and next field to inspect.
- Do not special-case one user's filenames as universal rules. Environment-specific paths belong in generated artifacts because intake found them, not in the core concept contract.
- Do not claim request-level metrics are unavailable until a bounded source-inventory review has been attempted or intentionally skipped with a reason.

## Derived Groupings

Where evidence supports it, derive summaries by:

- per request
- per day/week/month
- per source family
- per request type
- per route/tool family
- per agent/session type
- per project/topic/tag/graph community
- per object role
- per connection-review family
- per confidence level: measured, inferred, unreliable, unavailable

Per-request and time-window views are often more useful than totals. Totals show scale; grouped and per-unit metrics show whether the wiki is becoming harder to use.

## Output Contract

Generated collectors should leave aggregate artifacts that can be inspected and regenerated. A useful output normally includes:

- `scope`: what was scanned, excluded, and counted
- `operating_components`
- `activity`
- `static_structure`
- `connection_health`
- `usage`
- `session_source_audit`: checked, skipped, inaccessible, secondary, and primary request-trace candidates
- `trust_schema`
- `coverage_gaps`
- `concept_measurements`: how each visible concept was measured, inferred, or left unavailable
- `diagnostic_run_cost`

Intermediate files such as row dumps, edge lists, parser scratch files, or sampled event summaries are allowed, but they are not the Stage 2 result. Before the stage ends, reduce them into compact final artifacts that a report can consume:

- aggregate wiki/static metrics
- session or telemetry probe metrics
- measurement gaps and parser failures
- a checkpoint/evaluation note

If collection stops early, still write these final artifacts with `partial` status, source families attempted, samples processed, and the next repair. A run with only intermediate row files or raw debug logs should be treated as incomplete.

Keep final artifacts compact enough for report generation. If a source family is large, store aggregate distributions, percentiles, buckets, top examples by path/hash/title, and confidence notes instead of embedding full event lists. If the report writer would need to load hundreds of thousands of tokens of metrics or prior artifacts, Stage 2 has not reduced the evidence enough.

The final aggregate dataset should be report-ready, not a copy of every supporting artifact. Do not embed full static summaries, full session probes, full source audits, row windows, or long example lists inside aggregate metrics. Keep those as supporting files and put pointers, counts, percentiles, top-N examples, confidence labels, and measurement gaps in the aggregate. If the static or session section dominates the aggregate file, split it back out.

For large maps such as frontmatter keys, tags, folders, hot objects, missing targets, request labels, or session files, the aggregate should normally keep the distribution summary and a small top-N display subset that supports the report. Full maps and long tails belong in supporting artifacts. The report should remain understandable from the aggregate without requiring every long-tail entry.

Top lists should be deliberately reduced and self-describing: say what was ranked, what scope it came from, why it matters, and where the full supporting artifact lives if the tail is useful. If a top list becomes one of the largest parts of the aggregate, Stage 2 probably needs another reduction pass before reporting.

Connection-health results should preserve the concept families when measured or inferred: `stub`, `orphan`, `dead-end`, `broken-reference`, and `hot weak` candidates. If a family cannot be measured under the local connection model, mark it unavailable or not applicable; do not silently drop the concept from the final metrics when related evidence exists.

When any connection-health family is measured or inferred, expose a report-ready family summary with user-facing labels, counts, units, confidence, and caveats. Detailed examples can stay in supporting artifacts, but the report stage should not have to infer that `stub`, `orphan`, `dead-end`, `broken-reference`, or `hot weak` exists by scanning raw candidate lists.

Keep internal keys and display labels separate. Machine keys such as `dead_end`, `broken_reference_source_objects`, or local parser bucket names are acceptable inside JSON, but the aggregate should also carry report-facing labels such as `dead-end 후보`, `broken-reference 소스 객체`, or a Korean equivalent. Otherwise the report stage may leak implementation-shaped names to the user.

If a family cannot be measured, still include it as a named gap. Missing `connection_health`, `activity`, `static_structure`, or `usage` sections should be interpreted as a collection failure unless the checkpoint explains why the family is irrelevant or unavailable for this substrate.

Do not declare request-level usage unavailable from in-wiki logs alone. Before writing `tokens unavailable`, `elapsed unavailable`, `tool calls unavailable`, or `reads/request unavailable`, include a compact audit of likely external traces and why none produced a usable primary request trace. If the audit was not performed, mark usage as `not yet audited` rather than unavailable.

Do not let a project-local hidden folder stand in for all session evidence. If intake only found runtime files inside the wiki root, Stage 2 should still check whether an accessible user-level or editor-level session store exists. If such a store is target-matched but not parsed, request-level usage is `not yet parsed` or `partial`, not simply unavailable.

Every important metric should include:

- label
- value
- unit
- denominator or scope
- source family
- confidence
- method
- caveat

Do not store raw prompts, raw answers, or full note bodies by default. Paths, counts, ratios, hashes, examples, and confidence labels are preferred.

Machine-readable artifacts must parse before report generation. If a source contains raw multiline text, normalize it into short summaries, arrays, or a separate Markdown note instead of embedding unsafe text in JSON.

Do not leave truncated JSON under the final artifact name. For long-running collectors, write checkpoints first and write machine-readable artifacts in a way that leaves either a valid final JSON file or an explicitly named partial/scratch file. An interrupted run with invalid `02-aggregate-metrics.json` or `02-session-probe.json` should be treated as a Stage 2 failure, not a usable partial result.

The collection stage should be inspectable before report writing. Leave a short checkpoint that says which intake assumptions were used, which sources produced metrics, which sources failed or were skipped, and which values are measured, inferred, unreliable, or unavailable.

## Sanity Checks

Before handing metrics to the report stage, check for contradictions.

Useful checks:

- Count denominators agree across intake, aggregate metrics, and report. If they differ, explain the scope difference or rerun collection.
- Path handling is consistent. Do not mix absolute paths, relative paths, and display names in a way that makes trend or connection parsers silently miss known sources.
- If intake finds a non-empty source family such as logs, daily/weekly notes, session traces, or telemetry, a derived metric for that family should not become zero without explanation. Treat that as a parser/scope failure or an unavailable metric, not as evidence of no activity.
- Spot-check a few examples for each important metric family: entrypoints, logs/activity, session/request evidence, connection candidates, and size/depth.
- If a metric is based on a temporary parser, include enough method notes to let the user challenge the parser rather than only the value.

These checks should stay conceptual. They do not require a specific test framework or collector implementation.

## Repair Rule

When a user challenges a value, do not patch the report wording first.

1. Identify which concept was under-observed or mis-scoped.
2. Adjust intake assumptions, collector rules, telemetry scope, or parser scope.
3. Rerun collection.
4. Regenerate the report.
5. State what changed.

Common repair triggers include undercounted distributed logs, omitted session sources, cumulative token fields mistaken for per-request deltas, idle time folded into elapsed time, weak object recognition, or connection assumptions that ignore the user's actual routing model.
