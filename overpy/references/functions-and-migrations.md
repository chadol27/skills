# Functions and Migrations

## Contents

- [Function resolution](#function-resolution)
- [General conversion rules](#general-conversion-rules)
- [High-impact Workshop conversions](#high-impact-workshop-conversions)
- [Current OverPy helper functions](#current-overpy-helper-functions)
- [Repository-history migrations](#repository-history-migrations)
- [Upgrade procedure](#upgrade-procedure)

## Function resolution

Never infer a signature solely from the English Workshop label. Resolve a function in this order:

1. Check calls that already compile in the target project.
2. Use OverPy LSP completion and signature help. Type the receiver followed by `.` to discover member functions.
3. Search the installed package metadata:
   - `src/data/opy/functions.ts`
   - `src/data/opy/memberFunctions.ts`
   - `src/data/opy/macros.ts`
   - `src/data/actions.ts`
   - `src/data/values.ts`
4. Decompile a minimal Workshop action/value using the same installed version.
5. Consult the current official source.

Named arguments use the exact camelCase metadata names. Defaults are version-specific. Do not preserve an obsolete positional call merely because it parses on an older compiler.

## General conversion rules

- Convert ordinary Workshop names to camelCase: `Set Match Time` becomes `setMatchTime`.
- Convert actions and values whose first parameter is a player, array, string, or similar receiver into member calls: `Teleport(Event Player, pos)` becomes `eventPlayer.teleport(pos)`.
- Convert constant-producing wrappers into enum members: `Hero(Ana)` becomes `Hero.ANA`, `Color(White)` becomes `Color.WHITE`, and `Button(Interact)` becomes `Button.INTERACT`.
- Convert arithmetic, comparison, Boolean, membership, and indexing actions to operators.
- Convert control actions to structured syntax where available.
- Use lower camelCase for function and argument names. Enum members are uppercase unless current completion shows otherwise.

These are heuristics, not guarantees. Use metadata for exceptions.

## High-impact Workshop conversions

| Workshop form | OverPy form | Note |
| --- | --- | --- |
| `Abort` | `return` | Applies to the current rule/subroutine flow. |
| `Abort If(condition)` | `if condition:` then `return` | Use indentation. |
| `Add(a, b)` | `a + b` | Similar for subtract, multiply, divide, modulo, and power. |
| `And(a, b)` / `Or(a, b)` / `Not(a)` | `a and b` / `a or b` / `not a` | Preserve Workshop reevaluation semantics. |
| `Compare(a, ==, b)` | `a == b` | Also use `!=`, `<`, `<=`, `>`, and `>=`. |
| `Array(a, b)` / `Empty Array` | `[a, b]` / `[]` | Use literals. |
| `Value In Array(array, i)` | `array[i]` | Nested reads are supported. |
| `Array Contains(array, value)` | `value in array` | Note operand order. |
| `Append To Array(array, value)` | `array.concat(value)` | Returns a copy; `array.append(value)` mutates a variable. |
| `Array Slice(array, start, count)` | `array.slice(start, count)` | The third value is a count. |
| `Count Of(array)` | `len(array)` | Also used for enum member count. |
| `First Of(array)` / `Last Of(array)` | `array[0]` / `array[-1]` | Confirm empty-array behavior when relevant. |
| `Filtered Array` | `array.filter(lambda item[, index]: condition)` | Prefer this over legacy comprehension syntax. |
| `Mapped Array` | `array.map(lambda item[, index]: expression)` | Lambda names are local placeholders. |
| `Is True For All` / `Is True For Any` | `array.all(lambda item: condition)` / `array.any(...)` | Current functional syntax. |
| `Custom String("Score: {0}", score)` | `"Score: {}".format(score)` or `f"Score: {score}"` | Custom strings are the default. |
| `String Replace` | `string.replace(old, new)` | Member call. |
| `String Slice` | `string.substring(start, length)` | Verify parameter semantics through completion. |
| `String Split` | `string.split(separator)` | Member call. |
| `Char In String` | `string.charAt(index)` | Member call. |
| `Vector(x, y, z)` | `vect(x, y, z)` | Components are `.x`, `.y`, and `.z`. |
| `Direction Towards(a, b)` | `a.directionTowards(b)` | Receiver is the first vector/position. |
| `Distance Between(a, b)` | `a.distanceTo(b)` | Receiver is the first argument. |
| `Call Subroutine(name)` | `name()` | Synchronous call. |
| `Start Rule(name, behavior)` | `startRule(name, behavior)` | Asynchronous subroutine execution. |
| `Loop` | `loop()` | Replaces older `goto RULE_START`. |
| `Wait` | `wait(duration, behavior)` | Defaults exist; inspect the installed signature. |
| `Absolute Value` / `Square Root` | `abs()` / `sqrt()` | Mathematical short names. |
| degree trig functions | `sinDeg()`, `cosDeg()`, `tanDeg()`, `asinDeg()`, `acosDeg()`, `atan2Deg()` | Radian variants omit `Deg`. |

## Current OverPy helper functions

These helpers are OverPy conveniences rather than direct one-to-one Workshop names. Confirm their current signatures before use:

- `debug(expression)` prints both the expression text and value for quick diagnostics.
- `print(value)` creates a simple debug HUD.
- `arrayToString(value, ...)` renders arrays rather than only their first element.
- `timeToString(seconds)` renders time as `H:MM:SS` with fractional seconds when present.
- `buttonToString(button)` improves input-binding formatting.
- `getRealClosestPlayer`, `getRealClosestPlayers`, and `getRealFarthestPlayer` filter dead or unspawned players that otherwise appear at the origin.
- `Player.getEffectiveHero()` accounts for Echo duplication.
- `Player.getOppositeTeam()` returns the other team.
- `Array.reverse()` and `Array.unique()` are built-in macros.
- `hudHeader`, `hudSubheader`, and `hudSubtext` reduce `hudText` arguments.
- `lineIntersectsSphere`, `getSign`, `hsl`, and `log` provide common derived operations.
- `splitDictArray` and `tabular` construct parallel arrays from compile-time tabular data.

## Repository-history migrations

Apply these when upgrading old source. The right-hand form is current in the `v9.7.9` source snapshot.

| Older form | Current form | Context |
| --- | --- | --- |
| `async(subroutine, behavior)` | `startRule(subroutine, behavior)` | Renamed in 2026 to match the Workshop action. |
| `AsyncBehavior.RESTART` / `.NOOP` | `StartRuleBehavior.RESTART` / `.NOOP` | Renamed with `startRule`. |
| `stopChasingVariable(variable)` | `stopChasing(variable)` | Renamed in 2026. |
| `raycast(...).hitPosition()` | `raycastHitPosition(...)` | The hit-position query is now a direct function. Re-check related raycast functions individually. |
| `chase(variable, destination, rateOrDuration, ...)` | `chaseAtRate(...)` or `chaseOverTime(...)` | Split by chase mode. |
| generic `createWorkshopSetting(...)` | typed `createWorkshopSettingBool`, `createWorkshopSettingInt`, `createWorkshopSettingFloat`, `createWorkshopSettingHero`, or enum variant | Split for Workshop-aligned signatures. |
| `const NAME = value` | `macro NAME = value` | Constants are AST macros in modern OverPy. |
| `RULE_CONDITION` | `ruleCondition` | The rule-condition value was normalized. |
| `goto RULE_START` | `loop()` | Use the dedicated loop form. |
| `printLog(...)` | `logToInspector(...)` | Renamed before the current snapshot. |
| `holygrail` | `__holygrail__` | Internal/special value rename; avoid direct use when automatic tag handling works. |
| older member macro prefix forms | normal dot member syntax | Use `receiver.method(...)`. |
| uppercase or inconsistent argument names | camelCase named arguments | Re-check every keyword argument during upgrade. |

Hero, map, gamemode, custom-setting, and enum names change as Overwatch changes. Do not create a static alias layer without checking the target OverPy version. Decompile current settings or use completion.

## Upgrade procedure

1. Record the old and target OverPy versions.
2. Compile unchanged source with the target version and preserve the complete diagnostic list.
3. Replace known renamed functions and syntax forms mechanically only when semantics match.
4. Re-open each changed call in signature help and verify receiver, argument order, named arguments, defaults, return type, and reevaluation options.
5. Check enum, hero, map, mode, and settings identifiers against current completion.
6. Compare compiled Workshop output when optimization or decompilation behavior may have changed.
7. Test concurrency, waits, chases, event context, strings, and translations in-game.
8. Remove compatibility aliases only after all includes compile against the target version.
