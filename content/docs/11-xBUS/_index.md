---
title: 11. External bus xBUS
type: docs
weight: 11
BookToC: false
---

# 11. External bus xBUS

## 11.1 External bus registers

### External bus auxiliary setting register (XBUS_AUX):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bUART0_TX</td><td>R0</td><td>Indicate the transmission status of UART0, 1 means it is transmitting</td><td>0</td></tr>
    <tr><td>6</td><td>bUART0_RX</td><td>R0</td><td>Indicate the receiving status of UART0, 1 means it is receiving</td><td>0</td></tr>
    <tr><td>5</td><td>bSAFE_MOD_ACT</td><td>R0</td><td>Indicate the safe mode status, 1 means that it is currently in safe mode</td><td>0</td></tr>
    <tr><td>4</td><td>bALE_CLK_EN</td><td>RW</td><td>ALE pin clock output enable, this bit is 1 to allow ALE to output the system's main frequency of 12 when there is no xBUS operation, that is, Fsys / 12; this bit is 0 to disable the output of clock signals and only output when necessary to access the external bus Low 8-bit address latch signal to reduce EMI</td><td>0</td></tr>
    <tr><td>3</td><td>GF2</td><td>RW</td><td>General flag bit 2: user can define by himself, can be cleared or set by software</td><td>0</td></tr>
    <tr><td>2</td><td>bDPTR_AUTO_INC</td><td>RW</td><td>Enable auto-increment DPTR by 1 after the MOVX_@DPTR instruction is completed</td><td>0</td></tr>
    <tr><td>1</td><td>reserved</td><td>R0</td><td>reserved</td><td>0</td></tr>
    <tr><td>0</td><td>DPS</td><td>RW</td><td><p>Dual DPTR data pointer selection bits:</p>This bit is 0 to select DPTR0. This bit is 1 to select DPTR1</td><td>0</td></tr>
    
</table>


### External bus speed configuration register (XBUS_SPEED):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bXBUS1_SETUP</td><td>RW</td><td>Select XBUS1 settling time: if this bit is 0, then 2 clock cycles. If this bit is 1, then 3 clock cycles</td><td>1</td></tr>
    <tr><td>6</td><td>bXBUS1_HOLD</td><td>RW</td><td>Select XBUS1 hold time: if this bit is 0, it will take 1 clock cycle. If this bit is 1, it will take 2 clock cycles</td><td>1</td></tr>
    <tr><td>5</td><td>bXBUS1_WIDTH1</td><td>RW</td><td>XBUS1 bus pulse width high</td><td>1</td></tr>
    <tr><td>4</td><td>bXBUS1_WIDTH0</td><td>RW</td><td>XBUS1 bus pulse width low bit</td><td>1</td></tr>
    <tr><td>3</td><td>bXBUS0_SETUP</td><td>RW</td><td>Select XBUS0 settling time: If this bit is 0, it will take 2 clock cycles. If this bit is 1, it will take 3 clock cycles</td><td>1</td></tr>
    <tr><td>2</td><td>bXBUS0_HOLD</td><td>RW</td><td>Select XBUS0 hold time: if this bit is 0, it will take 1 clock cycle. If this bit is 1, it will take 2 clock cycles</td><td>1</td></tr>
    <tr><td>1</td><td>bXBUS0_WIDTH1</td><td>RW</td><td>XBUS0 bus pulse width high order</td><td>1</td></tr>
    <tr><td>0</td><td>bXBUS0_WIDTH0</td><td>RW</td><td>XBUS0 bus pulse width low bit</td><td>1</td></tr>
    
</table>

## 11.2 External bus pins

<div>
    <p align="center">Table 11.2.1 External bus pin list</p>
</div>

