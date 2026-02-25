<h1 align="center">Foot Coding</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/foot-coding/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/foot-coding.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/foot-coding/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/foot-coding.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

A Windows desktop application built entirely with the Win32 API in Object Pascal (Borland Delphi 7), providing a multi-format number and encoding converter. The program was written without the VCL framework, creating all UI elements (windows, buttons, listboxes, edit controls) directly through Win32 API calls, and implements core conversion algorithms with inline x86 assembly for `IntToStr` and `StrToInt`. Development discontinued on 2013-11-23.

## Features

- **13 conversion operations** selectable from a listbox:
  - ASCII to Hexadecimal and reverse
  - ASCII to Base64 and reverse
  - Decimal to Hexadecimal and reverse
  - Decimal to Binary and reverse
  - Decimal to Octal and reverse
  - Decimal to Roman numerals
  - Year to Century (with Roman numeral output)
  - Date (DDMMYYYY) to day of the week
- **Pure Win32 API GUI** -- no VCL; all windows, controls, fonts, and message handling are created manually via `CreateWindowExA`, `SendMessageA`, and a custom `WindowProc`
- **Inline x86 assembly** -- `IntToStr` and `StrToInt` are implemented in pure x86 assembly within Object Pascal `asm` blocks for educational purposes
- **Clipboard integration** -- paste input from clipboard and copy output to clipboard
- **XP visual styles** -- Windows XP manifest embedded for themed common controls
- **Custom application icon** -- embedded via resource compiler (32x32 and 64x64 ICO files)
- **Semi-transparent window** -- uses `SetLayeredWindowAttributes` with configurable alpha

## Technologies

- **Object Pascal** (Borland Delphi 7)
- **Win32 API** -- `Windows.pas`, `Messages.pas` (no VCL forms)
- **x86 inline assembly** -- for integer/string conversion routines
- **Windows Resource Compiler** -- for icons and XP manifest

## Project Structure

```
foot-coding/
├── FC.dpr                        # Main program source (Win32 API window creation, message loop, conversion dispatch)
├── External Uses/
│   └── MyUtils.pas               # Utility unit with all conversion algorithms (Hex, Binary, Octal, Base64, Roman numerals, day-of-week)
├── ConIcon/
│   ├── ConIcon.rc                # Resource script defining application icons
│   ├── Icon.ico                  # Main application icon
│   ├── 32x32.ico                 # Small icon for taskbar/title bar
│   └── 64x64.ico                 # Large icon
├── XPManifest/
│   ├── XPManifest.Manifest       # Windows XP common controls manifest
│   └── XPManifest.rc             # Resource script for the manifest
├── Clear.bat                     # Cleans Delphi build artifacts
└── LICENSE
```

## Conversion Algorithms

The `MyUtils.pas` unit implements the following from scratch (no standard library dependencies):

| Function | Description |
|----------|-------------|
| `StrToHex` / `HexToStr` | ASCII string to hexadecimal representation and reverse |
| `IntToHex` / `HexToInt` | Integer to hexadecimal string with configurable digit padding |
| `IntToBin` / `BinToInt` | Integer to binary string and reverse |
| `IntToOct` / `OctToInt` | Integer to octal and reverse |
| `StrToB64` / `B64ToStr` | Base64 encoding and decoding using a custom 6-bit codec |
| `IntToRom` | Integer to Roman numeral string |
| `CenturyCalc` | Year to century number |
| `AnyDay` | Date (day, month, year) to day-of-week name (Portuguese) |
| `IntToStr` / `StrToInt` | Integer/string conversion in pure x86 assembly |

## Installation

1. Install **Borland Delphi 7** (or a compatible Object Pascal compiler)
2. Open `FC.dpr` in the Delphi IDE
3. Compile and run (F9)

The application targets **Windows XP and later** (32-bit).

## Usage

1. Launch the application -- a dark-themed window titled "Foot Coding v1.0" appears
2. Select a conversion operation from the listbox (e.g., "01 - Hex <- ASCII")
3. Type or paste input text in the left "Entrada" (Input) field
4. Click **Processar** (Process) to perform the conversion
5. The result appears in the right "Saida" (Output) field
6. Use **Copiar >** to copy the result to clipboard, **< Colar** to paste from clipboard, or **Limpar** to clear both fields

## Contributing

This project is no longer actively maintained. You may submit small bug fixes or documentation improvements via Pull Request, but reviews may be infrequent and contributions are not guaranteed to be merged.

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.
