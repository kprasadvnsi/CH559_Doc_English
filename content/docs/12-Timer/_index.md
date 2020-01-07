---
title: 12. Timer
type: docs
weight: 12
BookToC: false
---

# 12. Timer

## 12.1 Timer 0/1

Timer0 / 1 are two 16-bit timer / counters. TCON and TMOD are used to configure Timer0 and Timer1. TCON is used for timer / counter T0 and T1 start control and overflow interrupt and external interrupt control. Each timer is a 16-bit timing unit consisting of two 8-bit registers. The high byte counter of timer 0 is TH0, the low byte is TL0; the high byte counter of timer 1 is TH1, and the low byte is TL1. Timer 1 can also be used as a baud rate generator for UART0.


<div>
    <p align="center">Table 12.1.1 List of Timer 0/1 related registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>TH1 </td><td>8Dh </td><td>Timer1 count high byte</td><td> xxh</td></tr>
    <tr><td>TH0 </td><td>8Ch </td><td>Timer0 count high byte</td><td> xxh</td></tr>
    <tr><td>TL1 </td><td>8Bh </td><td>Timer1 count low byte </td><td>xxh</td></tr>
    <tr><td>TL0 </td><td>8Ah </td><td>Timer0 count low byte </td><td>xxh</td></tr>
    <tr><td>TMOD</td><td> 89h</td><td> Timer0 / 1 mode register </td><td>00h</td></tr>
    <tr><td>TCON</td><td> 88h</td><td> Timer0 / 1 control register </td><td>00h</td></tr>
    
</table>

### Timer/Counter 0/1 Control Register (TCON):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>TF1 </td><td>RW </td><td>Timer1 overflow interrupt flag bit, it is automatically cleared after entering Timer 1 interrupt</td><td> 0</td></tr>
    <tr><td>6 </td><td>TR1 </td><td>RW </td><td>Timer1 start / stop bit, set to 1 to start, set or cleared by software</td><td> 0</td></tr>
    <tr><td>5 </td><td>TF0 </td><td>RW </td><td>Timer0 overflow interrupt flag bit, enters timer 0 and is automatically cleared after interrupt</td><td> 0</td></tr>
    <tr><td>4 </td><td>TR0 </td><td>RW </td><td>Timer0 start / stop bit, set to 1 to start, set or cleared by software</td><td> 0</td></tr>
    <tr><td>3 </td><td>IE1 </td><td>RW </td><td>INT1 Interrupt request flag bit for external interrupt 1; it is automatically cleared after entering interrupt</td><td> 0</td></tr>
    <tr><td>2 </td><td>IT1 </td><td>RW </td><td>INT1 External interrupt 1 trigger mode control bit. This bit is 0 to select the external interrupt to trigger at low level. This bit is 1 to select the external interrupt to trigger at falling edge.</td><td> 0</td></tr>
    <tr><td>1 </td><td>IE0 </td><td>RW </td><td>INT0 Interrupt request flag for external interrupt 0. It is automatically cleared after entering interrupt</td><td> 0</td></tr>
    <tr><td>0 </td><td>IT0 </td><td>RW </td><td>INT0 External interrupt 0 trigger mode control bit. This bit is 0 to select the external interrupt to trigger at low level. This bit is 1 to select the external interrupt to trigger at falling edge.</td><td> 0</td></tr>
    
</table>

### Timer/Counter 0/1 Mode Register (TMOD):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>bT1_GATE</td><td> RW</td><td> The gate enable bit controls whether Timer1 start is affected by external interrupt signal INT1. If this bit is 0, whether Timer/Counter 1 is started has nothing to do with INT1. If this bit is 1, it can be started only when INT1 pin is high and TR1 is 1</td><td> 0</td></tr>
    <tr><td>6 </td><td>bT1_CT</td><td> RW </td><td>Timing or counting mode selection bit, this bit is 0 to work in timing mode. This bit is 1 to work in counting mode, using the falling edge of T1 pin as clock </td><td>0</td></tr>
    <tr><td>5 </td><td>bT1_M1</td><td> RW </td><td>Timer/Counter 1 mode selection high</td><td> 0</td></tr>
    <tr><td>4 </td><td>bT1_M0</td><td> RW </td><td>Timer/Counter 1 mode selection low</td><td> 0</td></tr>
    <tr><td>3 </td><td>bT0_GATE</td><td>RW </td><td> Gating enable bit, controls whether Timer0 start is affected by external interrupt signal INT0. If this bit is 0, the timer/counter 0 is started regardless of INT0. If this bit is 1, only the INT0 pin is high and TR0 can be started </td><td>0</td></tr>
    <tr><td>2 </td><td>bT0_CT </td><td>RW </td><td>Timing or counting mode selection bit, this bit is 0 to work in timing mode; this bit is 1 to work in counting mode, using the falling edge of T0 pin as clock </td><td>0</td></tr>
    <tr><td>1 </td><td>bT0_M1 </td><td>RW </td><td>Timer/Counter 0 Mode selection high</td><td> 0</td></tr>
    <tr><td>0 </td><td>bT0_M0 </td><td>RW </td><td>Timer/Counter 0 Mode selection low</td><td> 0</td></tr>
    
