---
title: 16. USB
type: docs
weight: 16
BookToC: false
---

# 16.USB Controller

## 16.1 Introduction to USB Controller

The CH559 has a built-in USB controller and dual USB transceivers. 
The features are as follows:

1. Supports USB Host and USB Device functions
2. Supports USB 2.0 full speed 12Mbps or low speed 1.5Mbps
3. Support USB control transfers, bulk transfers, interrupt transfers, iso transfers 
4. Provide dual-port Root-HUB in USB host mode, can manage two USB devices 
   at the same time
5. Support data packets up to 64 bytes, built-in FIFO, support interrupts 
   and DMA

The USB related registers of the CH559 are divided into 3 groups, and some of the 
registers are multiplexed between host and device mode.
 
1. USB global register 
2. USB device controller register 
3. USB host controller register  

## 16.2 Global Register

<center>Table 16.2.1 USB Global Register</center>
<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset Value</th>
    </tr>
    <tr><td>USB_RX_LEN</td><td>D1h</td><td>USB Receive Length Register (RO)</td> <td>0xxx&nbsp;xxxxb</td></tr>
    <tr><td>USB_INT_FG</td><td>D8h</td><td>USB Interrupt Flag Register</td> <td>0010 0000b</td></tr>
    <tr><td>USB_INT_ST</td><td>D9h</td><td>USB Interrupt Status Register (RO)</td> <td>00xx xxxxb</td></tr>
    <tr><td>USB_MIS_ST</td><td>DAh</td><td>USB Miscellaneous Status Register (RO)</td> <td>xx10 1000b</td></tr>
    <tr><td>USB_INT_EN</td><td>E1h</td><td>USB interrupt enable register</td><td>0000 0000b</td></tr> 
    <tr><td>USB_CTRL  </td><td>E2h</td><td>USB control register</td><td>0000 0110b</td></tr>
    <tr><td>USB_DEV_AD</td><td>E3h</td><td>USB device address register</td><td>0000 0000b</td></tr>
    <tr><td>USB_DMA_AH</td><td>E7h</td><td>DMA current buffer address high byte (RO)</td> <td>000x xxxxb</td></tr>
    <tr><td>USB_DMA_AL</td><td>E6h</td><td>DMA Low byte of current buffer address (RO)</td> <td>xxxx xxx0b</td></tr>
    <tr><td>USB_DMA   </td><td>E6h</td><td>USB_DMA_AL and USB_DMA_AH form a sfr16</td> <td>xxxxh</td></tr>
</table>

### USB Receive Length Register (USB_RX_LEN):

<table>
   <tr>
      <th>Bit</th><th>Name</th><th>Access</th><th>Description</th><th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>bUSB_RX_LEN</td><td>RO</td><td>number of bytes received by the current USB endpoint</td> <td>xxh</td></tr>
</table>

### USB Interrupt Flag Register (USB_INT_FG bit addressable):

<table>
   <tr>
      <th>Bit</th> <th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>U_IS_NAK</td><td>RO</td><td><b>USB Device mode:</b><br> a 1 indicates that a NAK response was received during the current USB transfer; a 0 indicates that a non-NAK response was received</td><td>0</td></tr>
   <tr><td>6</td><td>U_TOG_OK</td><td>RO</td><td>The current USB transfer DATA0/1 toggle flag match status. If set to is 1 the toggle is as expected and the data are valid. If this bit is 0 data are out of sync, data may be invalid.</td><td>0</td></tr>
   <tr><td>5</td><td>U_SIE_FREE</td><td>RO</td><td>Idle status bit of the USB protocol processor. This bit is 0 to indicate busy and USB transfer is in progress. This bit is 1 to indicate USB idle.</td><td>1</td></tr>
   <tr><td>4</td><td>UIF_FIFO_OV</td><td>RW</td><td>USB FIFO overflow interrupt flag bit. A 1 in this bit indicates a FIFO overflow has occured. Automatically cleared by direct bit access or writing 1 to the register</td> <td>0</td> </tr>
   <tr><td>3</td><td>UIF_HST_SOF</td><td>RW</td><td><b>USB Host mode:</b><br>SOF timer interrupt flag bit. This bit is 1 to indicate a SOF interrupt. This interrupt is triggered by the completion of a SOF packet transfer. Automatically cleared by direct bit access or writing 1 to the register</td> <td>0</td> </tr>
   <tr><td>2</td><td>UIF_SUSPEND</td><td>RW</td><td>USB bus suspend or wake event interrupt flag bit. This bit is 1 to indicate an interrupt. This interrupt is triggered by a USB suspend or wake event. Automatically cleared by direct bit access or writing 1 to the register</td> <td>0</td> </tr>
   <tr><td>1</td><td>UIF_TRANSFER</td><td>RW</td><td>USB transfer complete interrupt flag bit. This bit is 1 to indicate that there is an interrupt. The interrupt is reset by 0. A USB transfer completion trigger. Automatically cleared by direct bit access or writing 1 to the register</td> <td>0</td> </tr>
   <tr><td>0</td><td>UIF_DETECT<br>UIF_BUS_RST</td><td>RW</td><td>
      <b>USB Host mode:</b><br>
         set to 1 by hardware when a connection change is detectet, automatically cleared by direct bit access or writing 1 to the register.<br>
      <b>USB Device mode:</b><br>
         set to 1 by hardware when a USB device reset is recieved, automatically cleared by direct bit access or writing 1 to the register.
      </td><td>0</td></tr>
</table>

