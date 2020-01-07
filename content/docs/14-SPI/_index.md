---
title: 14. SPI
type: docs
weight: 14
BookToC: false
---

# 14. Synchronous Serial Interface SPI

## 14.1 Introduction to SPI

CH559 chip provides 2 SPI interfaces for high-speed synchronous data transmission with peripherals.

SPI0 features:

1. Support master mode and slave mode.
2. Support mode 0 and mode 3 clock mode.
3. Optional 3-wire full duplex or 2-wire half duplex mode.
4. Optional MSB high bit is transmitted first or LSB low bit is transmitted first.
5. The clock frequency is adjustable, up to half of the system main frequency.
6. Built-in 3-byte receive FIFO and 1-byte transmit FIFO.
7. Support multiple interrupts.

SPI1 features:

1. Only support master mode, MSB high bit is sent first.
2. Support mode 0 and mode 3 clock mode.
3. Optional 3-wire full duplex or 2-wire half duplex mode.
4. The clock frequency is adjustable, up to half of the system main frequency.

## 14.2 SPI Register

<div>
    <p align="center">Table 14.2.1 List of SPI Related Registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>SPI0_SETUP</td><td> FCh</td><td> SPI0 setup register</td><td> 00h</td></tr>
    <tr><td>SPI0_S_PRE</td><td> FBh</td><td> SPI0 Slave Mode Preset Data Register</td><td> 20h</td></tr>
    <tr><td>SPI0_CK_SE</td><td> FBh</td><td> SPI0 Clock Divider Setting Register</td><td> 20h</td></tr>
    <tr><td>SPI0_CTRL </td><td>FAh </td><td>SPI0 Control Register</td><td> 02h</td></tr>
    <tr><td>SPI0_DATA </td><td>F9h </td><td>SPI0 data transmit / receive register</td><td> xxh</td></tr>
    <tr><td>SPI0_STAT </td><td>F8h </td><td>SPI0 status register</td><td> 08h</td></tr>
    <tr><td>SPI1_CK_SE</td><td> B7h</td><td> SPI1 clock divider setting register</td><td> 20h</td></tr>
    <tr><td>SPI1_CTRL </td><td>B6h </td><td>SPI1 control register</td><td> 02h</td></tr>
    <tr><td>SPI1_DATA </td><td>B5h </td><td>SPI1 data transmission and reception register</td><td> xxh</td></tr>
    <tr><td>SPI1_STAT </td><td>B4h </td><td>SPI1 status register</td><td> 08h</td></tr>
</table>

### 14.2.1 SPI0 Related Registers

### SPI0 setup register (SPI0_SETUP):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bS0_MODE_SLV</td><td>RW</td><td>SPI0 master-slave mode selection bit, if this bit is 0, SPI0 is master mode. If this bit is 1, SPI0 is slave mode / device mode</td><td>0</td></tr>
    <tr><td>6</td><td>bS0_IE_FIFO_OV</td><td>RW</td><td>FIFO overflow interrupt enable bit in slave mode, this bit is 1 to enable FIFO overflow interrupt. If this bit is 0, FIFO overflow does not generate interrupt</td><td>0</td></tr>
    <tr><td>5</td><td>bS0_IE_FIRST</td><td>RW</td><td>Receive the first byte to complete the interrupt enable bit in slave mode. When this bit is 1, the interrupt is triggered when the first data byte is received in slave mode. When the bit is 0, no interrupt is generated when the first byte is received.</td><td>0</td></tr>
    <tr><td>4</td><td>bS0_IE_BYTE</td><td>RW</td><td>Data byte transfer completion interrupt enable bit, this bit is 1 to enable byte transfer completion interrupt. When this bit is 0, byte transfer completion does not generate an interrupt</td><td>0</td></tr>
    <tr><td>3</td><td>bS0_BIT_ORDER</td><td>RW</td><td>Bit order control bit of the data byte. If this bit is 0, the MSB high-order bit comes first. If this bit is 1, the LSB low-order bit comes first.</td><td>0</td></tr>
    <tr><td>2</td><td>reserved</td><td>R0</td><td>reserved</td><td>0</td></tr>
    <tr><td>1</td><td>bS0_SLV_SELT</td><td>R0</td><td>Chip select active status bit in slave mode, this bit is 0 if it is not currently selected. This bit is 1 if it is currently selected</td><td>0</td></tr>
    <tr><td>0</td><td>bS0_SLV_PRELOAD</td><td>R0</td><td>Preload data status bit in slave mode, this bit is 1 to indicate that it is currently in the preload status after the chip select is valid but before the data has been transferred</td><td>0</td></tr>
    
    
