---
title: 9. Interrupt
type: docs
weight: 9
BookToC: false
---

# 9. Interrupt

The CH559 chip supports 14 sets of interrupt signal sources, including 6 sets of interrupts compatible with the standard MCS51: INT0, T0, INT1, T1, UART0, T2, and extended 8 sets of interrupts: SPI0, TMR3, USB, ADC, UART1, PWM1 GPIO, WDOG, among which GPIO interrupt can be selected from 7 I/O pins.


## 9.1 Register description

<div>
    <p align="center">Table 9.1.1 Interrupt vector table</p>
</div>

<table>
    <tr>
        <th>Interrupt Source</th><th>Entry Address</th><th>Interrupt number</th><th>Description</th><th>Default priority</th>
    </tr>
    <tr><td>INT_NO_INT0</td><td>0x0003</td><td>0</td><td><p>External interrupt 0 or LED control card interrupt:</p><p>When bLED_OUT_EN = 0 is external interrupt 0;</p><p>When bLED_OUT_EN = 1 is LED control card interrupt</p></td><td rowspan="14">High priority<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>&darr;<br>Low priority</td></tr>
    <tr><td>INT_NO_TMR0</td><td>0x000B</td><td>1</td><td>Timer 0 interrupt</td></tr>
    <tr><td>INT_NO_INT1</td><td>0x0013</td><td>2</td><td>External interrupt 1</td></tr>
    <tr><td>INT_NO_TMR1</td><td>0x001B</td><td>3</td><td>Timer 1 interrupt</td></tr>
    <tr><td>INT_NO_UART0</td><td>0x0023</td><td>4</td><td>UART0 interrupt</td></tr>
    <tr><td>INT_NO_TMR2</td><td>0x002B</td><td>5</td><td>Timer 2 interrupt</td></tr>
    <tr><td>INT_NO_SPI0</td><td>0x0033</td><td>6</td><td>SPI0 interrupt</td></tr>
    <tr><td>INT_NO_TMR3</td><td>0x003B</td><td>7</td><td>Timer 3 interrupt</td></tr>
    <tr><td>INT_NO_USB</td><td>0x0043</td><td>8</td><td>USB interrupt</td></tr>
    <tr><td>INT_NO_ADC</td><td>0x004B</td><td>9</td><td>ADC interrupt</td></tr>
    <tr><td>INT_NO_UART1</td><td>0x0053</td><td>10</td><td>UART1 interrupt</td></tr>
    <tr><td>INT_NO_PWM1</td><td>0x005B</td><td>11</td><td>PWM1 interrupt</td></tr>
    <tr><td>INT_NO_GPIO</td><td>0x0063</td><td>12</td><td>GPIO interrupt</td></tr>
    <tr><td>INT_NO_WDOG</td><td>0x006B</td><td>13</td><td>Watchdog timer interrupt</td></tr>
</table>

<div>
    <p align="center">Table 9.1.2 Interrupt related register list</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>IP_EX</td><td>E9h</td><td>Extended interrupt priority control register</td><td>00h</td></tr>
    <tr><td>IE_EX</td><td>E8h</td><td>Extended interrupt enable register</td><td>00h</td></tr>
    <tr><td>GPIO_IE</td><td>CFh</td><td>GPIO interrupt enable register</td><td>00h</td></tr>
    <tr><td>IP</td><td>B8h</td><td>Interrupt Priority Control Register</td><td>00h</td></tr>
    <tr><td>IE</td><td>A8h</td><td>interrupt enable register</td><td>00h</td></tr>
</table>