</table>

<div>
    <p align="center">Table 12.1.2 bTn_M1 and bTn_M0 select Timern operating mode (n = 0, 1)</p>
</div>

<table>
    <tr>
        <th>bTn_M1</th><th>bTn_M0</th><th>Timern working mode (n = 0, 1)</th>
    </tr>
    <tr><td>0</td><td> 0</td><td> Mode 0: 13-bit timer / counter n. The counting unit consists of the lower 5 bits of TLn and THn. The upper 3 bits of TLn are invalid. When the count changes from all 13 bits to all 0s, the overflow flag TFn is set and the initial value needs to be reset</td></tr>
    <tr><td>0</td><td> 1</td><td> Mode 1: 16-bit timer / counter n. The counting unit consists of TLn and THn. When the count changes from all 16 bits to all 0s, the overflow flag TFn is set and the initial value needs to be reset</td></tr>
    <tr><td>1</td><td> 0</td><td> mode 2: 8-bit reload timer / counter n, the counting unit uses TLn, THn as the reload counting unit. When the count changes from all 8 bits to all 0s, the overflow flag TFn is set and the initial value is automatically loaded from THn</td></tr>
    <tr><td>1</td><td> 1</td><td> Mode 3: If it is timer / counter 0, then timer / counter 0 is divided into two parts TL0 and TH0, TL0 is used as an 8-bit timer / counter and occupies all control bits of Timer0; and TH0 also serves as another 8-bit timer Use, occupy TR1, TF1 and interrupt resources of Timer1, and Timer1 is still available at this time, but the start control bit TR1 and overflow flag bit TF1 cannot be used. In the case of Timer / Counter 1, entering Mode 3 will stop Timer / Counter 1.</td></tr>
    
</table>

### Timern count low byte (TLn) (n = 0, 1):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>TLn</td><td>RW</td><td>Timern count low byte</td><td>xxh</td></tr>
    
</table>

### Timern count high byte (THn) (n = 0, 1):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>TLn</td><td>RW</td><td>Timern count high byte</td><td>xxh</td></tr>
    
</table>

## 12.2 Timer2

Timer2 is a 16-bit auto-reload timer/counter. It is configured through the T2CON and T2MOD registers. The high byte counter of timer 2 is TH2 and the low byte is TL2. Timer2 can be used as the baud rate generator of UART0. It also has two signal level capture functions. The capture count is stored in RCAP2 and T2CAP1 registers.

<div>
    <p align="center">Table 12.2.1 List of Timer2 related registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>TH2 </td><td>CDh</td><td> Timer2 Counter High Byte</td><td> 00h</td></tr>
    <tr><td>TL2 </td><td>CCh</td><td> Timer2 counter low byte</td><td> 00h</td></tr>
    <tr><td>T2COUNT </td><td>CCh</td><td> TL2 and TH2 form 16-bit SFR</td><td> 0000h</td></tr>
    <tr><td>T2CAP1H </td><td>CDh</td><td> Timer2 Capture 1 data high byte (read only)</td><td> xxh</td></tr>
    <tr><td>T2CAP1L </td><td>CCh</td><td> Timer2 capture 1 data low byte (read only)</td><td> xxh</td></tr>
    <tr><td>T2CAP1 </td><td>CCh</td><td> T2CAP1L and T2CAP1H form a 16-bit SFR</td><td> xxxxh</td></tr>
    <tr><td>RCAP2H </td><td>CBh</td><td> Count Reload/Capture 2 Data Register High Byte</td><td> 00h</td></tr>
    <tr><td>RCAP2L </td><td>CAh</td><td> Count Reload/Capture 2 Data Register Low Byte</td><td> 00h</td></tr>
    <tr><td>RCAP2 </td><td>CAh</td><td> RCAP2L and RCAP2H form a 16-bit SFR</td><td> 0000h</td></tr>
    <tr><td>T2MOD </td><td>C9h</td><td> Timer2 mode register</td><td> 00h</td></tr>
    <tr><td>T2CON </td><td>C8h</td><td> Timer2 Control Register</td><td> 00h</td></tr>
    
</table>