<table>
    <tr>
        <th>GPIO</th><th>Direct Address Mode Pin</th><th>Multiplexed Address Mode Pin</th><th>Description</th>
    </tr>
    <tr><td>P3.7</td><td>RD</td><td>RD</td><td>External bus read signal output pin, active low, rising edge sampling input</td></tr>
    <tr><td>P3.6</td><td>WR</td><td>WR</td><td>External bus write signal output pin, active low</td></tr>
    <tr><td rowspan="2">P0.0~P0.7</td><td>D0~D7</td><td>D0~D7</td><td>8-bit bidirectional data bus</td></tr>
    <tr><td></td><td>A0~A7</td><td>Multiplex lower 8-bit address A [0:7] output, latched by external circuit controlled by ALE</td></tr>
    <tr><td>P4.0~P4.5</td><td>A0~A5</td><td>Unused</td><td>Direct bus address A [0: 5] output pin, P4_DIR output must be set</td></tr>
    <tr><td>P3.5</td><td>A6</td><td>Unused</td><td>Bus direct address A6 output pin</td></tr>
    <tr><td rowspan="2">P2.7</td><td>A7</td><td></td><td>Bus direct address A7 output pin</td></tr>
    <tr><td></td><td>A15</td><td>Bus address A15 output pin</td></tr>
    <tr><td>P2.0~P2.6</td><td>A8~A14</td><td>A8~A14</td><td>Bus address A [8:14] output pin</td></tr>
    <tr><td>P3.4</td><td>XCS0</td><td>XCS0</td><td>Chip select 0 output pin, address range 4000h~7FFFh, active low</td></tr>
    <tr><td>P3.3</td><td>!A15</td><td>!A15</td><td>Bus address A15 inverting output pin, equivalent to chip select 1 output, address range 8000h~FFFFh, active low, only available in ALE disabled state</td></tr>
    <tr><td>P5.5</td><td>!A15</td><td>!A15</td><td>Bus address A15 inverting output pin, equivalent to chip select 1 output, address range 8000h~FFFFh, active low, available only in ALE enabled state</td></tr>
    <tr><td rowspan="2">P5.4</td><td></td><td>ALE</td><td>Multiplexed low 8-bit address latch control output pin, active high</td></tr>
    <tr><td>ALE</td><td></td><td>System frequency 12-division clock Fsys / 12 output pin, duty cycle 1/12</td></tr>
    
</table>


Some of the above unused pins such as address output and chip select output in the external bus state can be used for other modules according to the GPIO multiplexing priority order, and the unused pins in P4.0~P4.5 can also be used. Set P4_DIR to keep the input status.


When bXBUS_CS_OE = 1, the inverting signal of bus address A15 will select the output pin according to the ALE output status. When ALE is allowed to output, !A15 chooses to output from P5.5. When ALE is disabled to output, !A15 chooses from P3.3 output. The ALE output status is determined by the combination of bUH1_DISABLE, bXBUS_EN, bXBUS_AL_OE, and bALE_CLK_EN, refer to Table 11.2.2 below.


<div>
    <p align="center">Table 11.2.2 P5.4 Pin Multiplexing ALE Output Status Table</p>
</div>

<table>
    <tr>
        <th>bUH1_DISABLE</th><th>bXBUS_EN</th><th>bXBUS_AL_OE</th><th>bALE_CLK_EN</th><th>P5.4 pin function description</th>
    </tr>
    <tr><td>0</td><td>x</td><td>x</td><td>x</td><td>Disable ALE output, priority as HM (P5.5 as HP)</td></tr>
    <tr><td>1</td><td>0</td><td>x</td><td>0</td><td>Disable ALE output, used as XB by default (P5.5 as XA)</td></tr>
    <tr><td>1</td><td>0</td><td>x</td><td>1</td><td>ALE only outputs the system's 12th-frequency clock signal</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>0</td><td>Disable ALE output, default for XB (P5.5 is used as XA)</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>1</td><td>ALE only outputs the system's 12th-frequency clock signal</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>0</td><td>ALE outputs the lower 8-bit address latch signal only on the bus</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>1</td><td>ALE outputs the lower 8-bit address latch signal when accessing the bus, and outputs the system's main frequency 12-frequency clock signal when idle</td></tr>
    
</table>