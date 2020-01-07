---
title: 15. ADC
type: docs
weight: 15
BookToC: false
---

# 15. Analog-to-digital converter ADC

## 15.1 ADC Introduction

The CH559 chip provides 10-bit or 11 optional successive approximation analog-to-digital converters. The converter has 8 analog signal input channels, which can be time-sharing acquisition.

ADC main features:

1. Select 10-bit or 11-bit resolution.
2. ADC analog input voltage range: 0 to VDD33.
3. Maximum 1MSPS sampling rate.
4. Supports automatic alternate channel mode for automatic alternate conversion between two input channels.
5. Built-in 2-level FIFO, supporting automatic sampling and DMA.

## 15.2 ADC Register

<div>
    <p align="center">Table 15.2.1 ADC related register list</p>
</div>

<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>ADC_EX_SW  </td><td>F7h</td><td> ADC extended analog switch control register</td><td> 00h</td></tr>
    <tr><td>ADC_SETUP  </td><td>F6h</td><td> ADC setting register</td><td> 08h</td></tr>
    <tr><td>ADC_FIFO_H </td><td>F5h</td><td> FIFO high byte of ADC (read only)</td><td> 0xh</td></tr>
    <tr><td>ADC_FIFO_L </td><td>F4h</td><td> FIFO low byte of ADC (read only)</td><td> xxh</td></tr>
    <tr><td>ADC_FIFO   </td><td>F4h</td><td> ADC_FIFO_L and ADC_FIFO_H form a 16-bit SFR</td><td> 0xxxh</td></tr>
    <tr><td>ADC_CHANN  </td><td>F3h</td><td> ADC channel selection register</td><td> 00h</td></tr>
    <tr><td>ADC_CTRL   </td><td>F2h</td><td> ADC control register</td><td> 00h</td></tr>
    <tr><td>ADC_STAT   </td><td>F1h</td><td> ADC status register</td><td> 04h</td></tr>
    <tr><td>ADC_CK_SE  </td><td>EFh</td><td> ADC clock division setting register</td><td> 10h</td></tr>
    <tr><td>ADC_DMA_CN </td><td>EEh</td><td> DMA Remaining Count Register</td><td> 00h</td></tr>
    <tr><td>ADC_DMA_AH </td><td>EDh</td><td> DMA current buffer address high byte</td><td> 0xh</td></tr>
    <tr><td>ADC_DMA_AL </td><td>ECh</td><td> DMA current buffer address low byte</td><td> xxh</td></tr>
    <tr><td>ADC_DMA    </td><td>ECh</td><td> ADC_DMA_AL and ADC_DMA_AH form a 16-bit SFR</td><td> 0xxxh</td></tr>
    
</table>

### DMA current buffer address (ADC_DMA):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ADC_DMA_AH</td><td>RW</td><td>The current high byte of the DMA address, which can be preset to the initial value. It automatically increases after DMA. Only the lower 4 bits are valid. The upper 4 bits are fixed at 0. Only the first 4K of xRAM are supported.</td><td>0xh</td></tr>
    <tr><td>[7:0]</td><td>ADC_DMA_AL</td><td>RW</td><td>Low byte of the current DMA address, which can be preset to the initial value. It is automatically increased after DMA. Only the upper 7 bits are valid. The lowest bit is fixed at 0. Only even addresses are supported.</td><td>xxh</td></tr>
    
</table>

### DMA Remaining Count Register (ADC_DMA_CN):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ADC_DMA_CN</td><td>RW</td><td>The current remaining DMA count, which can be preset to the initial value, and is automatically reduced after the DMA operation</td><td>00h</td></tr>
    
</table>

### Clock divider setting register (ADC_CK_SE):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td>bADC_CHK_CLK_SEL</td><td>RW</td><td>AIN7 level detection delay clock frequency selection, if this bit is 0, low speed 1x clock frequency. If this bit is 1, high speed 4x clock frequency</td><td>0</td></tr>
    <tr><td>[6:0]</td><td>MASK_ADC_CK_SE</td><td>RW</td><td>ADC clock division factor to set the internal ADC working clock</td><td>10h</td></tr>
    
