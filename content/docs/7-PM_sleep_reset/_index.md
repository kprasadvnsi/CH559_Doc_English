---
title: 7. Power management
type: docs
weight: 7
BookToC: false
---

# 7. Power management, sleep and reset

## 7.1 External power input

The CH559 chip has an internal working voltage of 3.3V, and the input/output voltage of the I/O pin is 3.3V. The I/O pins except P1.0~P1.7, XI, XO, and RST can withstand 5V voltage input. The CH559 chip has a 5V to 3.3V low dropout voltage regulator that supports an external 3.3V or 5V supply voltage input. The two supply voltage input modes refer to the table below.

<table>
    <tr>
        <th>External supply voltage</th><th>VIN5 pin voltage: external voltage 3.3V~5V</th><th>VDD33 pin voltage: internal voltage 3.3V</th>
    </tr>
    <tr><td>3.3V includes less than 3.6V</td><td>Input external 3.3V voltage to voltage regulator, must be grounded to not less than 0.1uF decoupling capacitor</td><td>Input external 3.3V as internal working power supply, must be grounded to ground not less than 0.1uF decoupling capacitor</td></tr>
    <tr><td>5V includes greater than 3.6V</td><td>Input external 5V voltage to voltage regulator, must be grounded to not less than 0.1uF decoupling capacitor</td><td>Internal voltage regulator 3.3V output and 3.3V internal working power input, must be grounded to not less than 3.3uF decoupling capacitor</td></tr>
</table>

After the power is turned on or the system is reset, the CH559 is running by default. When some function modules are not needed, the clocks of these modules can be turned off to reduce power consumption. When the CH559 does not need to run at all, the PD in the PCON can be set to sleep. In the sleep state, external wake-up can be selected via USB, UART0, UART1, SPI0 and some GPIOs.

## 7.2 Power and Sleep Control Registers

<div>
    <p align="center">Table 7.2.1 List of Power and Sleep Control Registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>WDOG_COUNT</td><td>FFh</td><td>Watchdog Count Register</td><td>00h</td></tr>
    <tr><td>RESET_KEEP</td><td>FEh</td><td>reset holding register</td><td>00h</td></tr>
    <tr><td>WAKE_CTRL</td><td>EBh</td><td>Sleep Wake Control Register</td><td>00h</td></tr>
    <tr><td>SLEEP_CTRL</td><td>EAh</td><td>Sleep control register</td><td>00h</td></tr>
    <tr><td>PCON</td><td>87h</td><td>Power Control Register</td><td>10h</td></tr>   
</table>

### Watchdog Count Register (WDOG_COUNT):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>WDOG_COUNT</td><td>RW</td><td>The current count of the watchdog is 0FFh. It overflows when it is 00h. When it overflows, it automatically sets the interrupt flag bWDOG_IF_TO to 1</td><td>00h</td></tr>
</table>

### Reset holding register (RESET_KEEP):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>RESET_KEEP</td><td>RW</td><td>Reset the holding register, the value can be modified manually, except for the power-on reset to clear it, any other reset does not affect the value</td><td>00h</td></tr>
</table>

