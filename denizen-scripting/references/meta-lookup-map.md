# Denizen Meta Lookup Map

Use this to choose the right Denizen Meta Documentation section quickly. Meta is the exact reference layer; the Beginner's Guide is the learning/explanation layer.

Base docs:

- Commands: `https://meta.denizenscript.com/Docs/Commands`
- Tags: `https://meta.denizenscript.com/Docs/Tags`
- Object Types: `https://meta.denizenscript.com/Docs/ObjectTypes`
- Mechanisms: `https://meta.denizenscript.com/Docs/Mechanisms`
- Events: `https://meta.denizenscript.com/Docs/Events`
- Language Explanations: `https://meta.denizenscript.com/Docs/Languages`
- NPC Actions: `https://meta.denizenscript.com/Docs/Actions`

## Decision Map

- Need to make something happen: use Commands.
- Need to read data: use Tags.
- Need to know what kind of value a tag returns: use Object Types.
- Need to change an object's property/state: use Mechanisms, usually with `adjust`.
- Need to react to Minecraft/server activity: use Events.
- Need to understand parser behavior, queues, script containers, event switches, operators, flags, or tags conceptually: use Language Explanations.
- Need Citizens NPC assignment/interact behavior: use NPC Actions plus the relevant container docs.

## Commands

Commands are Denizen script entries that usually start with `-` in a queue. Look up command docs when you need syntax, arguments, waitability, saved entries, determinations, or examples.

High-value command lookups:

- `run`: calling tasks and passing definitions.
- `define`, `definemap`: queue-local values.
- `flag`: persistent object/server/player state.
- `if`, `choose`: branching.
- `repeat`, `foreach`, `while`: loops.
- `wait`, `waituntil`: timing and queue delay.
- `webget`: HTTP calls and webhook/API integration.
- `determine`: event/procedure output.
- `execute`: running server/player commands.
- `narrate`, `announce`: player-facing messages.
- `debug`: troubleshooting output.
- `stop`: guard-clause exits.
- `customevent`: custom world event flow.
- `adjust`: applying object mechanisms.

Check command docs for:

- exact syntax line
- whether `~command` wait syntax is supported
- `save:<name>` and `<entry[name]...>` outputs
- related tags and guide pages
- examples using lists, maps, durations, or objects

## Tags

Tags read data and return typed objects. Look up tag docs when a script needs dynamic values, chained attributes, fallbacks, or context data.

High-value tag bases:

- `<player>` and `PlayerTag` attributes.
- `<server>` for online players, server flags, worlds, NPCs, and global state.
- `<context>` inside events and command containers.
- `<entry[name]>` after commands that save results.
- `<util>` for UUIDs, time, random, parsing helpers, and general utilities.
- `<script>` for script metadata.
- `<queue>` for current queue information.
- `<list[...]>` and ListTag attributes.
- `<map[...]>` and MapTag attributes.
- `<element[...]>` and ElementTag text/number/boolean helpers.

Check tag docs for:

- return type
- valid input parameters
- whether a tag is context-dependent
- whether it can return null
- related mechanisms or commands

Do not add `if_null` or similar fallbacks until you know missing data is expected behavior.

## Events

Events belong in `world` containers. Look up event docs when you need exact event lines, contexts, cancellation support, determinations, switches, or generated examples.

Common event categories:

- Block: block breaks, places, burns, explodes, forms, grows.
- Entity: damage, death, spawn, target, explode, move.
- Item: drop, pickup, craft, consume, inventory flows.
- Player: join, quit, command, chat, interact, move, teleport.
- Server: start, shutdown, scripts reload, ticks/time-based events.
- World: weather, time, chunk, portal, world state.
- NPC: Citizens/Denizen NPC behavior.

Check event docs for:

- exact event line variants
- whether to use `on` or `after`
- contexts such as `<context.location>` or `<context.entity>`
- `Cancellable True/False`
- valid determinations
- `Has Player` / `Has Location` generated switches
- event-specific switches like `in:<area>`, `flagged:<flag>`, or `permission:<node>`

Use `on` when you must cancel or determine the event result. Use `after` for observation/follow-up.

## Object Types

Object Types explain what values are, how they identify themselves, what attributes/mechanisms they support, and whether they can hold flags.

Common object types to check:

- `ElementTag`: text, booleans, numbers.
- `ListTag`: lists and list operations.
- `MapTag`: map/key-value data.
- `DurationTag`: time durations such as `5s`, `23h`.
- `TimeTag`: absolute times.
- `PlayerTag`: online/offline player data.
- `EntityTag`: mobs, projectiles, players-as-entities.
- `LocationTag`: positions and vectors.
- `WorldTag`: world data.
- `ItemTag`: item stacks and metadata.
- `InventoryTag`: inventories.
- `MaterialTag`: block/item material types.
- `CuboidTag`, `EllipsoidTag`, `PolygonTag`: areas.
- `ScriptTag`: script metadata.
- `QueueTag`: running queue data.

Use Object Types when a tag chain is confusing. Identify the type returned at each step, then choose valid attributes from that type.

## Mechanisms

Mechanisms change object properties and are usually applied with `adjust`, `adjustblock`, or specific commands.

Look up mechanisms when you need to:

- change entity custom names, AI, health, visibility, equipment, or velocity
- change item display name, lore, enchantments, flags, or custom data
- change inventory properties
- modify material/block state
- adjust player state or client-facing display
- apply global server/system changes

Check mechanism docs for:

- object type
- input type
- whether the change persists
- related tags to verify the result
- version or platform notes

If a mechanism says it resets on restart or requires players to rejoin, account for that in the script design.

## Language Explanations

Use Language Explanations for semantics that are not one command/tag/event:

- operators and comparables
- `/ex`, `/exs`, `/denizen debug`, `/denizen submit`
- script syntax and parser behavior
- script containers
- script events and event switches
- tag system and fallbacks
- object system
- flag system
- queue behavior
- data actions
- durations, ticks, numbers, and decimals

If the problem is "why did Denizen parse this strangely?", start here.

## Common Lookup Paths

Webhook task:

1. Commands: `webget`, `define`, `run`, `debug`
2. Tags: `secret`, `entry`, `map`
3. Object Types: `MapTag`, `ElementTag`
4. Language: queues and command entries

Command argument handler:

1. Commands: `if`, `choose`, `define`, `stop`, `narrate`
2. Tags: `context.args`, `ElementTag.is_integer`, `ListTag`
3. Events/containers: command container docs if needed
4. Language: operators and parser rules

Event cancellation:

1. Events: exact event line, cancellable status, determinations
2. Commands: `determine`, `if`, `stop`
3. Tags: event-specific `<context.*>`
4. Language: script events and event switches

Timer or auto-shutdown flow:

1. Commands: `wait`, `run`, `flag`, `if`, `execute`
2. Tags: `server.online_players`, `server.has_flag`, `util.random.uuid`
3. Object Types: `DurationTag`, `TimeTag`, `ListTag`
4. Language: queues, flags, ticks/durations

Area or region behavior:

1. Events: event line and `Has Location` switches
2. Object Types: `CuboidTag`, `LocationTag`, `AreaObject`
3. Tags: location/area containment tags
4. Mechanisms only if area objects or notes need mutation

Entity/projectile behavior:

1. Events: entity damage/spawn/death/target/move/explode events
2. Object Types: `EntityTag`, `LocationTag`, `VectorObject`
3. Commands: `spawn`, `teleport`, `adjust`, `hurt`, `remove`, `shoot`
4. Mechanisms: entity AI, velocity, name, gravity, persistence