### USB Interrupt Status Register(USB_INT_ST):
<table>
   <tr>
      <th>Bit</th> <th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUIS_IS_NAK</td><td>RO</td><td><b>USB Device mode:</b><br>this bit is 1 to indicate that a NAK busy response was received during the current USB transfer. Same as U_IS_NAK</td><td> 0</td></tr>
   <tr><td>6</td><td>bUIS_TOG_OK</td><td>RO</td><td>The current USB transfer DATA0 / 1 synchronization flag match status. This bit is 1 to indicate synchronization; this bit to 0 indicates not to synchronize. Same as U_TOG_OK</td> <td>0</td></tr>
   <tr><td>[5:4]</td><td>bUIS_TOKEN</td><td>RO</td><td>Transaction PID:<li>00: indicates an OUT packet;</li> <li>01: indicates a SOF packet;</li><li>10: indicates an IN packet;</li><li>11: indicates a SETUP packet.</li></td><td>xxb</td></tr>
   <tr><td>[3:0]</td><td>MASK_UIS_ENDP<br>MASK_UIS_H_RES</td><td>RO</td><td>
      <b>USB Device mode:</b><br>
         indicates the endpoint number of the current USB transfer 0000b=EP0 1111=EP15<br>
      <b>USB HOST mode:</b><br>  
         PID ID of of the current USB transfer 0000b=no reponse or timeout other values represend the response PID
      </td><td>xxxxb</td></tr>
</table>

### USB Miscellaneous Status Register (USB_MIS_ST): 
<table>
   <tr>
      <th>Bit</th> <th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUMS_SOF_PRES</td><td>RO</td><td><b>USB Host mode:</b><br>The SOF packet indicates the status bit. This bit is 1 to indicate that a SOF packet will be sent. At this time, any other USB data packets will be automatically postponed.</td> <td>x</td> </tr>
   <tr><td>6</td><td>bUMS_SOF_ACT </td><td>RO</td><td><b>USB Host mode:</b><br>SOF packet transfer status, this bit is 1 to indicate that a SOF packet is being sent; this bit to 0 indicates that the transmission is complete or idle</td> <td>x</td></tr>
   <tr><td>5</td><td>bUMS_SIE_FREE</td><td>RO</td><td>Idle status bit of the USB protocol processor. This bit is 0 to indicate busy and USB transmission is in progress. This bit is 1 to indicate that USB is idle. Same as U_SIE_FREE</td> <td>1</td></tr> 
   <tr><td>4</td><td>bUMS_R_FIFO_RDY</td><td>RO</td><td>USB receive FIFO data ready status bit, this bit is 0 means the receive FIFO is empty; this bit is 1 means the receive FIFO is not empty</td> <td>0</td></tr>
   <tr><td>3</td><td>bUMS_BUS_RESET</td><td>RO</td><td>USB bus reset status bit, this bit is 0 means there is no USB bus reset at present; this bit is 1 means the USB bus is currently reset</td> <td>1</td></tr> 
   <tr><td>2</td><td>bUMS_SUSPEND</td><td>RO</td><td>USB suspend status bit, this bit is 0 to indicate that there is currently USB activity; this bit is 1 to indicate that there has been no USB activity for a period of time, requesting suspension</td> <td>0</td> </tr>
   <tr><td>1</td><td>bUMS_H1_ATTACH</td><td>RO</td><td><b>USB host mode:</b><br> the USB device connection status bit of the HUB1 port. A 1 in this bit indicates that the USB device is connected to HUB1.</td> <td>0</td> </tr>
   <tr><td>0</td><td>bUMS_H0_ATTACH</td><td>RO</td><td><b>USB host mode:</b><br> the USB device connection status bit of the HUB0 port. This bit is 1 to indicate that the HUB0 is connected to a USB device.</td> <td>0</td> </tr>
</table>
 
### USB interrupt enable register (USB_INT_EN):
<table>
   <tr>
      <th>Bit</th> <th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUIE_DEV_SOF</td><td>RW</td><td><b>USB Host mode:</b><br>This bit is 1 to enable USB device mode to receive SOF packet interrupt; 0 is disabled</td><td>0</td></tr> 
   <tr><td>6</td><td>bUIE_DEV_NAK</td><td>RW</td><td><b>USB Device mode:</b><br>This bit is 1 to enable to receive NAK interrupt; 0 is disabled</td><td>0</td></tr> 
   <tr><td>5</td><td>Reserved</td><td>RO</td><td>reserved </td><td>0</td></tr>
   <tr><td>4</td><td>bUIE_FIFO_OV</td><td>RW</td><td>This bit is 1 to enable FIFO overflow interrupt; this bit is 0 to disable</td><td>0</td></tr>
   <tr><td>3</td><td>bUIE_HST_SOF</td><td>RW</td><td>This bit is 1 to enable USB host mode SOF timer interrupt; 0 is disabled</td><td>0</td></tr>
   <tr><td>2</td><td>bUIE_SUSPEND</td><td>RW</td><td>This bit is 1 Enable the USB bus suspend or wake event interrupt; 0 disables</td><td>0</td></tr>
   <tr><td>1</td><td>bUIE_TRANSFER</td><td>RW</td><td>This bit is 1 to enable the USB transfer completion interrupt; this bit is 0 to disable</td><td>0</td></tr>
   <tr><td>0</td><td>bUIE_DETECT<br>bUIE_BUS_RST</td><td>RW</td>
   <td><b>USB Host Mode:</b><br>
          Set to 1 to enable USB device connect or disconnect event interrupt. If set to 0 no interrupts on status changes will be generated<br>
       <b>USB Device Mode:</b><br> 
          Set to 1 to enable USB Reset event interrupt. If set to 0 no interrupts on USB Reset events will be generated</td>
   <td>0</td></tr>
</table>