### Timer/Counter 2 Control Register (T2CON):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>TF2 </td><td>RW</td><td> When bT2_CAP1_EN = 0, it is the overflow interrupt flag of Timer2. When Timer2 counts from 16 bits are all 1 to all 0, setting the overflow flag to 1 requires software to clear it; when RCLK = 1 or TCLK When = 1, this bit will not be set to 1</td><td> 0</td></tr>
    <tr><td>7</td><td>CAP1F</td><td>RW</td><td> When bT2_CAP1_EN = 1, it is Timer2 capture 1 interrupt flag, which is triggered by the valid edge of T2 and needs to be cleared by software</td><td> 0</td></tr>
    <tr><td>6</td><td>EXF2 </td><td>RW</td><td> Timer2 external trigger flag. When EXEN2 = 1, it is set by T2EX valid edge trigger. Need to be cleared by software.</td><td> 0</td></tr>
    <tr><td>5</td><td>RCLK </td><td>RW</td><td> UART0 receive clock selection, this bit is 0 to select the baud rate generated by Timer1 overflow pulse; this bit is 1 to select the baud rate generated by Timer2 overflow pulse</td><td> 0</td></tr>
    <tr><td>4</td><td>TCLK </td><td>RW</td><td> UART0 transmit clock selection, this bit is 0 to select the baud rate generated by Timer1 overflow pulse; this bit is 1 to select the baud rate generated by Timer2 overflow pulse</td><td> 0</td></tr>
    <tr><td>3</td><td>EXEN2</td><td>RW</td><td> T2EX trigger enable bit, this bit is 0 to ignore T2EX. This bit is 1 to enable triggering reload or capture on T2EX valid edge</td><td> 0</td></tr>
    <tr><td>2 </td><td>TR2 </td><td>RW</td><td> Timer2 start / stop bit, set to 1 to start, set or cleared by software</td><td> 0</td></tr>
    <tr><td>1 </td><td>C_T2</td><td>RW</td><td> Timer2 clock source selection bit, this bit is 0 to use the internal clock; this bit is 1 to use the edge count based on the falling edge of the T2 pin</td><td> 0</td></tr>
    <tr><td>0</td><td>CP_RL2</td><td>RW</td><td> Timer2 function selection bit. If RCLK or TCLK is 1, this bit should be forced to 0. If this bit is 0, Timer2 is used as a timer / counter, and it can automatically reload the initial count value when the counter overflows or the T2EX level changes; this bit is 1 to enable the capture 2 function of Timer2 and capture the valid edge of T2EX</td><td> 0</td></tr>
        
</table>

### Timer/Counter 2 Mode Register (T2MOD):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th colspan="2">Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bTMR_CLK</td><td>RW</td><td colspan="2">The fast clock mode enable of the T0 / T1 / T2 timer of the fast clock has been selected. If this bit is 1, the system clock Fsys without frequency division is used as the count clock. If this bit is 0, the frequency division clock is used. This bit has no effect on the timer that selects the standard clock</td><td>0</td></tr>
    <tr><td>6</td><td>bT2_CLK</td><td>RW</td><td colspan="2">Timer2 internal clock frequency selection bit. This bit is 0 to select the standard clock. The timing / counting mode is Fsys / 12. UART0 clock mode is Fsys / 4. This bit is 1 to select the fast clock. = 0) or Fsys (bTMR_CLK = 1), the UART0 clock mode is Fsys / 2 (bTMR_CLK = 0) or Fsys (bTMR_CLK = 1)</td><td>0</td></tr>
    <tr><td>5</td><td>bT1_CLK</td><td>RW</td><td colspan="2">Timer1 internal clock frequency selection bit. This bit is 0 to select the standard clock Fsys / 12. For 1 to select the fast clock Fsys / 4 (bTMR_CLK = 0) or Fsys (bTMR_CLK = 1)</td><td>0</td></tr>
    <tr><td>4</td><td>bT0_CLK</td><td>RW</td><td colspan="2">Timer0 internal clock frequency selection bit. This bit is 0 for the standard clock Fsys / 12. For 1 it is the fast clock Fsys / 4 (bTMR_CLK = 0) or Fsys (bTMR_CLK = 1)</td><td>0</td></tr>
    <tr><td>3</td><td>bT2_CAP_M1</td><td>RW</td><td>Timer2 capture mode high</td><td rowspan="2"><p>Capture mode selection:</p><p>X0: from falling edge to falling edge</p><p>01: From any edge to any edge, that is, the level changes</p><p>11: from rising edge to rising edge</p></td><td>0</td></tr>
    <tr><td>2</td><td>bT2_CAP_M0</td><td>RW</td><td>Timer2 capture mode low</td><td>0</td></tr>
    <tr><td>1</td><td>T2OE</td><td>RW</td><td colspan="2">Timer2 clock output enable bit, this bit is 0 to disable the output. This bit is 1 to enable the T2 pin output clock, the frequency is half of the Timer2 overflow rate</td><td>0</td></tr>
    <tr><td>0</td><td>bT2_CAP1_EN</td><td>RW</td><td colspan="2">Capture 1 mode is enabled when RCLK = 0, TCLK = 0, CP_RL2 = 1, C_T2 = 0, T2OE = 0, this bit is 1 to enable capture 1 function to capture the valid edge of T2. This bit is 0 to disable capture 1</td><td>0</td></tr>
    
</table>


### Count Reload / Capture 2 Data Register (RCAP2):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>RCAP2H</td><td>RW</td><td>High byte of reload value in timer / counter mode. High byte of timer captured by CAP2 in capture mode</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>RCAP2L</td><td>RW</td><td>Low byte of reload value in timer / counter mode. Low byte of timer captured by CAP2 in capture mode</td><td>00h</td></tr>
    
</table>

### Timer2 counter (T2COUNT), only valid when bT2_CAP1_EN = 0:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>TH2</td><td>RW</td><td>High byte of current counter</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>TL2</td><td>RW</td><td>Low byte of current counter</td><td>00h</td></tr>
    
</table>

