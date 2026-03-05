# Copilot Instructions for Foot Coding

## Project Overview

Foot Coding is a **discontinued** Windows desktop application built in **Object Pascal (Borland Delphi 7)** and discontinued on **2013-11-23**. The repository is preserved as a historical and educational reference. No new features or bug fixes are planned.

The application is a multi-format number and encoding converter with 13 operations (ASCII↔Hex, ASCII↔Base64, Decimal↔Hex/Binary/Octal, Decimal→Roman numerals, Year→Century, Date→Day of week). It is notable for building its entire GUI directly through **Win32 API** calls without the VCL framework and for implementing `IntToStr`/`StrToInt` in **inline x86 assembly**.

**Language:** Object Pascal (Borland Delphi 7) | **Status:** Discontinued | **Version:** 1.0

## Repository Structure

```
foot-coding/
├── .github/
│   └── copilot-instructions.md   # This file
├── ConIcon/
│   ├── ConIcon.rc                # Resource script defining application icons
│   ├── Icon.ico                  # Main application icon
│   ├── 32x32.ico                 # Small icon for taskbar / title bar
│   └── 64x64.ico                 # Large icon
├── External Uses/
│   └── MyUtils.pas               # Utility unit: all conversion algorithms
├── XPManifest/
│   ├── XPManifest.Manifest       # Windows XP common controls manifest (XML)
│   └── XPManifest.rc             # Resource script that embeds the manifest
├── Clear.bat                     # Cleans Delphi build artifacts
├── CONTRIBUTING.md               # Historical build information
├── FC.dpr                        # Main program — Win32 API window creation, message loop, conversion dispatch
└── LICENSE
```

### Key Source Files

- **`FC.dpr`** — The entire application in a single Delphi project file. Registers a window class (`FrmFCPrincipal`), creates all controls manually via `CreateWindowExA`/`CreateMyComponent`, runs the Win32 message loop, and dispatches conversion operations in `WindowProc`.
- **`External Uses/MyUtils.pas`** — Standalone unit containing all conversion algorithms (`StrToHex`, `HexToStr`, `IntToHex`, `HexToInt`, `IntToBin`, `BinToInt`, `IntToOct`, `OctToInt`, `StrToB64`, `B64ToStr`, `IntToRom`, `AnyDay`, `CenturyCalc`) plus `IntToStr`/`StrToInt` implemented in pure x86 assembly.
- **`ConIcon/ConIcon.rc`** — Resource script that embeds `Icon.ico`, `32x32.ico`, and `64x64.ico` as named icon resources (`CONICON`, `ICO32`, `ICO64`).
- **`XPManifest/XPManifest.rc`** — Resource script that embeds the XP common controls manifest as resource `24`.
- **`Clear.bat`** — Removes Delphi intermediate build artifacts (`*.~??`, `*.dcu`, `*.opt`, `*.dsm`, `*.dsk`, `*.cfg`, `*.ddp`, `*.dof`, `*.bdsproj`, `*.identcache`).

## Technology Stack

| Component          | Details                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------|
| **Language**       | Object Pascal (Borland Delphi 7) with inline x86 assembly                                |
| **IDE**            | Borland Delphi 7 (or compatible: Embarcadero RAD Studio)                                 |
| **UI Framework**   | Pure Win32 API — no VCL; all controls created via `CreateWindowExA` and `SendMessageA`   |
| **Win32 APIs**     | `Windows.pas`, `Messages.pas`, `SetLayeredWindowAttributes`, `GlobalAlloc`/`GlobalLock`  |
| **Assembly**       | Inline x86 `asm` blocks in `MyUtils.pas` for `IntToStr` and `StrToInt`                   |
| **Resources**      | Windows Resource Compiler — icons (`ConIcon.rc`) and XP manifest (`XPManifest.rc`)       |
| **Clipboard**      | `OpenClipboard`, `GetClipboardData`, `SetClipboardData`, `GlobalAlloc`                   |
| **Platform**       | Windows XP and later (Win32, 32-bit)                                                     |

## Build Instructions (Historical)

There is no automated build system. Compilation requires a Windows machine with Borland Delphi 7 or a compatible IDE.

1. Open `FC.dpr` in **Borland Delphi 7** (or Embarcadero RAD Studio).
2. Compile and run: **F9** (build and run) or **Ctrl+F9** (compile only).

The resource files (`ConIcon/ConIcon.res` and `XPManifest/XPManifest.res`) are referenced via `{$R}` directives in `FC.dpr` and are compiled automatically by the Delphi IDE using the Windows Resource Compiler.

> **Note:** Run `Clear.bat` to remove Delphi temporary files before committing.

## Architecture and Design Patterns

- **Single-file application** — all Win32 API bootstrapping, message handling, and UI creation live in `FC.dpr`. Conversion logic is isolated in `External Uses/MyUtils.pas`.
- **Pure Win32 API GUI (no VCL)** — the main window and all controls (`ListBox`, `Edit`, `Button`, `Static`) are created manually with `CreateWindowExA`. There are no `.dfm` form definitions.
- **Custom `WindowProc`** — a single `stdcall` callback handles all window messages: `WM_COMMAND` (button clicks, listbox selection), `WM_CTLCOLORLISTBOX`, `WM_CTLCOLORSTATIC`, `WM_CTLCOLOREDIT` (dark-theme colouring), and `WM_DESTROY`.
- **Semi-transparent window** — `SetLayeredWindowAttributes` with `LWA_ALPHA` (alpha = 255, fully opaque but layered) is applied to the main form.
- **Inline x86 assembly** — `IntToStr` and `StrToInt` in `MyUtils.pas` are implemented in pure `asm` blocks for educational purposes.

