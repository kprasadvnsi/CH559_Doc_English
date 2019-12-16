---
title: 10. I/O Port
type: docs
weight: 10
BookToC: false
---

# 10. I/O Port

## 10.1 Introduction to GPIO

CH559 provides up to 45 I / O pins, some pins have alternate functions. Among them, the input and output of ports P0 ~ P3 and the output of P4 can be bit-addressed.

If the pin is not configured as an alternate function, the default is the general-purpose I/O pin state. When used as a general-purpose digital I/O, all I/O ports have a true “read-modify-write” function, which supports bit manipulation instructions such as SETB or CLR to independently change the direction of certain pins or port levels.

## 10.2 GPIO Register

All registers and bits in this section are expressed in a common format: the lowercase "n" represents the serial number of the port (n = 0, 1, 2, 3), and the lowercase "x" represents the serial number of the bit (x = 0, 1, 2, 3, 4, 5, 6, 7).

<div>
    <p align="center">Table 10.2.1 GPIO Register List</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>P0</td><td>80h</td><td>P0 port input and output register</td><td>FFh</td></tr>
    <tr><td>P0_DIR</td><td>C4h</td><td>P0 port direction control register</td><td>00h</td></tr>
    <tr><td>P0_PU</td><td>C5h</td><td>Port 0 pull-up enable register</td><td>00h/FFh</td></tr>
    <tr><td>P1</td><td>90h</td><td>P1 port input and output register</td><td>FFh</td></tr>
    <tr><td>P1_IE</td><td>B9h</td><td>P1 port input enable register</td><td>FFh</td></tr>
    <tr><td>P1_DIR</td><td>BAh</td><td>P1 port direction control register</td><td>00h</td></tr>
    <tr><td>P1_PU</td><td>BBh</td><td>P1 port pull-up enable register</td><td>FFh</td></tr>
    <tr><td>P2</td><td>A0h</td><td>P2 port input and output register</td><td>FFh</td></tr>
    <tr><td>P2_DIR</td><td>BCh</td><td>P2 port direction control register</td><td>00h</td></tr>
    <tr><td>P2_PU</td><td>BDh</td><td>P2 port pull-up enable register</td><td>FFh</td></tr>
    <tr><td>P3</td><td>B0h</td><td>P3 port input and output register</td><td>FFh</td></tr>
    <tr><td>P3_DIR</td><td>BEh</td><td>P3 port direction control register</td><td>00h</td></tr>
    <tr><td>P3_PU</td><td>BFh</td><td>P3 port pull-up enable register</td><td>FFh</td></tr>
    <tr><td>P4_OUT</td><td>C0h</td><td>P4 port output register</td><td>00h</td></tr>
    <tr><td>P4_IN</td><td>C1h</td><td>P4 port input register (read only)</td><td>FFh</td></tr>
    <tr><td>P4_DIR</td><td>C2h</td><td>P4 port direction control register</td><td>00h</td></tr>
    <tr><td>P4_PU</td><td>C3h</td><td>P4 port pull-up enable register</td><td>FFh</td></tr>
    <tr><td>P4_CFG</td><td>C7h</td><td>P4 port configuration register</td><td>00h</td></tr>
    <tr><td>P5_IN</td><td>C7h</td><td>P5 port input register (read only)</td><td>00h</td></tr>
    <tr><td>PIN_FUNC</td><td>CEh</td><td>Pin Function Select Register</td><td>00h</td></tr>
    <tr><td>PORT_CFG</td><td>C6h</td><td>Port Configuration Register</td><td>0Fh</td></tr>
    <tr><td>XBUS_SPEED</td><td>FDh</td><td>Bus speed configuration register</td><td>FFh</td></tr>
    <tr><td>XBUS_AUX</td><td>A2h</td><td>Bus auxiliary setting register</td><td>00h</td></tr>

</table>

