---
name: denizen-scripting
description: Write, edit, review, and troubleshoot DenizenScript scripts for Minecraft servers, especially .dsc files, task/world/command/procedure containers, event handlers, tags, flags, definitions, webget integrations, reload errors, and parser-sensitive Denizen syntax. Use when Codex is asked to implement Denizen automation, tune existing Denizen scripts, explain Denizen behavior, or inspect Denizen runtime errors.
---

# Denizen Scripting

## Workflow

1. Inspect the current `.dsc` files before changing behavior. Preserve local script style, naming, messages, and existing containers.
2. If a script folder has local instructions such as `AGENTS.md`, read them before editing.
3. Use the official docs by task:
   - Beginner concepts and examples: `https://guide.denizenscript.com/`
   - Command, tag, event, object, mechanism, and language lookup: `https://meta.denizenscript.com/`
4. Prefer small edits to existing containers over rewrites unless the user asks for a redesign.
5. Validate with the strongest available signal: Denizen reload/runtime logs when available, otherwise static inspection with line numbers and targeted searches.

## Reference Decision Tree

- Initial design or concept refresh: read `references/official-guide-summary.md`.
- Finding the right official reference page: read `references/meta-lookup-map.md`.
- Parser behavior, queues, containers, event switches, operators, tag fallbacks, or flags: read `references/meta-language-summary.md`.
- Writing or reviewing normal Denizen syntax: read `references/core-patterns.md`.
- Debugging reload errors or touching command arguments, task calls, webhooks, timers, or cancellation flags: read `references/common-pitfalls.md`.

## Implementation Rules

- Keep new reusable behavior in a `task` when multiple events or commands call the same logic.
- Add `debug: false` when local project style expects quiet containers. In this user's Denizen repo, new `world`, `task`, `command`, and `procedure` containers should include it.
- Guard `context.args.size` before any `context.args.get[n]` access.
- Avoid compound conditions that reference an indexed argument after a size check; split the checks into sequential `if` blocks.
- Define messages or values with spaces before passing them to another task, then pass named definitions with `def.name:<[name]>`.
- Use server flags for cross-queue or persistent state. Use definitions for queue-local temporary values.
- For web/API work, prefer secret-backed URLs or headers when the project already has a secret, and use `~webget` for network calls.
- For event cancellation or determinations, confirm whether the event must be `on` rather than `after`.

## Review Checklist

- Container names, `type`, indentation, and section keys are valid.
- Event lines are static text and contain no runtime tags.
- Every indexed context argument is guarded before access.
- `run` calls pass definitions in a parser-safe shape.
- Long waits use the intended time unit and `system` when real elapsed time matters.
- Flags used for cancellation or race control are cleared or tokenized.
- User-facing messages match the requested language and tone.
- The answer states whether validation was runtime reload/testing or static review only.
