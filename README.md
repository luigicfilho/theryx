# Theryx Verilog

> [!WARNING]
> This is an alpha version, many IEEE 1364 are not supported, expect many bugs, you can open issues if you find any.
> 
> I plan to release the source code, but with a more mature version.
>
> This has a goal to be used as plain VS Code extension, you still need all your EDA tools, the objective is just improve productivity in the design of RTL.

> Visual Studio Code Extension — 1-to-1 RTL Structural Viewer, STARC/RMM Linter, and Verilog (IEEE 1364) Language Intelligence

---

## Overview

Theryx Verilog is a professional hardware engineering extension designed for digital design engineers, FPGA developers, and ASIC architects working with Verilog (IEEE 1364). It brings direct structural visibility and deep language intelligence directly into the editor without requiring external synthesis toolchains:

- **1-to-1 Structural Inspection:** Procedural blocks, continuous assignments, and arithmetic/logical operators are converted directly into their physical circuit equivalents (logic gates, multiplexers, comparators, arithmetic units, D flip-flops, and transparent latches).
- **Interactive RTL Schematic:** Full-featured diagram viewer with bidirectional cross-probing, real-time net tracing, hierarchical submodule drill-down with breadcrumb navigation, and vector SVG export.
- **Language Intelligence (LSP):** Multi-file definition navigation, rich hover inspection with bus widths and signal directions, contextual autocompletion with automated port mapping, and resilient parsing with error recovery.
- **Automated Quick Fixes (`Ctrl + .`):** One-click corrections for industrial lint violations (STARC and RMM design rules) and automated syntax repairs (missing punctuation, delimiters, and block closures).
- **Industrial STARC / RMM Linter:** Live static design rule checking for semiconductor design quality, fully configurable per workspace via JSON.
- **Hardware Semantic Highlighting:** Dedicated token coloring that classifies signals by architectural role (clock, reset, registers, wires, ports, constants) with 4 built-in hardware themes.

---

## Features

### 1. Interactive 1-to-1 Structural RTL Schematic

- **Direct Hardware Mapping (1-to-1 Elaboration):**
  - **Logic Gates:** Standard symbols for primitive gates (`AND`, `OR`, `XOR`, `NOT`, `NAND`, `NOR`).
  - **Multiplexers:** Ternary conditional operators (`sel ? a : b`) elaborate directly into physical 2:1 multiplexers (`MUX`).
  - **Arithmetic and Comparators:** Visual symbols for addition (`+`), subtraction (`-`), multiplication (`*`), and relational/equality comparators (`==`, `!=`, `<`, `>`, `<=`, `>=`).
  - **Bus Slicing and Concatenation:** Dedicated grouping nodes for bit concatenations (`{a, b}`) and bus slices (`[msb:lsb]`, `[index]`).
  - **Sequential Logic (D Flip-Flops):** Synchronous state storage (`DFF`) with clock edge indicators, asynchronous reset polarity detection, and initialization values.
  - **Transparent Latch Inference:** Immediate visual detection and flagging of inferred `LATCH` nodes caused by incomplete combinational `if` or `case` branches.
- **Bidirectional Cross-Probing:**
  - Clicking any wire or port in the schematic navigates directly to and selects the signal in the active RTL editor.
  - Selecting or moving the cursor across any signal identifier in the code highlights the corresponding net and bus width in the open schematic view.
- **Interactive Net Tracing:**
  - Clicking any net or pin highlights the entire electrical path in high-visibility cyan while dimming unrelated circuit paths.
- **Hierarchical Drill-Down and Breadcrumbs:**
  - Double-clicking any submodule instance navigates into its internal schematic, resolving module definitions across single or multi-file workspaces.
  - Toolbar breadcrumbs allow tracing the hierarchy level and returning to parent modules with a single click.
- **Canvas Navigation Controls:**
  - Smooth pan, interactive zoom in/out, automatic fit-to-screen, and one-click SVG export for design reviews and documentation.

### 2. Language Intelligence and Resilient Parser (IEEE 1364)

- **Resilient Parser with Lookahead Error Recovery:**
  - Lookahead-based recovery prevents cascading errors: missing commas in port lists, parameters, nets, or concatenations produce localized diagnostics without breaking the rest of the file or corrupting the Abstract Syntax Tree (AST).
  - Native support for unconnected and open instance ports (such as `.cout()`).
