---
title: 5. Special function register SFR
type: docs
weight: 5
BookToC: false
---

# 5. Special function register SFR

The following abbreviations may be used in describing the registers in this manual:

| Abbreviation | Description |
|--------------|-------------|
| RO | Indicates access type: read only |
| WO | Indicates the access type: write only, the value read is invalid |
| RW | Indicates access type: readable and writable |
| h | End with hexadecimal number |
| b | Expressed as a binary number |

## 5.1 SFR profile and address distribution

The CH559 uses the special function registers SFR and xSFR to control, manage, and set the operating mode.

The SFR occupies the 80h-FFh address range of the internal data storage space and can only be accessed by direct address mode instructions.

Registers whose address is x0h or x8h are bit-addressable, which avoids changing the value of other bits when accessing a specific bit; registers other than a multiple of 8 can only be accessed byte by byte.

Some SFRs can only write data in safe mode, but are read-only in non-secure mode, such as: GLOBAL_CFG, PLL_CFG, CLOCK_CFG, SLEEP_CTRL, WAKE_CTRL.

Some SFRs have one or more aliases, such as: SPI0_CK_SE/SPI0_S_PRE, UDEV_CTRL/UHUB0_CTRL, UEP1_CTRL/UH_SETUP, UEP2_CTRL/UH_RX_CTRL, UEP2_T_LEN/UH_EP_PID, UEP3_CTRL/UH_TX_CTRL, UEP3_T_LEN/UH_TX_LEN, P5_PIN/P4_CFG.

Partial addresses correspond to multiple independent SFRs, for example: TL2/T2CAP1L, TH2/T2CAP1H, SAFE_MOD/CHIP_ID, T3_COUNT_L/T3_CK_SE_L, T3_COUNT_H/T3_CK_SE_H, SER1_FIFO/SER1_RBR/SER1_THR/SER1_DLL, SER1_IER/SER1_DLM, SER1_IIR/SER1_FCR, SER1_ADDR/SER1_DIV, ROM_CTRL/ROM_STATUS.

xSFR occupies the 2440h-298Fh address range of the xdata type of the external data storage space, or the 40H-8Fh address range of the pdata type. xSFR can only be accessed in bytes by indirect addressing via the MOVX instruction. 

The default is based on the DPTR pointer; however, after bXIR_XSFR is set, the faster R0 or R1 can be used as a pdata type pointer to access xSFRs named `pU*` and `pLED_*`.

Some xSFRs have one or more aliases, for example: UEP2_3_MOD/UH_EP_MOD, UEP2_DMA_H/UH_RX_DMA_H, UEP2_DMA_L/UH_RX_DMA_L, UEP2_DMA/UH_RX_DMA, UEP3_DMA_H/UH_TX_DMA_H, UEP3_DMA_L/UH_TX_DMA_L, UEP3_DMA/UH_TX_DMA.

Partial addresses correspond to multiple independent xSFRs, for example: LED_DATA/LED_FIFO_CN.

The CH559 contains all the registers of the 8051 standard SFR, and other device control registers have been added. The specific SFR is shown in the table below.
<div>
    <p align="center">Table 5.1 Special Function Register Table</p>
</div>

![SFR_Table](/docs/5-SFR/images/SFR_Table.png "SFR Table")

Remarks: (1), Text in red means it can be addressed by bit; (2), the following is the corresponding description of the color box.


<table>
    <tr>
        <th>Color</th>
        <th>Description</th>
    </tr>
    <tr><td bgcolor="#a6a6a6"></td><td>Register address</td></tr>
    <tr><td bgcolor="#cc99ff"></td><td>SPI0 related register</td></tr>
    <tr><td bgcolor="#99ccff"></td><td>ADC related register</td></tr>
    <tr><td bgcolor="#ccffcc"></td><td>USB related register</td></tr>
    <tr><td bgcolor="#ccffff"></td><td>Timer/Counter 2 Related Registers</td></tr>
    <tr><td bgcolor="#ffff99"></td><td>Port setting related register</td></tr>
    <tr><td bgcolor="#ff99cc"></td><td>SPI1 related register</td></tr>
    <tr><td bgcolor="#00ffff"></td><td>PWM1 and PWM2 related registers</td></tr>
    <tr><td bgcolor="#ffff00"></td><td>UART1 related register</td></tr>
    <tr><td bgcolor="#808000"></td><td>Timer/Counter 0 and 1 related registers</td></tr>
    <tr><td bgcolor="#008080"></td><td>Flash-ROM related register</td></tr>
</table>

## 5.2 SFR types and reset values

