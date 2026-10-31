# STM32F103C8 learning-board Keil blank template

Clone this repository and open the folder in VS Code. Open the repository root so Codex can read `AGENTS.md` and `.agents/skills/stm32f103-keil-vscode/SKILL.md`.

## Quick start

```powershell
git clone https://github.com/wdf-ossa/stm32-learning-board-aht20-oled.git
code stm32-learning-board-aht20-oled
```

Install the recommended extensions shown by VS Code: OpenAI Codex and Microsoft C/C++ IntelliSense. The recommendations are in `.vscode/extensions.json`.

## Build and flash

The Keil project is `oled.uvprojx`. VS Code is the editor/AI workspace; use Keil MDK and the STM32F1 device pack to compile and the configured ST-LINK path to flash. This repository does not provide a standalone GCC/CMake build. Preserve the STM32F103C8 target and its startup/RTE/memory settings.

The firmware entry point is intentionally empty. Add only modules needed by the next task. Hardware connections and board-specific electrical limits are in `BOARD_REFERENCE.md` and the bundled schematic/interface docs; the skill itself remains toolchain-focused and contains no board pin map.
