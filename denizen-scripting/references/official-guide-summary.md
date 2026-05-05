# Denizen Beginner's Guide Summary

Use this as a quick concept map for the official Denizen Beginner's Guide. When exact syntax matters, confirm with Meta Documentation.

Sources:

- Guide home and table of contents: `https://guide.denizenscript.com/`
- Task scripts: `https://guide.denizenscript.com/guides/first-steps/task-script.html`
- Tags: `https://guide.denizenscript.com/guides/first-steps/tags.html`
- World scripts and events: `https://guide.denizenscript.com/guides/first-steps/world-script.html`
- If command: `https://guide.denizenscript.com/guides/basics/if-command.html`
- Definitions: `https://guide.denizenscript.com/guides/basics/definitions.html`
- Flags: `https://guide.denizenscript.com/guides/basics/flags.html`
- Loops: `https://guide.denizenscript.com/guides/basics/loops.html`
- Player commands: `https://guide.denizenscript.com/guides/basics/player-commands.html`
- Procedures: `https://guide.denizenscript.com/guides/basics/procedures.html`

## Guide Structure

The Beginner's Guide is concept-first. It teaches Denizen in this rough order:

1. Run commands manually with `/ex`.
2. Put commands into task scripts.
3. Use tags to read dynamic Minecraft and Denizen data.
4. Use world scripts and events to react to server/player actions.
5. Use `if`, definitions, flags, loops, player commands, queues, `run`, and procedures for real scripts.

Treat the Guide as the explanation layer and Meta Documentation as the syntax/reference layer.

## Task Scripts

- A task is a standalone script container that runs a list of Denizen commands.
- Tasks can be run manually with `/ex run task_name` or from another script with `run task_name`.
- Script names matter more than file names; multiple script containers can live in one `.dsc` file.
- The `script:` key contains the command list.
- Quoting may be needed for normal command arguments with spaces, but task definition passing is parser-sensitive. Prefer defining a value first, then passing it as a named definition such as `def.message:<[message]>`.

Use a task when reusable behavior should be called from commands, events, or other tasks.

## Tags

- Tags are dynamic expressions inside `<...>`.
- Tags can read player, NPC, entity, world, server, item, location, list, and custom data.
- Tags can chain subtags, for example `<player.location.world.name>`.
- Some tags accept input, for example list or lookup-style tags.
- A tag should be valid in the current context. Event-only context tags do not exist in unrelated tasks.

Use tags to read information; use commands to change state.

## World Scripts And Events

- A world script uses `type: world` and an `events:` block.
- Event lines are static text such as `after player breaks block:` or `on player places stone:`.
- Do not put runtime tags in event lines.
- `after` observes an event after it happened.
- `on` runs before or during the event and is the normal choice when cancelling or determining outcome.
- Event context tags like `<context.material>` depend on the specific event.

Use world scripts when behavior should trigger automatically from server, player, entity, block, or world events.

## If / Else

- `if` runs a nested block only when the condition passes.
- Boolean tags can be used directly, and `!` negates them.
- Comparisons include equality, numeric comparison, containment, and matcher checks.
- `&&` means all conditions must pass; `||` means any condition may pass.
- For complex mixed logic, prefer grouping or sequential guard clauses over very long conditions.
- `stop` is useful for early exits after a guard fails.

Use guard clauses for validations that should abort the script quickly.

## Definitions

- Definitions are short-term queue-local memory.
- Create them with `define`.
- Read them as `<[name]>`.
- Definitions disappear when the queue ends.
- Definitions do not automatically transfer into a new queue created by `run`; pass them explicitly with `def`, `defmap`, or named `def.name`.

Use definitions for temporary calculations, parsed arguments, selected entities, and messages passed into tasks.

## Flags

- Flags are long-term script-controlled memory attached to objects.
- Common targets include `server`, players, NPCs, entities, and locations.
- Server flags are useful for global automation state.
- Flags can persist indefinitely unless cleared or expired.
- The flag command uses data-action style syntax, for example `flag server name:value`.
- Use expirations for cooldowns or time-bounded state when appropriate.

Use flags for state that must survive across queues, events, players, or reloads.

## Loops

- `repeat` runs a block a fixed number of times.
- `foreach` runs once for each list entry and is usually better than manually indexing a list.
- `while` runs while a condition remains true.
- Use `wait` inside long-running or repeated loops so the server is not locked in one tick.
- Avoid `while true` style loops unless there is a very deliberate escape and delay strategy.
- `repeat stop`, `foreach stop`, and `while stop` end the relevant loop; `next` skips to the next iteration.

Prefer `foreach` for lists and be cautious with `while`.

## Player Commands

- Command containers use `type: command`.
- Required command metadata usually includes `name`, `description`, `usage`, `permission`, and `script`.
- Arguments are available through `<context.args>`.
- Tab completions improve UX but do not validate behavior.
- Never trust command input. Validate missing, extra, invalid, nonexistent, or unsafe arguments before using them.

Always guard indexed argument access with `context.args.size` first.

## Procedures

- Procedures are reusable value-returning scripts, used like custom tags.
- Procedures must `determine` a value.
- Procedures should not change external state.
- Use tasks for actions and procedures for questions/calculations.
- Passing lists through procedure context can be surprising; prefer a single object input or a map-like shape when preserving structure matters.

Use a procedure when the caller needs a computed value inline inside a tag expression.

## Problem-Solving Habit

- Reload after changes when possible.
- Read the exact Denizen error line and nearby script lines.
- Patch the reported file/path first before redesigning.
- If only static review is possible, say so and ask for reload output if runtime certainty matters.