</table>

### SPI0 clock divider setting register (SPI0_CK_SE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SPI0_CK_SE</td><td>RW</td><td>Setting the SPI0 Clock Divider in Master Mode</td><td>20h</td></tr>
    
</table>

### SPI0 Slave Mode Preset Data Register (SPI0_S_PRE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SPI0_S_PRE</td><td>RW</td><td>Preload the first transfer data in slave mode</td><td>20h</td></tr>
    
</table>

### SPI0 control register (SPI0_CTRL):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td> bS0_MISO_OE </td><td>RW </td><td>MISO output enable control bit for SPI0, this bit is 1 to enable output. This bit is 0 to disable output</td><td> 0 </td></tr>
    <tr><td>6</td><td> bS0_MOSI_OE </td><td>RW </td><td>SPI0 MOSI output enable control bit, this bit is 1 to enable output. This bit is 0 to disable output</td><td> 0 </td></tr>
    <tr><td>5</td><td> bS0_SCK_OE  </td><td>RW </td><td>SCK output enable control bit for SPI0, this bit is 1 to enable output. This bit is 0 to disable output</td><td> 0 </td></tr>
    <tr><td>4</td><td> bS0_DATA_DIR</td><td> RW</td><td> SPI0 data direction control bit. This bit is 0 to output data. Only write FIFO is used as a valid operation to start an SPI transmission. This bit is 1 to input data and write or read FIFO are both a valid operation to start a SPI transmission</td><td> 0 </td></tr>
    <tr><td>3</td><td> bS0_MST_CLK </td><td>RW </td><td>SPI0 Master clock mode control bit, this bit is 0 for mode 0, the default low level when SCK is idle. 1 for mode 3, SCK default high level</td><td> 0 </td></tr>
    <tr><td>2</td><td> bS0_2_WIRE  </td><td>RW </td><td>SPI0 2-wire half-duplex mode enable bit. If this bit is 0, 3-wire full-duplex mode, including SCK, MOSI, and MISO. This bit is 1 2-wire half-duplex mode, including SCK, MISO.</td><td> 0 </td></tr>
    <tr><td>1</td><td> bS0_CLR_ALL </td><td>RW </td><td>This bit is 1 to clear the SPI0 interrupt flag and FIFO, which needs to be cleared by software</td><td> 1 </td></tr>
    <tr><td>0</td><td> bS0_AUTO_IF </td><td>RW </td><td>Enable bit for automatically clearing the byte reception completion interrupt flag through a valid FIFO operation. When this bit is 1, the byte reception completion interrupt flag S0_IF_BYTE is automatically cleared during a valid FIFO read and write operation.</td><td> 0 </td></tr>
    
</table>

### SPI0 data transmit and receive register (SPI0_DATA):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SPI0_DATA</td><td>RW</td><td>Including sending and receiving two physically separate FIFOs, read operations correspond to receive data FIFOs, write operations correspond to send data FIFOs, and valid read and write operations can initiate an SPI transmission</td><td>xxh</td></tr>
    
</table>

### SPI0 status register (SPI0_STAT):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th colspan="2">Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td> S0_FST_ACT </td><td>R0 </td><td colspan="2">This bit is 1 to indicate that the current status is completed when the first byte is received in slave mode </td><td> 0 </td></tr>
    <tr><td>6</td><td> S0_IF_OV   </td><td>RW </td><td colspan="2">FIFO overflow flag in slave mode. A 1 in this bit indicates a FIFO overflow interrupt. A 0 in this bit indicates no interrupt. Clear by direct bit access or write 1 to clear. When bS0_DATA_DIR = 0, the interrupt is triggered by the sending FIFO empty. When bS0_DATA_DIR = 1, the interrupt is triggered by the receiving FIFO full.</td><td> 0 </td></tr>
    <tr><td>5</td><td> S0_IF_FIRST</td><td> RW</td><td colspan="2"> Receives the first byte completion interrupt flag bit in slave mode. A 1 in this bit indicates that the first byte was received. Clear by direct bit access or write 1 Clear</td><td> 0 </td></tr>
    <tr><td>4</td><td> S0_IF_BYTE </td><td>RW </td><td colspan="2">Data byte transfer complete interrupt flag. When this bit is 1, it indicates that one byte transfer is completed. Direct bit access Clear or write 1 clear, or clear by FIFO valid operation when bS0_AUTO_IF = 1</td><td> 0 </td></tr>
    <tr><td>3</td><td> S0_FREE    </td><td>R0 </td><td colspan="2">SPI0 idle flag bit, this bit is 1 means there is no SPI shift at present, usually in the gap period between data bytes</td><td> 1 </td></tr>
    <tr><td>2</td><td> S0_T_FIFO  </td><td>R0 </td><td colspan="2">SPI0 transmit FIFO count, valid value is 0 or 1</td><td> 0 </td></tr>
    <tr><td>1</td><td> S0_R_FIFO1 </td><td>R0 </td><td>SPI0 receive FIFO count bit 1</td><td>Valid values are 0 or 1 or 2 or 3</td><td> 0 </td></tr>
    <tr><td>0</td><td> S0_R_FIFO0 </td><td>R0 </td><td>SPI0 receive FIFO count bit 0</td><td></td><td> 0 </td></tr>
    
