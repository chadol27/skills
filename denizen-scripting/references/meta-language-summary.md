# Denizen Meta Language Summary

Use this as a compact map of `https://meta.denizenscript.com/Docs/Languages`. The Meta page is the technical reference layer for Denizen language concepts. Confirm exact command, tag, event, and object syntax in the linked Meta sections before changing parser-sensitive code.

Source:

- Language Explanations: `https://meta.denizenscript.com/Docs/Languages`

## How To Use The Meta Language Page

- Search it when the question is about how Denizen itself parses or interprets scripts.
- Use Guide docs for learning flow; use Meta Language docs for technical semantics.
- Use the top category list to jump by system: common terminology, comparables, console commands, scripting language, object system, script command system, script container system, script events, and tag system.

## Common Terminology

- `tick` normally means 1/20th of a second on a Minecraft server.
- `number` inputs are integer-like and may round decimals.
- `decimal` inputs allow fractional values.

When tuning timers or motion scripts, verify whether an argument expects ticks, seconds, durations, numbers, or decimals.

## Operators And Comparables

Common comparable operators include:

- equality: `==`, `equals`
- inequality: `!=`
- numeric comparison: `>`, `<`, `>=`, `<=`
- named numeric comparison: `more`, `less`, `or_more`, `or_less`
- collection checks: `contains`, `in`
- matcher checks: `matches`

Inside tags, raw `<` and `>` can collide with tag syntax. Prefer named comparison forms or escaped forms when a comparison operator is inside a tag argument.

## Console Commands And Debugging

- `/ex` runs a single Denizen script command in-game or from console.
- `/ex` creates a new queue each time, so queue-local definitions usually disappear immediately after that command.
- `/exs` creates a sustained queue and can preserve queue state between executions, but waits can block that sustained queue.
- `/denizen debug` toggles Denizen debug output.
- Add `debug: false` to individual script containers to reduce spam while keeping global debug available.
- Avoid globally disabling debug when troubleshooting, because it can hide real errors.
- `/denizen debug -r` plus `/denizen submit` records and shares debug output for support.

Use debug output and reload logs as primary evidence when fixing runtime Denizen failures.

## Script Files, Syntax, And Names

- Denizen scripts are stored in `.dsc` files.
- Script syntax is YAML-like, but Denizen applies its own parsing rules on top.
- Files and folders are mostly organization; script container names are global.
- Multiple containers can live in one file.
- Avoid duplicate container names across the full scripts folder.
- Indentation still matters because keys, child keys, lists, and values are hierarchical.

When reviewing a file, check both YAML-like indentation and Denizen-specific command syntax.

## Script Containers

Script containers are the basic unit inside `.dsc` files.

Common container types:

- `task`: reusable command list. Requires `script:`.
- `world`: event handlers. Requires `events:`.
- `command`: player/server command behavior.
- `procedure`: value-returning logic that determines a result.
- `data`: structured information read by other scripts, not command logic.
- other specialized containers exist for NPCs, items, formats, inventory, enchantments, economy, and more.

Task definitions can be documented with `definitions:` and passed by `run`. World events can include specific event lines and determinations.

## Script Events

- Modern script events support `on` and `after`.
- Use `on` when cancelling or changing the event result.
- Use `after` for follow-up behavior after the event completes.
- Standard event controls include `cancelled:<true/false>`, `ignorecancelled:true`, and `priority:#`.
- Lower numeric priority fires earlier; the default priority is `0`.
- Events expose context tags such as `<context.cancelled>` and event-specific contexts.

If an event handler needs a context tag, verify that the specific event exposes it in Meta event docs.

## Event Switches

Script event switches add extra requirements with `name:<value>` inside the event line.

Examples of switch concepts:

- `priority:#`
- `ignorecancelled:true`
- `cancelled:true`
- event-specific switches such as `every:#` on compatible events
- filters such as type, location, material, item, or other event-specific requirements

Prefer explicit switches when they make the event more precise, but confirm each switch is supported by that event.

## Flags

- Flags are persistent data attached to objects such as server, players, NPCs, entities, or locations.
- Server flags act like global script state and are stored in Denizen's server flag data.
- Flags can have nested map-like keys with dot notation.
- Flags can expire, but expiration is checked when flag tags are read; it is not an active event firing at the exact expiration time.
- Some internal flags use `__` prefixes. Avoid inventing normal project flags with that prefix.

Use definitions for temporary queue-local values and flags for persistent/cross-queue state.

## Object System

- Denizen uses typed objects such as players, entities, locations, items, inventories, lists, maps, scripts, durations, elements, and more.
- Tags usually return objects, not plain strings.
- Object tags can chain attributes, for example reading a player's location, then world, then name.
- Some objects are unique and can hold flags; others require specialized mechanisms or commands.

When a tag chain fails, identify the object type at each step rather than treating the whole tag as text.

## Tag System

- Tags use `<...>` and can chain attributes.
- Tag fallbacks can hide expected missing-data errors.
- Common fallback concepts include `if_null`, `exists`, and `is_truthy`.
- Do not add fallbacks to hide errors that should never happen.
- Prefer fallbacks only when missing context is valid behavior, such as code that intentionally supports console and player execution.

If a tag should always exist, let it error during development instead of masking it.

## Script Command System

- Commands are script entries in queues.
- Some commands can be waited on with `~` when they do asynchronous or delayed work.
- Commands can create entries that are later read through entry tags.
- Queue behavior matters for waits, definitions, task calls, and async-like operations.

For network calls, server-sensitive waits, and commands that save entries, check command docs for waitability and entry output.

## Parser-Sensitive Habits

- Treat Meta Language docs as the source for Denizen parser behavior.
- Do not assume plain YAML, shell, or JavaScript quoting rules apply.
- Prefer simple, sequential command entries over dense one-liners when tags, quotes, or definitions are involved.
- When a reload error points at a line, inspect nearby previous lines too; parser errors can surface after the real mistake.
- When exact behavior matters, verify in Meta command/tag/event docs and then reload on the server.
