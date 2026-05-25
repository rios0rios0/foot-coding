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