<table>
    <tr>
        <th>Function type</th><th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td rowspan="11">System settings register</td><td>B</td><td>F0h</td><td>B register</td><td>0000 0000b</td></tr>
    <tr><td>ACC</td><td>E0h</td><td>accumulator</td><td>0000 0000b</td></tr>
    <tr><td>PSW</td><td>D0h</td><td>Program status register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="2">GLOBAL_CFG</td><td rowspan="2">B1h</td><td>Global configuration register (in the bootloader state)</td><td>1110 0000b</td></tr>
    <tr><td>Global configuration register (in application state)</td><td>1100 0000b</td></tr>
    <tr><td>CHIP_ID</td><td>A1h</td><td>Chip ID (read only)</td><td>0101 1001b</td></tr>
    <tr><td>SAFE_MOD</td><td>A1h</td><td>Safe mode control register (write only)</td><td>0000 0000b</td></tr>
    <tr><td>DPH</td><td>83h</td><td>Data address pointer is 8 bits high</td><td>0000 0000b</td></tr>
    <tr><td>DPL</td><td>82h</td><td>Data address pointer is lower 8 bits</td><td>0000 0000b</td></tr>
    <tr><td>DPTR</td><td>82h</td><td>DPL and DPH form a 16-bit SFR</td><td>0000h</td></tr>
    <tr><td>SP</td><td>81h</td><td>Stack pointer</td><td>0000 0111b</td></tr>
    <tr><td rowspan="7">Clock, sleep, and<br> power control registers</td><td>WDOG_COUNT</td><td>FFh</td><td>Watchdog count register</td><td>0000 0000b</td></tr>
    <tr><td>RESET_KEEP</td><td>FEh</td><td>Reset holding register (in power-on reset state)</td><td>0000 0000b</td></tr>
    <tr><td>WAKE_CTRL</td><td>EBh</td><td>Sleep wake control register</td><td>0000 0000b</td></tr>
    <tr><td>SLEEP_CTRL</td><td>EAh</td><td>Sleep control register</td><td>0000 0000b</td></tr>
    <tr><td>CLOCK_CFG</td><td>B3h</td><td>System clock configuration register</td><td>1001 1000b</td></tr>
    <tr><td>PLL_CFG</td><td>B2h</td><td>PLL clock configuration register</td><td>1101 1000b</td></tr>
    <tr><td>PCON</td><td>87h</td><td>Power control register (in power-on reset state)</td><td>0001 0000b</td></tr>
    <tr><td rowspan="5">Interrupt control register</td><td>IP_EX</td><td>E9h</td><td>Extended interrupt priority control register</td><td>0000 0000b</td></tr>
    <tr><td>IE_EX</td><td>E8h</td><td>Extended interrupt enable register</td><td>0000 0000b</td></tr>
    <tr><td>GPIO_IE</td><td>CFh</td><td>GPIO interrupt enable register</td><td>0000 0000b</td></tr>
    <tr><td>IP</td><td>B8h</td><td>Interrupt priority control register</td><td>0000 0000b</td></tr>
    <tr><td>IE</td><td>A8h</td><td>Interrupt enable register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="8">Flash-ROM register</td><td>ROM_DATA_H</td><td>8Fh</td><td>flash-ROM data register high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_DATA_L</td><td>8Eh</td><td>Flash-ROM data register low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_DATA</td><td>8Eh</td><td>ROM_DATA_L and ROM_DATA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>ROM_STATUS</td><td>86h</td><td>flash-ROM status register (read only)</td><td>1000 0000b</td></tr>
    <tr><td>ROM_CTRL</td><td>86h</td><td>flash-ROM control register (write only)</td><td>0000 0000b</td></tr>
    <tr><td>ROM_ADDR_H</td><td>85h</td><td>flash-ROM address register high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_ADDR_L</td><td>84h</td><td>Flash-ROM address register low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>ROM_ADDR</td><td>84h</td><td>ROM_ADDR_L and ROM_ADDR_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td rowspan="24">Port setting register</td><td>XBUS_SPEED</td><td>FDh</td><td>External bus speed configuration register</td><td>1111 1111b</td></tr>
    <tr><td>XBUS_AUX</td><td>FDh</td><td>External bus auxiliary setting register</td><td>0000 0000b</td></tr>
    <tr><td>PIN_FUNC</td><td>CEh</td><td>Pin function select register</td><td>0000 0000b</td></tr>
    <tr><td>P4_CFG</td><td>C7h</td><td>P4 port configuration register</td><td>0000 0000b</td></tr>
    <tr><td>P5_IN</td><td>C7h</td><td>P5 port input register (read only)</td><td>0000 0000b</td></tr>
    <tr><td>PORT_CFG</td><td>C6h</td><td>Port configuration register</td><td>0000 1111b</td></tr>
    <tr><td rowspan="2">P0_PU</td><td rowspan="2">C5h</td><td>P0 port pull-up enable register (En_P0_Pullup=0)</td><td>0000 0000b</td></tr>
    <tr><td>P0 port pull-up enable register (En_P0_Pullup=1)</td><td>1111 1111b</td></tr>
    <tr><td>P0_DIR</td><td>C4h</td><td>P0 port direction control register</td><td>0000 0000b</td></tr>
    <tr><td>P4_PU</td><td>C3h</td><td>P4 port pull-up enable register</td><td>1111 1111b</td></tr>
    <tr><td>P4_DIR</td><td>C2h</td><td>P4 port direction control register</td><td>0000 0000b</td></tr>
    <tr><td>P4_IN</td><td>C1h</td><td>P4 port input register (read only)</td><td>1111 1111b</td></tr>
    <tr><td>P4_OUT</td><td>C0h</td><td>P4 port output register</td><td>0000 0000b</td></tr>
    <tr><td>P3_PU</td><td>BFh</td><td>P3 port direction control register</td><td>1111 1111b</td></tr>
    <tr><td>P3_DIR</td><td>BEh</td><td>P3 port pull-up enable register</td><td>0000 0000b</td></tr>
    <tr><td>P2_PU</td><td>BDh</td><td>P2 port pull-up enable register</td><td>1111 1111b</td></tr>
    <tr><td>P2_DIR</td><td>BCh</td><td>P2 port direction control register</td><td>0000 0000b</td></tr>
    <tr><td>P1_PU</td><td>BBh</td><td>P1 port pull-up enable register</td><td>1111 1111b</td></tr>
    <tr><td>P1_DIR</td><td>BAh</td><td>P1 port direction control register</td><td>0000 0000b</td></tr>
    <tr><td>P1_IE</td><td>B9h</td><td>P1 port input enable register</td><td>1111 1111b</td></tr>
    <tr><td>P3</td><td>B0h</td><td>P3 port input and output registers</td><td>1111 1111b</td></tr>
    <tr><td>P2</td><td>A0h</td><td>P2 port input and output registers</td><td>1111 1111b</td></tr>
    <tr><td>P1</td><td>90h</td><td>P1 port input and output registers</td><td>1111 1111b</td></tr>
    <tr><td>P0</td><td>80h</td><td>P0 port input and output registers</td><td>1111 1111b</td></tr>   
    <tr><td rowspan="6">Timer/Counter 0 and 1 registers</td><td>TH1</td><td>8Dh</td><td>Timer1 count high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>TH0</td><td>8Ch</td><td>Timer0 count high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>TL1</td><td>8Bh3</td><td>Timer1 count low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>TL0</td><td>8Ah</td><td>Timer0 count low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>TMOD</td><td>89h</td><td>Timer0/1 mode register</td><td>0000 0000b</td></tr>
    <tr><td>TCON</td><td>88h</td><td>Timer0/1 Control Register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="2">UART0 register</td><td>SBUF</td><td>99h</td><td>UART0 data register</td><td>xxxx xxxxb</td></tr>
    <tr><td>SCON</td><td>98h</td><td>UART0 control register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="11">Timer/Counter 2 Registers</td><td>TH2</td><td>CDh</td><td>Timer2 counter high byte</td><td>0000 0000b</td></tr>
    <tr><td>TL2</td><td>CCh</td><td>Timer2 counter low byte</td><td>0000 0000b</td></tr>
    <tr><td>T2COUNT</td><td>CCh</td><td>TL2 and TH2 form a 16-bit SFR</td><td>0000h</td></tr>
    <tr><td>T2CAP1H</td><td>CDh</td><td>Timer2 capture 1 data high byte (read only)</td><td>xxxx xxxxb</td></tr>
    <tr><td>T2CAP1L</td><td>CCh</td><td>Timer2 capture 1 data low byte (read only)</td><td>xxxx xxxxb</td></tr>
    <tr><td>T2CAP1</td><td>CCh</td><td>T2CAP1L and T2CAP1H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>RCAP2H</td><td>CBh</td><td>Count reload/capture 2 data register high byte</td><td>0000 0000b</td></tr>
    <tr><td>RCAP2L</td><td>CAh</td><td>Count reload/capture 2 data register low byte</td><td>0000 0000b</td></tr>
    <tr><td>RCAP2</td><td>CAh</td><td>RCAP2L and RCAP2H form a 16-bit SFR</td><td>0000h</td></tr>
    <tr><td>T2MOD</td><td>C9h</td><td>Timer2 mode register</td><td>0000 0000b</td></tr>
    <tr><td>T2CON</td><td>C8h</td><td>Timer2 Control Register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="19">Timer/counter 3 registers</td><td>T3_FIFO_H</td><td>AFh</td><td>Timer3 FIFO high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_FIFO_L</td><td>AEh</td><td>Timer3 FIFO low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_FIFO</td><td>AEh</td><td>T3_FIFO_L and T3_FIFO_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>T3_DMA_AH</td><td>ADh</td><td>DMA current buffer address high byte</td><td>0000 xxxxb</td></tr>
    <tr><td>T3_DMA_AL</td><td>ACh</td><td>DMA current buffer address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>T3_DMA</td><td>ACh</td><td>T3_DMA_AL and T3_DMA_AH form a 16-bit SFR</td><td>0xxxh</td></tr>
    <tr><td>T3_DMA_CN</td><td>ABh</td><td>DMA residual count register</td><td>0000 0000b</td></tr>
    <tr><td>T3_CTRL</td><td>AAh</td><td>Timer3 Control Register</td><td>0000 0010b</td></tr>
    <tr><td>T3_STAT</td><td>A9h</td><td>Timer3 Status Register</td><td>0000 0000b</td></tr>
    <tr><td>T3_END_H</td><td>A7h</td><td>Timer3 counts the final high byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_END_L</td><td>A6h</td><td>Timer3 counts the final low byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>T3_END</td><td>A6h</td><td>T3_END_L and T3_END_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>T3_COUNT_H</td><td>A5h</td><td>Timer3 current count high byte (read only)</td><td>0000 0000b</td></tr>
    <tr><td>T3_COUNT_L</td><td>A4h</td><td>Timer3 current count low byte (read only)</td><td>0000 0000b</td></tr>
    <tr><td>T3_COUNT</td><td>A4h</td><td>T3_COUNT_L and T3_COUNT_H form a 16-bit SFR</td><td>0000h</td></tr>
    <tr><td>T3_CK_SE_H</td><td>A5h</td><td>Timer3 clock divider setting high byte</td><td>0000 0000b</td></tr>
    <tr><td>T3_CK_SE_L</td><td>A4h</td><td>Timer3 clock divider sets the low byte</td><td>0010 0000b</td></tr>
    <tr><td>T3_CK_SE</td><td>A4h</td><td>T3_CK_SE_L and T3_CK_SE_H form a 16-bit SFR</td><td>0020h</td></tr>
    <tr><td>T3_SETUP</td><td>A3h</td><td>Timer3 setup register</td><td>0000 0100b</td></tr>
    <tr><td rowspan="5">PWM1 and PWM2 registers</td><td>PWM_CYCLE</td><td>9Fh</td><td>PWM cycle period register</td><td>xxxx xxxxb</td></tr>
    <tr><td>PWM_CK_SE</td><td>9Eh</td><td>PWM clock divider setting register</td><td>0000 0000b</td></tr>
    <tr><td>PWM_CTRL</td><td>9Dh</td><td>PWM control register</td><td>0000 0010b</td></tr>
    <tr><td>PWM_DATA</td><td>9Ch</td><td>PWM1 data register</td><td>xxxx xxxxb</td></tr>
    <tr><td>PWM_DATA2</td><td>9Bh</td><td>PWM2 data register</td><td>xxxx xxxxb</td></tr>
    <tr><td rowspan="6">SPI0 register</td><td>SPI0_SETUP</td><td>FCh</td><td>SPI0 setting register</td><td>0000 0000b</td></tr>
    <tr><td>SPI0_S_PRE</td><td>FBh</td><td>SPI0 slave mode preset data register</td><td>0010 0000b</td></tr>
    <tr><td>SPI0_CK_SE</td><td>FBh</td><td>SPI0 clock divider setting register</td><td>0010 0000b</td></tr>
    <tr><td>SPI0_CTRL</td><td>FAh</td><td>SPI0 control register</td><td>0000 0010b</td></tr>
    <tr><td>SPI0_DATA</td><td>F9h</td><td>SPI0 data transceiver register</td><td>xxxx xxxxb</td></tr>
    <tr><td>SPI0_STAT</td><td>F8h</td><td>SPI0 status register</td><td>0000 1000b</td></tr>
    <tr><td rowspan="4">SPI1 register</td><td>SPI1_CK_SE</td><td>B7h</td><td>SPI1 clock divider setting register</td><td>0010 0000b</td></tr>
    <tr><td>SPI1_CTRL</td><td>B6h</td><td>SPI1 control register</td><td>0000 0010b</td></tr>
    <tr><td>SPI1_DATA</td><td>B5h</td><td>SPI1 data transceiver register</td><td>xxxx xxxxb</td></tr>
    <tr><td>SPI1_STAT</td><td>B4h</td><td>SPI1 status register</td><td>0000 1000b</td></tr>
    <tr><td rowspan="12">UART1 register</td><td>SER1_DLL</td><td>9Ah</td><td>UART1 Baud Rate Divisor Latch Low Byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>SER1_FIFO</td><td>9Ah</td><td>UART1 data FIFO read and write registers</td><td>xxxx xxxxb</td></tr>
    <tr><td>SER1_DIV</td><td>97h</td><td>UART1 prescaler divisor register</td><td>0xxx xxxxb</td></tr>
    <tr><td>SER1_ADDR</td><td>97h</td><td>UART1 bus address preload register</td><td>1111 1111b</td></tr>
    <tr><td>SER1_MSR</td><td>96h</td><td>UART1 Modem MODEM Status Register (Read Only)</td><td>1111 0000b</td></tr>
    <tr><td>SER1_LSR</td><td>95h</td><td>UART1 line status register (read only)</td><td>0110 0000b</td></tr>
    <tr><td>SER1_MCR</td><td>94h</td><td>UART1 Modem MODEM Control Register</td><td>0000 0000b</td></tr>
    <tr><td>SER1_LCR</td><td>93h</td><td>UART1 line control register</td><td>0000 0000b</td></tr>
    <tr><td>SER1_IIR</td><td>92h</td><td>UART1 Interrupt Identification Register (Read Only)</td><td>0000 0001b</td></tr>
    <tr><td>SER1_FCR</td><td>92h</td><td>FIFO control register (write only)</td><td>0000 0000b</td></tr>
    <tr><td>SER1_DLM</td><td>91h</td><td>UART1 baud rate divisor latch high byte</td><td>1000 0000b</td></tr>
    <tr><td>SER1_IER</td><td>91h</td><td>UART1 interrupt enable register</td><td>0000 0000b</td></tr>
    <tr><td rowspan="13">ADC register</td><td>ADC_EX_SW</td><td>F7h</td><td>ADC Extended Analog Switch Control Register</td><td>0000 0000b</td></tr>
    <tr><td>ADC_SETUP</td><td>F6h</td><td>ADC setup register</td><td>0000 1000b</td></tr>
    <tr><td>ADC_FIFO_H</td><td>F5h</td><td>FIFO high byte of ADC (read only)</td><td>0000 0xxxb</td></tr>
    <tr><td>ADC_FIFO_L</td><td>F4h</td><td>ADC FIFO low byte (read only)</td><td>xxxx xxxxb</td></tr>
    <tr><td>ADC_FIFO</td><td>F4h</td><td>ADC_FIFO_L and ADC_FIFO_H form a 16-bit SFR</td><td>0xxxh</td></tr>
    <tr><td>ADC_CHANN</td><td>F3h</td><td>ADC channel select register</td><td>0000 0000b</td></tr>
    <tr><td>ADC_CTRL</td><td>F2h</td><td>ADC control register</td><td>0000 0000b</td></tr>
    <tr><td>ADC_STAT</td><td>F1h</td><td>ADC status register</td><td>0000 0100b</td></tr>
    <tr><td>ADC_CK_SE</td><td>EFh</td><td>ADC clock divider setting register</td><td>0001 0000b</td></tr>
    <tr><td>ADC_DMA_CN</td><td>EEh</td><td>DMA residual count register</td><td>0000 0000b</td></tr>
    <tr><td>ADC_DMA_AH</td><td>EDh</td><td>DMA current buffer address high byte</td><td>0000 xxxxb</td></tr>
    <tr><td>ADC_DMA_AL</td><td>ECh</td><td>DMA current buffer address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>ADC_DMA</td><td>ECh</td><td>ADC_DMA_AL and ADC_DMA_AH form a 16-bit SFR</td><td>0xxxh</td></tr>
    <tr><td rowspan="29">USB register</td><td>USB_DMA_AH</td><td>E7h</td><td>DMA current buffer address high byte (read only)</td><td>000x xxxxb</td></tr>
    <tr><td>USB_DMA_AL</td><td>E6h</td><td>DMA current buffer address low byte (read only)</td><td>xxxx xxx0b</td></tr>
    <tr><td>USB_DMA</td><td>E6h</td><td>USB_DMA_AL and USB_DMA_AH form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UHUB1_CTRL</td><td>E5h</td><td>USB host HUB1 port control register</td><td>1100 x000b</td></tr>
    <tr><td>UHUB0_CTRL</td><td>E4h</td><td>USB host HUB0 port control register</td><td>0100 x000b</td></tr>
    <tr><td>UDEV_CTRL</td><td>E4h</td><td>USB Device Port Control Register</td><td>0100 x000b</td></tr>
    <tr><td>USB_DEV_AD</td><td>E3h</td><td>USB Device Address Register</td><td>0000 0000b</td></tr>
    <tr><td>USB_CTRL</td><td>E2h</td><td>USB Control Register</td><td>0000 0110b</td></tr>
    <tr><td>USB_INT_EN</td><td>E1h</td><td>USB Interrupt Enable Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP4_T_LEN</td><td>DFh</td><td>Endpoint 4 Transmit Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP4_CTRL</td><td>DEh</td><td>Endpoint 4 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_T_LEN</td><td>DDh</td><td>Endpoint 0 Transmit Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP0_CTRL</td><td>DCh</td><td>Endpoint 0 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>USB_HUB_ST</td><td>DBh</td><td>SB Host HUB Port Status Register (Read Only)</td><td>0000 0000b</td></tr>
    <tr><td>USB_MIS_ST</td><td>DAh</td><td>SB Miscellaneous Status Register (Read Only)</td><td>xx10 1000b</td></tr>
    <tr><td>USB_INT_ST</td><td>D9h</td><td>USB Interrupt Status Register (Read Only)</td><td>00xx xxxxb</td></tr>
    <tr><td>USB_INT_FG</td><td>D8h</td><td>USB Interrupt Flag Register</td><td>0010 0000b</td></tr>
    <tr><td>UEP3_T_LEN</td><td>D7h</td><td>Endpoint 3 Transmit Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UH_TX_LEN</td><td>D7h</td><td>USB host send length register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP3_CTRL</td><td>D6h</td><td>Endpoint 3 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UH_TX_CTRL</td><td>D6h</td><td>USB host transmit endpoint control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_T_LEN</td><td>D5h</td><td>Endpoint 2 Transmit Length Register</td><td>0000 0000b</td></tr>
    <tr><td>UH_EP_PID</td><td>D5h</td><td>USB Host Token Setting Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_CTRL</td><td>D4h</td><td>Endpoint 2 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UH_RX_CTRL</td><td>D4h</td><td>USB host receive endpoint control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP1_T_LEN</td><td>D3h</td><td>Endpoint 1 Transmit Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP1_CTRL</td><td>D2h</td><td>Endpoint 1 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UH_SETUP</td><td>D2h</td><td>USB Host Auxiliary Settings Register</td><td>0000 0000b</td></tr>
    <tr><td>USB_RX_LEN</td><td>D1h</td><td>USB Receive Length Register (Read Only)</td><td>0xxx xxxxb</td></tr>
    <tr><td rowspan="22">USB xSFR register</td><td>UEP4_1_MOD</td><td>2446h</td><td>Endpoint 1, 4 mode control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_3_MOD</td><td>2447h</td><td>Endpoint 2, 3 Mode Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UH_EP_MOD</td><td>2447h</td><td>USB Host Endpoint Mode Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_DMA_H</td><td>2448h</td><td>Endpoints 0 and 4 Buffer Start Address High Bytes</td><td>000x xxxxb</td></tr>
    <tr><td>UEP0_DMA_L</td><td>2449h</td><td>Endpoints 0 and 4 Buffer Start Address Low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP0_DMA</td><td>2448h</td><td>UEP0_DMA_L and UEP0_DMA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UEP1_DMA_H</td><td>244Ah</td><td>Endpoint 1 Buffer Start Address High Byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP1_DMA_L</td><td>244Bh</td><td>Endpoint 1 Buffer Start Address Low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP1_DMA</td><td>244Ah</td><td>UEP1_DMA_L and UEP1_DMA_H form 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UEP2_DMA_H</td><td>244Ch</td><td>Endpoint 2 Buffer start address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP2_DMA_L</td><td>244Dh</td><td>Endpoint 2 Buffer Start Address Low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP2_DMA</td><td>244Ch</td><td>UEP2_DMA_L and UEP2_DMA_H form 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UH_RX_DMA_H</td><td>244Ch</td><td>USB host receive buffer start address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UH_RX_DMA_L</td><td>244Dh</td><td>USB host receive buffer start address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_RX_DMA</td><td>244Ch</td><td>UH_RX_DMA_L and UH_RX_DMA_H form 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UEP3_DMA_H</td><td>244Eh</td><td>Endpoint 3 Buffer start address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP3_DMA_L</td><td>244Fh</td><td>Endpoint 3 Buffer Start Address Low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP3_DMA</td><td>244Eh</td><td>UEP3_DMA_L and UEP3_DMA_H form 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UH_TX_DMA_H</td><td>244Eh</td><td>USB host transmit buffer start address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UH_TX_DMA_L</td><td>244Fh</td><td>USB host send buffer start address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_TX_DMA</td><td>244Eh</td><td>UH_TX_DMA_L and UH_TX_DMA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>pU*</td><td>254*h</td><td>After bXIR_XSFR is set to 1, this name is used to address the above xSFR with pdata type, which is faster than xdata type addressing.</td><td></td></tr>
    <tr><td rowspan="13">LED Control Card xSFR Register</td><td>LED_STAT</td><td>2880h</td><td>LED Status Register</td><td>010x 0000b</td></tr>
    <tr><td>LED_CTRL</td><td>2881h</td><td>LED Control Register</td><td>0000 0010b</td></tr>
    <tr><td>LED_FIFO_CN</td><td>2882h</td><td>FIFO Count Status Register (Read Only)</td><td>0000 0000b</td></tr>
    <tr><td>LED_DATA</td><td>2882h</td><td>LED data register (write only)</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_CK_SE</td><td>2883h</td><td>LED Clock Divider Setting Register</td><td>0001 0000b</td></tr>
    <tr><td>LED_DMA_AH</td><td>2884h</td><td>DMA current buffer address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_AL</td><td>2885h</td><td>DMA Current buffer address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA</td><td>2884h</td><td>LED_DMA_AL and LED_DMA_AH Compose 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>LED_DMA_CN</td><td>2886h</td><td>LED DMA Remaining Count Register</td><td>xxxx xxxxb</td></tr>
    <tr><td>LED_DMA_XH</td><td>2888h</td><td>DMA Current auxiliary buffer address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>LED_DMA_XL</td><td>2889h</td><td>DMA Current auxiliary buffer address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>LED_DMA_X</td><td>2888h</td><td>LED_DMA_XL and LED_DMA_XH form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>pLED_*</td><td>298*h</td><td>After bXIR_XSFR is set to 1, this name is used to address the above xSFR with pdata type, which is faster than xdata type addressing.</td><td></td></tr>
