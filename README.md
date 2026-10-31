# STM32F103C8T6 学习板空白 Keil 模板

本仓库只保留可复用 Keil/RTE 骨架、板卡资料和 Codex skill；不包含 AHT20、OLED、I²C、UI 等驱动或应用代码。

- `oled.uvprojx`：STM32F103C8 空白 Keil 工程，Source Group 只含空白 `main.c`。
- `RTE/`、`DebugConfig/`：Keil 器件启动和 RTE 配置。
- `BOARD_REFERENCE.md`：板载模块、引脚、电源和复用提醒。
- 原理图 PDF 和引脚说明 JPG：硬件原始资料。
- `.agents/skills/stm32-learning-board-keil/SKILL.md`：本板 Codex skill；`AGENTS.md`：仓库约定。

在 Keil 打开 `oled.uvprojx`，确认本机有 STM32F1 DFP/RTE 与 Arm Compiler，再按 F7 编译。目标按 64 KB Flash/20 KB SRAM 设置。
