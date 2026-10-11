# Core Denizen Patterns

## Official Docs

- Use the Beginner's Guide for learning-oriented explanations and canonical examples: `https://guide.denizenscript.com/`
- Use Meta Documentation for exact syntax and available commands, tags, object types, mechanisms, events, and language explanations: `https://meta.denizenscript.com/`
- Prefer a direct Meta lookup for commands such as `run`, `define`, `flag`, `webget`, `wait`, `determine`, and `execute`.
- Check the server's installed Denizen/addon versions for version-sensitive features; live Meta docs may describe a newer build than the target server.

## Containers

Every new `world`, `task`, `command`, and `procedure` container created by this skill must include `debug: false`. The setting is optional in Denizen syntax, but required by this skill's scripting convention.
Use two spaces for each indentation level in Denizen code examples and scripts.

Task container:

```yaml
example_task:
  type: task
  debug: false
  script:
  - narrate "Hello"
```

World event container:

```yaml
example_events:
  type: world
  debug: false
  events:
    after player joins:
    - run example_task
```

Command container:

```yaml
example_command:
  type: command
  debug: false
  name: example
  description: Example command.
  usage: /example
  permission: example.command
  tab completions:
    1: on|off|status
  script:
  - if <context.args.is_empty>:
    - narrate "/example on|off|status"
    - stop
```

Procedure container:

```yaml
example_check:
  type: procedure
  debug: false
  definitions: input
  script:
  - determine <[input].is_truthy>
```

## Definitions And Flags

- Use definitions for short-lived values inside the current queue.
- Read a definition as `<[name]>`.
- Use flags for state that must survive across queues, events, players, or restarts.
- Use server flags for global server automation state.

```yaml
- define message "Server started"
- flag server auto_shutdown_token:<util.random.uuid>
- if <server.has_flag[auto_shutdown_token]>:
  - narrate "Timer active"
```

## Task Calls

Source: [Meta `run` command](https://meta.denizenscript.com/Docs/Commands/run) — **Checked: 2026-10-11**.

`run` starts a new queue. Definitions in the caller do not automatically exist in that queue; pass only the values the target task needs. Prefer named definitions for clarity:

```yaml
notify_task:
  type: task
  debug: false
  definitions: message
  script:
  - narrate <[message]>

notify_events:
  type: world
  debug: false
  events:
    after player joins:
    - define message "<player.name> joined"
    - run notify_task def.message:<[message]>
```

Avoid inline quoted `def:` values for strings with spaces when the local server has shown parser sensitivity.

`run` supports several documented forms. Check the current [Meta `run` command page](https://meta.denizenscript.com/Docs/Commands/run) for exact syntax and edge cases:

- `def.name:<value>` passes an explicitly named definition; repeat it for multiple values.
- `defmap:<map>` passes a `MapTag` as named definitions.
- `def:<value>|<value>` passes positional definitions, mapped by the target container's `definitions:` key or numbered names when no key is declared.
- Pass lists/maps as typed objects when they must retain structure; consult the command docs for the supported single-definition form rather than flattening them into text.
- `run` is waitable. Use `~run` only when the caller needs to wait for the new queue to finish or needs its saved entry; verify this against the target Denizen version.

Example of multiple named values:

```yaml
- run notify_task def.message:<[message]> def.source:startup
```

Avoid assuming a child queue will inherit queue-local definitions. Use explicit definitions for data and flags or another persistent store only when state genuinely must cross queues or survive longer.

## Events

- Use `after` for observation or follow-up behavior that does not change the original event.
- Use `on` when the script must cancel or determine the event outcome.
- Keep event lines static; do not put runtime tags in the event line.
- Use context tags only inside the event or command context that provides them.

## Web/API Calls

For HTTP calls, prefer Denizen's `webget` command. Check the [official Meta `webget` command page](https://meta.denizenscript.com/Docs/Commands/webget) for exact syntax, waitability, entry outputs, headers, data/body arguments, and failure handling before writing the command. **Checked: 2026-10-11** for the guidance in this section.

Use `SecretTag` for URLs or header values when secrets already exist. Do not read `.env` or `.env*` files to discover secrets.

Treat remote response data as untrusted. Check request failure/status before parsing or taking action, and avoid exposing credentials or sensitive response contents in debug output. Confirm the installed Denizen version supports the chosen `webget` syntax and tags.