</table>

### ADC status register (ADC_STAT):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>bADC_IF_DMA_END  </td><td>RW</td><td> DMA complete interrupt flag. A 1 in this bit indicates an interrupt; a 0 in this bit indicates no interrupt. Cleared on write 1 or cleared on ADC_DMA_CN</td><td>0</td></tr>
    <tr><td>6 </td><td>bADC_IF_FIFO_OV  </td><td>RW</td><td> This bit is 1 to indicate FIFO overflow interrupt; if this bit is 0, there is no interrupt. Write 1 clear</td><td>0</td></tr>
    <tr><td>5 </td><td>bADC_IF_AIN7_LOW </td><td>RW</td><td> This bit is 1 to indicate that the AIN7 low-level interrupt was detected.</td><td>0</td></tr>
    <tr><td>4 </td><td>bADC_IF_ACT      </td><td>RW</td><td> This bit is 1 to indicate an ADC conversion completion interrupt. Write 1 to clear it.</td><td>0</td></tr>
    <tr><td>3 </td><td>bADC_AIN7_INT    </td><td>R0</td><td> This bit is 1 to indicate the delay state of the AIN7 input low level</td><td>0</td></tr>
    <tr><td>2 </td><td>bADC_CHANN_ID    </td><td>R0</td><td> is the current channel identification flag in the automatic alternate channel mode. 0 means AIN0 or AIN6. 1 means AIN1 or AIN4 or AIN7</td><td>0</td></tr>
    <tr><td>2 </td><td>bADC_DATA_OK     </td><td>RO</td><td> In the manual channel selection mode, the ADC conversion is completed and the result is ready. A 1 indicates that the ADC data is ready and the ADC converter is idle. A 0 indicates that the ADC is in progress and the data is not ready.</td><td>1</td></tr>
    <tr><td>[1:0]</td><td> MASK_ADC_FIFO_CNT</td><td> R0 </td><td>ADC FIFO current count</td><td>00b</td></tr>
    
</table>

MASK_ADC_FIFO_CNT consists of bADC_FIFO_CNT1 and bADC_FIFO_CNT0, which is used to display the ADC's FIFO count.

<table>
    <tr>
        <th>MASK_ADC_FIFO_CNT</th><th>Description</th>
    </tr>
    <tr><td>00b </td><td>FIFO is empty.If the FIFO is read, it will directly return the current ADC result value.</td></tr>
    <tr><td>01b </td><td>1 data in FIFO</td></tr>
    <tr><td>10b </td><td>FIFO is full, there are 2 data in FIFO</td></tr>
    <tr><td>11b </td><td>unknown error</td></tr>
    
</table>

### ADC Control Register (ADC_CTRL):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>bADC_SAMPLE     </td><td>RW </td><td>In the manual sampling mode, it is the sampling control bit. Set it first and then clear it to 0 to generate a high-level pulse to start the ADC once. In the automatic sampling mode, it is the sampling pulse state of automatic sampling.</td><td>0</td></tr>
    <tr><td>6 </td><td>bADC_SAMP_WIDTH </td><td>RW </td><td>Sampling pulse width control bit in auto sampling mode, 0 for 1 ADC clock width; 1 for 2 ADC clock width</td><td>0</td></tr>
    <tr><td>5 </td><td>bADC_CHANN_MOD1 </td><td>RW </td><td>ADC channel mode high</td><td>0</td></tr>
    <tr><td>4 </td><td>bADC_CHANN_MOD0 </td><td>RW </td><td>ADC channel mode low</td><td>0</td></tr>
    <tr><td>[3:0] </td><td>MASK_ADC_CYCLE </td><td>RW</td><td> ADC running cycle number, 0 means manual sampling; non-zero value means setting the running cycle of automatic sampling (counted by ADC clock)</td><td>0000b</td></tr>
    
