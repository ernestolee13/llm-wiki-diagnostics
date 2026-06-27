# 01. 구조 분석 (Intake Discovery)

This is a lightweight discovery contract. Its output is not a diagnosis; it is the context that lets later collection and reporting avoid hardcoding one user's wiki shape.

## Intake Execution Contract

Before any broad target scan, create the first inspectable artifact: a small run manifest or intake checkpoint. Then discover source families into compact artifacts rather than terminal scrollback.

The first minutes of intake should look like this:

1. Create an output area and write the run boundary: target wiki, guidance files, user hints, excluded previous diagnostics, and source families to inspect.
2. Write a source-inventory skeleton before creating any complex collector. It may list planned source families with `unchecked` status, expected role, cap, and unknowns.
3. Discover target source families at a bounded level and update the source inventory.
4. Store candidate paths, counts, roles, caps, and parseability in the source inventory artifact.
5. Print only a short summary: artifact path, candidate counts, selected primary source families, and important gaps.

If a discovery command would print a long list of paths, session files, trace records, logs, links, or JSON rows, redirect or summarize it into an artifact first. Long raw stdout is a failed intake reduction, even when the information is useful.

Do not begin a large parser or collector until both an initial run manifest and a source-inventory skeleton exist. These two files are the minimum recovery point for intake. If the next step is metric collection, Stage 2 should add a collection checkpoint before any larger parser is written.

## Goal

Produce a small `wiki_intake_profile` that answers:

- What counts as wiki operating components: guide/schema memory, metadata/schema rules, entrypoints, indexes, logs, retrieval routes, telemetry/session sources, generated output, and archive?
- How does the user expect an LLM to navigate the knowledge base?
- Which usage/session sources exist, and which agent or runtime produced them?
- Which assumptions are strong enough to proceed, and which require one targeted user confirmation?

Do not assume Obsidian wikilinks, Claude Code, Codex, Graphify, vector search, graph DBs, frontmatter, or subindexes are universal. Treat all of them as possible routing surfaces.

## Discovery Quality Questions

Before moving to collection, the intake should be able to answer:

- What evidence from this target environment, not from the generic contract, supports the inferred wiki model?
- If the user says a different file, graph, tool, or session family is authoritative, which counts or interpretations would change?
- Which sources are live knowledge, which are operating context, which are request/session evidence, and which are generated or archive material?
- What important evidence may live outside the wiki root, and was it checked, skipped, inaccessible, or intentionally excluded?
- Which assumptions are strong enough to proceed, and which should be shown as report caveats or user questions?

## Research-Informed Intake Questions

Use these as discovery prompts, not as required schema fields.

- Content inventory versus content audit: can the run identify the objects first, then separately decide which ones are healthy, stale, redundant, trivial, or improvement candidates?
- Wiki maintenance surfaces: does the local system expose anything equivalent to orphan/lonely pages, dead-end pages, wanted/missing pages, long pages, old pages, most-linked pages, or broken references?
- Search and retrieval intent: is there any source that records zero-result searches, fallback searches, selected results, query refinements, route changes, or user feedback?
- Request trace shape: does any session, hook, trace, or telemetry source expose request boundaries, timestamps, child operations, attributes, token fields, selected objects, or status fields?
- Graph reachability: if the wiki uses graph edges, Graphify, semantic neighbors, tags, or custom routes, can the run identify connected components, isolated neighborhoods, hub nodes, and disconnected important nodes?
- Quality and lifecycle: are freshness, provenance, review status, citation/source, owner, duplicate/alias drift, or archive decisions visible anywhere?

If these questions cannot be answered from local evidence, carry them forward as source gaps. Do not pretend the metric is unavailable just because the first file-tree scan did not expose it.

## Practical Defaults

When no custom structure is obvious, a useful first-pass assumption is that a typical LLM wiki may have:

- a guide/schema memory that tells the agent how to use the wiki
- metadata, frontmatter, tag, relation, graph, or naming schema that says how knowledge objects should be typed, trusted, routed, or filtered
- one or more indexes, hubs, dashboards, or maps of content
- a log, changelog, daily note stream, or activity record
- ordinary content pages or knowledge objects

Use these as discovery starting points only. If the wiki uses graph routes, vector search, tags, frontmatter relations, Bases/Dataview, MCP tools, or another retrieval layer, describe that layer instead of forcing the guide/index/log model.

## Discovery Pass

Scan first, ask later.

This stage must inspect the target wiki or target connectors at a safe aggregate level. Reading this Markdown contract is not enough. If the run is allowed to use only this package as guidance, that means only the guidance source is restricted; it does not forbid listing target files, sampling safe metadata, inspecting representative structured keys, or counting source families in the target wiki.