### USB Control Register (USB_CTRL): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUC_HOST_MODE</td><td>RW</td><td>USB working mode selection bit, this bit is 0 to select USB device mode (DEVICE); this bit is 1 to select USB host mode (HOST)</td><td>0</td></tr>
   <tr><td>6</td><td>bUC_LOW_SPEED</td><td>RW</td><td>USB bus signal transmission rate selection bit, this bit is 0 to select full speed 12Mbps; this bit is 1 to select low speed 1.5Mbps</td><td>0</td></tr>
   <tr><td>5</td><td>bUC_DEV_PU_EN</td><td>RW</td><td>USB device enable and internal pull-up resistor control bit in USB device mode. This bit is 1 to enable USB device transmission and enable internal pull-up resistor</td><td>0</td></tr>
   <tr><td>[5:4]</td><td>bUC_SYS_CTRL</td><td>RW</td><td>
       <b>Devicemode:</b>(bUC_HOST_MODE=0)
          <li>00b: Disable USB device function, no pull-up resistor</li>
          <li>01b: Enable USB device function, no pull-up resitor use external pull-up</li>
          <li>10b: Enable USB device function, internal pull-up on</li>
          <li>11b: Enable USB device function, internal pull-up weak</li>
       <b>Hostmode:</b>(bUC_HOST_MODE=1)
          <li>00b: normal working state</li> 
          <li>01b: force DP/DM to output SE0 state</li>
          <li>10b: force DP/DM to output J state</li> 
          <li>11b: force DP/DM to output K state/wake up</li>
   </td><td>00b</td></tr>
   <tr><td>3</td><td>bUC_INT_BUSY </td><td>RW</td><td>USB transfer completion interrupt flag is automatically suspended before the interrupt flag is not cleared. If this bit is 1, it is automatically suspended before the interrupt flag UIF_TRANSFER is not cleared. It will automatically answer the busy NAK for the device mode, and automatically suspend the subsequent transmission for the host mode; Bit 0 does not pause</td><td>0</td></tr>
   <tr><td>2</td><td>bUC_RESET_SIE</td><td>RW</td><td>USB protocol processor software reset control bit. When this bit is 1, the USB protocol processor is reset forcibly. It needs to be cleared by software.</td><td>1</td></tr>
   <tr><td>1</td><td>bUC_CLR_ALL  </td><td>RW</td><td>This bit is 1 to clear the USB interrupt flag and FIFO, which needs to be cleared by software</td><td>1</td></tr>
   <tr><td>0</td><td>bUC_DMA_EN   </td><td>RW</td><td>This bit is 1 to enable the USB DMA and DMA interrupts; 0 to disable the enable</td><td>0</td></tr>
</table>

### USB device address register (USB_DEV_AD):
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th><th>Description</th><th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUDA_GP_BIT</td><td>RW</td><td>USB general-purpose flag bit: user-defined, can be cleared or set by software</td><td>0</td></tr>
   <tr><td>[6:0]</td><td>MASK_USB_ADDR</td><td>RW</td><td>In the host mode, it is the address of the currently operating USB device; in the device mode, it is the address of the USB device</td><td>00h</td></tr>
</table>


## 16.3 Device Registers In USB device mode

The CH559 provides five groups of two-way endpoints 0, 1, 2, 3, and 4. The 
maximum packet size for each endpoint is 64 bytes. 
Endpoint 0 is the default endpoint used for control (SETUP) tranfers, 
sending and receiving uses the same 64-byte data buffer.<br> 
Endpoint 1, Endpoint 2, Endpoint 3 each includes a sending endpoint IN 
and a receiving endpoint OUT. Each Endpoint can have its own independent 64-byte 
buffer or double 64-byte data buffer, which supports control, bulk, interrupt 
and iso transfers.
Endpoint 4 is a bit different since it has no own DMA control but shares DMA and 
with Endpoint 0. It supports control, bulk, interrupt and iso transfers.

Each pair of endpoints has its own control register UEPn_CTRL and its own 
transfer size register UEPn_T_LEN (n = 0/1/2/3/4), which is used to set the 
toggle bit of the endpoint in response to OUT transaction, IN transaction and 
the sending data Length, etc.<br> 
The pull-up resistor of the USB bus, as necessary for a USB device, can be set 
by software at any time. When bUC_DEV_PU_EN is set in the USB control register 
USB_CTRL, the CH559 internally connects a 1k5 pullup to the DP pin or DM pin 
according to the settings of bUD_LOW_SPEED and thus enable the USB device 
function as low or full speed device. 

In case of any USB event such as USB bus reset, USB bus suspend, wake-up, USB Setup 
or USB transfer event, the USB protocol processor will set the corresponding 
interrupt flag and generates an interrupt request. 
The application has to query the interrupt flag register USB_INT_FG to determine 
the event, or if USB interrupt mode is used take the neccessary actions in an 
USB interrupt service routine. 

There are 3 major events which should be handled by the firmware:<br>
+ UIF_BUS_RST: issued by the host to reset the usb device 
+ UIF_SUSPEND: issued by the host to put the usb device into sleep mode 
+ UIF_TRANSFER: data transfers to the verious endpoints 

For UIF_TRANSFER the USB interrupt status register USB_INT_ST provides information
about type of transfer in the bUIS_TOKEN field and endpoint number in the 
MASK_UIS_ENDP field. Both fields have to be evaluated to perform the next action 
on the bus.
If the toggle bit bUEP_R_TOG of the OUT transaction for the endpoint is set in 
advance, U_TOG_OK or bUIS_TOG_OK can be used to check whether toggle bit of the 
actual received data packet matches the expected toogle bit of the endpoint. 
If the bit is set data packe is valid. If not the data the data should be discarded. 

Each time the USB send or receive interrupt is processed, the toggle bit of the 
corresponding endpoint should be modified correctly to synchronize the next 
data packet sent and check whether the next received data packet is synchronized. 
In addition, by setting bUEP_AUTO_TOG, the toggle bit will change automatically 
after each successful transmission or successful reception. The data to be sent 
by each endpoint is in its own buffer, and the size of the data to be sent 
is independently set in UEPn_T_LEN. The data received by each endpoint is in 
its own buffer, but the length of the received data is all in the USB receive 
length Register USB_RX_LEN, which can be distinguished according to the current 
endpoint number when USB receive interrupt

