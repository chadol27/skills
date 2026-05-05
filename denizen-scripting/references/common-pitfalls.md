# Common Denizen Pitfalls

## Command Arguments

Always guard before indexed access:

```yaml
- if <context.args.size> < 2:
    - narrate "/example remove [index]"
    - stop
- if !<context.args.get[2].is_integer>:
    - narrate "Index must be a number."
    - stop
- define index <context.args.get[2]>
```

Avoid this shape because indexed tags can still be evaluated too early in some parser paths:

```yaml
- if <context.args.size> < 2 || !<context.args.get[2].is_integer>:
    - stop
```

When adding command branches, update all of these together:

- `usage`
- `tab completions`
- argument guards
- permission behavior if the command exposes a new privileged action

## Task Calls With Messages

If a message has spaces or user-facing text, define it first:

```yaml
- define message "Server started"
- run discord_webhook_send def.message:<[message]>
```

Avoid passing quoted messages inline through `def:` when reload logs show `Run command path is missing required 'path:' prefix`, invalid script-name errors, or `Must define a SCRIPT to be run`.

## Timers And Cancellation

For long real-world timers, use `wait ... system` and tokenized flags:

```yaml
- define token <util.random.uuid>
- flag server auto_shutdown_token:<[token]>
- wait 23h system
- if !<server.has_flag[auto_shutdown_token]> || <server.flag[auto_shutdown_token]> != <[token]>:
    - stop
```

Check exact units. `5m` and `5s` are very different, and startup delay changes the final warning/stop time.

## Webhooks And Webget

Keep webhook send logic in a reusable task when multiple events call it.

Prefer an existing Denizen secret such as `<secret[discord_webhook]>` over passing raw webhook URLs around.

Check the official `webget` command docs before adding POST bodies, headers, saved entries, or failure handling. Keep examples in this skill minimal so they do not lock in stale command syntax.

## Static Review Limits

If no live server reload is available, state that validation was static only. For runtime confidence, ask the user to run or provide output from:

```text
/ex reload
```

Then inspect the Denizen reload log/error lines and patch the reported file first.