</table>

MASK_ADC_CHANN consisting of bADC_CHANN_MOD1 and bADC_CHANN_MOD0 is the ADC channel control mode flag.

<table>
    <tr>
        <th>MASK_ADC_CHANN</th><th>Description</th>
    </tr>
    <tr><td>00b </td><td>Manually select channel mode, set ADC_CHANN to select current input channel</td></tr>
    <tr><td>01b </td><td>Auto alternate channel mode, switch between AIN0 and AIN1 automatically</td></tr>
    <tr><td>10b </td><td>Auto Alternate Channel Mode, automatically alternate between AIN6 and AIN4</td></tr>
    <tr><td>11b </td><td>Auto Alternate Channel Mode, automatically alternate between AIN6 and AIN7</td></tr>
    
</table>

### ADC channel selection register (ADC_CHANN):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ADC_CHANN</td><td>RW</td><td>Select the current ADC analog input channel, select one from the 8 channels, bits 0~7 correspond to AIN0~AIN7 respectively</td><td>00h</td></tr>
    
</table>

### ADC's FIFO port (ADC_FIFO):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>[7:0]</td><td>ADC_FIFO_H</td><td>RO</td><td>ADC FIFO high byte, only the lower 4 bits are valid, the upper 4 bits are fixed to 0</td><td>0xh</td></tr>
    <tr><td>[7:0]</td><td>ADC_FIFO_L</td><td>RO</td><td>ADC FIFO low byte</td><td>xxh</td></tr>
    
</table>

### ADC setup register (ADC_SETUP):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7</td><td> bADC_DMA_EN      </td><td>RW</td><td> This bit is 1 to enable the DMA and DMA interrupts of the ADC. 0 to disable the enable</td><td>0</td></tr>
    <tr><td>6</td><td> bADC_IE_FIFO_OV  </td><td>RW</td><td> This bit is 1 to enable the FIFO overflow interrupt. This bit is 0 to disable the enable</td><td>0</td></tr>
    <tr><td>5</td><td> bADC_IE_AIN7_LOW </td><td>RW</td><td> This bit is 1 to enable detection of AIN7 low-level interrupt</td><td>0</td></tr>
    <tr><td>4</td><td> bADC_IE_ACT      </td><td>RW</td><td> This bit is 1 to enable the ADC conversion completion interrupt. This bit is 0 to disable the enable</td><td>0</td></tr>
    <tr><td>3</td><td> bADC_CLOCK       </td><td>RO</td><td> Current level of the internal ADC clock signal</td><td>0</td></tr>
    <tr><td>2</td><td> bADC_POWER_EN    </td><td>RW</td><td> ADC power control bit of the sampling conversion module. This bit is 0 to turn off the power of the ADC module and enter the sleep state. A bit of 1 to turn on</td><td>0</td></tr>
    <tr><td>1</td><td> bADC_EXT_SW_EN   </td><td>RW</td><td> Power control bit of the extended analog switch module. This bit is 0 to disable the extended analog switch module. 1 to enable the bit.</td><td>0</td></tr>
    <tr><td>0</td><td> bADC_AIN7_CHK_EN </td><td>RW</td><td> Detect the power control bit of AIN7 low-level module. This bit is 0 to disable detection of AIN7 low-level module. 1 to enable</td><td>0</td></tr>
    
</table>

### ADC extended analog switch control register (ADC_EX_SW):