<center>Table 16.3.1 USB Device Related Register List</center> 
<table>
    <tr>
        <th>Name</th><th>Address</th><th>Description</th><th>Reset Value</th>
    </tr>
    <tr><td>UEP1_CTRL</td> <td>D2h</td><td>Endpoint 1 Control Register</td><td>0000&nbsp;0000b</td></tr>
    <tr><td>UEP1_T_LEN</td><td>D3h</td><td>Endpoint 1 Send Length Register</td> <td>0xxx xxxxb</td></tr>
    <tr><td>UEP2_CTRL</td><td>D4h</td><td>Endpoint 2 Control Register</td> <td>0000 0000b</td></tr>
    <tr><td>UEP2_T_LEN</td><td>D5h</td><td>Endpoint 2 Send Length Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP3_CTRL</td><td>D6h</td><td>Endpoint 3 control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP3_T_LEN</td><td>D7h</td><td>Endpoint 3 Send Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP0_CTRL</td><td>DCh</td><td>Endpoint 0 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_T_LEN</td><td>DDh</td><td>Endpoint 0 Send Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UEP4_CTRL</td><td>DEh</td><td>Endpoint 4 Control Register</td><td>0000 0000b</td></tr>
    <tr><td>UEP4_T_LEN</td><td>DFh</td><td>Endpoint 4 Send Length Register</td><td>0xxx xxxxb</td></tr>
    <tr><td>UDEVCTRL</td><td>E4h</td><td>Device Control Register</td><td>0100 x000b</td></tr>
    <tr><td>UEP4_1_MOD</td><td>2446h</td><td>Endpoint 1, 4 mode control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP2_3_MOD</td><td>2447h</td><td>Endpoint 2, 3 mode control register</td><td>0000 0000b</td></tr>
    <tr><td>UEP0_DMA_H</td><td>2448h</td><td>Endpoint 0/4 DMA buffer start high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP0_DMA_L</td><td>2449h</td><td>Endpoint 0/4 DMA buffer start low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP1_DMA_H</td><td>244Ah</td><td>Endpoint 1 DMA start high Byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP1_DMA_L</td><td>244Bh</td><td>Endpoint 1 DMA start low Byte</td><td>xxxx xxxxb</td></tr>
    <tr><td>UEP2_DMA_H</td><td>244Ch</td><td>Endpoint 2 buffer start address high byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP2_DMA_L</td><td>244Dh</td><td>Endpoint 2 buffer start address low byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>UEP3_DMA_H</td><td>244Eh</td><td>Endpoint 3 Buffer Start Address High Byte</td><td>000x xxxxb</td></tr>
    <tr><td>UEP3_DMA_L</td><td>244Fh</td><td>Endpoint 3 Buffer Start Address Low Byte</td><td>xxxx xxx0b</td></tr>
    <tr><td>pUEP*</td><td>254*h</td><td>after bXIR_XSFR is set, this names can be used to address the above xSFR as pdata, which is faster than xdata type addressing</td><td></td></tr>
</table>

### Endpoint n Control Register (UEPn_CTRL): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUEP_R_TOG</td><td>RW</td><td>Synchronous trigger bit expected by the receiver of USB endpoint n (handling SETUP / OUT transactions). This bit is 0 for DATA0; 1 for DATA1</td><td>0</td></tr>
   <tr><td>6</td><td>bUEP_T_TOG</td><td>RW</td><td>Synchronous trigger bit prepared by the USB endpoint n's transmitter (processing IN transaction). This bit is 0 to send DATA0; 1 to send DATA1</td><td>0</td></tr>
   <tr><td>5</td><td>Reserved</td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>[3:2]</td><td>bUEP_R_RES</td><td>RW</td>
      <td>Handshake for a SETUP or OUT ransfers:
        <li>00: send a ACK handshake to the host(ready)</li>
        <li>01: send nothing timeout to the host(used on iso transfers)</li>
        <li>10: send a NAK handshake to the host(busy)</li>
        <li>11: send a STALL handshake to the host(error)</li>
      </td><td>00b</td></tr>
   <tr><td>[1:0]</td><td>bUEP_T_RES</td><td>RW</td>
      <td>Handshake recieved for IN Transfers:
        <li>00: recived a ACK handshake from the host(ready)</li>
        <li>01: recived no handshake from the host(timeout or iso)</li>
        <li>10: recived a NAK handshake from the host(busy)</li>
        <li>11: recived a STALL handshake from the host(error)</li>
      </td><td>00b</td></tr>
</table>

### Endpoint n Transmit Length Register (UEPn_T_LEN):
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>bUEPn_T_LEN<br>bUEP2_T_LEN</td><td>RW</td><td>Set the number of data bytes to be sent by USB endpoint n (n = 0/1/3/4) <br> bUEP2_T_LEN Set the number of data bytes to be sent by USB endpoint 2</td><td>xxh<br>00h</td></tr>
</table>

### USB Device Physical Port Control Register (UDEV_CTRL):
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td></td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>6</td><td>bUD_RECV_DIS</td><td>RW</td><td>USB device physical port receiver disable bit, this bit is 1 to disable the receiver and there is no static power; this bit is 0 to enable the receiver and generate static power</td><td>1</td></tr>
   <tr><td>5</td><td>bUD_DP_PD_DIS</td><td>RW</td><td>USB device port DP pin internal pull-down resistor disable bit, this bit is 1 to disable internal pull-down resistor; this bit is 0 to enable DP internal pull-down resistor</td><td>0</td></tr>
   <tr><td>4</td><td>bUD_DM_PD_DIS</td><td>RW</td><td>USB device port DM pin internal pull-down resistor disable bit, this bit is 1 to disable internal pull-down resistor; this bit is 0 to enable DM internal pull-down resistor</td><td>0</td></tr>
   <tr><td>3</td><td>bUD_DIFF_IN </td><td>R0</td><td>Current differential input status between DP and DM pins</td><td>x</td></tr>
   <tr><td>2</td><td>bUD_LOW_SPEED</td><td>RW</td><td>USB device physical port low speed mode enable bit, this bit is 1 to select 1.5Mbps low speed mode; this bit is 0 to select 12Mbps full speed mode</td><td>0</td></tr>
   <tr><td>1</td><td>bUD_GP_BIT</td><td>RW</td><td>USB device mode general flag: user can define it by himself, it can be cleared or set by software</td><td>0</td></tr>
   <tr><td>0</td><td>bUD_PORT_EN</td><td>RW</td><td>USB device physical port enable bit, this bit is 1 to enable the physical port; this bit is 0 to disable the physical port</td><td>0</td></tr>