### Sleep wake control register (WAKE_CTRL), which can be written only in safe mode:
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bWAK_BY_USB</td><td>RW</td><td>USB event wake-up enable, this bit is 0, no wake-up is allowed</td><td>0</td></tr>
    <tr><td>6</td><td>bWAK_RXD1_LO</td><td>RW</td><td>UART1 receives an input low wake-up enable. This bit is 0 to disable wake-up.<br>Select XA/XB differential input in iRS485 mode, select RXD1 or RXD1_ pin according to bIER_PIN_MOD1=1/0 in non-iRS485 mode</td><td>0</td></tr>
    <tr><td>5</td><td>bWAK_P1_5_LO</td><td>RW</td><td>P1.5 low wake enable, 0 disable wakeup</td><td>0</td></tr>
    <tr><td>4</td><td>bWAK_P1_4_LO</td><td>RW</td><td>P1.4 Low wake enable, 0 disable wakeup</td><td>0</td></tr>
    <tr><td>3</td><td>bWAK_P0_3_LO</td><td>RW</td><td>P0.3 Low wake enable, 0 disable wakeup</td><td>0</td></tr>
    <tr><td>2</td><td>bWAK_CAP3_LO</td><td>RW</td><td>Timer3 captures the input low wake enable, and 0 disables wakeup.<br>Select CAP3 or CAP3_ pin according to bTMR3_PIN_X=0/1</td><td>0</td></tr>
    <tr><td>1</td><td>bWAK_P3_2E_3L</td><td>RW</td><td>P3.2 Edge change and P3.3 low wake enable, 0 disable wakeup</td><td>0</td></tr>
    <tr><td>0</td><td>bWAK_RXD0_LO</td><td>RW</td><td>UART0 receives input low wake enable, 0 disables wakeup.<br>Select RXD0 or RXD0_ pin according to bUART0_PIN_X=0/1</td><td>0</td></tr>
</table>

### Sleep Control Register (SLEEP_CTRL), which can be written only in Safe Mode:
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bSLP_OFF_USB</td><td>RW</td><td>USB clock off control, this bit is 1 Off clock</td><td>0</td></tr>
    <tr><td>6</td><td>bSLP_OFF_ADC</td><td>RW</td><td>ADC clock off control, this bit is 1 Off clock</td><td>0</td></tr>
    <tr><td>5</td><td>bSLP_OFF_UART1</td><td>RW</td><td>UAR1 clock off control, this bit is 1 off clock</td><td>0</td></tr>
    <tr><td>4</td><td>bSLP_OFF_P1S1</td><td>RW</td><td>PWM1 and SPI1 clock off control, this bit is 1 OFF clock</td><td>0</td></tr>
    <tr><td>3</td><td>bSLP_OFF_SPI0</td><td>RW</td><td>SPI0 clock off control, this bit is 1 off clock</td><td>0</td></tr>
    <tr><td>2</td><td>bSLP_OFF_TMR3</td><td>RW</td><td>Timer3 clock off control, this bit is 1 off clock</td><td>0</td></tr>
    <tr><td>1</td><td>bSLP_OFF_LED</td><td>RW</td><td>LED-CTRL Clock off control, this bit is 1 Off clock</td><td>0</td></tr>
    <tr><td>0</td><td>bSLP_OFF_XRAM</td><td>RW</td><td>xRAM clock off control, this bit is 1 off clock</td><td>0</td></tr>
</table>

### Power Control Register (PCON):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>SMOD</td><td>RW</td><td>When using Timer 1 to generate UART0 baud rate, select the communication baud rate of UART0 mode 1, 2, 3: 0-slow mode; 1-fast mode</td><td>0</td></tr>
    <tr><td>6</td><td>reserved</td><td>RO</td><td>reserved</td><td>0</td></tr>
    <tr><td>5</td><td>bRST_FLAG1</td><td>R0</td><td>chip last reset flag high</td><td>0</td></tr>
    <tr><td>4</td><td>bRST_FLAG0</td><td>R0</td><td>chip last reset flag low</td><td>1</td></tr>
    <tr><td>3</td><td>GF1</td><td>RW</td><td>universal flag 1: User can define it by itself, can be cleared or set by software.</td><td>0</td></tr>
    <tr><td>2</td><td>GF0</td><td>RW</td><td>general flag 0: user can define it himself, can be cleared or set by software</td><td>0</td></tr>
    <tr><td>1</td><td>PD</td><td>RW</td><td>sleep mode enable, set to sleep after 1 wake-up hardware automatically cleared</td><td>0</td></tr>
    <tr><td>0</td><td>reserved</td><td>R0</td><td>reserved</td><td>0</td></tr>

