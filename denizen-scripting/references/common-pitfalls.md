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

## Determine And Queue Flow

Source: [Meta `determine` command](https://meta.denizenscript.com/Docs/Commands/determine) — **Checked: 2026-10-11**.

`determine` sets the script's outcome and ends the current queue by default, similarly to `stop`. If commands must continue afterward, use `determine passively`:

```yaml
- determine passively cancelled
- narrate "This command still runs"
```

Without `passively`, commands after `determine` will not run. For a procedure, ending the queue after determining its result is normally intended. If making multiple determinations in one queue, use `passively` on each determination that must not end that queue.

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

Check the [official Meta `webget` command page](https://meta.denizenscript.com/Docs/Commands/webget) before adding POST bodies, headers, saved entries, or failure handling. Current syntax uses `save:<name>` and returned data is read through entry tags such as `<entry[request].failed>`; check the docs for exact tags and whether the call should use `~webget` (normally yes). **Checked: 2026-10-11**.

Treat remote responses as untrusted input. Check `<entry[request].failed>` and the status/result shape before parsing or acting on a response, and avoid logging secret-bearing URLs, headers, or response data. Use configured `SecretTag` values for credentials; never paste secret values into scripts or examples. A secret must be supplied as a whole URL or header value, not interpolated into a public URL string.

Confirm the target server's Denizen version supports the documented syntax and tags before relying on them, especially when maintaining older servers.

## Version And Addon Compatibility

- Check the installed Denizen and relevant addon versions when an API appears missing, changed, or inconsistent with Meta docs.
- Verify events, contexts, switches, commands, tags, and mechanisms individually; similar names do not guarantee availability or identical behavior across versions/addons.
- Prefer current Meta documentation, but treat the running server's reload/debug output as authoritative for its installed build. Report any version mismatch rather than silently rewriting around it.
- For nontrivial compatibility work, note the environment in a compact record before diagnosing:

  ```text
  Minecraft:
  Server platform/build:
  Denizen:
  Relevant addons and versions:
  ```

  Use version output, startup logs, or the server's plugin/version listing; do not infer addon presence from script names alone.

## Reload Error Triage

Before editing a reported failure, collect:

1. The exact error text and the complete relevant reload/debug log block (not only a paraphrase).
2. The named file and line/container, plus nearby lines before and after the reported location. Parser errors may surface after the actual malformed entry.
3. Steps that reproduce the issue and whether it occurs on reload, command execution, or a particular event.
4. Minecraft/platform, Denizen, and relevant addon versions when feature support may matter.

Fix the earliest concrete error first, reload again, and then investigate any subsequent errors. Do not redesign unrelated containers based only on one parser message. Never ask the user to share credentials or unredacted secret-bearing output.

## Script Format And Validation

`.dsc` files use Denizen's YAML-like syntax and parser rules. A generic YAML parser or formatter cannot prove a script will load and may rewrite valid Denizen syntax. Prefer Denizen's own reload as the validation step; otherwise perform focused static checks and label them static-only.

Static review can safely include targeted searches for `context.args.get[`, new container declarations and their `debug: false` keys, duplicate container names, runtime tags in event headers, and changed call sites. These checks require human interpretation: a text match cannot prove argument guards short-circuit safely or that a tag/context exists. Do not report static searches as a parser pass or runtime test.

## Static Review Limits

If no live server reload is available, state that validation was static only. For runtime confidence, ask the user to run or provide output from:

```text
/ex reload
```

Then inspect the complete relevant Denizen reload log block and patch the earliest reported failure first. Ask for the server/version details above if the error could be version- or addon-dependent.