### Port configuration register (PORT_CFG):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:4]</td><td>bPn_DRV</td><td>RW</td><td>Pn port output drive capability selection, this bit is 0 to select the drive current 5mA level. This bit is 1 for P0 / P2 / P3 to select the drive current 20mA level, and for P1 to select the drive current 10mA level</td><td>0000b</td></tr>
    <tr><td>[3:0]</td><td>bPn_OC</td><td>RW</td><td>Pn port open-drain output enable, this bit is 0 to set the port as push-pull output. This bit is 1 to set the port to be open-drain output</td><td>1111b</td></tr>
    
</table>

### Pn port input and output register (Pn):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>Pn.0~Pn.7</td><td>RW</td><td>Pn.x pin status input and data output bits, bit-addressable</td><td>FFh</td></tr>
    
</table>

### Pn port direction control register (Pn_DIR):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>Pn_DIR</td><td>RW</td><td>Pn.x pin direction setting</td><td>00h</td></tr>
    
</table>

### P0 port pull-up enable register (P0_PU) and Pn port pull-up enable register (Pn_PU), where n = 1/2/3:

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td rowspan="2">[7:0]</td><td rowspan="2">P0_PU</td><td rowspan="2">RW</td><td>P0.x pin pull-up resistor is enabled (when En_P0_Pullup = 0 is configured)</td><td>00h</td></tr>
    <td>P0.x pin pull-up resistor is enabled (when En_P0_Pullup = 1 is configured)</td><td>FFh</td></tr>
    <tr><td>[7:0]</td><td>Pn_PU</td><td>RW</td><td>Pn.x pin pull-up resistor is enabled, this bit is 0 to disable pull-up; this bit is 1 to enable pull-up</td><td>FFh</td></tr>
    
</table>

The configuration of the Pn port is implemented by the combination of the bit bPn_OC in the PORT_CFG, the port direction control register Pn_DIR, and the port pull-up enable register Pn_PU, as follows.

<div>
    <p align="center">Table 10.2.2 Port configuration register combinations</p>
</div>

<table>
    <tr>
        <th>bPn_OC</th><th>Pn_DIR</th><th>Pn_PU</th><th>Description</th>
    </tr>
    <tr><td>0</td><td>0</td><td>0</td><td>High-impedance input mode, no pull-up resistor on pin</td></tr>
    <tr><td>0</td><td>0</td><td>1</td><td>Pull-up input mode, pins have pull-up resistors</td></tr>
    <tr><td>0</td><td>1</td><td>x</td><td>Push-pull output mode, with symmetrical driving ability, can output or absorb large current</td></tr>
    <tr><td>1</td><td>0</td><td>0</td><td>High-impedance input weak quasi-bidirectional mode, open-drain output, no pull-up resistor on pin</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>High-impedance input quasi-bidirectional mode, open-drain output, no pull-up resistor on the pin, when the output changes from low to high, it automatically drives high for 2 clock cycles to speed up the conversion</td></tr>
    <tr><td>1</td><td>0</td><td>1</td><td>Weak quasi-bidirectional mode (imitation of 8051), open-drain output, support input, pin with pull-up resistor</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>Quasi-bidirectional mode (standard 8051), open-drain output, support input, pins have pull-up resistors, when the output changes from low to high, it automatically drives high for 2 clock cycles to speed up the conversion</td></tr>
    
</table>

Ports P0~P3 support pure input or push-pull output and quasi-bidirectional modes. Port P4 supports pure input or push-pull output and other modes. Each pin has a freely controllable internal pull-up resistor connected to VDD33, and a protection diode connected to GND.

Figure 10.2.1 is the equivalent schematic of the P1.x pin of the P1 port. After removing P1_IE and AIN and ADC_CHANN, it can be applied to the P0, P2, and P3 ports.

<div>
    <p align="center">Figure 10.2.1 I/O pin equivalent schematic</p>
</div>

![GPIO_schematic](/docs/10-GPIO/images/GPIO_schematic.png "GPIO schematic")