</table>

### 14.2.2 SPI1 Register Description

### SPI1 status register (SPI1_STAT):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:5]</td><td>reserved</td><td>RO</td><td>reserved</td><td>000b</td></tr>
    <tr><td>4</td><td>bS1_IF_BYTE</td><td>RO</td><td>Data byte transfer completed interrupt flag bit. When this bit is 1, it indicates that a byte transfer is completed. Direct bit access Cleared or written 1 cleared, or cleared by valid FIFO operation when bS1_AUTO_IF = 1</td><td>0</td></tr>
    <tr><td>3</td><td>bS1_FREE</td><td>RW</td><td>SPI1 idle flag bit, this bit is 1 means there is no SPI shift at present, usually in the gap period between data bytes</td><td>1</td></tr>
    <tr><td>[2:0]</td><td>reserved</td><td>RO</td><td>reserved</td><td>000b</td></tr>
    
</table>

### SPI1 data transmit and receive register (SPI1_DATA):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SPI1_DATA</td><td>RW</td><td>It is actually an SPI data shift register. Read is used to receive data and write is used to send data. Effective read and write operations can start an SPI transfer.</td><td>xxh</td></tr>
    
</table>

### SPI1 control register (SPI1_CTRL):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>
    <tr><td>7</td><td> bS1_MISO_OE </td><td>RW </td><td>MISO1 output enable control bit for SPI1, this bit is 1 to enable output. This bit is 0 to disable output</td><td>0</td></tr>
    <tr><td>6</td><td> Reserved    </td><td>RO </td><td>reserved</td><td>0</td></tr>
    <tr><td>5</td><td> bS1_SCK_OE  </td><td>RW </td><td>SCK1 output enable control bit for SPI1, this bit is 1 to enable SCK1 output, if bS1_2_WIRE = 0, then MOSI1 output enable will be enabled at the same time. This bit is 0 to disable output</td><td>0</td></tr>
    <tr><td>4</td><td> bS1_DATA_DIR</td><td> RW</td><td> SPI1 data direction control bit. This bit is 0 to output data. Only write SPI1_DATA as a valid operation to start an SPI transmission. This bit is 1 to input data. Writing or reading SPI1_DATA is a valid operation to start a SPI. transmission</td><td>0</td></tr>
    <tr><td>3</td><td> bS1_MST_CLK </td><td>RW </td><td>SPI1 clock mode control bit, this bit is 0 for mode 0, SCK1 defaults to low level when idle. This bit is 1 for mode 3, SCK1 defaults to high level</td><td>0</td></tr>
    <tr><td>2</td><td> bS1_2_WIRE  </td><td>RW </td><td>SPI1 2-wire half-duplex mode enable bit, this bit is 0, then 3-wire full duplex mode, including SCK1, MOSI1, MISO1. This bit is 1, then 2-wire half-duplex mode, including SCK1, MISO1</td><td>0</td></tr>
    <tr><td>1</td><td> bS1_CLR_ALL </td><td>RW </td><td>This bit is 1 to clear the SPI1 interrupt flag and FIFO, which needs to be cleared by software</td><td>1</td></tr>
    <tr><td>0</td><td> bS1_AUTO_IF </td><td>RW </td><td>Enable bit for automatically clearing the byte reception completion interrupt flag through the SPI1_DATA valid operation. When this bit is 1, it automatically clears the byte reception completion interrupt flag bS1_IF_BYTE when the SPI1_DATA is valid</td><td>0</td></tr>
    
</table>

### SPI1 clock divider setting register (SPI1_CK_SE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>SPI1_CK_SE</td><td>RW</td><td>Set the SPI1 clock division factor</td><td>20h</td></tr>
    
</table>

## 14.3 SPI Transmission Format

