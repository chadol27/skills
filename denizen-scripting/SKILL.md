---
name: denizen-scripting
description: Write, edit, review, migrate, and troubleshoot DenizenScript for Minecraft servers, especially .dsc files, task/world/command/procedure containers, event handlers, tags, flags, definitions, webget integrations, reload errors, version compatibility, and parser-sensitive syntax. Use when asked to implement Denizen automation, tune scripts, explain Denizen behavior, or inspect runtime errors.
---

# Denizen Scripting

## Workflow

1. Inspect the current `.dsc` files before changing behavior. Preserve project conventions and check for duplicate container names across the scripts folder.
2. If a script folder has local instructions such as `AGENTS.md`, read them before editing. Follow the `debug: false` convention below in addition to any project-specific conventions.
3. For troubleshooting, obtain the exact error text and surrounding log lines, the affected file/container, reproduction steps, and server versions before changing behavior. Identify Minecraft/platform, Denizen, and relevant addon versions when version-dependent behavior or an error is involved. Use current official docs by task, and confirm compatibility with the installed version when possible:
   - Beginner concepts and examples: `https://guide.denizenscript.com/`
   - Command, tag, event, object, mechanism, and language lookup: `https://meta.denizenscript.com/`
4. Prefer small edits to existing containers over rewrites unless the user asks for a redesign. Remember `.dsc` is Denizen's YAML-like format, not plain YAML; generic YAML validation does not establish Denizen validity.
5. Validate with the strongest available signal: Denizen reload/runtime logs when available, otherwise static inspection with line numbers and targeted searches. Be explicit about what was and was not runtime-tested.
6. Treat local reference summaries as navigation aids, not canonical syntax. Recheck parser-sensitive or version-sensitive details in official docs; when updating a summary, record the exact official source and date checked for the material changed.

## Reference Decision Tree

- Initial design or concept refresh: read `references/official-guide-summary.md`.
- Finding the right official reference page: read `references/meta-lookup-map.md`.
- Parser behavior, queues, containers, event switches, operators, tag fallbacks, or flags: read `references/meta-language-summary.md`.
- Writing or reviewing normal Denizen syntax: read `references/core-patterns.md`.
- Debugging reload errors or touching command arguments, task calls, webhooks, timers, or cancellation flags: read `references/common-pitfalls.md`.

## Implementation Rules

- Keep new reusable behavior in a `task` when multiple events or commands call the same logic.
- Include `debug: false` in every new `world`, `task`, `command`, and `procedure` container created by this skill.
- Use two spaces per indentation level in every Denizen script and code example.
- Guard `context.args.size` before any `context.args.get[n]` access.
- Avoid compound conditions that reference an indexed argument after a size check; split the checks into sequential `if` blocks.
- Define messages or values with spaces before passing them to another task, then pass named definitions with `def.name:<[name]>`.
- Use server flags for cross-queue or persistent state. Use definitions for queue-local temporary values.
- For web/API work, prefer secret-backed URLs or headers when the project already has a secret, and use `~webget` for network calls.
- For event cancellation or determinations, confirm whether the event must be `on` rather than `after`.
- Do not assume a command, tag, event, context, mechanism, or addon feature exists on the target server: check exact Meta docs and version/addon availability before using it.
- Never include credentials or secret values in scripts, examples, logs, or commits. Reuse configured Denizen secrets; do not inspect secret files to discover values.
- For API calls, verify HTTP method, headers/data format, waitability, saved-entry tags, and failure/status handling against the current `webget` documentation.

## Review Checklist

- Container names, `type`, indentation, and section keys are valid.
- Event lines are static text and contain no runtime tags.
- Every indexed context argument is guarded before access.
- `run` calls pass definitions in a parser-safe shape.
- New `world`, `task`, `command`, and `procedure` containers include `debug: false`.
- Denizen script indentation advances in two-space increments and uses spaces, not tabs.
- Script container names are unique across the scripts folder.
- Targeted text searches for risky patterns were reviewed; search results are evidence for manual review, not a Denizen parser/linter result.
- Long waits use the intended time unit and `system` when real elapsed time matters.
- Flags used for cancellation or race control are cleared or tokenized.
- User-facing messages match the requested language and tone.
- Version-specific features and addon dependencies are confirmed for the target server.
- External calls handle expected failures and do not expose credentials in debug output.
- The answer states whether validation was runtime reload/testing or static review only.