</table>

## 5.3 General purpose 8051 register

<div>
    <p align="center">Table 5.3.1 General 8051 Register List</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>B</td><td>F0h</td><td>B register</td><td>00h</td></tr>
    <tr><td>A, ACC</td><td>E0h</td><td>accumulator</td><td>00h</td></tr>
    <tr><td>PSW</td><td>D0h</td><td>program status register</td><td>00h</td></tr>
    <tr><td rowspan="2">GLOBAL_CFG</td><td rowspan="2">B1h</td><td>Global configuration register (in the bootloader state)</td><td>E0h</td></tr>
    <tr><td>Global configuration register (in application state)</td><td>C0h</td></tr>
    <tr><td>CHIP_ID</td><td>A1h</td><td>Chip ID Identifier (Read Only)</td><td>59h</td></tr>
    <tr><td>SAFE_MOD</td><td>A1h</td><td>Safety Mode Control Register (Write Only)</td><td>00h</td></tr>
    <tr><td>PCON</td><td>87h</td><td>power control register (in power-on reset state)</td><td>10h</td></tr>
    <tr><td>DPH</td><td>83h</td><td>data address pointer high 8 bit</td><td>00h</td></tr>
    <tr><td>DPL</td><td>82h</td><td>data address pointer low 8 bits</td><td>00h</td></tr>
    <tr><td>DPTR</td><td>82h</td><td>DPL and DPH form 16-bit SFR</td><td>0000h</td></tr>
    <tr><td>SP</td><td>81h</td><td>stack pointer</td><td>07h</td></tr>
    
