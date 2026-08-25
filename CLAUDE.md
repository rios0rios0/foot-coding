# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Foot Coding is a **discontinued** Windows desktop application (Object Pascal / Borland Delphi 7). Discontinued 2013-11-23. No new features or bug fixes are planned. The repository is preserved as a historical and educational reference.

## Architecture

Single-file Win32 API application — no VCL framework. `FC.dpr` contains all window creation, message handling, and UI. `External Uses/MyUtils.pas` contains all conversion algorithms. GUI controls are created manually via `CreateWindowExA` and dispatched through a single `WindowProc` callback.

`IntToStr` and `StrToInt` in `MyUtils.pas` are implemented in pure x86 assembly. Do not replace them with Pascal equivalents.

## Build

No automated build system, CI, or tests. Open `FC.dpr` in Borland Delphi 7, press F9. Run `Clear.bat` to remove build artifacts before committing.

## Conventions

- **Discontinued** — do not suggest feature additions, library upgrades, or framework migration (VCL/FMX).
- **No VCL** — do not introduce VCL or third-party component packages.
- **No external dependencies** — `MyUtils.pas` uses only `Windows.pas` types and the Pascal standard library.
- **Portuguese UI** — button labels and UI strings are in Brazilian Portuguese. Do not translate.
- **Single-unit logic** — all conversion algorithms stay in `MyUtils.pas`.
- **PascalCase** — `h` prefix for Win32 handles (`hFrm`, `hLst`, `hFont`).
- **Binary resources** — `*.ico` and `*.res` files cannot be edited as text. `{$R}` directives in `FC.dpr` reference `.res` files compiled from `.rc` by the IDE.

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body "<imperative description>"`
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body "<description>"`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