The SPI master mode supports two transmission modes, Mode 0 and Mode 3. You can select by setting bit bSn_MST_CLK in the SPI control register SPIn_CTRL. CH559 always samples MISO data on the rising edge of CLK. The data transmission format is shown in the figure below.

Mode 0: bSn_MST_CLK = 0

![spi_mode_0_tim](/docs/14-SPI/images/spi_mode_0_tim.png "SPI Mode 0 Timing Diagram")

<div>
    <p align="center">Figure 14.3.1 SPI Mode 0 Timing Diagram</p>
</div>

Mode 3: bSn_MST_CLK = 1

![spi_mode_3_tim](/docs/14-SPI/images/spi_mode_3_tim.png "SPI Mode 3 Timing Diagram")

<div>
    <p align="center">Figure 14.3.2 SPI Mode 3 Timing Diagram</p>
</div>

## 14.4 SPI Configuration

### 14.4.1 SPI Master Mode Configuration

In SPI master mode, the SCK pin outputs a serial clock, and the chip select output pin can be designated as any I/O pin.

SPI0 configuration steps:

1. Set the SPI clock frequency division setting register SPI0_CK_SE to configure the SPI clock frequency.
2. Set bit bS0_MODE_SLV of the SPI setting register SPI0_SETUP to 0, and configure it as the master mode.
3. Set the bit bS0_MST_CLK of the SPI control register SPI0_CTRL and set it to mode 0 or 3 as required.
4. Set the bS0_SCK_OE and bS0_MOSI_OE bits of the SPI control register SPI0_CTRL to 1, and set the bS0_MISO_OE bit to 0, set the direction of P1 port bSCK, bMOSI as the output, bMISO as the input, and the chip select pin as the output.

Data sending process:

1. Write the SPI0_DATA register, write the data to be sent to the FIFO, and automatically start an SPI transfer.
2. Wait for S0_FREE to be 1, indicating that the transmission is complete, and can continue to send the next byte.

Data receiving process:

1. Write the SPI0_DATA register and write any data such as 0FFh to the FIFO to start an SPI transfer.
2. Wait for S0_FREE to be 1, indicating that the reception is complete. You can read SPI0_DATA to get the received data.
3. If the previous bS0_DATA_DIR is set to 1, the above read operation will also start the next SPI transmission, otherwise it will not start.

### 14.4.2 SPI Slave Mode Configuration

Only SPI0 supports slave mode. In slave mode, the SCK pin is used to receive the serial clock of the connected SPI master.

1. Set bit bS0_MODE_SLV of SPI0 setting register SPI0_SETUP to 1, and configure it as slave mode.
2. Set the bits bS0_SCK_OE and bS0_MOSI_OE of the SPI0 control register SPI0_CTRL to 0, set bS0_MISO_OE to 1, set the direction of P1 port bSCK, bMOSI and bMISO, and chip select pins as inputs. When the SCS chip select is active (low level), MISO will automatically enable the output. At the same time, it is recommended to set the MISO pin to high-impedance input mode (bP1_OC = 0, P1_DIR [6] = 0, P1_PU [6] = 0), so that MISO does not output during chip select invalidation, which facilitates sharing the SPI bus.
3. Optional, set the SPI slave mode preset data register SPI0_S_PRE, which is used to automatically load into the buffer for external output after being selected by the chip for the first time. After 8 serial clocks, that is, the first data byte transfer is completed, CH559 gets the first byte data (possibly a command code) from the external SPI host, and the external SPI host exchanges the preset data in SPI0_S_PRE (possibly Is a status value). Bit 7 of the register SPI0_S_PRE will be automatically loaded on the MISO pin during the low period of SCK after the SPI chip select is active. For SPI mode 0, if CH559 is preset with bit 7 of SPI0_S_PRE, then the external SPI host will select the SPI chip select When the data is valid but not yet transmitted, the preset value of bit 7 of SPI0_S_PRE can be obtained by querying the MISO pin, so that the value of bit 7 of SPI0_S_PRE can be obtained only by validating the SPI chip select.

### Data sending process:

Query S0_IF_BYTE or wait for an interrupt. After each SPI data byte transfer is completed, write to the SPI0_DATA register and write the data to be sent to the FIFO. Or wait for S0_FREE to change from 0 to 1 to continue sending the next byte.

### Data receiving process:

Query S0_IF_BYTE or wait for an interrupt. After each SPI data byte transfer is completed, read the SPI0_DATA register to get the received data from the FIFO. Query MASK_S0_RFIFO_CNT (that is, S0_R_FIFO1 and S0_R_FIFO0) to get the number of bytes remaining in the FIFO.