### Conversion Dispatch

`WindowProc` handles `WM_COMMAND` for the **Processar** button. It reads the selected item from the listbox, extracts the two-digit operation number with `Copy(string(TxtLine), 0, 2)`, and dispatches to the appropriate function from `MyUtils.pas` via a `case` statement (operations 01–13).

### UI Layout

| Control          | Purpose                                          |
|------------------|--------------------------------------------------|
| `hLst`           | `ListBox` showing the 13 conversion operations  |
| `hEdtEntrada`    | Multiline `Edit` — user input ("Entrada")        |
| `hEdtSaida`      | Multiline `Edit` — conversion result ("Saida")  |
| `hBtnProces`     | "Processar" — triggers conversion               |
| `hBtnLimpar`     | "Limpar" — clears both edit fields              |
| `hBtnColar`      | "< Colar" — pastes clipboard into input field   |
| `hBtnCopiar`     | "Copiar >" — copies output field to clipboard   |

## Conversion Algorithms (`MyUtils.pas`)

| Function              | Description                                                   |
|-----------------------|---------------------------------------------------------------|
| `StrToHex`/`HexToStr` | ASCII string ↔ hexadecimal representation                    |
| `IntToHex`/`HexToInt` | Integer ↔ hexadecimal string with configurable digit padding  |
| `IntToBin`/`BinToInt` | Integer ↔ binary string                                       |
| `IntToOct`/`OctToInt` | Integer ↔ octal (both directions return `LongInt`)            |
| `StrToB64`/`B64ToStr` | Base64 encoding/decoding using a custom 6-bit codec           |
| `IntToRom`            | Integer → Roman numeral string (uses lookup table up to 1000) |
| `CenturyCalc`         | Year → century number (returned as `string`)                  |
| `AnyDay`              | Date (D, M, Y) → day-of-week name in Brazilian Portuguese     |
| `IntToStr`/`StrToInt` | Integer/string conversion in pure x86 assembly                |

## CI/CD Pipeline

There is **no automated CI/CD pipeline**. The project is discontinued and compiled manually in the Delphi IDE on Windows.

## Tests and Linting

This project has no automated tests or linters. No testing infrastructure exists.

## Development Workflow

Since this project is discontinued, the only expected changes are archival corrections or documentation updates:

1. Edit `.pas` source files or documentation directly.
2. Open `FC.dpr` in Borland Delphi 7 / RAD Studio to verify compilation if source changes are made.
3. Run `Clear.bat` to remove build artifacts before committing.
4. Commit only source files (`*.dpr`, `*.pas`, `*.rc`, `*.ico`, `*.bat`, `*.Manifest`). Do **not** commit compiled artifacts (`*.exe`, `*.dcu`, `*.opt`, `*.res`, etc.).

## Coding Conventions

- **Object Pascal style** — `PascalCase` for types and routines, `begin`/`end` blocks, `h` prefix for Win32 handles (`hFrm`, `hLst`, `hFont`), `Str`/`Int`/`Hex` function naming from the Delphi standard library style.
- **Single-unit conversion logic** — all algorithms stay in `MyUtils.pas`; avoid splitting into additional units.
- **Portuguese UI strings** — button labels, static text, and listbox items are in Brazilian Portuguese (e.g., `'Processar'`, `'Limpar'`, `'Entrada:'`, `'Saída:'`). Preserve this convention.
- **No VCL dependency** — do not introduce VCL or any third-party component packages.
- **No external library dependencies** — `MyUtils.pas` uses only `Windows.pas` types and the Pascal standard library; do not add `uses` clauses for external libraries.

## Common Tasks

| Task                              | How                                                                 |
|-----------------------------------|---------------------------------------------------------------------|
| Build the project                 | Open `FC.dpr` in Delphi 7, press **F9**                            |
| Compile without running           | Press **Ctrl+F9** in Delphi 7                                      |
| Clean build artifacts             | Run `Clear.bat` in the project root                                |
| Add a new conversion operation    | Add a string to `ListboxContent` in `FC.dpr`, add a `case` branch in `WindowProc`, implement the algorithm in `MyUtils.pas` |
| Change window transparency        | Modify the `Transparence` argument to `CreateMyForm` in `FC.dpr`   |

## Notes for AI Assistants

- This is a **Windows-only Win32 application**; all handle types, API calls, and resource identifiers are Windows-specific.
- The project is **discontinued** — avoid suggesting feature additions, library upgrades, or refactoring to VCL/FMX.
- UI strings are in **Brazilian Portuguese**; do not translate them.
- The `*.ico` and `*.res` files are binary resources — they cannot be edited as plain text.
- `IntToStr` and `StrToInt` in `MyUtils.pas` are **intentionally implemented in x86 assembly** for educational purposes; do not replace them with Pascal equivalents.
- The `{$R}` directives in `FC.dpr` reference pre-compiled `.res` files; the IDE compiles `.rc` → `.res` automatically.
