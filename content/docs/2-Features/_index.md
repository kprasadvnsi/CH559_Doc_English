---
title: 2. Features
type: docs
weight: 2
BookToC: false
---

# 8-bit enhanced USB microcontroller CH559

## 2. Features

+ Core: Enhanced E8051 core, compatible with MCS51 instruction set, 79% of instructions are single-byte single-cycle instructions, average command speed is 8~15 times faster than standard MCS51, special XRAM data fast copy instruction, double DPTR pointer.

+ ROM: 64KB non-volatile memory Flash-ROM, support 100K erasing, can be used for program storage space; or can be divided into 60KB program storage area and 1KB data storage area and 3KB boot code BootLoader/ISP program area.

+ RAM: 256 bytes internal iRAM, can be used for fast data temporary storage and stack; 6KB on-chip xRAM, can be used for large data temporary storage and DMA direct memory access; support for external expansion of 32KB external SRAM.

+ USB: Embedded USB controller and dual USB transceiver, support USB-Host host mode and USB-Device device mode, support USB 2.0 full speed 12Mbps or low speed 1.5Mbps, USB host mode can manage two simultaneously through dual port Root-HUB USB devices. Supports up to 64 bytes of data packets, built-in FIFO, and support for DMA.

+ Timer: 4 groups of timers, T0/T1/T2 are standard MCS51 timers; T2 is extended to support 2 signal captures; TMR3 has 8 levels of FIFOs, supports DMA, supports signal capture sampling and 16-bit PWM output.

+ PWM: 3 sets of PWM outputs, PWM1/PWM2 are 2 8-bit PWM outputs; TMR3 supports 16-bit PWM outputs.

+ UART: 2 sets of asynchronous serial port, UART0 is standard MCS51 serial port; UART1 is compatible with 16C550, built-in 8-stage FIFO, supports Modem signal, supports RS485 half-duplex mode, supports preset local address for automatic matching in multi-machine communication.

+ SPI: 2 sets of SPI controllers with clock frequency up to half of the system's main frequency Fsys, supporting serial data input and output simplex multiplexing. SPI0 has built-in FIFO and supports Master/Slave master-slave mode; SPI1 only supports host mode.

+ ADC: 8-channel 10-bit or 11-bit A/D analog-to-digital converter with built-in 2-stage FIFO, support for DMA, support for up to 1MSPS sample rate, and support for two-channel automatic wheel test.

+ LED-CTRL: LED screen control card data transmission interface, built-in 4-level FIFO, support DMA, support 1/2/4 data line interface, clock frequency up to half of the system frequency Fsys.

+ XBUS: 8-bit parallel external bus, compatible with standard MCS51 bus, used to connect off-chip SRAM memory or other peripherals, support direct 15-bit address or ALE multiplexed lower 8-bit address, support 4 kinds of bus access speed.

+ GPIO: Supports up to 45 GPIO pins (including XI/XO and RST and USB signal pins), 3.3V voltage output, and supports 5V withstand voltage input except P1.0~P1.7, XI, XO, RST.

+ Interrupt: Supports 14 sets of interrupt sources, including 6 sets of interrupts compatible with standard MCS51 (INT0, T0, INT1, T1, UART0, T2), and extended 8 sets of interrupts (SPI0, TMR3, USB, ADC, UART1, PWM1) , GPIO, WDOG), where the GPIO interrupt can be selected from 7 pins.

+ Watch-Dog: 8-bit preset watchdog timer WDOG, support for timed interrupts.

+ Reset: Supports 4 kinds of reset signal sources, built-in power-on reset, support software reset and watchdog overflow reset, optional external input input reset.

+ Clock: Built-in 12MHz clock source, external crystal can be supported by multiplexed GPIO pin, built-in PLL is used to generate USB clock and system frequency Fsys of required frequency.

+ Power: Built-in 5V to 3.3V low dropout voltage regulator with internal working voltage of 3.3V and support for external 3.3V or 5V power input. Support low-power sleep, support USB, UART0, UART1, SPI0 and some GPIO external wake-up.

+ The chip has a unique ID number built in, supporting ID numbers and verification.