### Interrupt Enable Register (IE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>EA</td><td>RW</td><td>Global interrupt enable control bit, this bit is 1 and E_DIS is 0 to enable interrupts. this bit is 0 to mask all interrupt requests</td><td>0</td></tr>
    <tr><td>6</td><td>E_DIS</td><td>RW</td><td>Global interrupt disable control bit, this bit is 1 to mask all interrupt requests. this bit is 0 and EA is 1 to enable interrupts. This bit is typically used to temporarily disable interrupts during flash-ROM operations</td><td>0</td></tr>
    <tr><td>5</td><td>ET2</td><td>RW</td><td>Timer 2 interrupt enable bit, this bit is 1 to enable T2 interrupt. it is 0 to mask</td><td>0</td></tr>
    <tr><td>4</td><td>ES</td><td>RW</td><td>Asynchronous serial port 0 interrupt enable bit, this bit is 1 to enable UART0 interrupt. masked to 0</td><td>0</td></tr>
    <tr><td>3</td><td>ET1</td><td>RW</td><td>Timer 1 interrupt enable bit. This bit is 1 to enable the T1 interrupt. it is masked to 0.</td><td>0</td></tr>
    <tr><td>2</td><td>EX1</td><td>RW</td><td>External interrupt 1 enable bit, this bit is 1 to enable the INT1 interrupt. masked to 0</td><td>0</td></tr>
    <tr><td>1</td><td>ET0</td><td>RW</td><td>Timer 0 interrupt enable bit, this bit is 1 to enable the T0 interrupt. masked to 0</td><td>0</td></tr>
    <tr><td>0</td><td>EX0</td><td>RW</td><td>External interrupt 0 and LED control card interrupt enable bit, this bit is 1 to enable INT0 / LED interrupt, selected by bLED_OUT_EN. masked to 0</td><td>0</td></tr>
    
</table>

### Extended interrupt enable register (IE_EX):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>IE_WDOG</td><td>RW</td><td>Watchdog timer interrupt enable bit, this bit is 1 to enable WDOG interrupt; 0 to mask</td><td>0</td></tr>
    <tr><td>6</td><td>IE_GPIO</td><td>RW</td><td>GPIO interrupt enable bit, this bit is 1 to enable interrupts enabled in GPIO_IE; 0 to mask all interrupts in GPIO_IE</td><td>0</td></tr>
    <tr><td>5</td><td>IE_PWM1</td><td>RW</td><td>PWM1 interrupt enable bit. This bit is 1 to enable PWM1 interrupt; 0 to mask 0</td><td>0</td></tr>
    <tr><td>4</td><td>IE_UART1</td><td>RW</td><td>asynchronous serial port 1 interrupt enable bit, this bit is 1 to enable UART1 interrupt; 0 to mask</td><td>0</td></tr>
    <tr><td>3</td><td>IE_ADC</td><td>RW</td><td>ADC analog-to-digital conversion interrupt enable bit, this bit is 1 to enable ADC interrupt; 0 to mask</td><td>0</td></tr>
    <tr><td>2</td><td>IE_USB</td><td>RW</td><td>USB interrupt enable bit, this bit is 1 to enable USB interrupt; 0 to mask</td><td>0</td></tr>
    <tr><td>1</td><td>IE_TMR3</td><td>RW</td><td>Timer 3 interrupt enable bit, this bit is 1 to enable Timer3 interrupt; 0 to mask</td><td>0</td></tr>
    <tr><td>0</td><td>IE_SPI0</td><td>RW</td><td>SPI0 interrupt enable bit, this bit is 1 to enable SPI0 interrupt; 0 to mask</td><td>0</td></tr>
    
</table>

### GPIO interrupt enable register (GPIO_IE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bIE_IO_EDGE</td><td>RW</td><td><p>GPIO edge interrupt mode enable:</p><p>
This bit is 0 to select the level interrupt mode.If the GPIO pin input valid level, bIO_INT_ACT is 1 and the interrupt is always requested.When the GPIO input invalid level, bIO_INT_ACT is 0 and the interrupt request is canceled.</p><p>
This bit is 1 to select the edge interrupt mode. When a valid edge is input to the GPIO pin, the interrupt flag bIO_INT_ACT is generated and an interrupt is requested. This interrupt flag cannot be cleared by software. It can only be reset or in level interrupt mode or enter the corresponding interrupt service routine Is automatically cleared</p></td><td>0</td></tr>
    <tr><td>6</td><td>bIE_RXD1_LO</td><td>RW</td><td>This bit is 1 to enable UART1 receive pin interrupt (level mode is active low, edge mode falling edge is active). this bit is 0 to disable. Select XA/XB differential input in iRS485 mode, select RXD1 or RXD1_ pin according to bIER_PIN_MOD1 = 1/0 in non-iRS485 mode</td><td>0</td></tr>
    <tr><td>5</td><td>bIE_P5_5_HI</td><td>RW</td><td>This bit is 1 to enable the P5.5 interrupt (level mode is active high and edge mode is active on rising edge); this bit is 0 to disable</td><td>0</td></tr>
    <tr><td>4</td><td>bIE_P1_4_LO</td><td>RW</td><td>This bit is 1 to enable the P1.4 interrupt (level mode is active low, edge mode is active on falling edge); this bit is 0 to disable</td><td>0</td></tr>
    <tr><td>3</td><td>bIE_P0_3_LO</td><td>RW</td><td>This bit is 1 to enable the P0.3 interrupt (active in low level in level mode and valid in falling edge in edge mode); this bit is 0 to disable</td><td>0</td></tr>
    <tr><td>2</td><td>bIE_P5_7_HI</td><td>RW</td><td>This bit is 1 to enable the P5.7 interrupt (level mode is active high and edge mode is active on rising edge); this bit is 0 to disable</td><td>0</td></tr>
    <tr><td>1</td><td>bIE_P4_1_LO</td><td>RW</td><td>This bit is 1 to enable the P4.1 interrupt (active in low level in level mode and valid in falling edge in edge mode); this bit is 0 to disable</td><td>0</td></tr>
    <tr><td>0</td><td>bIE_RXD0_LO</td><td>RW</td><td>This bit is 1 to enable the UART0 receive pin interrupt (level mode is active low, edge mode is active falling edge); this bit is 0 to disable. Select RXD0 or RXD0_ pin according to bUART0_PIN_X = 0/1</td><td>0</td></tr>
    