- **Contextual Autocompletion (`Ctrl + Space`):**
  - Scope-aware suggestions for ports (`input`, `output`, `inout`), internal nets (`wire`), registers (`reg`), and parameters (`parameter`, `localparam`) with bus dimensions and directions.
  - **Auto-Connect Module Ports (RMM-2.2):** Typing `(` on a module instance offers actions to automatically populate all declared ports or only remaining unconnected ports with tab stops.
  - **Dynamic Port Filtering:** Typing `.` inside an instance lists only ports that have not yet been connected, preventing duplicates.
  - **IEEE 1364 Code Snippets:** Templates for sequential blocks, combinational blocks, modules, conditionals, and compiler directives.
- **Go to Definition (`F12` / `Ctrl + Click`):**
  - Instant navigation to signal and register declarations within the current file.
  - Multi-file workspace resolution: jumps to submodule declarations located across `.v` and `.vh` files in the project.
  - Direct navigation to port declarations when clicking on named port connections (`.port(net)`).
- **Rich Hover Inspection:**
  - Displays signal direction, net type, and formatted bus width (e.g., `[7:0]`, `4 bits`, or scalar).
  - Displays evaluated constants and parameter values.
  - Displays complete submodule signatures, port lists, and file paths.

### 3. Automated Quick Fixes (`Ctrl + .`)

One-click automated repairs for design violations and syntax issues:

- **Semiconductor Design Rules (STARC and RMM):**
  - **STARC-2.1.1 and STARC-2.1.2:** Converts blocking assignments (`=`) to non-blocking (`<=`) in sequential blocks, and vice versa in combinational blocks (available per line or for the entire block).
  - **STARC-2.3.1:** Automatically inserts missing `default: ;` clauses in `case` statements.
  - **STARC-Latch:** Inserts an `else` branch in combinational `if` statements to eliminate accidental latch inference.
  - **RMM-2.2:** Automatically rewrites positional module instantiations to named port connections (`.port(signal)`), matching the target module definition.
- **Automated Syntax Corrections:**
  - Inserts missing commas in port declarations, parameter lists, nets, and concatenations.
  - Removes trailing commas before closing delimiters.
  - Inserts missing semicolons (`;`) after declarations and assignments.
  - Inserts missing closing delimiters (`)`, `]`, `}`).
  - Inserts missing block keywords (`end`, `endcase`, `endmodule`).
  - Removes stray or unexpected tokens.

### 4. Industrial STARC / RMM Linter

Continuous background static analysis ensuring synthesizability and industry design practices:

- **STARC-1.1.1:** Naming conventions for clock signals (`clk*`) and active-low reset signals (`*rst_n`).
- **STARC-2.1.1:** Prohibits mixing blocking and non-blocking assignments within the same procedural block.
- **STARC-2.1.2:** Enforces non-blocking assignments in sequential logic and blocking assignments in combinational logic.
- **STARC-2.3.1:** Mandates `default` coverage in `case` statements.
- **STARC-Latch:** Warns against combinational conditionals lacking full assignment branches.
- **RMM-2.2:** Enforces explicit named port mapping in all module instantiations.

### 5. Hardware Semantic Highlighting

Deep semantic classification that visually distinguishes signals by their electrical and architectural role:

- Dedicated coloring for Clock signals (`clk`, `sys_clk`, etc.) and Reset networks (`rst_n`, `reset`, etc.).
- Clear differentiation between registers (`reg`) and continuous nets (`wire`).
- Directional identification for `input`, `output`, and `inout` ports.
- Prominent styling for parameters, constants, and module types.

### 6. Interactive CodeLens

- **Theryx: Show Full Module Circuit:** Displayed above `module` declarations to open the complete top-level circuit diagram.
- **Theryx: Inspect Isolated Subcircuit:** Displayed above individual `assign` statements and `always` blocks to isolate and inspect only the logic generated by that specific statement.

---

## Configuration

### 1. Linter Rules Configuration (`.theryx-rules.json`)

Linter rules can be customized per project by placing a `.theryx-rules.json` file in the workspace root.