</table>

### USB endpoint 1, 4 mode control register (UEP4_1_MOD): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUEP1_RX_EN</td><td>RW</td><td>This bit is 0 to disable endpoint 1 reception; 1 to enable endpoint 1 reception (OUT)</td><td>0</td></tr>
   <tr><td>6</td><td>bUEP1_TX_EN</td><td>RW</td><td>This bit is 0 to disable endpoint 1 send; enable endpoint 1 send (IN) for 1</td><td>0</td></tr>
   <tr><td>5</td><td>reserved   </td><td>RO</td><td>reserve</td><td>0</td></tr>
   <tr><td>4</td><td>bUEP1_BUF_MOD</td><td>RW</td><td>endpoint 1 data buffer mode control bit</td><td>0</td></tr>
   <tr><td>3</td><td>bUEP4_RX_EN</td><td>RO</td><td>This bit is 0 to disable endpoint 4 reception; enable 1 to enable endpoint 4 reception (OUT)</td><td>0</td></tr>
   <tr><td>2</td><td>bUEP4_TX_EN</td><td>RW</td><td>This bit is 0 to disable endpoint 4 transmission; 1 to enable endpoint 4 transmission (IN)</td><td>0</td></tr>
   <tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>00b</td></tr>
</table>

The combination of bUEP4_RX_EN and bUEP4_TX_EN controls the data buffering of 
USB endpoints 0 and 4 Zone mode, refer to the table below.
 
<center>Table 16.3.2 Endpoints 0 and 4 buffer mode</center>
<table>
   <tr>
      <th>bUEP4_RX_EN</th><th>bUEP4_TX_EN</th><th>Structure description: starting from UEP0_DMA from low to high</th>
   </tr>
   <tr><td>0</td><td>0</td><td>Endpoint 0 single 64-byte transmit and receive shared buffers (IN and OUT)</td></tr>
   <tr><td>1</td><td>0</td><td>Endpoint 0 single 64 Bytes send and receive common buffer; Endpoint 4 single 64-byte receive buffer (OUT)</td></tr>
   <tr><td>0</td><td>1</td><td>Endpoint 0 and single send 64-byte shared buffer; Endpoint 4 single 64-byte send buffer (IN)</td></tr>
   <tr><td>1</td><td>1</td><td>Endpoint 0 single 64-byte transmit and receive buffer; Endpoint 4 single 64-byte receive buffer (OUT); Endpoint 4 single 64-byte send buffer (IN).<br> The entire 192 bytes are arranged as follows: UEP0_DMA + 0 Address: Endpoint 0 is shared for sending and receiving; UEP0_DMA + 64 Address: Endpoint 4 is receiving; UEP0_DMA + 128 Address: Endpoint 4 is sending</td></tr>
</table>

### USB endpoint 2, 3 mode control register (UEP2_3_MOD):
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUEP3_RX_EN</td><td>RW</td><td>This bit is 0 to disable endpoint 3 reception; 1 to enable endpoint 3 reception (OUT)</td><td>0</td></tr>
   <tr><td>6</td><td>bUEP3_TX_EN</td><td>RW</td><td>This bit is 0 to disable endpoint 3 transmission; enable 1 to enable endpoint 3 transmission (IN )</td><td>0</td></tr>
   <tr><td>5</td><td>Reserved </td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>4</td><td>bUEP3_BUF_MOD</td><td>RW</td><td>Endpoint 3 Data Buffer Mode Control Bit</td><td>0</td></tr>
   <tr><td>3</td><td>bUEP2_RX_EN</td><td>RO</td><td>This bit is 0 to disable endpoint 2 reception; 1 to enable endpoint 2 reception (OUT)</td><td>0</td></tr>
   <tr><td>2</td><td>bUEP2_TX_EN</td><td>RW</td><td>This bit is 0 to disable Endpoint 2 Send; Enable Endpoint 2 Send (IN) for 1</td><td>0</td></tr>
   <tr><td>1</td><td>Reserved</td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>0</td><td>bUEP2_BUF_MOD</td><td>RW</td><td>Endpoint 2 Data Buffer Mode Control Bit</td><td>0</td></tr>
</table>

Controlled by bUEPn_RX_EN and bUEPn_TX_EN and bUEPn_BUF_MOD (n = 1/2/3) respectively. 
For the data buffer mode of USB endpoints 1, 2, and 3, refer to the following 
table. Among the dual 64-byte buffer modes, the first 64-byte buffer is selected 
according to bUEP_*_TOG = 0 during USB data transfer, and the last 64-byte buffer 
is selected according to bUEP_*_TOG = 1 to realize automatic switching. 

<center>Table 16.3.3 Endpoint n buffer mode (n = 1/2/3) </center>
<table>
   <tr>
      <th>bUEPn_RX_EN</th><th>bUEPn_TX_EN</th><th>bUEPn_BUF_MOD</th><th>Structure description: starting from UEPn_DMA from low to high/th>
   </tr>
   <tr><td>0</td><td>0</td><td>x</td><td>The endpoint is disabled and the UEPn_DMA buffer is not used</td></tr>
   <tr><td>1</td><td>0</td><td>0</td><td>Single 64-byte receive buffer (OUT)</td></tr>
   <tr><td>1</td><td>0</td><td>1</td><td>Double 64-byte receive buffer, selected by bUEP_R_TOG</td></tr> 
   <tr><td>0</td><td>1</td><td>0</td><td>Single 64-byte receive buffer (IN)</td></tr> 
   <tr><td>0</td><td>1</td><td>1</td><td>Double 64-byte receive buffer, pass bUEP_T_TOG selection</td></tr> 
   <tr><td>1</td><td>1</td><td>0</td><td>single 64-byte receive buffer; single 64-byte transmit buffer</td></tr>
   <tr><td>1</td><td>1</td><td>1</td><td>Double 64-byte receive buffer, selected by bUEP_R_TOG; double 64-byte send buffer, selected by bUEP_T_TOG. All 256 bytes are arranged as follows: UEPn_DMA + 0 address: endpoint receives when bUEP_R_TOG = 0; UEPn_DMA + 64 address: endpoint receives when bUEP_R_TOG = 1; UEPn_DMA + 128 address: endpoint sends when bUEP_T_TOG = 0; UEPn_DMA + 192 address: bUEP_T_TOG = 1:00 sent by endpoint</td></tr>