### Timer2 capture 1 data (T2CAP1), only valid when bT2_CAP1_EN = 1:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T2CAP1H</td><td>RW</td><td>High byte of timer captured by CAP1</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T2CAP1L</td><td>RW</td><td>Low byte of timer captured by CAP1</td><td>xxh</td></tr>
    
</table>

## 12.3 Timer3

<div>
    <p align="center">Table 12.3.1 List of Timer3 related registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>T3_FIFO_H </td><td>AFh</td><td> FIFO high byte of Timer3</td><td> xxh</td></tr>
    <tr><td>T3_FIFO_L </td><td>AEh</td><td> FIFO low byte of Timer3</td><td> xxh</td></tr>
    <tr><td>T3_FIFO </td><td>AEh</td><td> T3_FIFO_L and T3_FIFO_H form a 16-bit SFR</td><td> xxxxh</td></tr>
    <tr><td>T3_DMA_AH </td><td>ADh</td><td> DMA current buffer address high byte</td><td> 0xh</td></tr>
    <tr><td>T3_DMA_AL </td><td>ACh</td><td> DMA current buffer address low byte</td><td> xxh</td></tr>
    <tr><td>T3_DMA </td><td>ACh</td><td> T3_DMA_AL and T3_DMA_AH form a 16-bit SFR </td><td>0xxxh</td></tr>
    <tr><td>T3_DMA_CN </td><td>ABh</td><td> DMA Remaining Count Register </td><td>00h</td></tr>
    <tr><td>T3_CTRL </td><td>AAh</td><td> Timer3 Control Register </td><td>02h</td></tr>
    <tr><td>T3_STAT </td><td>A9h</td><td> Timer3 status register </td><td>00h</td></tr>
    <tr><td>T3_END_H </td><td>A7h</td><td> Timer3 count end high byte </td><td>xxh</td></tr>
    <tr><td>T3_END_L </td><td>A6h</td><td> Timer3 low end byte count </td><td>xxh</td></tr>
    <tr><td>T3_END </td><td>A6h</td><td> T3_END_L and T3_END_H form a 16-bit SFR </td><td>xxxxh</td></tr>
    <tr><td>T3_COUNT_H </td><td>A5h</td><td> Timer3 current count high byte (read only) </td><td>00h</td></tr>
    <tr><td>T3_COUNT_L </td><td>A4h</td><td> Timer3 current count low byte (read only) </td><td>00h</td></tr>
    <tr><td>T3_COUNT </td><td>A4h</td><td> T3_COUNT_L and T3_COUNT_H form a 16-bit SFR </td><td>0000h</td></tr>
    <tr><td>T3_CK_SE_H </td><td>A5h</td><td> Timer3 clock division setting high byte </td><td>00h</td></tr>
    <tr><td>T3_CK_SE_L </td><td>A4h</td><td> Timer3 clock division setting low byte </td><td>20h</td></tr>
    <tr><td>T3_CK_SE </td><td>A4h</td><td> T3_CK_SE_L and T3_CK_SE_H form a 16-bit SFR </td><td>0020h</td></tr>
    <tr><td>T3_SETUP </td><td>A3h</td><td> Timer3 setting register </td><td>04h</td></tr>
    
</table>

### Timer3 setup register (T3_SETUP):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bT3_IE_END</td><td>RW</td><td>This bit is 1 to enable the capture mode count timeout interrupt or the PWM mode cycle end interrupt. This bit is 0 to disable the enable</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_IE_FIFO_OV</td><td>RW</td><td>This bit is 1 to enable the FIFO overflow interrupt. This bit is 0 to disable the enable</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_IE_FIFO_REQ</td><td>RW</td><td>This bit is 1 to enable capture mode FIFO> = 4 interrupt or PWM mode FIFO <= 3 interrupt; this bit is 0 to disable enable</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IE_ACT</td><td>RW</td><td>This bit is 1 to enable the capture mode input signal to activate the interrupt or PWM mode data to trigger the interrupt; this bit is 1 to disable the enable</td><td>0</td></tr>
    <tr><td>3</td><td>reserve</td><td>R0</td><td>reserve</td><td>0</td></tr>
    <tr><td>2</td><td>bT3_CAP_IN</td><td>R0</td><td>Input level of the current capture pin after noise filtering</td><td>1</td></tr>
    <tr><td>1</td><td>bT3_CAP_CLK</td><td>RW</td><td>This bit is 1 to enable input capture without minimum pulse width limitation.It is only valid when T3_CK_SE is 1.It is used to capture high speed signals.</td><td>0</td></tr>
    <tr><td>0</td><td>bT3_EN_CK_SE</td><td>RW</td><td>This bit is 1 to enable access to the clock divider setting register; this bit is 0 to enable access to the current count register</td><td>0</td></tr>
    
</table>

### Timer3 current count (T3_COUNT), only valid when bT3_EN_CK_SE = 0:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_COUNT_H</td><td>RO</td><td>Timer3 current count high byte</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>T3_COUNT_L</td><td>RO</td><td>Timer3 current count low byte</td><td>00h</td></tr>
    
</table>