<table>
    <tr>
        <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
    </tr>
    <tr><td>7 </td><td>bADC_SW_AIN7_H </td><td>RW </td><td>AIN7 channel internal analog switch connection control, this bit is 1 internally connects AIN7 to VDD33. This bit is 0 to disconnect AIN7 from VDD33</td><td>0</td></tr>
    <tr><td>6 </td><td>bADC_SW_AIN6_L </td><td>RW </td><td>AIN6 channel internal analog switch connection control, this bit is 1 internally connects AIN6 to GND. This bit is 0 to disconnect AIN6 from GND</td><td>0</td></tr>
    <tr><td>5 </td><td>bADC_SW_AIN5_H </td><td>RW </td><td>AIN5 channel internal analog switch connection control, this bit is 1 internally connects AIN5 to VDD33. This bit is 0 to disconnect AIN5 and VDD33</td><td>0</td></tr>
    <tr><td>4 </td><td>bADC_SW_AIN4_L </td><td>RW </td><td>AIN4 channel internal analog switch connection control, this bit is 1 internally connects AIN4 to GND. This bit is 0 to disconnect AIN4 from GND</td><td>0</td></tr>
    <tr><td>3 </td><td>bADC_EXT_SW_SEL</td><td> RW</td><td> On-resistance value selection bit of the internal analog switch. This bit is 0 to select high resistance, about 800Ω. This bit is 1 to select low resistance, about 300Ω</td><td>0</td></tr>
    <tr><td>2 </td><td>bADC_RESOLUTION</td><td> RW</td><td> ADC resolution selection bit, this bit is 0 to select 10-bit resolution. This bit is 1 to select 11-bit resolution</td><td>0</td></tr>
    <tr><td>1 </td><td>bADC_AIN7_DLY1 </td><td>RW </td><td>Delay control bit for detecting AIN7 low level 1</td><td>0</td></tr>
    <tr><td>0 </td><td>bADC_AIN7_DLY0 </td><td>RW </td><td>Delay control bit for detecting AIN7 low level 0</td><td>0</td></tr>
    
</table>

bADC_AIN7_DLY1 and bADC_AIN7_DLY0 form MASK_ADC_AIN7_DLY, which is used to select the delay after detecting the level change of AIN7: 00 is no delay, 01 is the longest delay, 10 is the longer delay, and 11 is the shorter delay.

## 15.3 ADC Functions

ADC sampling mode configuration steps:

1. Set the bADC_POWER_EN bit in the ADC setting register ADC_SETUP to 1 to enable the ADC module.
2. Set the clock divider setting register ADC_CK_SE, select the clock frequency, the highest frequency is 12MHz, and it is recommended not to be lower than 1MHz.
3. Clear the existing data in the FIFO. If you need to use interrupt or DMA, then make the relevant settings here.
4. For the automatic sampling mode, the ADC channel selection register ADC_CHANN should be set first.
5. Set bADC_SAMPLE and MASK_ADC_CYCLE in the ADC control register ADC_CTRL.If MASK_ADC_CYCLE is set to 0, it is a manual sampling mode.If MASK_ADC_CYCLE is set to a non-zero value, it is an automatic sampling mode.At this time, MASK_ADC_CYCLE is a continuous and automatic sampling. Clock cycle.
6. For the manual sampling mode, set the ADC channel selection register ADC_CHANN to select the ADC analog signal input channel.
7. If it is in manual sampling mode, you need to set the bADC_SAMPLE bit to 1 and clear it after delaying at least one ADC clock cycle to complete an analog signal sampling and start an ADC conversion.
8. Wait for the bADC_IF_ACT bit in the ADC status register ADC_STAT to be 1, indicating that the ADC conversion is complete, and the result data can be read through the ADC_FIFO.
9. Or read MASK_ADC_FIFO_CNT of ADC status register ADC_STAT to get the FIFO count, and then read some data through ADC_FIFO. It is recommended to discard the first ADC result data, because there may be incomplete sampling.
10.  For DMA steps: Set ADC_DMA as the start address value of the user-defined data buffer, set ADC_DMA_CN as the user-defined DMA remaining count, and set the bADC_DMA_EN bit in ADC_SETUP to 1 to enable the DMA function.
11.  There are 12 bits of ADC result data, among which bits 0 ~ 10 are ADC values, bits 11 are flag bits, bits 12 ~ 15 are always 0. For manual channel selection mode, bit 11 is always 0; for automatic alternate channel mode, bit 11 indicates the channel identification flag of the ADC value, refer to bADC_CHANN_ID description.