</table>

### USB Endpoint n Buffer Start Address (UEPn_DMA) (n = 0/1/2/3): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>UEPn_DMA_H</td><td>RW</td><td>Endpoint n buffer start address high byte, only the lower 5 bits are valid, the upper 3 bits are fixed to 0</td><td>xxh</td></tr>
   <tr><td>[7:0]</td><td>UEPn_DMA_L</td><td>RW</td><td>Endpoint n buffer start address low byte, only the upper 7 bits are valid, the lowest bit is fixed to 0, only even address is supported</td><td>xxh</td></tr>
</table>
Note: The length of the buffer to receive data is >= min(maximum packet length 
that may be received + 2 bytes, 64 bytes)<br>

## 16.4 Host Register In USB host mode

The CH559 provides a set of bidirectional host endpoints, including a sending 
endpoint OUT and a receiving endpoint IN. The maximum data packet length is 64 
bytes. It supports control transfers, bulk transfers, interrupt transfers and 
iso transfers. <br>
Every USB transaction initiated by the host endpoint always sets 
the interrupt flag UIF_TRANSFER automatically after processing is complete. 
The application program can directly query or analyze and analyze the interrupt 
flag register USB_INT_FG in the USB interrupt service routine, and perform 
corresponding processing according to each interrupt flag; and, if UIF_TRANSFER 
is valid, then it needs to continue to analyze the USB interrupt status register 
USB_INT_ST. The response PID identifier MASK_UIS_H_RES of the transmission 
transaction is processed accordingly. If the synchronization trigger bit 
bUH_R_TOG of the IN transaction of the host receiving endpoint is set in 
advance, you can use U_TOG_OK or bUIS_TOG_OK determines whether the 
synchronization trigger bit of the currently received data packet matches 
the synchronization trigger bit of the host receiving endpoint. If the data 
is synchronized, the data is valid; if the data is not synchronized, the data 
should be discarded. Each time the USB send or receive interrupt is processed, 
the synchronization trigger bit of the corresponding host endpoint should be 
modified correctly to synchronize the next data packet sent and detect whether 
the next received data packet is synchronized; in addition, by setting 
bUEP_AUTO_TOG It can be realized that the corresponding synchronization trigger 
bit is automatically flipped after sending successfully or receiving successfully. 

The USB host token setting register UH_EP_PID is shared with the the USB 
endpoint 1 control register in the USB device mode. It is used to set the 
endpoint number of the target device to be operated and the token PID packet 
identifier of the USB transfer transaction. The data corresponding to the SETUP 
token and the OUT token are provided by the host sending endpoint. 
The data to be sent is in the UH_TX_DMA buffer and the length of the data to be 
sent is set in UH_TX_LEN; the data corresponding to the IN token is returned by 
the target device to the host Receiving endpoint, the received data is stored 
in the UH_RX_DMA buffer, and the length of the received data is stored in 
USB_RX_LEN.

<center>Table 16.4.1 USB host related register list</center>
<table>
    <tr>
        <th>Name</th> <th>Address</th> <th>Description</th> <th>Reset Value</th>
    </tr>
    <tr><td>UH_SETUP  </td><td>D2h</td><td>USB host auxiliary setting register          </td><td>0000&nbsp;0000b</td></tr>
    <tr><td>UH_RX_CTRL</td><td>D4h</td><td>USB host receiving endpoint control register </td><td>0000 0000b</td></tr> 
    <tr><td>UH_EP_PID </td><td>D5h</td><td>USB host token setting register              </td><td>0000 0000b</td></tr>
    <tr><td>UH_TX_CTRL</td><td>D6h</td><td>USB host sending endpoint control Register   </td><td>0000 0000b</td></tr>
    <tr><td>UH_TX_LEN </td><td>D7h</td><td>USB host transmit length register            </td><td>0xxx xxxxb</td></tr>
    <tr><td>USB_HUB_ST</td><td>DBh</td><td>USB host HUB port status register (read only)</td><td>0000 0000b</td></tr>
    <tr><td>UHUB0_CTRL</td><td>E4h</td><td>USB host HUB0 port control register          </td><td>0100 x000b</td></tr>
    <tr><td>UHUB1_CTRL</td><td>E5h</td><td>USB host HUB1 port control register          </td><td>1100 x000b</td></tr>
    <tr><td>UH_EP_MOD  </td><td>2447h</td><td>Endpoint mode control register                  </td><td>0000 0000b</td></tr> 
    <tr><td>UH_RX_DMA_H</td><td>244Ch</td><td>USB host receive buffer start address high byte </td><td>000x xxxxb</td></tr>
    <tr><td>UH_RX_DMA_L</td><td>244Dh</td><td>USB host receive buffer start address low byte  </td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_RX_DMA  </td><td>244Ch</td><td>UH_RX_DMA_L and UH_RX_DMA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>UH_TX_DMA_H</td><td>244Eh</td><td>USB host send buffer start address high byte </td><td>000x xxxxb</td></tr>
    <tr><td>UH_TX_DMA_L</td><td>244Fh</td><td>USB host send buffer start address low byte  </td><td>xxxx xxx0b</td></tr>
    <tr><td>UH_TX_DMA  </td><td>244Eh</td><td>UH_TX_DMA_L and UH_TX_DMA_H form a 16-bit SFR</td><td>xxxxh</td></tr>
    <tr><td>pUH_*      </td><td>254*h</td><td>After bXIR_XSFR is set, this names are used to address the above xSFR as a pdata type, which is faster than xdata type addressing</td></tr>
</table>

