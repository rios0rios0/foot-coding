# Contributing

> **This project was discontinued in November 2013 and is no longer actively maintained.**
> The repository is preserved as a historical reference. No new features or bug fixes are planned.

## Historical Build Information

This project was built using the following tools and technologies:

- **Language:** Object Pascal (Borland Delphi 7) with inline x86 assembly
- **IDE:** Borland Delphi 7
- **UI Framework:** Pure Win32 API (no VCL) — `CreateWindowExA`, `SendMessageA`, custom `WindowProc`
- **APIs:** `Windows.pas`, `Messages.pas`, `SetLayeredWindowAttributes` (semi-transparent window)
- **Resource Compiler:** Windows Resource Compiler for icons and XP manifest

### Build Steps (Historical)

1. Open `FC.dpr` in Borland Delphi 7 (or compatible Object Pascal compiler)
2. Compile and run (`F9`)

> **Note:** Targets Windows XP and later (32-bit). Icon resources in `ConIcon/`, XP manifest in `XPManifest/`. `Clear.bat` cleans Delphi build artifacts.