A valid intake should leave environment-specific evidence such as real candidate path patterns, rough counts, role guesses, and representative structures. If the target wiki cannot be inspected, mark the intake as partial and explain what access or scope is missing.

Look for:

- Guide/schema memory: `CLAUDE.md`, `AGENTS.md`, README files, skill folders, MCP/tool rules, local agent instructions.
- Schema and classification surfaces: frontmatter/property conventions, tag systems, relation fields, graph edge types, folder naming rules, Bases/Dataview schemas, validation rules, or source/provenance fields.
- Entrypoints and routing surfaces: `index.md`, `_index.md`, dashboards, maps of content, project hubs, tag conventions, Bases/Dataview views, Graphify exports, graph DB routes, vector/search indexes, custom MCP tools.
- Operational records: `log.md`, `_log.md`, daily/weekly notes, changelogs, append-only activity files.
- Quality and trust surfaces: source fields, provenance notes, created/updated dates, status fields, review markers, citation links, feedback records, or validation logs.
- Usage records: Codex/Claude/Cursor/local LLM transcripts, query logs, hook JSONL, MCP middleware logs, graph/vector/search traces. These may live inside the wiki, in project-local runtime folders, or in user-level agent stores outside the wiki root. Treat configuration files, skill documents, ordinary logs, backups, and generated diagnostic files as supporting context or exclusions, not as session transcripts, unless they contain structured request/runtime events.
- Usage-context evidence: project memory, agent memory, skill documents, command/routing rules, prompt-routing state, recurring workflow notes, and startup guides. These may not be telemetry, but they explain how the user tends to query, write, ingest, index, lint, or navigate the wiki.
- Session boundaries: whether traces include main sessions, subagents, compacted summaries, retries, duplicated runs, or unrelated sessions that only mention the same tools.
- Session-parser opportunity: if query/hook telemetry is absent, identify whether existing wiki-use sessions are structured enough for a temporary parser to infer request windows, tool-call counts, document/object reads, depth, elapsed time, and token signals.
- Exclusions: generated reports, plugin build outputs, tests, dependencies, archives, templates, backup folders, runtime folders, agent scratchpads, and diagnostic artifacts unless the user says those are part of the target wiki.
- Examples and validation outputs: treat as reference material unless the user explicitly says the diagnostic package itself is the target.

Keep fresh-run boundaries clear. Previous diagnostic reports, validation directories, temporary collectors, scratch JSON, copied sample metrics, or old HTML reports are not target evidence. They may explain how this diagnostic package was developed, but they should not define the current wiki's document counts, session counts, request cost, or connection health unless the user explicitly asks for a before/after comparison.

Separate two evidence scopes:

- `live wiki content`: files counted as knowledge objects for size, connection, trust, and content-quality metrics.
- `usage/session evidence`: runtime or telemetry traces that may explain request behavior even when they are not live wiki content.
- `usage-context evidence`: memories, skills, startup instructions, routing rules, and workflow notes that explain how to interpret sessions or choose parser targets.

Do not let content exclusions silently remove usable session evidence. Excluding hidden folders or generated files from `live wiki content` must not automatically exclude project-local or user-level session traces from `usage/session evidence`. If a candidate source is skipped for privacy, size, format, or relevance, record the reason at an aggregate level.

Do not stop at sources inside the wiki root when the user's actual agent history is likely stored elsewhere. The intake should explicitly record whether project-local runtime folders, user-level agent/session stores, editor chat stores, and local LLM history stores were checked, inaccessible, intentionally skipped, or absent. If a sandbox prevents checking a likely source, preserve that as an access/scope gap rather than concluding that request-level evidence is unavailable.

Use breadth before depth for large external stores. Intake should not fully crawl a huge user-level history before writing anything. First record existence, rough size, candidate patterns, access status, and a sampling cap; then continue to a bounded target-matching probe in Stage 2. If the store is too large to scan promptly, that is a scoped partial result, not a reason to leave no checkpoint.

Make the first write early. Before building a collector, deep-scanning sessions, or preparing the final report, write a small run manifest or intake checkpoint with the target wiki, output location, current-run boundary, excluded derived artifacts, user hints, source families to inspect, and current unknowns. The file can be revised later. Its purpose is to prevent long terminal-only discovery and to make partial progress inspectable.

Prefer source inventories over source dumps. Intake should answer "what kind of evidence exists and can it support later metrics?" It should not pull full logs, transcripts, previous reports, large JSON artifacts, or long file-path lists into the conversation just to decide what to parse. For large trees, output counts, role buckets, and a few bounded examples instead of printing every path.

Candidate discovery should be artifact-first. If the run finds many possible session files, subagent traces, logs, graph files, search traces, indexes, or generated artifacts, write those candidates to a source-inventory artifact with counts, roles, and caps. Terminal output should normally say only how many candidates were found and where the inventory was saved. Do not print dozens or hundreds of candidate paths as the primary discovery result.

