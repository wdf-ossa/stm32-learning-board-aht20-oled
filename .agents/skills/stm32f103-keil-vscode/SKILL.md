---
name: stm32f103-keil-vscode
description: Develop or troubleshoot STM32F103 firmware in a repository opened with VS Code and Codex, especially Keil uVision projects.
---

# STM32F103 firmware workflow

Use for STM32F103 C projects opened from their repository root in VS Code with Codex. This skill is toolchain-aware and does not assume a specific board, pin map, peripheral set, or firmware framework.

## Workflow

1. **Identify the project before editing.** Read the README and `AGENTS.md`; inspect the Keil `.uvprojx`, source groups, RTE/startup files, linker memory settings, compiler selection, and existing source modules. If no schematic or board reference exists, do not invent pin assignments.
2. **Preserve the selected toolchain.** Keep Keil/SPL, HAL/CubeMX, bare-metal, or another existing framework consistent with the repository. Do not convert a Keil project to HAL/CMake merely because VS Code is the editor. Avoid editing generated startup, RTE, vendor-pack, or build-output files unless the task requires it.
3. **Design small changes.** Keep `main.c` focused on application flow. Put each reusable peripheral or service in a focused `.c/.h` module with a narrow API. Initialize hardware once in a clear place; avoid duplicate peripheral ownership.
4. **Respect Cortex-M constraints.** Check target memory and clock configuration before changing startup or timing. Keep interrupt handlers short and defer lengthy work. Bound polling and blocking waits with timeouts and explicit error paths. Avoid dynamic allocation unless the project has a clear bounded policy.
5. **Use the repository's build path.** For an existing `.uvprojx`, use Keil to build/download unless the repository explicitly provides a tested VS Code build task. VS Code/Codex editing support does not by itself provide the Arm toolchain, probe drivers, or flash configuration.
6. **Verify in layers.** Review the diff; build with the configured toolchain; then distinguish download success from actual on-board behavior. Report compile, flash, and hardware observations separately. Do not claim a physical test unless performed on the target.

## VS Code and Codex

Open the cloned repository folder, not an individual source file. Keep durable project constraints in `AGENTS.md`, hardware facts in board documentation, and this skill limited to the reusable development workflow. Repository-local skills live under `.agents/skills/`.

## Stop and ask for missing evidence

Ask for board documentation when a task requires pin assignment, external voltage limits, or peripheral wiring that the repository does not establish. Ask before changing the project's framework, linker map, startup/vector table, or target device when the requested behavior can be implemented without that migration.
