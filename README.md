# MolView - Total Commander WLX Plugin (64-bit)

MolView is a fast Lister plugin for Total Commander that renders chemical files in 2D and 3D.
It uses OpenBabel for format conversion and OpenGL for interactive display.

## Features
- Broad chemistry format support (`.mol`, `.sdf`, `.mol2`, `.smi`, `.smiles`, `.pdb`, `.xyz`, `.cml`)
- 2D structure rendering with SVG parsing and crisp label rendering
- 3D molecule rendering from STL with rotation, zoom, and reset
- Asynchronous loading (UI remains responsive while converting/parsing)
- Multi-molecule navigation for SDF files
- Built-in Total Commander thumbnail preview (`ListGetPreviewBitmap`)
- Thumbnail preview is always rendered in 2D, independent of INI/start mode
- Plugin options dialog for OpenBabel path/options and startup behavior
- Logging support for diagnostics

## Supported Formats
| Extension | Format |
|---|---|
| `.mol` | MDL Molfile V2000/V3000 |
| `.sdf` | MDL Structure Data File (multi-molecule supported) |
| `.mol2` | Tripos MOL2 |
| `.smi`, `.smiles` | SMILES |
| `.pdb` | Protein Data Bank |
| `.xyz` | XYZ Cartesian coordinates |
| `.cml` | Chemical Markup Language |

## Requirements
- Total Commander 64-bit (10.x or newer)
- OpenBabel 3.x
  - Example path: `C:\Program Files\OpenBabel-3.1.1\obabel.exe`
  - Alternatively place `obabel.exe` and required DLLs next to `MolView.wlx64`

## Installation
### Recommended (auto install)
1. Place plugin files in one folder (including `MolView.wlx64` and `pluginst.inf`).
2. Install via Total Commander plugin install flow using the INF package.

### Manual
1. Copy `MolView.wlx64` and `molview.ini` to a plugin folder, e.g. `%COMMANDER_PATH%\Plugins\WLX\MolView\`.
2. In Total Commander open `Configuration -> Plugins -> Lister Plugins -> Add`.
3. Select `MolView.wlx64`.
4. Restart Total Commander.

## Controls
| Key / Action | Function |
|---|---|
| Mouse wheel | Zoom in/out |
| Left drag (2D) | Pan |
| Left drag (3D) | Rotate |
| Double click / Right double click | Reset view |
| `R` | Reset view |
| `T` | Toggle 2D/3D mode |
| `Left` / `Up` | Previous molecule (SDF) |
| `Right` / `Down` | Next molecule (SDF) |

## Configuration
`molview.ini` example:

```ini
[MolView]
OBabelPath=C:\Program Files\OpenBabel-3.1.1\obabel.exe
OBabelOpts=-xC -xs
```

INI search order:
1. Path provided by Total Commander (`ListSetDefaultParams`)
2. Plugin directory (same folder as `MolView.wlx64`)

## Build (Lazarus/FPC)
### Prerequisites
- Lazarus 3.x
- FPC 3.2.x (x86_64-win64)
- No LCL dependency required

### Build settings
- Target OS: `Windows`
- Target CPU: `x86_64`
- Output: `MolView.wlx64`
- Optimization: `-O2`

Then build the library project (`MolViewPlugin.lpr`) in Release 64.

## Project Structure
```text
MolViewPlugin.lpr        DLL entry + exported WLX functions
uWLXExports.pas          WLX API (load, detect, commands, preview bitmap)
uWin32Window.pas         Win32 child window, WGL context, input handling
uOBabelRunner.pas        OpenBabel process execution and output handling
uSVGParser.pas           SVG parser into internal scene geometry
uGLRenderer.pas          2D OpenGL renderer + viewport fitting
uGLFont.pas              GDI-based text texture rendering
uSTLParser.pas           STL parse + direct 3D OpenGL rendering
uConfig.pas              INI config loading/saving
uLogger.pas              File logging
```

## Notes
- 3D generation depends on OpenBabel input quality and available coordinates.
- If OpenBabel `--gen3d` fails for specific structures, fallback handling is applied where possible.