### P1 port input enable register (P1_IE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>P1_IE</td><td>RW</td><td>P1.x pin input enable, if this bit is 0, the pin is used for ADC analog input, digital input is disabled. If this bit is 1, the digital input is enabled</td><td>FFh</td></tr>
    
</table>

## 10.3 P4 port

### P4 port output register (P4_OUT):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_OUT.0~P4_OUT.7</td><td>RW</td><td>P4.x pin data output bit, bit-addressable</td><td>00h</td></tr>
    
</table>

### P4 port input register (P4_IN):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_IN</td><td>R0</td><td>P4.x pin status input bit</td><td>FFh</td></tr>
    
</table>

### P4 port pull-up enable register (P4_PU):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_PU</td><td>RW</td><td>P4.x pin pull-up resistor is enabled, this bit is 0 to disable pull-up; this bit is 1 to enable pull-up</td><td>FFh</td></tr>
    
</table>

### P4 port direction control register (P4_DIR):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>P4_DIR</td><td>RW</td><td>P4.x pin direction setting, this bit is 0 for input; this bit is 1 for output</td><td>00h</td></tr>
    
</table>

### P4 port configuration register (P4_CFG) and P5 port input register (P5_IN):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>P5.7</td><td>R0</td><td>P5.7 pin status input bit</td><td>0</td></tr>
    <tr><td>6</td><td>bIO_INT_ACT</td><td>R0</td><td><p>GPIO interrupt request activation status:</p><p>When bIE_IO_EDGE = 0, this bit is 1 to indicate the GPIO input active level, interrupt will be requested, and 0 to indicate the input invalid level.</p><p>When bIE_IO_EDGE = 1, this bit is used as an edge interrupt flag. A value of 1 indicates that a valid edge was detected.This bit cannot be cleared by software. It can only be automatically cleared during reset or level interrupt mode or when entering the corresponding interrupt service routine zero</p></td><td>0</td></tr>
    <tr><td>5</td><td>P5.5</td><td>R0</td><td>P5.5 pin status input bit with built-in controllable pull-down resistor</td><td>0</td></tr>
    <tr><td>4</td><td>P5.4</td><td>R0</td><td>P5.4 pin status input bit with built-in controllable pull-down resistor</td><td>0</td></tr>
    <tr><td>3</td><td>bSPI0_PIN_X</td><td>RW</td><td>SPI0 pin SCS / SCK mapping enable. If this bit is 0, P1.4 / P1.7 is used. If this bit is 1, P4.6 / P4.7 is used</td><td>0</td></tr>
    <tr><td>2</td><td>bP4_DRV</td><td>RW</td><td>P4 port output drive capability selection, this bit is 0 to select the drive current 5mA level. This bit is 1 to select the drive current 20mA level</td><td>0</td></tr>
    <tr><td>1</td><td>P5.1</td><td>R0</td><td>P5.1 pin status input bit with built-in controllable pull-down resistor</td><td>0</td></tr>
    <tr><td>0</td><td>P5.0</td><td>R0</td><td>P5.0 pin status input bit with built-in controllable pull-down resistor</td><td>0</td></tr>
    
</table>

## 10.4 GPIO Multiplexing and Mapping

Some of the I / O pins of CH559 have alternate functions. After power-on, they are all general-purpose I / O pins. After different function modules are enabled, the corresponding pins are configured as the corresponding function pins of the respective function modules.


