# KEYSKING STM32F103C8T6 学习板硬件速查

本仓库是空白 Keil 模板；本文件描述硬件，不提供任何外设驱动或应用代码。MCU 为 STM32F103C8T6（Cortex-M3），工程按 64 KB Flash/20 KB SRAM 设置。板上有 8 MHz HSE、32.768 kHz LSE；电源路径为 USB/ST-LINK 5 V 防倒灌，AP2112K-3.3 生成 3.3 V。ST-LINK SWD：PA13=SWDIO、PA14=SWCLK；NRST 按键；BOOT0 低为正常 Flash 启动，BOOT1/PB2 需同时核对。CR1220 接 VBAT。

## 引脚和外设

| 模块 | 引脚 | 供电/注意事项 |
|---|---|---|
| CH1116 OLED，128×64 | PB6=SCL, PB7=SDA | 资料写 0x7A（8 位写地址），7 位地址 0x3D；3.3 V I²C。 |
| AHT20 温湿度 | PB6=SCL, PB7=SDA | 资料写 0x70（8 位写地址），7 位地址 0x38；与 OLED 共总线。 |
| I²C 扩展 | PB6=SCL, PB7=SDA | 检查地址冲突及 3.3 V 上拉。 |
| USB 转串口 CH343P | USART2：PA2=MCU TX，PA3=MCU RX | USB Type-C；UART 端 TX/RX 交叉。 |
| 蓝牙接口 | USART3：PB10=TX, PB11=RX | 检查电平并共地。 |
| RGB LED | PA6=蓝，PA7=绿，PB0=红 | 带限流电阻，确认极性。 |
| KEY1/KEY2 | PB12/PB13 | 按下接地，输入需上拉和消抖。 |
| EC11 | A=PA8，B=PA9，按键=PB15 | PB15 无外部上拉；A/B 有上拉和滤波。 |
| NTC | PA4/ADC | NTC 10 kΩ@25°C，β=3950；10 kΩ 分压电阻；PA4 为中点。 |
| 电位器 | PA5/ADC | 3.3 V-GND 分压。 |
| HC-SR04 | TRIG=PA11，ECHO=PA10 | 排针有 5 V；确认 Echo 电平兼容，必要时分压。 |
| TCRT5000 | PB14 | 3.3 V 数字信号，极性实测。 |
| 舵机 | PB8/PWM | 5 V，留意启动电流和压降。 |
| DRV8833 | PA0=PWM1，PA1=PWM2 | 5 V 电机电源，按驱动器真值表控制。 |
| 继电器 | PB5 | 三极管驱动，触点高压有危险。 |
| 无源蜂鸣器 | PB9/PWM | 三极管驱动，需 PWM。 |
| WS2812C 阵列（约 11 颗） | PB4/DATA | 5 V；PB4 开漏、5.1 kΩ 上拉至 5 V；注意 JTAG 复用。 |
| 用户 GPIO | PB2、PA12、PB3、PB1、PC13 等 | 使用前核对 BOOT/JTAG/SWD 和板上负载。 |

## 电平、复用与资料

- 原理图地址 0x70/0x7A 是含 R/W 位的 8 位写地址；软件 I²C 通常用 7 位地址 0x38/0x3D。
- PB6/PB7 是共享开漏 I²C，总线上拉到 3.3 V。勿将其配置成推挽或把上拉接 5 V。
- PB3/PB4/PA15 默认复用 JTAG；用作 GPIO 时打开 AFIO 并关闭 JTAG、保留 PA13/PA14 的 SWD。PC13..PC15 输出驱动弱，不带负载。
- 5 V 外设不代表 MCU 所有输入耐 5 V，尤其检查 HC-SR04 Echo。电机、舵机和灯带需评估 5 V 电源能力并共地。
- 原理图许可说明：个人非营利用途，转发需注明 KEYSKING，商业用途另行确认授权。

附带原始资料：学习板电路原理图 PDF 与引脚/接口说明 JPG。