</table>

### Interrupt Priority Control Register (IP):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>PH_FLAG</td><td>R0</td><td>High priority interrupt executing flag</td><td>0</td></tr>
    <tr><td>6</td><td>PL_FLAG</td><td>R0</td><td>Low Priority Interrupt Execution Flag</td><td>0</td></tr>
    <tr><td>5</td><td>PT2</td><td>RW</td><td>Timer 2 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>4</td><td>PS</td><td>RW</td><td>UART0 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>3</td><td>PT1</td><td>RW</td><td>Timer 1 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>2</td><td>PX1</td><td>RW</td><td>Interrupt priority control bit for external interrupt 1</td><td>0</td></tr>
    <tr><td>1</td><td>PT0</td><td>RW</td><td>timer 0 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>0</td><td>PX0</td><td>RW</td><td>External Interrupt 0 and Interrupt Priority Control Bit for LED Control Card Interrupt</td><td>0</td></tr>
    
</table>

### Extended interrupt priority control register (IP_EX):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bIP_LEVEL</td><td>R0</td><td>Current interrupt nesting level flag bit. If this bit is 0, it means no interrupt or nested level 2 interrupt.If this bit is 1, it means current nested level 1 interrupt.</td><td>0</td></tr>
    <tr><td>6</td><td>bIP_GPIO</td><td>RW</td><td>GPIO interrupt priority control bit</td><td>0</td></tr>
    <tr><td>5</td><td>bIP_PWM1</td><td>RW</td><td>PWM1 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>4</td><td>bIP_UART1</td><td>RW</td><td>UART1 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>3</td><td>bIP_ADC</td><td>RW</td><td>ADC interrupt priority control bit</td><td>0</td></tr>
    <tr><td>2</td><td>bIP_USB</td><td>RW</td><td>USB interrupt priority control bit</td><td>0</td></tr>
    <tr><td>1</td><td>bIP_TMR3</td><td>RW</td><td>Timer3 interrupt priority control bit</td><td>0</td></tr>
    <tr><td>0</td><td>bIP_SPI0</td><td>RW</td><td>SPI0 interrupt priority control bit</td><td>0</td></tr>
</table>


The IP and IP_EX registers are used to set the interrupt priority. If a bit is set to 1, the corresponding interrupt source is set to a high priority. If a bit is cleared to 0, the corresponding interrupt source is set to a low priority . For the same level interrupt source, the system has a default priority order. The default priority order is shown in Table 9.1.1. Its PH_FLAG and PL_FLAG combination indicates the priority of the current interrupt.


<div>
    <p align="center">Table 9.1.3 Current interrupt priority status indication</p>
</div>

<table>
    <tr>
        <th>PH_FLAG</th><th>PL_FLAG</th><th>Current interrupt priority status</th>
    </tr>
    <tr><td>0</td><td>0</td><td>No interruption currently</td></tr>
    <tr><td>0</td><td>1</td><td>Low priority interrupt is currently executing</td></tr>
    <tr><td>1</td><td>0</td><td>High priority interrupt is currently executing</td></tr>
    <tr><td>1</td><td>1</td><td>Unexpected state, unknown error</td></tr>
    
</table>