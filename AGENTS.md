# Codex project instructions - learning-board blank template

- This public repository is a blank Keil/RTE skeleton; it contains no peripheral drivers or application implementation.
- Use `BOARD_REFERENCE.md` and bundled schematic/interface docs for pins, voltage, and conflicts.
- I2C 7-bit addresses are AHT20 `0x38` and OLED `0x3D`; PB6/PB7 are shared open-drain pins.
- Preserve STM32F103C8T6, RTE/startup setup, 64 KB Flash and 20 KB RAM unless asked to migrate.
- Check voltage, shared pins, and JTAG/SWD before choosing GPIO. Report physical validation separately from compilation.
