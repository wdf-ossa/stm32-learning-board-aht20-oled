---
name: stm32-learning-board-keil
description: Create and maintain reusable Keil projects for the KEYSKING STM32F103C8T6 learning board using its documented hardware pin map and safety constraints.
---

# Learning-board Keil skill

Use for work on the KEYSKING STM32F103C8T6 learning board.

1. Read `BOARD_REFERENCE.md` before selecting pins or connecting hardware. Use the bundled schematic/interface image for board-specific facts.
2. This repository is intentionally blank; no AHT20, OLED, I2C or UI driver implementation is present. Add only modules required by the task.
3. Preserve the Keil/RTE/startup setup, STM32F103C8T6 target, 64 KB Flash and 20 KB SRAM unless asked to migrate.
4. PB6/PB7 are shared 3.3 V open-drain I2C. If implementing those devices, use 7-bit AHT20 `0x38` and OLED `0x3D`; board labels `0x70`/`0x7A` are 8-bit write addresses.
5. Check voltage tolerance and pin conflicts first. PB3/PB4/PA15 are JTAG by default; disable JTAG but preserve SWD when repurposing them. PB4 has the WS2812 open-drain/pull-up circuit.
6. Keep `main.c` as orchestration; use one `.c/.h` pair per module and add source files to the Keil target.
7. Build in Keil when available and distinguish build success from physical board verification.