</table>

### B register (B):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>B</td><td>RW</td><td>Arithmetic operation registers, mainly used for multiplication and division, bit-addressable</td><td>00h</td></tr>
</table>

### A accumulator (A, ACC):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>A/ACC</td><td>RW</td><td>Arithmetic accumulator, bit addressable</td><td>00h</td></tr>
</table>

### Program Status Register (PSW):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>CY</td><td>RW</td><td><p>Carry flag: Used to record the carry or borrow of the most significant bit when performing arithmetic and logic operations.</p><p>When the 8-bit addition is performed, the most significant bit is set, otherwise it is cleared.</p><p>when 8-bit subtraction is performed If the borrow is borrowed, the bit is set, otherwise it is cleared.</p><p>The logic instruction can make the bit bit or clear.</p></td><td>0</td></tr>
    <tr><td>6</td><td>AC</td><td>RW</td><td>Auxiliary carry flag: When recording and subtracting, the lower 4 bits have a carry or borrow from the upper 4 bits, AC is set, otherwise cleared.</td><td>0</td></tr>
    <tr><td>5</td><td>F0</td><td>RW</td><td>Universal flag bit addressable by bit 0: User can define it himself, can be cleared or set by software</td><td>0</td></tr>
    <tr><td>4</td><td>RS1</td><td>RW</td><td>Register bank select bit high</td><td>0</td></tr>
    <tr><td>3</td><td>RS0</td><td>RW</td><td>Register bank select bit low</td><td>0</td></tr>
    <tr><td>2</td><td>OV</td><td>RW</td><td>Overflow flag: When adding or subtracting, the operation result exceeds 8 binary digits, then OV is set to 1, the flag overflows, otherwise cleared 0</td><td>0</td></tr>
    <tr><td>1</td><td>F1</td><td>RW</td><td>Universal flag bit addressable by bit 1: User can define it, can be cleared or set by software</td><td>0</td></tr>
    <tr><td>0</td><td>P</td><td>R0</td><td>Parity flag: Record the parity of 1 in accumulator A after the execution of the instruction. P1 for odd number 1 and P for even number 1</td><td>0</td></tr>
