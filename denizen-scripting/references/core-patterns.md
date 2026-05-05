# Core Denizen Patterns

## Official Docs

- Use the Beginner's Guide for learning-oriented explanations and canonical examples: `https://guide.denizenscript.com/`
- Use Meta Documentation for exact syntax and available commands, tags, object types, mechanisms, events, and language explanations: `https://meta.denizenscript.com/`
- Prefer a direct Meta lookup for commands such as `run`, `define`, `flag`, `webget`, `wait`, `determine`, and `execute`.

## Containers

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

Prefer named definitions when calling reusable tasks:

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

## Events

- Use `after` for observation or follow-up behavior that does not change the original event.
- Use `on` when the script must cancel or determine the event outcome.
- Keep event lines static; do not put runtime tags in the event line.
- Use context tags only inside the event or command context that provides them.

## Web/API Calls

For HTTP calls, prefer Denizen's `webget` command. Check the official Meta command docs for exact syntax, waitability, entry outputs, headers, data/body arguments, and failure handling before writing the command.

Use `SecretTag` for URLs or header values when secrets already exist. Do not read `.env` or `.env*` files to discover secrets.