</table>

<div>
    <p align="center">Table 7.2.2 Description of the chip's last reset flag</p>
</div>

<table>
    <tr>
        <th>bRST_FLAG1</th><th>bRST_FLAG0</th><th>Reset flag description</th>
    </tr>
    <tr><td>0</td><td>0</td><td>Software reset, source: bSW_RESET=1 and (bBOOT_LOAD=0 or bWDOG_EN=1)</td></tr>
    <tr><td>0</td><td>1</td><td>Power-on reset, source: VDD33 pin voltage is lower than detection level</td></tr>
    <tr><td>1</td><td>0</td><td>Watchdog reset, source: bWDOG_EN=1 and watchdog timeout</td></tr>
    <tr><td>1</td><td>1</td><td>External pin is manually reset, source: En_P5.7_RESET=1 and P5.7 input high</td></tr>
</table>

## 7.3 Reset Control

The CH559 has four reset sources: power-on reset, external reset, software reset, watchdog reset, and the latter three are hot resets.

### 7.3.1 Power-on reset

Power-on reset POR is generated by the on-chip voltage detection circuit. The POR circuit continuously monitors the supply voltage of the VDD33 pin. Below the detection level, Vpot generates a power-on reset, and the hardware automatically delays Tpor to maintain the reset state. After the delay expires, CH559 runs. Only the power-on reset causes the CH559 to reload the configuration information and clear RESET_KEEP. Other thermal resets do not affect.

### 7.3.2 External reset

An external reset is generated by a high level applied to the RST pin. The reset process is triggered when the configuration information En_P5.7_RESET is 1 and the high level on the RST pin lasts longer than Trst. When the external high level signal is cancelled, the hardware automatically delays Trdl to maintain the reset state. After the delay expires, CH559 starts from the 0 address.

### 7.3.3 Software Reset

The CH559 supports an internal software reset to actively reset the CPU state and re-run without external intervention. Set bSW_RESET in the global configuration register GLOBAL_CFG to 1, software reset, and automatically delay Trdl to maintain the reset state. After the delay expires, CH559 starts from 0 address, and bSW_RESET bit is automatically cleared by hardware.

When bSW_RESET is set, if bBOOT_LOAD=0 or bWDOG_EN=1, bRST_FLAG1/0 will be indicated as a software reset after reset; when bSW_RESET is set to 1, if bBOOT_LOAD=1 and bWDOG_EN=0, then bRST_FLAG1/0 will not generate new The reset flag, but keeps the previous reset flag unchanged.

For a chip with an ISP boot program, after the power-on reset, run the boot program, which resets the chip to the application state according to the software reset. This software reset only causes bBOOT_LOAD to be cleared, and does not affect the state of bRST_FLAG1/0. (Because bBOOT_LOAD=1 before reset), when switching to the application state, bRST_FLAG1/0 still indicates the power-on reset state.

### 7.3.4 Watchdog Reset

The watchdog reset is generated when the watchdog timer times out. The watchdog timer is an 8-bit counter that counts the clock frequency of the system's main frequency, Fsys/262144. When the 0FFh is turned to 00h, an overflow signal is generated.

The watchdog timer overflow signal will trigger the interrupt flag bWDOG_IF_TO to be 1, which is automatically cleared when the WDOG_COUNT is reloaded or when the corresponding interrupt service routine is entered.

Different timing periods Twdc are achieved by writing different count initial values to WDOG_COUNT. At 12MHz, the watchdog timing period Twdc at 00h is about 5.9 seconds and about 2.8 seconds at 80h.

If bWDOG_EN = 1 when the watchdog timer overflows, a watchdog reset is generated, and Trdl is automatically delayed to maintain the reset state. After the delay is over, CH559 starts from the 0 address.

To avoid being reset by the watchdog when bWDOG_EN=1, WDOG_COUNT must be reset in time to avoid overflow.