### Timer3 clock division setting register (T3_CK_SE), only valid when bT3_EN_CK_SE = 1:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_CK_SE_H</td><td>RW</td><td>Timer3 clock divider high byte, only the lower 4 bits are valid, the upper 4 bits are fixed to 0</td><td>00h</td></tr>
    <tr><td>[7:0]</td><td>T3_CK_SE_L</td><td>RW</td><td>Low byte of Timer3 clock divider</td><td>20h</td></tr>
    
</table>

### Timer3 count end value register (T3_END):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_END_H</td><td>RW</td><td>Timer3 count end high byte</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T3_END_L</td><td>RW</td><td>Timer3 count end low byte</td><td>xxh</td></tr>
    
</table>

### Timer3 status register (T3_STAT):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bT3_IF_DMA_END</td><td>RW</td><td>DMA Complete Interrupt Flag. A 1 in this bit indicates an interrupt. A 0 in this bit indicates no interrupt. Cleared on write 1 or cleared on T3_DMA_CN</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_IF_FIFO_OV</td><td>RW</td><td>A 1 in this bit indicates a FIFO overflow interrupt. A 0 in this bit indicates no interrupt. Write 1 clear</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_IF_FIFO_REQ</td><td>RW</td><td>When this bit is 1, it indicates that the FIFO data interrupt flag is requested. In capture mode, it is triggered by FIFO> = 4, and in PWM mode, it is triggered by FIFO <= 3. If this bit is 0, there is no interrupt. Write 1 clear</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IF_ACT</td><td>RW</td><td>When bT3_IE_ACT = 1, this bit is 1 to indicate that the capture mode input signal activates the interrupt or the PWM mode data triggers the interrupt. This bit is 0 without interrupt. Cleared on write 1 or cleared when accessing FIFO</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_IF_END</td><td>RW</td><td>When bT3_IE_ACT = 0, this bit is 1 to indicate the capture mode count timeout interrupt or PWM mode cycle end interrupt. If this bit is 0, there is no interrupt. Write 1 clear</td><td>0</td></tr>
    <tr><td>[3:0]</td><td>MASK_T3_FIFO_CNT</td><td>R0</td><td>Timer3's current FIFO count</td><td>0000b</td></tr>
    
</table>

### Timer3 control register (T3_CTRL):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bT3_CAP_M1</td><td>RW</td><td>Timer3 capture edge mode high. PWM data repeat mode high</td><td>0</td></tr>
    <tr><td>6</td><td>bT3_CAP_M0</td><td>RW</td><td>Timer3 capture edge mode low bit. PWM data repeat mode high bit</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_PWM_POLAR</td><td>RW</td><td>PWM output polarity control bit in PWM mode. If this bit is 0, the default low level is valid, and the high level is valid.If this bit is 1, the default high level is active, and the low level is valid.</td><td>0</td></tr>
    <tr><td>5</td><td>bT3_CAP_WIDTH</td><td>RW</td><td>In capture mode, the minimum capture pulse width setting bit. If this bit is 0, at least 4 frequency division clock cycles are valid.</td><td>0</td></tr>
    <tr><td>4</td><td>bT3_DMA_EN</td><td>RW</td><td>This bit is 1 to enable Timer3's DMA and DMA interrupts. 0 to disable it</td><td>0</td></tr>
    <tr><td>3</td><td>bT3_OUT_EN</td><td>RW</td><td>This bit is 1 to enable the PWM output of Timer3. This bit is 0 to disable</td><td>0</td></tr>
    <tr><td>2</td><td>bT3_CNT_EN</td><td>RW</td><td>This bit is 1 to enable Timer3 counting. This bit is 0 to pause counting</td><td>0</td></tr>
    <tr><td>1</td><td>bT3_CLR_ALL</td><td>RW</td><td>This bit is 1 to clear the Timer3 count and FIFO, which needs to be cleared by software</td><td>1</td></tr>
    <tr><td>0</td><td>bT3_MOD_CAP</td><td>RW</td><td>Timer3 mode selection bit. If this bit is 0, Timer3 works in timer/count or PWM mode.If this bit is 1, it works in capture mode.</td><td>0</td></tr>
    
</table>

In capture mode, bT3_CAP_M1 and bT3_CAP_M0 select the capture edge: when bT3_CAP_M1 and bT3_CAP_M0 are 00, the capture mode is turned off or suspended; when it is 01, the edge is activated by any edge, that is, the change in level, and the capture is from any edge to any edge; when it is 10 Activated by falling edge, capture from falling edge to falling edge. When it is 11, activate by rising edge, capture from rising edge to rising edge.

In PWM mode, bT3_CAP_M1 and bT3_CAP_M0 select the number of data repetitions: When bT3_CAP_M1 and bT3_CAP_M0 are 00, the data is not reused, that is, a new data is used every PWM cycle. When it is 01, the data is reused 4 times, that is, the same data is used for continuous 4 PWM cycles. When it is 10, the data is reused 8 times. When it is 11, the data is reused 16 times.


### DMA remaining count register (T3_DMA_CN):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_DMA_CN</td><td>RW</td><td>The current remaining DMA count, which can be preset to the initial value, and is automatically reduced after the DMA operation</td><td>00h</td></tr>
    
</table>

