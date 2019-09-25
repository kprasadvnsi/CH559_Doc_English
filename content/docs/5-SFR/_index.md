---
title: 5. Special function register SFR
type: docs
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

<p align="center">Table 5.1 Special Function Register Table</p>
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

    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
</table>