### USB host auxiliary setup register (UH_SETUP): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUH_PRE_PID_EN</td><td>RW</td><td>Low-speed preamble packet PRE PID enable bit, this bit is 1 to enable the USB host to communicate with the low-speed USB device through an external HUB; 0 to disable the low-speed preamble packet, there must be no HUB between the USB host and the low-speed USB device</td><td>0</td></tr>
   <tr><td>6</td><td>bUH_SOF_EN</td><td>RW</td><td>Automatically generate SOF packet enable bit. This bit is 1. The SOF packet is automatically generated by the USB host; 0 is not generated automatically, but it can be generated manually.</td><td>0</td></tr>
   <tr><td>[5:0]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>00h</td></tr>
</table>
 
### USB Host Receive Endpoint Control Register (UH_RX_CTRL): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUH_R_TOG</td><td>RW</td><td>The expected synchronization trigger bit for the USB host receiver (handling IN transactions). This bit is 0 for DATA0 and 1 for DATA1.</td><td>0</td></tr>
   <tr><td>[6:5]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>00b</td></tr>
   <tr><td>4</td><td>bUH_R_AUTO_TOG</td><td>RW</td><td>Automatically flip bUH_R_TOG control bit. This bit is 1 to automatically flip the bUH_R_TOG flag after the USB host receives successfully; 0 to not flip automatically, but can be manually switched.</td><td>0</td></tr>
   <tr><td>3</td><td>Reserved</td><td>RO</td><td>reserved</td><td>0</td></tr>
   <tr><td>2</td><td>bUH_R_RES</td><td>RW</td><td>USB host receiver response control bit for IN transaction, 0 means ACK or ready; 1 means no response, used for real-time / synchronous transfer with non-endpoint 0 of the target device</td><td>0</td></tr>
   <tr><td>[1:0]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>00b</td></tr>
</table>

### USB host token setting register (UH_EP_PID): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:4]</td><td>MASK_UH_TOKEN</td><td>RW</td><td>Set the token PID packet ID of this USB transfer transaction</td><td>0000b</td></tr>
   <tr><td>[3:0]</td><td>MASK_UH_ENDP </td><td>RW</td><td>Set the target to be operated this time Device endpoint number</td><td>0000b</td></tr>
</table>

### USB Host Send Endpoint Control Register (UH_TX_CTRL): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>Reserved </td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>6</td><td>bUH_T_TOG</td><td>RW</td><td>Synchronous trigger bit prepared by the USB host transmitter (handling SETUP / OUT transactions). This bit is 0 to send DATA0; 1 to send DATA1</td><td>0</td></tr>
   <tr><td>5</td><td>Reserved</td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>4</td><td>bUH_T_AUTO_TOG</td><td>RW</td><td>Automatically flip bUH_T_TOG control bit, this bit is 1 to automatically flip the bUH_T_TOG flag after the USB host sends successfully; 0 to not flip automatically, but can be manually switched</td><td>0</td></tr>
   <tr><td>[3:1]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>000b</td></tr>
   <tr><td>0</td><td>bUH_T_RES</td><td>RW</td><td>USB host transmitter response control bit for SETUP / OUT transaction, 0 means to expect ACK or ready; 1 means no response, used for real-time / synchronous transmission with non-endpoint 0 of the target device</td><td>0</td></tr>
</table>
 
### USB Host Send Length Register (UH_TX_LEN): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>UH_TX_LEN</td><td>RW</td><td>Sets the number of data bytes to be sent by the USB host send endpoint</td><td>xxh
</table>

### USB host HUB port status register (USB_HUB_ST): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUHS_H1_ATTACH</td><td>RO</td><td>The USB device connection status bit of the HUB1 port. A 1 in this bit indicates that a USB device is connected to HUB1; a 0 in this bit indicates no. Same as bUMS_H1_ATTACH</td><td>0</td></tr>
   <tr><td>6</td><td>bUHS_HM_LEVEL </td><td>RO</td><td>Record the state of the HM pin when the USB device is just connected to the HUB1 port. 0 means low level; 1 means high level. Used to determine full speed or low speed</td><td>0</td></tr>
   <tr><td>5</td><td>bUHS_HP_PIN   </td><td>RO</td><td>Current HP pin status, 0 for low level; 1 for high level</td><td>0</td></tr>
   <tr><td>4</td><td>bUHS_HM_PIN   </td><td>RO</td><td>Current HM pin status, 0 for low level; 1 for high level</td><td>0</td></tr>
   <tr><td>3</td><td>bUHS_H0_ATTACH</td><td>RO</td><td>The USB device connection status bit of the HUB0 port. A 1 in this bit indicates that a USB device is connected to HUB0; a 0 in this bit indicates no. Same as bUMS_H0_ATTACH</td><td>0</td></tr>
   <tr><td>2</td><td>bUHS_DM_LEVEL </td><td>RO</td><td>Record the state of the DM pin when the USB device is just connected to the HUB0 port. 0 means low level; 1 means high level. Used to determine full speed or low speed</td><td>0</td></tr>
   <tr><td>1</td><td>bUHS_DP_PIN   </td><td>RO</td><td>Current DP pin status, 0 for low level; 1 for high level</td><td>0</td></tr>
   <tr><td>0</td><td>bUHS_DM_PIN   </td><td>RO</td><td>Current DM pin status, 0 for low level; 1 for high level</td><td>0</td></tr>
</table>

