---
title: 6. Memory structure
type: docs
weight: 6
BookToC: false
---

# 6. Memory structure

## 6.1 Memory space

CH559 addressing space is divided into program storage space, internal data storage space, and external data storage space.

<div>
    <p align="center">Figure 6.1 Memory Structure</p>
</div>

![Memory_structure](/docs/6-Memory_Structure/images/mem_struct.png "Memory structure")

## 6.2 Program Storage Space

The program memory space is 64KB. As shown in Figure 6.1, it is all used for flash-ROM, including Code Flash area for saving instruction code, Data Flash area for saving non-volatile data, and Configuration Information area for configuration information.

The Data Flash address range is F000h to F3FFH. It supports single-byte read (8-bit), double-byte write (16-bit), and block erase (1K byte) operations. The data remains unchanged after the chip is powered off. Make Code Flash.

Code Flash includes application code for low address areas and bootstrap code for high address areas. You can also combine these two areas and Data Flash to hold a single application code.

Configuration Information Configuration Information A total of 16 bits of data, set by the programmer as needed, refer to Table 6.1.

<div>
    <p align="center">Table 6.2 Description of flash-ROM configuration information</p>
</div>

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>15</td><td>Code_Protect</td><td>Code and data protection mode in flash-ROM:<br> 0-Prohibit the programmer to read, the program is confidential; 1-Allow readout</td><td>0/1</td></tr>
    <tr><td>14</td><td>No_Boot_Load</td><td>Enable BootLoader boot code startup mode:<br>0-started from the 0000h address application.<br>1-start from the bootloader of the F400h address</td><td>1</td></tr>
    <tr><td>13</td><td>En_Long_Reset</td><td>Enable additional delay reset during power-on reset:<br>0-standard short reset; 1-wide reset, additional 87mS reset time</td><td>0</td></tr>
    <tr><td>12</td><td>XT_OSC_Strong</td><td>Select the external drive capability of the crystal oscillator: 0-standard; 1-enhanced</td><td>0</td></tr>
    <tr><td>11</td><td>En_P5.7_RESET</td><td>Enable P5.7 as a manual reset input pin: 0-disable; 1-enable RST</td><td>1</td></tr>
    <tr><td>10</td><td>En_P0_Pullup</td><td>Enable internal pull-up resistor for port P0 during system reset:<br>0-Pull-up resistor disabled after reset.<br>1-Pull-up resistor enabled after reset</td><td>1</td></tr>
    <tr><td>9</td><td>Must_1</td><td>(Automatically set to 1 by the programmer as needed)</td><td>1</td></tr>
    <tr><td>8</td><td>Must_0</td><td>(automatically set to 0 by the programmer as needed)</td><td>0</td></tr>
    <tr><td>[7:0]</td><td>All_1</td><td>(Automatically set to FFh by the programmer as needed)</td><td>FFh</td></tr>
</table>

## 6.3 Data Storage Space

The internal data storage space is 256 bytes in total. As shown in Figure 6.1, it is used for SFR and iRAM. iRAM is used for stack and fast data temporary storage. It can be subdivided into working registers R0-R7, bit variables bdata, bytes. Variable data, idata, etc.

The external data storage space is 64KB in total, as shown in Figure 6.1, and partially used for 6KB on-chip expansion of xRAM and xSFR. Except for the two reserved areas, the remaining 4000h to FFFFh address range is the external bus area.

## 6.4 flash-ROM register

<div>
    <p align="center">Table 6.4 List of flash-ROM operation registers</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>ROM_DATA_H</td><td>8Fh</td><td>flash-ROM data register high byte</td><td>xxh</td></tr>
    <tr><td>ROM_DATA_L</td><td>8Eh</td><td>flash-ROM data register low byte</td><td>xxh</td></tr>
    <tr><td>ROM_DATA</td><td>8Eh</td><td>ROM_DATA_L and ROM_DATA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>ROM_STATUS</td><td>86h</td><td>flash-ROM status register (read only)</td><td>80h</td></tr>
    <tr><td>ROM_CTRL</td><td>86h</td><td>flash-ROM control register (write only)</td><td>00h</td></tr>
    <tr><td>ROM_ADDR_H</td><td>85h</td><td>flash-ROM address register high byte</td><td>xxh</td></tr>
    <tr><td>ROM_ADDR_L</td><td>84h</td><td>flash-ROM address register low byte</td><td>xxh</td></tr>
    <tr><td>ROM_ADDR</td><td>84h</td><td>ROM_ADDR_L and ROM_ADDR_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    
</table>

### flash-ROM address register (ROM_ADDR):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ROM_ADDR_H</td><td>RW</td><td>Flash-ROM address high byte</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>ROM_ADDR_L</td><td>RW</td><td>Flash-ROM address low byte, only supports even address</td><td>xxh</td></tr>
</table>

### flash-ROM data register (ROM_DATA):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ROM_DATA_H</td><td>RW</td><td>flash-ROM data to be written high byte</td><td>xxh</td></tr>
    <tr><td>[7:0]</td><td>ROM_DATA_L</td><td>RW</td><td>flash-ROM data to be written low byte</td><td>xxh</td></tr>
</table>

### flash-ROM control register (ROM_CTRL):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ROM_CTRL</td><td>W0</td><td>flash-ROM control register</td><td>00h</td></tr>
</table>

