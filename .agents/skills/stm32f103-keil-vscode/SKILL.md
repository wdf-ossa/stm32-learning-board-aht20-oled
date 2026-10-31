---
name: stm32f103-keil-vscode
description: Develop firmware for the KEYSKING STM32F103C8T6 learning board in VS Code/Codex while preserving its Keil project and using the documented module pin map.
---

# STM32F103C8T6 learning-board firmware skill

Use this skill for firmware work on the KEYSKING STM32 learning board in this repository. The repository is a blank Keil template: no peripheral drivers or application implementation are included. Hardware facts below are board-specific; use them instead of guessing pins from another F103 board.

## Board and toolchain baseline

- MCU: STM32F103C8T6, Cortex-M3; project memory configuration is 64 KB Flash and 20 KB SRAM.
- Board crystals: 8 MHz HSE and 32.768 kHz LSE. A project must explicitly configure its intended clock source; changing the clock also affects SysTick, UART, timer periods, and software bus timing.
- Project type: Keil uVision `.uvprojx` with RTE/device startup files. VS Code is the editor and Codex workspace; use Keil MDK and the installed STM32F1 device pack for build/flash unless a verified VS Code build task is added.
- The blank `main.c` intentionally does nothing. Add only modules required by the task.

## Board module and pin reference

| Module | Board pins | Electrical and integration notes |
|---|---|---|
| CH1116 OLED, 128×64 | PB6=SCL, PB7=SDA | Board marking 0x7A is an 8-bit write address; software 7-bit address is 0x3D. 3.3 V I²C. |
| AHT20 temperature/humidity | PB6=SCL, PB7=SDA | Board marking 0x70 is an 8-bit write address; software 7-bit address is 0x38. Shares the bus with OLED. |
| I²C expansion header | PB6=SCL, PB7=SDA | Shares the board bus; check address collisions and keep pull-ups at 3.3 V. |
| USB serial / CH343P | USART2: PA2=MCU TX, PA3=MCU RX | USB Type-C; for an external UART, cross TX/RX and share ground. |
| Bluetooth header | USART3: PB10=TX, PB11=RX | Check module IO voltage and common ground. |
| RGB LED | PA6=blue, PA7=green, PB0=red | On-board current-limit resistors; confirm active polarity before writing a driver. |
| User key | KEY1=PB12 | Press connects to ground; configure input pull-up and debounce. |
| EC11 encoder | A=PA8, B=PA9, push=PB15 | PB15 has no external pull-up; enable internal pull-up. A/B have external pull-ups and filtering. |
| NTC thermistor | PA4/ADC | 10 kΩ at 25°C, beta 3950; paired with 10 kΩ fixed resistor, PA4 is divider midpoint. |
| Potentiometer | PA5/ADC | 3.3 V-to-ground divider. |
| HC-SR04 ultrasonic | TRIG=PA11, ECHO=PA10 | Header supplies 5 V. Check Echo voltage compatibility; add divider/level shifting if needed. Use timer capture for pulse width. |
| TCRT5000 line sensor | PB14 | 3.3 V digital signal; confirm output polarity on the physical board. |
| Servo | PB8/PWM | 5 V supply; watch startup current and rail droop. |
| DRV8833 motor interface | PA0=PWM1, PA1=PWM2 | 5 V motor supply; use the driver truth table and do not drive motor current from MCU pins. |
| Relay | PB5 | Board transistor drives the coil. Relay contacts may carry hazardous voltage; validate with safe low-voltage loads. |
| Passive buzzer | PB9/PWM | Transistor-driven; a passive buzzer needs a PWM tone. |
| WS2812C array, about 11 LEDs | PB4/DATA | 5 V array. PB4 is configured open-drain with 5.1 kΩ pull-up to 5 V per schematic; timing is strict. |
| Power headers | 5 V, 3.3 V, GND | Share ground with peripherals. A 5 V header does not make all MCU inputs 5 V tolerant. |
| User GPIO header | PB2, PA12, PB3, PB1, PC13, plus shared bus pins | Check boot/debug and on-board circuit conflicts before use. |
| ST-LINK SWD | PA13=SWDIO, PA14=SWCLK | Preserve SWD for download/debug. NRST is available on board. |

## Shared-pin, boot, and electrical constraints

- PB6/PB7 connect the OLED, AHT20, and expansion header. Treat them as one shared 3.3 V open-drain I²C bus. Do not configure them as push-pull or add a 5 V pull-up.
- The board documents 0x70/0x7A as 8-bit write-address notation. Drivers that accept 7-bit I²C addresses must receive 0x38/0x3D respectively.
- PB3, PB4, and PA15 default to JTAG functions. To use them as GPIO, enable AFIO and disable JTAG while preserving SWD. PB4 also has the WS2812 open-drain/pull-up circuit.
- BOOT0 low selects normal Flash startup. Check BOOT1/PB2 as well when recovering a board in system boot mode. The CR1220 backs up VBAT/RTC.
- Check external logic voltage before connecting HC-SR04 Echo or other 5 V modules. PC13..PC15 have limited output drive; do not use them to drive loads directly.
- The schematic states personal non-commercial use and attribution to KEYSKING for redistribution; commercial use requires separate authorization.

## Implementation workflow

1. Open the repository root in VS Code. Read `AGENTS.md`, this skill, `BOARD_REFERENCE.md`, and the schematic/interface images before changing hardware behavior.
2. Inspect the `.uvprojx`, source groups, RTE/startup files, linker memory range, and current compiler settings. Preserve the Keil/RTE structure; do not convert to HAL/CubeMX/CMake just because the editor is VS Code.
3. Keep `main.c` as application orchestration. Place each new peripheral in a focused `.c/.h` module, initialize it once, and add its source to the Keil Source Group.
4. Use bounded waits and explicit error returns for bus/peripheral communication. Keep interrupt handlers short and move lengthy work to the main loop. Respect the F103C8 flash/RAM budget.
5. Review the diff and compile with the configured Keil toolchain. Report compile, download, and physical behavior as separate verification results; do not claim a peripheral works unless tested on the board.

## VS Code workflow

- Clone the repository and open the repository folder, not an individual source file. `.vscode/extensions.json` recommends OpenAI Codex and Microsoft C/C++ IntelliSense.
- Codex reads `AGENTS.md` and the repo-local skill under `.agents/skills/stm32f103-keil-vscode/`.
- VS Code supports editing/AI assistance here; the current blank project is Keil-based and has no standalone GCC/CMake build configuration. Use Keil MDK to build and flash.