### DMA current buffer address (T3_DMA):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_DMA_AH</td><td>RW</td><td>The current high byte of the DMA address, which can be preset to the initial value. It automatically increases after DMA. Only the lower 4 bits are valid. The upper 4 bits are fixed to 0. Only the first 4K of xRAM are supported.</td><td>0xh</td></tr>
    <tr><td>[7:0]</td><td>T3_DMA_AL</td><td>RW</td><td>Low byte of the current DMA address, which can be preset to the initial value. It is automatically increased after DMA. Only the upper 7 bits are valid. The lowest bit is fixed at 0. Only even addresses are supported.</td><td>xxh</td></tr>
    
</table>

### FIFO port (T3_FIFO):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>T3_FIFO_H</td><td>RW</td><td>FIFO high byte of Timer3</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>T3_FIFO_L</td><td>RW</td><td>FIFO low byte of Timer3</td><td>xxh</td></tr>
    
</table>

## 12.4 PWM Function

Timer3 of CH559 has a 16-bit PWM function. In addition, there are two other 8-bit PWMs. The PWM can choose the default output polarity to be low or high. It can dynamically modify the PWM output duty cycle. After the simple RC resistor and capacitor perform integral low-pass filtering, various output voltages can be obtained, which is equivalent to a low-speed digital-to-analog converter DAC.

PWM3 output duty cycle = T3_FIFO / T3_END, the support range is 0% to 100%. If the value of T3_FIFO is greater than T3_END, it is treated as 100%.

PWM1 output duty cycle = PWM_DATA / PWM_CYCLE, support range 0% to 100%, if the value of PWM_DATA is greater than PWM_CYCLE, it will be treated as 100%.

PWM2 output duty cycle = PWM_DATA2 / PWM_CYCLE, the support range is 0% to 100%. If the value of PWM_DATA2 is greater than PWM_CYCLE, it is treated as 100%.

In practical applications, it is recommended to allow the PWM pin output and set the PWM output pin to push-pull output mode.

### 12.4.1 PWM1 and PWM2

<div>
    <p align="center">Table 12.4.1 List of PWM1 and PWM2 related registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>PWM_CYCLE</td><td>9Fh</td><td>3</td><td>xxh</td></tr>
    <tr><td>PWM_CK_SE</td><td>9Eh</td><td>3</td><td>00h</td></tr>
    <tr><td>PWM_CTRL</td><td>9Dh</td><td>3</td><td>02h</td></tr>
    <tr><td>PWM_DATA</td><td>9Ch</td><td>3</td><td>xxh</td></tr>
    <tr><td>PWM_DATA2</td><td>9Bh</td><td>3</td><td>xxh</td></tr>
    
</table>


### PWM2 data register (PWM_DATA2):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_DATA2</td><td>RW</td><td>Store current data of PWM2, duty cycle of PWM2 output active level = PWM_DATA2 / PWM_CYCLE</td><td>xxh</td></tr>
    
</table>

### PWM1 data register (PWM_DATA):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_DATA</td><td>RW</td><td>Store the current data of PWM1, the duty cycle of PWM1 output active level = PWM_DATA / PWM_CYCLE</td><td>xxh</td></tr>
    
</table>

### PWM control register (PWM_CTRL):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bPWM_IE_END</td><td>RW</td><td>This bit is 1 to enable the end of the PWM cycle or the MFM buffer empty interrupt</td><td>0</td></tr>
    <tr><td>6</td><td>bPWM2_POLAR</td><td>RW</td><td>When bPWM_MOD_MFM = 0, control the output polarity of PWM2. When this bit is 0, the default low level is valid, and the high level is valid. When the bit is 1, it is default high level and the low level is active</td><td>0</td></tr>
    <tr><td>6</td><td>bMFM_BUF_EMPTY</td><td>R0</td><td>When bPWM_MOD_MFM = 1, this bit is 1 to indicate that the MFM buffer is empty</td><td>0</td></tr>
    <tr><td>5</td><td>bPWM_POLAR</td><td>RW</td><td>Controls the polarity of PWM1 output. If this bit is 0, it defaults to low level, and the high level is valid. If this bit is 1, it defaults to high level, and the low level is valid.</td><td>0</td></tr>
    <tr><td>4</td><td>bPWM_IF_END</td><td>RW</td><td>PWM cycle end interrupt flag bit, this bit is 1 means there is an interrupt, cleared by writing 1 or cleared by writing PWM_CYCLE or cleared when reloading data</td><td>0</td></tr>
    <tr><td>3</td><td>bPWM_OUT_EN</td><td>RW</td><td>PWM1 output enable, this bit is 1 to enable PWM1 output</td><td>0</td></tr>
    <tr><td>2</td><td>bPWM2_OUT_EN</td><td>RW</td><td>When bPWM_MOD_MFM = 0, this bit is 1 to enable PWM2 output</td><td>0</td></tr>
    <tr><td>2</td><td>bMFM_BIT_CNT2</td><td>R0</td><td>When bPWM_MOD_MFM = 1, it indicates the current MFM encoding progress. This bit is 0 to indicate that the lower 4 bits are being processed. A 1 indicates that the upper 4 bits are being processed.</td><td>0</td></tr>
    <tr><td>1</td><td>bPWM_CLR_ALL</td><td>RW</td><td>This bit is 1 to clear the PWM1 and PWM2 counts and FIFOs and needs to be cleared by software</td><td>1</td></tr>
    <tr><td>0</td><td>bPWM_MOD_MFM</td><td>RW</td><td>MFM encoding mode is enabled, this bit is 0 for PWM mode. This bit is 1 for MFM mode  </td><td>0</td></tr>
    
