# OverPy Pitfalls and Review Checklist

## Contents

- [Language traps](#language-traps)
- [Workshop runtime traps](#workshop-runtime-traps)
- [Compiler and optimization traps](#compiler-and-optimization-traps)
- [Strings and translations](#strings-and-translations)
- [Settings and project layout](#settings-and-project-layout)
- [Final review checklist](#final-review-checklist)

## Language traps

- Python-like does not mean Python. OverPy compiles to Workshop values and actions, with Workshop types, reevaluation, limits, and edge cases.
- `range(start, stop, step)` in a Workshop `for` loop can include `stop`. For example, `range(1, 5, 2)` produces `1, 3, 5`.
- `Array.append` mutates and behaves like an extending append: appending `[3, 4]` to `[1, 2]` produces `[1, 2, 3, 4]`. Use `concat` for a returned copy. Wrap deliberately if a nested array is intended.
- `slice(start, count)` and string substring functions may not use Python/JavaScript end-index semantics. Check the current signature.
- Macro calls are expanded, not invoked at runtime. A changing expression inside an enum or macro remains inlined and may be reevaluated where used.
- `#!define` is raw textual substitution and can break precedence. Prefer `macro`.
- A `def` is a Workshop subroutine, not a general function. It has no parameters or return value.
- Named arguments depend on exact camelCase metadata names and may change between versions.
- Member-call conversion is common but not universal. Verify the receiver and return type.
- Switch cases fall through. Add `break` unless fallthrough is deliberate.
- Dictionary misses need an explicit `default` when fallback behavior matters. Do not depend on a dynamic missing-key result.
- `goto` labels must remain in the same rule and after the jump. Dynamic gotos create instruction-index coupling.

## Workshop runtime traps

### Chased values in rule conditions

Overwatch 2 may fail to re-trigger a rule condition while its referenced variable is actively chased. The condition may update only when the chase stops or reaches its destination.

Refactor so the chase stops at the threshold that must trigger, or use another verified update mechanism. Suppress `w_ow2_rule_condition_chase` only on the checked rule and only after in-game verification.

### Repeated `startRule` restarts

Repeatedly starting a subroutine with `Restart Rule` behavior while the prior execution still has a remaining `wait` can accumulate restart state and eventually crash the server. Repository documentation reports a shared limit around hundreds of accumulated restarts, not a safe per-rule budget.

Avoid high-frequency restart loops. Prefer a single long-lived subroutine, ignore-if-running behavior, explicit state, or cancellation that does not continually restart a waiting execution. Treat `w_start_rule_crash` as actionable.

### `waitUntil` truthiness

The Workshop may not treat a non-Boolean truthy expression in `waitUntil` as expected. Prefer an explicit Boolean expression:

```opy
waitUntil(counter > 0, Math.INFINITY)
```

Do not rely on `waitUntil(counter, ...)` unless `counter` is provably Boolean. Treat `w_wait_until` as actionable.

### Event context

`eventPlayer`, event damage, attacker, victim, and similar values are meaningful only for compatible event types. A subroutine inherits contextual values from its caller, so calling it from incompatible events can produce mismatched context. Keep context-dependent subroutines narrowly used and investigate event-variable warnings.

### Loop pacing

Workshop execution can be throttled or aborted by action limits. Ensure repeated loops yield appropriately. Never insert `wait()` into a semantic path without considering reevaluation and timing changes.

## Compiler and optimization traps

- Optimization is enabled by default. It can remove useless code, fold constants, and replace function patterns.
- `#!optimizeStrict` disables transformations that may change unusual Workshop type behavior, such as `value * 0`, `value + 0`, `value * 1`, or Boolean expressions returning a truthy operand. Decompilation adds it by default for safer round trips.
- `#!disableOptimizations` is a diagnostic tool, not a normal permanent workaround. Compare output and report suspected optimizer bugs.
- `#!optimizeForSize` trades runtime-oriented output for fewer elements. `#!optimizeForSizeAggressive` must be paired with it and can reduce readability further.
- Optimization directives are block-scoped from their declaration point. A directive inside a nested block does not necessarily affect surrounding code.
- Multidimensional writes expand into slice/concat trees and can cost many elements.
- Empty rules may be removed. Use `@Delimiter` for intentional organizational rules.
- Decompile once to establish an editable source, then maintain the `.opy`. Repeated compile/decompile cycling can lose higher-level structure.
- Compilation warnings can represent real game bugs. Do not suppress them merely to get clean output.

## Strings and translations

- Prefer custom strings without the legacy `l` modifier. Localized Workshop strings are limited to predefined phrases.
- OverPy splits long custom strings and excess placeholders, but output still inherits Workshop display limitations.
- Do not combine strings with `+`; use `.format()` or f-strings.
- A translated string must remain top-level. This is invalid in translation workflows: `"{}".format(t"Score")`. Prefer `t"Score: {}".format(value)`.
- Passing a translated string as an argument to another function can produce `TLErr`.
- Dynamic texture tags must be formed in a way the compiler can recognize. A complete literal tag is safest; a constant formatter may work, while a runtime formatter may not.
- Raw `<fg...>` and `<TX...>` tags depend on compiler handling and game behavior. Verify output in-game.

## Settings and project layout

- Unknown custom game setting names are accepted. This does not prove the setting exists or is spelled correctly, and unknown names are not translated for non-English output.
- Settings values must resolve at compile time to dictionaries, arrays, strings, numbers, or booleans. Runtime variables are invalid.
- `.opy.json` settings must be strict JSON. Comments and trailing commas cause editor syntax errors.
- `#!mainFile` must be the first directive in a module.
- Relative includes resolve from the including file, not necessarily the process working directory.
- Directory includes and macro redeclaration can make file order significant. Preserve project topology and validate from the real entry point.
- `#!writeToOutputFile` creates a sibling `.ws.txt` output and, in current versions, may also copy output to the clipboard. Avoid overwriting user-managed output unintentionally.

## Final review checklist

- Confirm every changed function and named argument with current completion or metadata.
- Confirm all rule annotations are valid for their event.
- Confirm player/global variables and explicit indices remain compatible.
- Confirm every loop, repeated rule, and `startRule` path has safe pacing and concurrency behavior.
- Confirm chase-dependent conditions and `waitUntil` expressions avoid known Workshop bugs.
- Confirm array mutation versus copy behavior and inclusive `range` semantics.
- Confirm string prefix, interpolation, translation placement, and tag behavior.
- Confirm optimization directives have the intended scope.
- Compile the actual main file with the target OverPy version.
- Review every compiler warning individually.
- Compare generated Workshop when migrating or changing optimizations.
- List scenarios that require in-game verification.