</table>

The state of the processor is stored in the status register PSW and the PSW supports bitwise addressing. The status word includes the carry flag, the auxiliary carry flag for BCD code processing, the parity flag, the overflow flag, and RS0 and RS1 for the working register bank selection. The area in which the working register set is located can be accessed either directly or indirectly.

<div>
    <p align="center">Table 5.3.2 RS1 and RS0 Working Register Group Selection Table</p>
</div>

<table>
    <tr>
        <th>RS1</th><th>RS0</th><th>Working register set</th>
    </tr>
    <tr><td>0</td><td>0</td><td>Group 0 (00h-07h)</td></tr>
    <tr><td>0</td><td>1</td><td>Group 1 (08h-0Fh)</td></tr>
    <tr><td>1</td><td>0</td><td>Group 2 (10h-17h)</td></tr>
    <tr><td>1</td><td>1</td><td>Group 3 (18h-1Fh)</td></tr>
</table>

<div>
    <p align="center">Table 5.3.3 Operations that affect the flag bit (X indicates that the flag bit is related to the operation result)</p>
</div>

<table>
    <tr>
        <th>Operation</th><th>CY</th><th>OV</th><th>AC</th>
    </tr>
    <tr><td>ADD</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>ADDC</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>SUBB</td><td>X</td><td>X</td><td>X</td></tr>
    <tr><td>MUL</td><td>0</td><td>X</td><td></td></tr>
    <tr><td>DIV</td><td>0</td><td>X</td><td></td></tr>
    <tr><td>DA A</td><td>X</td><td></td><td></td></tr>
    <tr><td>RRC A</td><td>X</td><td></td><td></td></tr>
    <tr><td>RLC A</td><td>X</td><td></td><td></td></tr>
    <tr><td>CJNE</td><td>X</td><td></td><td></td></tr>
    <tr><td>SETB C</td><td>1</td><td></td><td></td></tr>
    <tr><td>CLR C</td><td>0</td><td></td><td></td></tr>
    <tr><td>CPL C</td><td>X</td><td></td><td></td></tr>
    <tr><td>MOV C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ANL C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ANL C,/bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ORL C, bit</td><td>X</td><td></td><td></td></tr>
    <tr><td>ORL C,/bit</td><td>X</td><td></td><td></td></tr>