### Pin function selection register (PIN_FUNC):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bPWM1_PIN_X</td><td>RW</td><td>PWM1/PWM2 pin mapping enable bit, if this bit is 0, PWM1/2 uses P2.4/P2.5. If this bit is 1, PWM1/2 uses P4.3/P4.5</td><td>0</td></tr>
    <tr><td>6</td><td>bTMR3_PIN_X</td><td>RW</td><td>PWM3/CAP3 pin mapping enable bit. When this bit is 0, PWM3/CAP3 uses P1.2. When this bit is 1, PWM3/CAP3 uses P4.2</td><td>0</td></tr>
    <tr><td>5</td><td>bT2EX_PIN_X</td><td>RW</td><td>T2EX/CAP2 pin mapping enable bit. When this bit is 0, T2EX/CAP2 uses P1.1. When this bit is 1, T2EX/CAP2 uses P2.5</td><td>0</td></tr>
    <tr><td>4</td><td>bUART0_PIN_X</td><td>RW</td><td>UART0 pin mapping enable bit. When this bit is 0, RXD0/TXD0 uses P3.0/P3.1. When this bit is 1, RXD0/TXD0 uses P0.2/P0.3.</td><td>0</td></tr>
    <tr><td>3</td><td>bXBUS_EN</td><td>RW</td><td>xBUS external bus function enable bit, this bit is 0 to disable the external bus; this bit is 1 to enable the P0 port as an 8-bit data bus, and P3.6/P3.7 as write/read strobe control during bus access</td><td>0</td></tr>
    <tr><td>2</td><td>bXBUS_CS_OE</td><td>RW</td><td>xBUS external bus chip select output enable bit. This bit is 0 to disable the output of chip select and can be decoded by external circuits. P3.4 is set as CS0 when this bit is 1 (XCS0 chip select 0, active low), and When ALE is disabled, the bus address A15 is inverted and output to P3.3 (equivalent to chip select 1, active low)</td><td>0</td></tr>
    <tr><td>1</td><td>bXBUS_AH_OE</td><td>RW</td><td>xBUS External bus high 8-bit address output enable bit, this bit is 0 to disable output. When this bit is 1, during the MOVX_@DPTR instruction access to the external bus, the P2 port output bus address is the upper 8 bits</td><td>0</td></tr>
    <tr><td>0</td><td>bXBUS_AL_OE</td><td>RW</td><td>The lower 8-bit address output enable bit of the xBUS external bus. When this bit is 0, it is a multiplexed address mode. When accessing the external bus, the lower 8 bits of the address are multiplexed with the data bus as required, and the external circuit is controlled by ALE to latch. If this bit is 1, it is the direct address mode.The lower 8-bit addresses A0~A7 are output through P4.0~P4.5 and P3.5 and P2.7.</td><td>0</td></tr>
    
</table>

<div>
    <p align="center">Figure 10.2.1 I/O pin equivalent schematic</p>
</div>