Path and trace discovery commands should not stream their raw result lists to stdout. Redirect, capture, or summarize them into an artifact first, then print only a bounded summary such as candidate count, selected primary source family, skipped families, and artifact path. If a command would naturally produce more than a few lines, treat it as data collection, not conversation output.

Leave a `session_source_audit` or equivalent note. It should list each relevant family as checked, inaccessible, skipped, absent, or secondary, with a short reason. Common source families include project-local runtime folders, user-level agent project histories, user-level session directories, editor/plugin chat stores, local LLM histories, query logs, hooks, MCP wrappers, graph/search traces, and in-wiki session summaries. The names are examples; the important point is to show that request-level evidence was looked for beyond ordinary wiki files.

Separate in-wiki runtime folders from user-level session stores even when their labels look similar. A target-root hidden folder can explain local project state, but it should not be treated as proof that a user-level agent history was absent or unusable. If the environment exposes external session stores, audit them as separate candidates and prefer target-matched histories when they exist.

If the user supplies a usage hint such as an agent name, project name, session label, editor, or local LLM runtime, use it to prioritize candidate discovery. Treat the hint as routing evidence, not as a guarantee: verify that the source exists, roughly matches the target wiki, and contains parseable request or tool events.

For local self-run diagnostics, it is acceptable to inspect richer memories, skills, and representative session text to infer intent categories and parser scope. The important boundary is artifact design: store the extracted usage model, parser hints, and caveats, not unnecessary full transcripts.

For session, telemetry, graph, search, or other non-document evidence, leave a small source inventory. It should describe:

- source family and path or connector pattern
- whether it is live wiki content, usage evidence, configuration, cache, or generated output
- rough object count or file count
- representative structure at a safe level, such as top-level keys, event names, timestamp fields, token fields, object-reference fields, or graph/search result fields
- parseability: likely parseable, weak candidate, not parseable, unavailable, or skipped
- likely metrics it can support, such as request windows, tool calls, objects touched, elapsed time, tokens, route depth, selected rank, hit/fallback, or only source presence
- caveats, including mixed sessions, missing per-request fields, duplicated logs, privacy limits, or provider-specific schema

This inventory should be substrate-neutral. Do not name a source authoritative because it has a familiar filename; infer authority from structure, freshness, and how the user's workflow appears to use it.

Be strict about request-level authority. Agent lifecycle events, project summaries, replay summaries, session start/stop records, or hook status events are useful workload context, but they are not enough for per-request search/read/time/token metrics unless they expose user-visible request boundaries, timestamps, tool/object interactions, and usage fields.

When multiple request-trace candidates exist, choose and document a primary source by relevance to the target wiki, not by convenience. Prefer traces whose project path, working directory, project name, active skill, referenced entrypoints, or object paths match the target wiki. Record plausible but non-primary sources as secondary, skipped, or out-of-scope with a reason. A current Codex session, editor trace, or generic runtime log should not displace a target-matched Claude/local-LLM/session history just because it is easier to parse.

Avoid a purely generic inventory. It is fine to mention common possibilities such as sessions, telemetry, graph exports, or vector indexes, but the useful part of intake is the target-specific evidence: which of those actually appear, what shape they roughly have, and why they are or are not parseable.

Inspect candidate shape without dumping raw traces. For JSON, JSONL, chat, session, hook, graph, search sources, or Markdown activity logs, prefer top-level keys, value types, event-type counts, line counts, timestamp/usage-field availability, schema samples with preview/content fields omitted, and short redacted summaries. Do not print raw prompts, raw answers, full records, activity-log entries, or note bodies to terminal logs merely to decide whether a source is parseable. Avoid commands that dump raw trace or log lines by default; `head`, `cat`, or unrestricted `sed` over session/log files are not acceptable shape probes unless the output is redacted first. Use a small redacting probe that removes, hashes, or length-counts fields such as `prompt`, `answer`, `content`, `message`, `input_preview`, `output_preview`, `arguments`, `result`, `body`, and `tool_output` before output.

During discovery, establish denominator names before counting. Useful role families include:

- `scanned_files`: everything considered before exclusions
- `excluded_generated_or_archive`: build outputs, generated reports, dependencies, hidden runtime folders, agent scratchpads, archives, diagnostic artifacts, and similar non-live material
- `active_wiki_objects`: live knowledge objects after exclusions
- `content_objects`: ordinary content/knowledge objects, excluding guides, entrypoints, logs, daily utility pages, and generated output unless the user says otherwise
- `entrypoint_objects`: indexes, hubs, dashboards, maps, graph/vector route surfaces, or project subindexes
- `guide_or_startup_objects`: files expected to be read as operating context
- `schema_or_rule_surfaces`: metadata rules, frontmatter schemas, tag conventions, graph relation types, folder naming rules, source/provenance conventions, or validation rules
- `operational_record_sources`: global logs, project logs, daily/weekly notes, changelogs, or equivalent activity records
- `activity_events`: parsed log entries, dated notes, date mentions, filesystem events, or other activity proxies