</table>

### Data Address Pointer (DPTR):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>DPL</td><td>RW</td><td>Data pointer low byte</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>DPH</td><td>RW</td><td>Data pointer high byte</td><td>00h</td></tr>
</table>

DPL and DPH form a 16-bit data pointer DPTR for accessing xSFR, xBUS, xRAM data memory or program memory. The actual DPTR corresponds to the physical 16-bit data pointers of DPTR0 and DPTR1, which are dynamically selected by DPS in XBUS_AUX.

### Stack pointer (SP):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SP</td><td>RW</td><td>Stack pointer, mainly used for program calls and interrupt calls, and data in and out of the stack</td><td>07h</td></tr>
</table>

Stack specific functions: protect endpoints and protect the site, and manage them on a first-come, first-out basis. When the stack is pushed, the SP pointer is automatically incremented by 1, and the data or breakpoint information is saved. When the stack is taken, the SP pointer points to the data unit, and the SP pointer is automatically decremented by 1. The initial value of the SP after reset is 07h, and the corresponding default stack storage starts at 08h.

## 5.4 Special registers

Global configuration register (GLOBAL_CFG), writable only in safe mode:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:6]</td><td>Reserved</td><td>RO</td><td>Fixed value 11</td><td>11b</td></tr>
    <tr><td>5</td><td>bBOOT_LOAD</td><td>RO</td><td><p>The Boot loader status bit is used to distinguish between the ISP bootloader status or the application state: set when the power is turned on, and cleared to 0 when the software is reset.
