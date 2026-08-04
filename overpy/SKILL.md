---
name: overpy
description: Write, edit, review, migrate, and troubleshoot OverPy source files for the Overwatch Workshop. Use for .opy files, Workshop-to-OverPy conversions, OverPy rules and subroutines, variables, macros, enums, settings, translations, compiler directives, function-name migrations, compiler warnings, and OverPy CLI or LSP validation.
---

# OverPy

Produce maintainable `.opy` source that compiles to Overwatch Workshop code. Treat OverPy as its own language with Python-like syntax, not as Python.

## Required workflow

1. Inspect the target project before editing. Find its OverPy version, entry file, includes, variable declarations, naming conventions, compiler directives, and generated-output policy. Do not inspect secret or environment files.
2. Read [syntax.md](references/syntax.md) before creating or substantially rewriting `.opy` code.
3. Read [functions-and-migrations.md](references/functions-and-migrations.md) when translating Workshop actions/values, choosing a function signature, or updating older OverPy code.
4. Read [pitfalls.md](references/pitfalls.md) before final validation and whenever code uses waits, chases, `startRule`, translations, optimizations, gotos, multidimensional arrays, or custom settings.
5. Preserve explicit variable indices, subroutine indices, directives, and include topology unless the task requires changing them.
6. Prefer the project's installed OverPy version and its diagnostics over this skill's snapshot. Never invent a function name or argument order.
7. Compile the relevant entry file. Fix errors and investigate warnings instead of suppressing them globally.
8. Report the files changed, the validation command, and any warnings or unverified runtime assumptions.

## Resolve uncertain syntax or functions

Use this order of authority:

1. Existing compiling code in the same project.
2. The installed OverPy LSP completion, hover, and signature help.
3. The installed package's `src/data/opy/`, `src/data/actions.ts`, and `src/data/values.ts` metadata.
4. Decompile a minimal equivalent Workshop snippet and inspect the emitted `.opy`.
5. Consult the current official repository and README.

Do not silently fall back to raw Workshop names. Most ordinary names become camelCase, first-argument functions often become member calls, and constants generally become enum members, but exceptions are common.

## Authoring rules

- Use four spaces per indentation level.
- Declare descriptive `globalvar` and `playervar` names instead of consuming implicit `A`-style variables in new code.
- Keep rule annotations immediately inside the rule and before executable statements.
- Use `def` only for Workshop subroutines; subroutines have no parameters or return values.
- Use `macro` for parameterized inline logic or constants. Use `#!define` only when literal text substitution is intentionally required.
- Prefer structured control flow over `goto`. Prefer member-call and enum syntax over Workshop-shaped calls.
- Add waits to potentially repeated runtime paths when required for server safety, but do not add arbitrary waits that alter behavior.
- Keep warning suppression local with `@SuppressWarnings` after verifying the exact warning is safe.
- Add concise comments for Workshop engine quirks, index-sensitive declarations, and non-obvious optimization assumptions.

## Validation

Prefer the project-local CLI. Typical commands are:

```sh
npx overpy compile -i path/to/main.opy -o /tmp/main.workshop.txt
npx overpy decompile -i path/to/workshop.txt -o /tmp/decompiled.opy
```

If a global CLI is intentionally installed, `overpy compile` and `overpy decompile` are equivalent entry points. Use `--root` or `--main-file` when project layout requires it. Run `overpy --help` for version-specific options.

Compilation proves parser and compiler acceptance, not correct behavior in an Overwatch lobby. Identify behavior that still needs in-game testing, especially event context, reevaluation, chase behavior, concurrency, and resource limits.

## Source baseline

The bundled references were researched against official OverPy `v9.7.9`, commit `1e2688954302a402d076944b46db07efb14d7b61` (2026-06-29). When a project uses another version or the official repository has advanced, re-check current metadata and migration history before changing APIs.