These names are examples. The important rule is that later reports do not collapse different denominators into one vague `document count` or `log count`.

## When To Ask

Ask only when the answer changes collection or interpretation.

Good questions clarify:

- Whether a file is a guide, schema/rule surface, index, log, or ordinary content when it appears to play multiple roles.
- Which file family is the authoritative entrypoint?
- Whether `_index.md` files are content pages, folder indexes, or project hubs.
- Whether frontmatter, tags, folders, graph edges, Bases/Dataview, or a custom schema are part of the expected routing model.
- Whether logs are global-only, project-local, daily-note based, or mixed.
- Whether daily notes should be counted as logs, activity records, ordinary content, or excluded utility pages.
- Which agent/session family should be analyzed first when multiple traces exist.
- Whether subagent traces, compacted traces, or external session directories should be included, separated, or excluded.
- Whether session parsing should focus only on lookup-like requests, also include write/maintenance requests, or separate both.
- Whether links, tags, frontmatter, graph edges, semantic neighbors, or custom routes define "connected".
- Whether freshness, provenance, citations, review status, or user feedback are meaningful quality signals in this wiki.

Do not ask about thresholds, chart preferences, or wording before the first report. Those can be adjusted after a default report exists.

## Output Shape

Return or save aggregate metadata only:

If saving JSON, make it valid JSON and validate it before completion. Do not paste raw multiline excerpts into JSON strings unless they are escaped. Prefer arrays of short strings, path examples, field names, and summarized structure notes.

```json
{
  "schema_version": "llm-wiki-intake-profile-v0",
  "generated_at": "ISO-8601",
  "privacy_mode": "paths_and_aggregate_only",
  "detected_surfaces": {
    "guides": [],
    "schemas_or_rules": [],
    "entrypoints": [],
    "logs": [],
    "retrieval_surfaces": [],
    "quality_surfaces": [],
    "session_sources": [],
    "telemetry_sources": [],
    "generated_or_excluded": []
  },
  "source_inventory": [],
  "operating_component_profile": {
    "guide_or_startup": [],
    "schema_or_rule_surfaces": [],
    "entrypoints_or_indexes": [],
    "operational_records": [],
    "retrieval_routes": [],
    "telemetry_or_session_sources": [],
    "component_gaps": []
  },
  "inferred_model": {
    "wiki_substrate": "markdown|obsidian|graph|vector|hybrid|custom|unknown",
    "index_strategy": "flat|folder_indexes|project_hubs|graph|vector|custom|unknown",
    "log_strategy": "single_global|global_plus_project|daily_only|custom|unknown",
    "connection_model": "wikilink|tag|frontmatter|graph|semantic|hybrid|unknown",
    "quality_model": "provenance|freshness|review_status|citations|feedback|custom|unknown",
    "main_agent_candidates": [],
    "confidence": "measured|inferred|unavailable",
    "evidence": []
  },
  "page_role_overrides": [],
  "collector_hints": [],
  "report_hints": [],
  "session_scope_hints": [],
  "usage_context_hints": [],
  "questions": [],
  "assumptions_if_unanswered": []
}
```

## Downstream Contract

- Session collectors must use this profile before generic file-name heuristics.
- If measured query/hook telemetry is missing, generated session parsers should use this profile to choose relevant sessions and separate lookup, write/maintenance, mixed, and unrelated requests.
- Session parsers should use `usage_context_hints` from memories, skills, and guide documents to recognize local command names, route families, object roles, and request types.
- Reports must show important assumptions when they affect interpretation.
- If the user later corrects a role or routing assumption, regenerate the intake profile first, then rerun collectors and reports.
- Keep raw note bodies and raw prompts out of the profile unless the user explicitly opts in.
- Preserve the boundary between product instructions, sample data, generated validation output, and live wiki evidence.
- Preserve the boundary between previous diagnostic artifacts and current target evidence. If an old artifact is discovered, record it as excluded or reference-only unless the user explicitly requested comparison.

The intake stage should be inspectable on its own. Before metric collection, leave a short checkpoint that states what was counted as live wiki content, what was treated as session/telemetry evidence, what was excluded, which connection model is assumed, and which weak assumptions would change later metrics. If no such artifact exists, intake is not complete even when the terminal output contains useful observations.

If the checkpoint says the target wiki was not scanned, downstream stages should not treat the intake as complete. They should rerun intake with target evidence access or carry an explicit `partial_intake` caveat.