</table>

### PWM clock division setting register (PWM_CK_SE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_CK_SE</td><td>RW</td><td>Setting the PWM Clock Divisor</td><td>00h</td></tr>
    
</table>

### PWM cycle period register (PWM_CYCLE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>PWM_CYCLE</td><td>RW</td><td>Set the PWM cycle period, when the value is 00h, it means 100h</td><td>xxh</td></tr>
    
</table>


## 12.5 Timer Function

### 12.5.1 Timer0/1

1. Set T2MOD to select the internal clock frequency of the timer. If bTn_CLK (n = 0/1) is 0, then the clock corresponding to Timer0/1 is Fsys/12; if bTn_CLK is 1, then select Fsys by bTMR_CLK = 0 or 1. / 4 or Fsys as the clock.
2. Set the TMOD to configure the working mode of the timer.
3. Set the timer/counter initial values TLn and THn (n = 0/1).
4. Set the bit TRn (n = 0/1) in TCON to start or stop the timer/counter. It can be queried by bit TFn (n = 0/1) or detected by interrupt mode.

### Mode 0: 13-bit timer/counter

![Timer_Mode_0](/docs/12-Timer/images/tim_mod_0.png "Timer0/1 Mode 0")

<div>
    <p align="center">Figure 12.5.1.1 Timer0/1 Mode 0</p>
</div>

### Mode 1: 16-bit timer/counter

![Timer_Mode_1](/docs/12-Timer/images/tim_mod_1.png "Timer0/1 Mode 1")

<div>
    <p align="center">Figure 12.5.1.2 Timer0/1 Mode 1</p>
</div>

### Mode 2: Automatic reload 8-bit timer/counter

![Timer_Mode_2](/docs/12-Timer/images/tim_mod_2.png "Timer0/1 Mode 2")

<div>
    <p align="center">Figure 12.5.1.3 Timer0/1 Mode 2</p>
</div>


### Mode 3: Timer0 is decomposed into two independent 8-bit timers and counters, and the TR1 control bit of Timer1 is borrowed. Timer1 replaces the borrowed TR1 control bit by whether to start mode 3, Timer1 enters mode 3, and Timer1 stops running.

![Timer_Mode_3](/docs/12-Timer/images/tim_mod_3.png "Timer0/1 Mode 3")

<div>
    <p align="center">Figure 12.5.1.4 Timer0 Mode 3</p>
</div>


### 12.5.2 Timer2

Timer2 16-bit reload timer/counter mode:

1. Set the bits RCLK and TCLK in T2CON to 0, and select the non-serial port baud rate generator mode.
2. Set bit C_T2 in T2CON to 0. Select to use the internal clock, go to step (3). Can also set to 1 to select the falling edge of the T2 pin as the count clock, skip step (3).
3. Set T2MOD to select the internal clock frequency of the timer. If bT2_CLK is 0, the clock of Timer2 is Fsys / 12. If bT2_CLK is 1, then select Fsys / 4 or Fsys as the clock by T.R_CLK = 0 or 1.
4. Set the bit CP_RL2 of T2CON to 0 and select the 16-bit reload timer / counter function of Timer2.
5. Set RCAP2L and RCAP2H to the reload value after the timer overflows, set TL2 and TH2 to the initial value of the timer (generally the same as RCAP2L and RCAP2H), set TR2 to 1, and t.rt Timer2.
6. The current timer / counter status can be obtained by querying the TF2 or timer 2 interrupts.


![Timer2_16bit](/docs/12-Timer/images/tim2_16bit.png "Timer2 16-bit")

<div>
    <p align="center">Figure 12.5.2.1 Timer2 16-bit reload timer/counter</p>
</div>


Timer2 clock output mode:

Refer to the 16-bit reload timer / counter mode, and then set bit T2OE in T2MOD to 1 to enable the output of the TF2 frequency divided clock from the T2 pin.

Timer2 serial port 0 baud rate generator mode:

1. Set bit C_T2 in T2CON to 0 to choose to use the internal clock or set it to 1 to select the falling edge of the T2 pin as the clock. Set the RCLK and TCLK in T2CON to 1 or one of h.m as required. selecting the baud rate generator mode.
2. Set T2MOD to select the internal clock frequency of the timer. If bT2_CLK is 0, then the clock of Timer2 is Fsys / 4. If bT2_CLK is 1, then bTMR_CLK = 0 or 1 selects Fsys / 2 or Fsys as the clock.
(3) Set RCAP2L and RCAP2H to the reload value after the timer overflows, set TR2 to 1, and start Timer2.

![Timer2_baud_gen](/docs/12-Timer/images/tim2_uart0.png "Timer2 UART0 Baud Rate Generator")

