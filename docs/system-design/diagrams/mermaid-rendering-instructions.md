# Mermaid Diagram Rendering Instructions

This directory contains master Mermaid (`.mmd`) source files for all visual diagrams in the PyaarPremaKaadhal system design specification.

---

## 1. Required Tooling

Diagram rendering requires `@mermaid-js/mermaid-cli` (`mmdc`), an official command-line utility for converting Mermaid definition files into PNG images or SVG vector graphics.

### Installation Options

#### Option A: One-time Execution via `npx` (Recommended — No Install Required)
Ensure Node.js (v18 or higher) is installed, then execute commands directly using `npx`.

#### Option B: Global NPM Installation
```bash
npm install -g @mermaid-js/mermaid-cli
```

---

## 2. Exact Render Commands

Run the following commands from the project root directory (`c:\Users\Administrator\Downloads\Team-CodeRed-System-Design-IQOO`):

### Command 1: Render High-Level Architecture Overview
```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/architecture-overview.mmd -o docs/system-design/diagrams/architecture-overview.png --background "#FFF9F7"
```

### Command 2: Render On-Device AI Flow Sequence
```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/on-device-ai-flow.mmd -o docs/system-design/diagrams/on-device-ai-flow.png --background "#FFF9F7"
```

### Command 3: Render Tap-to-Connect Offline Discovery Flow
```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/tap-to-connect-flow.mmd -o docs/system-design/diagrams/tap-to-connect-flow.png --background "#FFF9F7"
```

### Command 4: Render Cosine Matching & Cosmic 101 Rarity Logic
```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/matching-flow.mmd -o docs/system-design/diagrams/matching-flow.png --background "#FFF9F7"
```

---

## 3. Batch Script for Automatic Conversion (All Diagrams)

### On Linux / macOS (Bash)
```bash
for file in docs/system-design/diagrams/*.mmd; do
    npx -p @mermaid-js/mermaid-cli mmdc -i "$file" -o "${file%.mmd}.png" --background "#FFF9F7"
done
```

### On Windows (PowerShell)
```powershell
Get-ChildItem docs/system-design/diagrams/*.mmd | ForEach-Object {
    $pngPath = $_.FullName.Replace(".mmd", ".png")
    npx -p @mermaid-js/mermaid-cli mmdc -i $_.FullName -o $pngPath --background "#FFF9F7"
}
```

---

## 4. SVG Vector Output Option

For publication or high-DPI scaling, render vector SVG outputs by changing the `-o` extension to `.svg`:

```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/architecture-overview.mmd -o docs/system-design/diagrams/architecture-overview.svg --background "#FFF9F7"
```