### flash-ROM status register (ROM_STATUS):
<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>Reserved</td><td>R0</td><td>Reserved</td><td>1</td></tr>
    <tr><td>6</td><td>bROM_ADDR_OK</td><td>R0</td><td>flash-ROM operation address valid status bit:<br>If the bit is 0, the parameter is invalid; 1 means the address is valid.</td><td>0</td></tr>
    <tr><td>[5:2]</td><td>Reserved</td><td>R0</td><td>Reserved</td><td>0000b</td></tr>
    <tr><td>1</td><td>bROM_CMD_ERR</td><td>R0</td><td>flash-ROM operation command error status bit:<br>A bit of 0 indicates that the command is valid. A value of 1 indicates an unknown command.</td><td>0</td></tr>
    <tr><td>0</td><td>bROM_CMD_TOUT</td><td>R0</td><td>flash-ROM operation result status bit:<br>A bit of 0 indicates that the operation was successful. A value of 1 indicates that the operation timed out.</td><td>0</td></tr>
</table>

## 6.5 flash-ROM operation steps

1. Erase the flash-ROM and change all the data bits in the target block to 1:
    1. enable the security mode, SAFE_MOD = 55h; SAFE_MOD = 0AAh;
    2. set the global configuration register GLOBAL_CFG to enable write enable (bCODE_WE or bDATA_WE corresponds to code or data);
    3. set the address register ROM_ADDR, write 16-bit target address, the actual only high 6 bits are valid;
    4. Set the operation control register ROM_CTRL to 0A6h to perform the block erase operation, and the program will automatically pause during the operation;
    5. After the operation is completed, the program resumes operation. At this time, the status register ROM_STATUS can be queried to check the operation status; if multiple blocks are to be erased, the steps (3), (4), and (5) are repeated;
    6. enter safe mode again, SAFE_MOD = 55h; SAFE_MOD = 0AAh;
    7. Set the global configuration register GLOBAL_CFG to enable write protection (bCODE_WE=0, bDATA_WE=0).

2. Write flash-ROM to change part of the data bits in the target double byte from 1 to 0:
    1. enable the security mode, SAFE_MOD = 55h; SAFE_MOD = 0AAh;
    2. set the global configuration register GLOBAL_CFG to enable write enable (bCODE_WE or bDATA_WE corresponds to code or data);
    3. set the address register ROM_ADDR, write 16-bit target address, the actual only 15 high;
    4. set the data register ROM_DATA, write 16 bits of data to be written, steps (3), (4) can be reversed;
    5. Set the operation control register ROM_CTRL to 09Ah to perform the write operation, and the program will automatically suspend the operation during the operation;
    6. After the operation is completed, the program resumes operation. At this time, the status register ROM_STATUS can be queried to check the operation status; if multiple data is to be written, the steps (3), (4), (5), and (6) are repeated;
    7. enter safe mode again, SAFE_MOD = 55h; SAFE_MOD = 0AAh;
    8. Set the global configuration register GLOBAL_CFG to enable write protection (bCODE_WE=0, bDATA_WE=0).

3. read flash-ROM:
    1. Read the code or data of the target address directly using the MOVC instruction or by a pointer to the program memory space.

## 6.6 On-board programming and ISP downloads

When the configuration information Code_Protect=1, the code and data in the CH559 chip flash-ROM can be read and written by the external programmer through the synchronous serial interface; when the configuration information Code_Protect=0, the code and data in the flash-ROM are protected. Can not be read, but can be erased, and re-powered after erasing to release code protection.

When the CH559 chip is pre-installed with the BootLoader bootloader, the CH559 can support multiple ISP download methods such as USB or asynchronous serial port to load the application; but without the bootloader, the CH559 can only be written to the bootloader by an external dedicated programmer. Or an application. In order to support on-board programming, five connection pins between CH559 and the programmer need to be reserved in the circuit.

<div>
    <p align="center">Table 6.6.1 Connection Pins to the Programmer</p>
</div>

<table>
    <tr>
        <th>Pin</th><th>GPIO</th><th>Pin description</th>
    </tr>
    <tr><td>RST</td><td>P5.7</td><td>Reset control pin in programming state, high level allows entry into programming state</td></tr>
    <tr><td>SCS</td><td>P1.4</td><td>Chip select input pin in programming state, default high level, active low</td></tr>
    <tr><td>SCK</td><td>P1.7</td><td>Clock input pin in programming state</td></tr>
    <tr><td>MOSI</td><td>P1.5</td><td>Data input pin in programming state</td></tr>
    <tr><td>MISO</td><td>P1.6</td><td>Data output pin in programming state</td></tr>
</table>

## 6.7 chip unique ID number

Each microcontroller is shipped with a unique ID number, the chip identification number. The ID data and its checksum are 8 bytes in total, and are stored in the area where the offset address of the dedicated read-only memory is 20h. It can be obtained by reading Code Flash similarly during the period when E_DIS is 1 to close the global interrupt. Please refer to the C language example program GETID.C for operation.

<div>
    <p align="center">Table 6.7.1 Chip ID Address Table</p>
</div>

<table>
    <tr>
        <th>Offset address</th><th>ID data description</th>
    </tr>
    <tr><td>20h, 21h</td><td>ID first word data, followed by the lowest byte and the next lowest byte of the ID number</td></tr>
    <tr><td>22h, 23h</td><td>ID second word data, followed by the next highest byte of the ID number, high byte</td></tr>
    <tr><td>24h, 25h</td><td>ID last word data, followed by the next highest byte and highest byte of the 48-bit ID number</td></tr>
    <tr><td>26h, 27h</td><td>16-bit cumulative sum of ID first word, second word, last word data, used for ID verification</td></tr>
</table>

The ID number can be used with the download tool to encrypt the target program. For general applications, just use the first 32 digits of the ID number.