# OverPy Syntax

## Contents

- [File structure](#file-structure)
- [Rules and subroutines](#rules-and-subroutines)
- [Variables and assignment](#variables-and-assignment)
- [Control flow](#control-flow)
- [Arrays and dictionaries](#arrays-and-dictionaries)
- [Strings](#strings)
- [Macros and enums](#macros-and-enums)
- [Settings](#settings)
- [Includes and compiler directives](#includes-and-compiler-directives)
- [Translations](#translations)
- [Starter example](#starter-example)

## File structure

- Use `#` for line comments and `/* ... */` for block comments.
- Use a `.opy` extension.
- Indent blocks with four spaces. A tab is interpreted as four spaces; do not mix styles.
- Place top-level declarations, directives, settings, rules, and subroutines at indentation level zero.
- Use `pass` for an intentionally empty block.

## Rules and subroutines

Declare a rule with a quoted name and a colon. Put annotations before actions:

```opy
rule "Track spawned players":
    @Event eachPlayer
    @Team all
    @Condition eventPlayer.hasSpawned()

    eventPlayer.isReady = true
```

Common annotations:

- `@Event global|eachPlayer|...` selects the event. Omission defaults to `global`.
- `@Condition expression` adds a Workshop rule condition; repeat it for multiple conditions.
- `@Team`, `@Hero`, and `@Slot` filter player events. Do not combine `@Hero` and `@Slot`.
- `@Disabled` emits a disabled rule.
- `@Delimiter` prevents an empty organizational rule from being optimized away.
- `@NewPage` inserts filler rules so the rule starts on a new Workshop page.
- `@SuppressWarnings warning_name ...` suppresses warnings only within that rule.

Declare a Workshop subroutine with `def`:

```opy
def resetRound():
    @Name "Reset round state"
    roundState = 0
```

Call it with `resetRound()`. Workshop subroutines cannot accept parameters or return values. Use a `macro` for parameterized inline logic.

## Variables and assignment

Declare named global and player variables at top level:

```opy
globalvar roundState 2
globalvar arenaCenter = vect(0, 0, 0)
playervar score
playervar isReady = false
```

The numeric index is optional. Preserve explicit indices when compatibility with existing Workshop code matters. Built-in variable names such as `A` through `DX` do not require declarations, but descriptive declarations are clearer.

Use normal-looking assignment and compound assignment:

```opy
roundState = 1
eventPlayer.score += 1
players.append(eventPlayer)
```

Player variables are accessed as members of a player expression. Many mutating array methods also require a variable target rather than an arbitrary temporary expression.

## Control flow

Use `if`, `elif`, and `else` with indentation:

```opy
if eventPlayer.score >= 10:
    eventPlayer.setStatusEffect(null, Status.PHASED_OUT, 1)
elif eventPlayer.score >= 5:
    eventPlayer.setMoveSpeed(125)
else:
    eventPlayer.setMoveSpeed(100)
```

Use `while` for Workshop `While`. Ensure a repeated path yields when necessary:

```opy
while roundState == 1:
    updateRound()
    wait(0.1)
```

Use `range` only in a `for` statement:

```opy
globalvar i

for i in range(1, 5, 2):
    values.append(i)
```

OverPy's Workshop loop range includes the stop value when reached; the example produces `[1, 3, 5]`. Do not assume Python's exclusive stop behavior.

Use `switch` for a value-based branch when it is clearer or smaller than a long equality chain:

```opy
switch roundState:
    case 0:
        prepareRound()
        break
    case 1:
        runRound()
        break
    default:
        endMatch()
```

Switch fallthrough is enabled. Add `break` unless continuing into the next case is intentional.

Use `goto label` only inside the same rule and only toward a later label. Use `goto loc + expression` only for deliberately index-sensitive code. Prefer structured flow.

## Arrays and dictionaries

Use array literals, indexing, membership, and member operations:

```opy
players = getAllPlayers()
firstPlayer = players[0]
containsHost = hostPlayer in players
alive = players.filter(lambda player: player.isAlive())
positions = players.map(lambda player: player.getPosition())
allReady = players.all(lambda player: player.isReady)
anyDead = players.any(lambda player: not player.isAlive())
```

`append` mutates a variable and flattens/extends an array argument. `concat` returns a copy. Use `slice(start, count)` with a count, not an end index.

Use dictionary literals only for compile-time key mapping. Keys and values are compiled into arrays:

```opy
macro HERO_COST = {
    Hero.ANA: 100,
    Hero.REINHARDT: 250,
    default: 0,
}

cost = HERO_COST[eventPlayer.getCurrentHero()]
```

A missing key resolves to the declared `default`. Without one, a statically known miss optimizes to `null`; do not depend on an unchecked dynamic miss. Use `splitDictArray()` or `tabular()` when parallel arrays are needed; consult current completion for signatures.

Assignments to `array[i][j]` and `array[i][j][k]` are supported through generated slice/concat operations. They can be element-expensive, so inspect compiled output in tight or size-constrained code.

## Strings

Use single or double quoted custom strings. Use `.format()` or f-strings for interpolation:

```opy
messageA = "Score: {}".format(eventPlayer.score)
messageB = f"Score: {eventPlayer.score}"
```

Strings support escapes such as `\n`, `\xHH`, and `\uHHHH`, and may contain literal newlines. Adjacent string literals concatenate.

String prefixes/modifiers include:

- No prefix: custom string; prefer this for ordinary text.
- `f"..."`: custom f-string.
- `t"..."`: translatable custom string.
- `w"..."`: raw Workshop string, primarily for settings or exact Workshop tokens.
- `l"..."`: legacy localized Workshop string; avoid in new code.

The Workshop limits each underlying string to 128 characters and three placeholders. OverPy splits longer expressions, but translated strings have additional placement restrictions documented below.

## Macros and enums

Use AST macros for constants and inline functions:

```opy
macro BASE_HEALTH = 250

macro clampScore(value, maximum=10):
    min(max(value, 0), maximum)

macro Player.applyLoadout(health=BASE_HEALTH):
    self.setMaxHealth(health)
    self.setMoveSpeed(110)
```

Macro expansion is syntax-tree based, so operator precedence is preserved. Member macros use `self`. Default and keyword arguments are supported.

Use `#!define` only for literal token substitution. Parenthesize arguments and the whole replacement when emulating an expression macro.

Use enums for named compile-time values:

```opy
enum RoundState:
    WAITING
    ACTIVE
    FINISHED = 10
```

Enum members are inlined. `len(RoundState)` returns the member count and `RoundState.toArray()` returns its values. Multiple declarations can extend an enum unless a member is redeclared.

JavaScript preprocessing is powerful but version- and environment-sensitive. Use it only when ordinary AST macros cannot express the required generation, and keep generated `.opy` deterministic.

## Settings

Settings expressions must optimize to dictionaries, arrays, strings, numbers, or booleans:

```opy
macro DEBUG = false

settings {
    "main": {
        "modeName": w"Arena Mode",
    },
    "gamemodes": {
        "general": {
            "respawnTime%": 50 if DEBUG else 100,
        },
    },
}
```

Import strict JSON settings with `settings "gamesettings.opy.json"`. Unknown setting names compile but are not translated for other output languages. Prefer decompiling settings copied from the current game when names are uncertain.

Enable Workshop extensions with `#!extensions` and the version-specific extension syntax shown by completion or decompiled output.

## Includes and compiler directives

`#!include "relative/path.opy"` inserts a file. A relative path is resolved from the file containing the directive; including a directory includes its files.

Put `#!mainFile "../main.opy"` at the very beginning of a module so compiling that module resolves the project entry point.

Useful directives include:

- `#!suppressWarnings ...` for verified program-wide exceptions; prefer rule-local suppression.
- `#!writeToOutputFile` to write a sibling `.ws.txt` file.
- `#!debugElementCount` to annotate element usage.
- `#!allowMacroRedeclaration` only for intentional multi-mode override architectures.
- `#!disableOptimizations` for debugging compiler behavior.
- `#!optimizeForSize` and `#!optimizeForSizeAggressive` for element pressure.
- `#!optimizeStrict` to preserve edge-case Workshop type behavior.

Optimization directives are scoped from their declaration within the current indentation block and have matching enable/disable forms. Confirm the exact current directive names through completion.

## Translations

Use `#!translations` and `t"..."` or `_()` according to the project's existing translation workflow. Important constraints:

- A translated value must be top-level in a display function or formatted translated string.
- Do not nest a translated string as an argument of an ordinary string or another function.
- Prefer `t"Score: {}".format(value)` over `"{}".format(t"Score")`.
- Use context when identical source text needs distinct translations.
- Unused translations are removed unless `#!keepUnusedTranslations` is enabled.

## Starter example

```opy
globalvar roundState = 0
playervar score = 0

macro SCORE_TO_WIN = 10

def resetPlayer():
    eventPlayer.score = 0
    eventPlayer.teleport(vect(0, 0, 0))

rule "Initialize":
    @Event global

    disableInspector()
    roundState = 1

rule "Award score":
    @Event playerDealtFinalBlow
    @Condition roundState == 1

    eventPlayer.score += 1
    if eventPlayer.score >= SCORE_TO_WIN:
        declarePlayerVictory(eventPlayer)
```