#### Creating the configuration file:
1. Open the Command Palette (`Ctrl + Shift + P` or `F1`).
2. Run:
   ```text
   Theryx: Criar Arquivo de Regras (.theryx-rules.json)
   ```
3. The `.theryx-rules.json` file will be created in the project root with JSON schema validation.

#### Configuration example:
```json
{
  "$schema": "https://raw.githubusercontent.com/theryx/theryx-verilog/main/rules/rules.schema.json",
  "version": "1.0.0",
  "rules": [
    {
      "id": "STARC-2.1.1",
      "severity": "error",
      "enabled": true,
      "description": "Do not mix blocking and non-blocking assignments."
    },
    {
      "id": "STARC-2.1.2",
      "severity": "error",
      "enabled": true,
      "description": "Enforce non-blocking assignments in clocked blocks."
    },
    {
      "id": "STARC-2.3.1",
      "severity": "warning",
      "enabled": true,
      "description": "Case statements must include a default clause."
    },
    {
      "id": "STARC-Latch",
      "severity": "warning",
      "enabled": true,
      "description": "Prevent accidental latch inference in combinational logic."
    },
    {
      "id": "RMM-2.2",
      "severity": "error",
      "enabled": true,
      "description": "Require named port connections in module instances."
    },
    {
      "id": "STARC-1.1.1",
      "severity": "info",
      "enabled": true,
      "description": "Enforce naming standards for clock and reset signals."
    }
  ]
}
```

Modifications to this file reload automatically in the editor. If the file is not present in the workspace, Theryx Verilog uses its default industrial ruleset.

### 2. Hardware Semantic Themes

Theryx Verilog provides 4 dedicated hardware color themes:

1. **theryx-dark (Default):** Modern high-visibility digital palette — Neon Cyan (Clock), Coral (Reset), Magenta (Reg), Mint (Wire), and Gold (Outputs).
2. **industrial-eda:** Engineering palette featuring high-distinction primary tones (Orange Clock, Red Reset, Blue Reg, Green Wire).
3. **high-contrast-pedagogic:** Maximum contrast primary colors tailored for teaching environments, video recordings, and projection screens.
4. **sync-vs-comb:** Strict architectural focus — warm tones for sequential state elements (Clock, Reset, Reg) and cool tones for combinational data paths (Wire, Inputs).

#### Changing the semantic theme:

- **Via Command Palette (Interactive):**
  1. Press `Ctrl + Shift + P`.
  2. Select:
     ```text
     Theryx: Selecionar Tema de Cores para Hardware (Semantic Highlighting)
     ```
  3. Choose your preferred theme from the list.

- **Via VS Code Settings (`settings.json`):**
  Set the `theryx.semanticTheme` property in user or workspace settings:
  ```json
  {
    "theryx.semanticTheme": "theryx-dark"
  }
  ```
  Supported options: `"theryx-dark"`, `"industrial-eda"`, `"high-contrast-pedagogic"`, `"sync-vs-comb"`.

---

## Installation (.vsix)

To install the distributed `.vsix` package:

### Method 1: Using the VS Code User Interface
1. Open Visual Studio Code.
2. Navigate to the **Extensions** view (`Ctrl + Shift + X`).
3. Click the **Views and More Actions** (`...`) menu at the top right of the Extensions panel.
4. Select **Install from VSIX...**.
5. Select the `theryx-verilog-0.1.0-alpha.vsix` package file.
6. Reload the window if prompted (`Ctrl + Shift + P` -> `Developer: Reload Window`).

### Method 2: Using the Command Line
Run the following command in your terminal:

```powershell
code --install-extension theryx-verilog-0.1.0-alpha.vsix --force
```

---

## System Requirements

- **Editor:** Visual Studio Code version 1.85.0 or higher.
- **Supported File Formats:** Verilog design files (`.v`) and header files (`.vh`).
- **Language Standard:** Verilog IEEE 1364.

---

## Author & License

- **Extension:** Theryx Verilog
- **Version:** 0.1.0-alpha
- **Author:** Luigi C. Filho
- **Organization:** LC Desenvolvimentos
- **License:** Distributed under the [MIT](LICENSE) license.