<table>
    <tr>
        <th>GPIO</th><th>Other functions: in order of priority from left to right</th>
    </tr>
    <tr><td>P0[0]</td><td> AD0, UDTR/bUDTR, P0.0</td></tr>
    <tr><td>P0[1]</td><td> AD1, URTS/bURTS, P0.1</td></tr>
    <tr><td>P0[2]</td><td> AD2, RXD_/bRXD_, P0.2</td></tr>
    <tr><td>P0[3]</td><td> AD3, TXD_/bTXD_, P0.3</td></tr>
    <tr><td>P0[4]</td><td> AD4, UCTS/bUCTS, P0.4</td></tr>
    <tr><td>P0[5]</td><td> AD5, UDSR/bUDSR, P0.5</td></tr>
    <tr><td>P0[6]</td><td> AD6, URI/bURI, P0.6</td></tr>
    <tr><td>P0[7]</td><td> AD7, UDCD/bUDCD, P0.7</td></tr>
    <tr><td>P1[0]</td><td> AIN0, T2/bT2, CAP1/bCAP1, P1.0</td></tr>
    <tr><td>P1[1]</td><td> AIN1, T2EX/bT2EX, CAP2/bCAP2, P1.1</td></tr>
    <tr><td>P1[2]</td><td> AIN2, PWM3/bPWM3, CAP3/bCAP3, P1.2</td></tr>
    <tr><td>P1[3]</td><td> AIN3, P1.3</td></tr>
    <tr><td>P1[4]</td><td> AIN4, SCS/bSCS, P1.4</td></tr>
    <tr><td>P1[5]</td><td> AIN5, MOSI/bMOSI, P1.5</td></tr>
    <tr><td>P1[6]</td><td> AIN6, MISO/bMISO, P1.6</td></tr>
    <tr><td>P1[7]</td><td> AIN7, SCK/bSCK, P1.7</td></tr>
    <tr><td>P2[0]</td><td> A8, P2.0</td></tr>
    <tr><td>P2[1]</td><td> MOSI1/bMOSI1, A9, P2.1</td></tr>
    <tr><td>P2[2]</td><td> MISO1/bMISO1, A10, P2.2</td></tr>
    <tr><td>P2[3]</td><td> SCK1/bSCK1, A11, P2.3</td></tr>
    <tr><td>P2[4]</td><td> PWM1/bPWM1, A12, P2.4</td></tr>
    <tr><td>P2[5]</td><td> TNOW/bTNOW, PWM2/bPWM2, A13, T2EX_/bT2EX_, CAP2_/bCAP2_, P2.5</td></tr>
    <tr><td>P2[6]</td><td> RXD1/bRXD1, A14, P2.6</td></tr>
    <tr><td>P2[7]</td><td> TXD1/bTXD1, DA7/bDA7, A15, P2.7</td></tr>
    <tr><td>P3[0]</td><td> RXD/bRXD, P3.0</td></tr>
    <tr><td>P3[1]</td><td> TXD/bTXD, P3.1</td></tr>
    <tr><td>P3[2]</td><td> LED0/bLED0, INT0/bINT0, P3.2</td></tr>
    <tr><td>P3[3]</td><td> LED1/bLED1, !A15, INT1/bINT1, P3.3</td></tr>
    <tr><td>P3[4]</td><td> LEDC/bLEDC, XCS0/bXCS0, T0/bT0, P3.4</td></tr>
    <tr><td>P3[5]</td><td> DA6/bDA6, T1/bT1, P3.5</td></tr>
    <tr><td>P3[6]</td><td> WR/bWR, P3.6</td></tr>
    <tr><td>P3[7]</td><td> RD/bRD, P3.7</td></tr>
    <tr><td>P4[0]</td><td> LED2/bLED2, A0, RXD1_/bRXD1_, P4.0</td></tr>
    <tr><td>P4[1]</td><td> A1, P4.1</td></tr>
    <tr><td>P4[2]</td><td> PWM3_/bPWM3_, CAP3_/bCAP3_, A2, P4.2</td></tr>
    <tr><td>P4[3]</td><td> PWM1_/bPWM1_, A3, P4.3</td></tr>
    <tr><td>P4[4]</td><td> LED3/bLED3, TNOW_/bTNOW_, TXD1_/bTXD1_, A4, P4.4</td></tr>
    <tr><td>P4[5]</td><td> PWM2_/bPWM2_, A5, P4.5</td></tr>
    <tr><td>P4[6]</td><td> XI, SCS_/bSCS_, P4.6</td></tr>
    <tr><td>P4[7]</td><td> XO, SCK_/bSCK_, P4.7</td></tr>
    <tr><td>P5[0]</td><td> DM/bDM, P5.0</td></tr>
    <tr><td>P5[1]</td><td> DP/bDP, P5.1</td></tr>
    <tr><td>P5[4]</td><td> HM/bHM, ALE, XB, P5.4</td></tr>
    <tr><td>P5[5]</td><td> HP/bHP, !A15, XA, P5.5</td></tr>
    <tr><td>P5[7]</td><td> RST/bRST, P5.7</td></tr>
    
</table>

The priority order from left to right described in the above table refers to the priority order when multiple functional modules compete to use the GPIO. For example, the P2 port has been set to the upper 8 bits of the output bus address. If only the A8~A11 addresses are actually used, then P2.4/P2.5 can still be used for higher priority PWM1/PWM2 functions, P2.6 It can still be used for RXD1 function, P2.7 can still be used for higher priority TXD1 or DA7 functions, so as to avoid wasting P2.4~P2.7 pins when A12~A15 address is not used.