</p><p>For chips with an ISP bootloader, this bit is 1 indicates that the software has never been reset, usually the ISP bootloader state that was run after power-up. this bit is 0 indicating that the software has been reset, usually the application state</p></td><td>1</td></tr>
    <tr><td>4</td><td>bSW_RESET</td><td>RW</td><td>Software reset control bit: Set to 1 to cause software reset, hardware auto-zero</td><td>0</td></tr>
    <tr><td>3</td><td>bCODE_WE</td><td>RW</td><td>Flash-ROM write enable bit: This bit is 0 for write protection; 1 for Flash-ROM writable erasable</td><td>0</td></tr>
    <tr><td>2</td><td>bDATA_WE</td><td>RW</td><td>DataFlash area write enable bit of Flash-ROM: This bit is 0 for write protection; 1 is for DataFlash area to be erasable and erasable</td><td>0</td></tr>
    <tr><td>1</td><td>bXIR_XSFR</td><td>RW</td><td><p>MOVX_@R0/R1 instruction access range control bits:</p><p>This bit is 0 to allow access to all xdata regions xRAM/xBUS/xSFR.</p><p>This bit is 1 for access to xSFR and cannot access xRAM/xBUS</p></td><td>0</td></tr>
    <tr><td>0</td><td>bWDOG_EN</td><td>RW</td><td>Watchdog reset enable bit: This bit is 0. The watchdog is only used as a timer. this bit is 1 to allow a watchdog reset when the timer overflows.</td><td>0</td></tr>
</table>

### Chip ID (CHIP_ID):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>CHIP_ID</td><td>RO</td><td>Fixed value 59h for identification chip</td><td>59h</td></tr>
</table>

### Safe Mode Control Register (SAFE_MOD):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SAFE_MOD</td><td>WO</td><td>Used to enter or terminate safe mode</td><td>00h</td></tr>
</table>

**Some SFRs can only write data in safe mode, and are always read-only in non-secure mode. Steps to enter safe mode:**

1. write 55h to the register.
2. then write AAh to the register.
3. After that, about 13 to 23 system main frequency cycles are in safe mode, and one or more security class SFRs or ordinary SFRs can be rewritten during the validity period.
4. automatically terminate the safe mode after the above validity period.
5. Or write any value to this register to terminate the safe mode early.