<div>
    <p align="center">Figure 12.5.2.2 Timer2 UART0 Baud Rate Generator</p>
</div>

Timer2 dual channel capture mode:

1. Set the bits RCLK and TCLK in T2CON to 0, and select the non-serial port baud rate generator mode.
2. Set the bit C_T2 in T2CON to 0. Select to use the internal clock and go to step (3); also set it to select the falling edge of the T2 pin as the count clock and skip step (3).
3. Set T2MOD to select the internal clock frequency of the timer. If bT2_CLK is 0, the clock of Timer2 is Fsys/12. If bT2_CLK is 1, then select Fsys/4 or Fsys as the clock by T.R_CLK = 0 or 1.
4. Set the bits bT2_CAP_M1 and bT2_CAP_M0 of T2MOD to select the corresponding edge capture mode.
5. Set the bit CP_RL2 of T2CON to 1 to select the capture function of T2EX pin by Timer2.
6. Set TL2 and TH2 to the initial values ​​of the timer, set TR2 to 1, and start Timer2.
7. When the CAP2 capture is completed, RCAP2L and RCAP2H will save the current TL2 and TH2 count values ​​and set EXF2 to generate an interrupt.The next captured RCAP2L and RCAP2H i.l be between the last captured RCAP2L and RCAP2H The difference is the signal width between the two valid edges.
8. If bit C_T2 in T2CON is 0 and bit bT2_CAP1_EN in T2MOD is 1, then the capture function of Timer2 on the T2 pin will be enabled at the same time. When CAP1 capture is completed, T2CAP1L and T2CAP1H will save the current TL2 and TH2 And the CAP1F bit is set to generate an interrupt.

![tim2_capture_mode](/docs/12-Timer/images/tim2_capture_mode.png "Timer2 capture mode")

<div>
    <p align="center">Figure 12.5.2.3 Timer2 capture mode</p>
</div>

### 12.5.3 Timer3

1. Set the bit bT3_EN_CK_SE in T3_SETUP to 1, enable the Timer3 clock division setting register T3_CK_SE, set the division factor, and count the clock to Fsys / T3_CK_SE. Clear T._EN_CK_SE after the setting is completed.
2. Set the T3_END counting end value or the total number of PWM cycles.
3. Turn on interrupt enable in T3_SETUP as required.
4. Set each control bit in T3_CTRL, select the mode, and clear bT3_CLR_ALL, set bT3_CNT_EN to start Timer3.
5. Set T3_DMA_AL and T3_DMA_AH and T3_DMA_CN as required, set bT3_DMA_EN to enable the DMA function.

![tim3_16bit_tpc](/docs/12-Timer/images/tim3_16bit_tpc.png "Timer3 16-bit timer/PWM/capture")

<div>
    <p align="center">Figure 12.5.3.1 Timer3 16-bit timer/PWM/capture</p>
</div>

Data format captured by Timer3:

Timer3 can be used to capture the width between two valid edges of the signal.The width is expressed by the divided clock count. During or after the capture, the width data can be obtained through DMA or directly reading the FIFO.The data is 16 bits. The most significant bit is the identification bit and the lower 15 bits are the width count. After signal capture is enabled, data will be generated every time a valid edge is detected or every timer overflows.Because the data generated at the first valid edge is the width from the time when signal capture is enabled, not two valid edges Width, so the first data captured is usually discarded.

(1). bT3_CAP_M1 and bT3_CAP_M0 = 11

The rising edge is valid, capturing the signal width from the rising edge to the rising edge, that is, the rising edge is activated. If the highest bit of the data is 1, it means that the signal width has overflowed the counting end value, that is, if the timing exceeds T3_END and the next rising edge has not been found, the width value must be added to the width of the next data whose highest bit is 0. The most significant bit is 0, which means that the width is the width between the end of a certain rising edge, and whether the lower 15 bits are accumulated forward according to whether the most significant bit of some previous data is 1. In this mode, it is recommended to set T3_END to detect ultra-wide signals or end signals for special purposes, and normal signals should not overflow.

For example, T3_END is set to 4000h, and the raw data sequence collected is as follows

1234h, 2345h, 0456h, C000h, C000h, 1035h, 3579h, C000h, 2468h, and 0987h are combined to obtain the interval between each rising edge: 1234h, 2345h, 0456h, 9035h, 3579h, 6468h, 0987h

(2). bT3_CAP_M1 and bT3_CAP_M0 = 10

Same as above, but the falling edge is valid, capturing the signal width from the rising edge to the falling edge, that is, the falling edge is activated.


(3). bT3_CAP_M1 and bT3_CAP_M0 = 01

Any edge is valid, capturing the signal width from any edge to any edge, that is, the level change is activated. If the highest bit of the data is 1, then the width is a high-level width, that is, from the rising edge to the falling edge. If the highest bit of the data is 0, then the width is a low-level width, that is, from the falling edge to the rising edge. The lower 15 bits after masking the most significant bits are width values counted by the divided clock. In this mode, it is recommended to set T3_END to a larger value to avoid timing overflow, but it must not exceed 15 digits of valid data.