### USB host HUBn port control register (UHUBn_CTRL) (n = 0, 1): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>bUH1_DISABLE </td><td>RW</td><td>For UHUB1_CTRL is the USB host HUB1 port pin disable bit, this bit is 1 to disable the HP / HM pin, releasing P5.5 / P5.4 will be used for other functions; this bit is 0 to enable P5.5 / P5.4 Used as HP / HM for HUB1 port</td><td>1</td></tr>
   <tr><td>6</td><td>bUH_RECV_DIS </td><td>RW</td><td>USB host HUBn port receiver disable bit, this bit is 1 to disable the receiver and there is no static power consumption; this bit is 0 to enable the receiver and generate static power consumption</td><td>1</td></tr>
   <tr><td>5</td><td>bUH_DP_PD_DIS</td><td>RW</td><td>USB host HUBn port DP / HP pin internal pull-down resistor disable bit, this bit is 1 to disable internal pull-down resistor; this bit is 0 to enable internal pull-down resistor</td><td>0</td></tr>
   <tr><td>4</td><td>bUH_DM_PD_DIS</td><td>RW</td><td>USB host HUBn port DM / HM pin internal pull-down resistor disable bit, this bit is 1 to disable internal pull-down resistor; this bit is 0 to enable internal pull-down resistor</td><td>0</td></tr>
   <tr><td>3</td><td>bUH_DIFF_IN  </td><td>R0</td><td>For UHUB0_CTRL is the current differential input state between the DP and DM pins For UHUB1_CTRL is the current differential input state between the HP and HM pins</td><td>x</td></tr>
   <tr><td>2</td><td>bUH_LOW_SPEED</td><td>RW</td><td>USB host HUBn port low speed mode enable bit, this bit is 1 to select 1.5Mbps low speed mode; this bit is 0 to select 12Mbps full speed mode</td><td>0</td></tr>
   <tr><td>1</td><td>bUH_BUS_RESET</td><td>RW</td><td>USB host HUBn port bus reset control bit, this bit is 1 to force the HUBn port to output USB bus reset; this bit is 0 to end the output</td><td>0</td></tr>
   <tr><td>0</td><td>bUH_PORT_EN  </td><td>RW</td><td>USB host HUBn port enable bit, this bit is 0 to disable the HUBn port; this bit is 1 to enable the HUBn port. This bit is automatically cleared when the USB device is disconnected</td><td>0</td></tr>
</table>
 
### USB Host Endpoint Mode Control Register (UH_EP_MOD): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>7</td><td>Reserved</td><td>RO</td><td>Reserved</td><td>0</td></tr>
   <tr><td>6</td><td>bUH_EP_TX_EN</td><td>RW</td><td>This bit is 0 to disable the USB host from sending endpoints to send data; this bit is 1 to enable the USB host to send endpoints to send data (SETUP / OUT)</td><td>0</td></tr>
   <tr><td>5</td><td>Reserved</td><td>RO</td><td>reserved</td><td>0</td></tr>
   <tr><td>4</td><td>bUH_EP_TBUF_MOD</td><td>RW</td><td>USB host transmit endpoint data buffer mode control bit</td><td>0</td></tr>
   <tr><td>3</td><td>bUH_EP_RX_EN</td><td>RO</td><td>This bit is 0 to disable the USB host receiving endpoint from receiving data; this bit is 1 to enable the USB host receiving endpoint to receive data (IN)</td><td>0</td></tr>
   <tr><td>[2:1]</td><td>Reserved</td><td>RO</td><td>reserved</td><td>00b</td></tr>
   <tr><td>0</td><td>bUH_EP_RBUF_MOD</td><td>RW</td><td>USB host receive endpoint data buffer mode control bit</td><td>0</td></tr>
</table>

Controlled by the combination of bUH_EP_TX_EN and bUH_EP_TBUF_MOD, the USB host sends 
endpoint data buffer mode, refer to the table below.

<center>Table 16.4.2 Host transmit buffer mode</center>                                                                                                   
<table>
   <tr>
      <th>bUH_EP_TX_EN</th><th>bUH_EP_TBUF_MOD</th><th>Structure description: Start with UH_TX_DMA</th>
   </tr>
   <tr><td>0</td><td>x</td><td>Endpoint is disabled, UH_TX_DMA buffer is not used</td></tr>
   <tr><td>1</td><td>0</td><td>Single 64-byte transmit buffer (SETUP / OUT)</td></tr>
   <tr><td>1</td><td>1</td><td>Double 64-byte transmit buffer, selected by bUH_T_TOG:<br>When bUH_T_TOG = 0, the first 64-byte buffer is selected;<br>when bUH_T_TOG = 1, the latter 64-byte buffer is selected</td></tr>
</table>

by the combination of bUH_EP_RX_EN and bUH_EP_RBUF_MOD to control the USB host receiving 
endpoint data buffer Mode, refer to the table below.
<center>Table 16.4.3 Host receive buffer mode</center> 

<table>
   <tr>
      <th>bUH_EP_RX_EN</th><th>bUH_EP_RBUF_MOD</th><th>Structure description: Start with UH_RX_DMA</th>
   </tr>
   <tr><td>0</td><td>x</td><td>Endpoint is disabled, UH_RX_DMA buffer is not used</td></tr>
   <tr><td>1</td><td>0</td><td>Single 64-byte receive buffer (IN)</td></tr>
   <tr><td>1</td><td>1</td><td>Double 64-byte receive buffer, selected by bUH_R_TOG:<br>When bUH_R_TOG = 0, select the first 64-byte buffer;<br>when bUH_R_TOG = 1, select the last 64-byte buffer</td></tr>
</table>
                                                                                                                                                                                                            
### USB Host receive buffer start address (UH_RX_DMA): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>UH_TX_DMA_H</td><td>RW</td><td>USB host send buffer start address high byte, only the lower 5 bits are valid, the upper 3 bits are fixed to 0</td><td>xxh</td></tr>
   <tr><td>[7:0]</td><td>UH_TX_DMA_L</td><td>RW</td><td>USB host send buffer start address low byte, only the upper 7 bits are valid, the lowest bit is fixed to 0, only supports even address</td><td>xxh</td></tr>
</table>
 
### USB Host transmit start address (UH_TX_DMA): 
<table>
   <tr>
      <th>Bit</th><th>Name</th> <th>Access</th> <th>Description</th> <th>Reset value</th>
   </tr>
   <tr><td>[7:0]</td><td>UH_TX_DMA_H</td><td>RW</td><td>USB host send buffer start address high byte, only the lower 5 bits are valid, the upper 3 bits are fixed to 0</td><td>xxh</td></tr>
   <tr><td>[7:0]</td><td>UH_TX_DMA_L</td><td>RW</td><td>USB host send buffer start address low byte, only the upper 7 bits are valid, the lowest bit is fixed to 0, only supports even address</td><td>xxh</td></tr>
</table>



