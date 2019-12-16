---
title: 4. Pin descriptions
type: docs
weight: 4
BookToC: false
---

# 8-bit enhanced USB microcontroller CH559

## 4. Pin descriptions

### CH559L/T pin definitions.

<table>
    <tr>
        <th colspan="2">Pin number</th>
        <th rowspan="2">Pin name <br>(function after reset)</th>
        <th rowspan="2">Alternate functions</th>
        <th rowspan="2">Alternate functions description</th>
    </tr>
    <tr>
        <th>SSOP-20</th>
        <th>LQFP-48</th>
    </tr>
    <tr>
        <td>19</td><td>41</td><td>VIN5</td><td>V5</td><td>The 5V external power input of the internal 5V->3.3V voltage regulator requires an external 0.1uF power supply decoupling capacitor.</td>
    </tr>
    <tr>
        <td>20</td><td>42</td><td>VDD33</td><td>VDD/VCC</td><td>Internal voltage regulator output and internal 3.3V working power input. When the power supply voltage is less than 3.6V, connect VIN5 to input external power supply. When the power supply voltage is greater than 3.6V, connect 3.3uF power supply decoupling capacitor.</td>
    </tr>
    <tr>
        <td>18</td><td>18</td><td>GND</td><td>VSS</td><td>Common ground terminal.</td>
    </tr>
    <tr>
        <td>--</td><td>40</td><td>P0.0</td><td>AD0/UDTR</td><td rowspan="8"><p>P0 port: The default is an 8-bit open-drain bidirectional port. You can enable the internal pull-up resistor to be turned into a quasi-bidirectional port by setting P0_PU.</p><p>P0 temporarily switches to push-pull output as a bidirectional data bus AD0~AD7 when accessing the external bus, or outputs the lower 8 bits of the address as needed when accessing the external bus in multiplexed address mode.</p><p>UDTR, URTS: Modem signal output of UART1.</p><p>UCTS, UDSR, URI, UDCD: UART1 modem signal input.</p><p>RXD_, TXD_: RXD, TXD pin mapping.</p></td>
    </tr>
    <tr>
        <td>--</td><td>39</td><td>P0.1</td><td>AD1/URTS</td>
    </tr>
    <tr>
        <td>17</td><td>38</td><td>P0.2</td><td>AD2/RXD_</td>
    </tr>
    <tr>
        <td>16</td><td>37</td><td>P0.3</td><td>AD3/TXD_</td>
    </tr>
     <tr>
        <td>--</td><td>36</td><td>P0.4</td><td>AD4/UCTS</td>
    </tr>
    <tr>
        <td>--</td><td>35</td><td>P0.5</td><td>AD5/UDSR</td>
    </tr>
    <tr>
        <td>--</td><td>34</td><td>P0.6</td><td>AD6/URI</td>
    </tr>
    <tr>
        <td>--</td><td>33</td><td>P0.7</td><td>AD7/UDCD</td>
    </tr>
    <tr>
        <td>--</td><td>43</td><td>P1.0</td><td>AIN0/T1/CAP1</td><td rowspan="8"><p>AIN0~AIN7: 8-channel ADC analog signal input.</p><p>T2: External count input/clock output of timer/counter 2.</p><p>T2EX: Timer/Counter 2 Reload/Capture Input.</p><p>CAP1, CAP2: Capture input of timer/counter 2 1, 2.</p><p>CAP3/PWM3: Timer/Event Counter 3 Capture Input/PWM Output.</p><p>SCS, MOSI, MISO, SCK: SPI0 interface, SCS is the chip select input, MOSI is the master output/slave input, MISO is the master input/slave output, and SCK is the serial clock.</p></td>
    </tr>
    <tr>
        <td>--</td><td>44</td><td>P1.1</td><td>AIN1/T2EX/CAP2</td>
    </tr>
    <tr>
        <td>1</td><td>45</td><td>P1.2</td><td>AIN2/PWM3/CAP3</td>
    </tr>
    <tr>
        <td>--</td><td>46</td><td>P1.3</td><td>AIN3</td>
    </tr>
    <tr>
        <td>2</td><td>47</td><td>P1.4</td><td>AIN4/SCS</td>
    </tr>
    <tr>
        <td>3</td><td>48</td><td>P1.5</td><td>AIN5/MOSI</td>
    </tr>
    <tr>
        <td>4</td><td>1</td><td>P1.6</td><td>AIN6/MISO</td>
    </tr>
    <tr>
        <td>5</td><td>2</td><td>P1.7</td><td>AIN7/SCK</td>
    </tr>
    <tr>
        <td>--</td><td>21</td><td>P2.0</td><td>A8</td><td rowspan="8"><p>When P2 accesses the external bus, it will automatically switch to the push-pull output temporarily, and output the upper 8 bits of the address A8~A15 as needed.</p><p>MOSI1, MISO1, SCK1: SPI1 interface, MOSI1 is the master output, MISO1 is the master input, and SCK1 is the serial clock output.</p><p>PWM1, PWM2: PWM1 output, PWM2 output.</p><p>TNOW: UART1 is sending an output indication.</p><p>T2EX_/CAP2_: T2EX/CAP2 pin mapping.</p><p>RXD1, TXD1: UART1 serial data input, serial data output.</p><p>DA7: Output address A7 when accessing the external bus in direct address mode.</p></td>
    </tr>
    <tr>
        <td>--</td><td>22</td><td>P2.1</td><td>MOSI1/A9</td>
    </tr>
    <tr>
        <td>--</td><td>23</td><td>P2.2</td><td>MISO1/A10</td>
    </tr>
    <tr>
        <td>--</td><td>24</td><td>P2.3</td><td>SCK1/A11</td>
    </tr>
    <tr>
        <td>--</td><td>25</td><td>P2.4</td><td>PWM1/A12</td>
    </tr>
    <tr>
        <td>11</td><td>26</td><td>P2.5</td><td>TNOW/PWM2/A13/T2EX_/CAP2_</td>
    </tr>
    <tr>
        <td>12</td><td>27</td><td>P2.6</td><td>RXD1/A14</td>
    </tr>
    <tr>
        <td>13</td><td>28</td><td>P2.7</td><td>TXD1/DA7/A15</td>
    </tr>
    <tr>
        <td>--</td><td>4</td><td>P3.0</td><td>RXD</td><td rowspan="8"><p>RXD, TXD: UART0 serial data input, serial data output.</p><p>INT0, INT1: External interrupt 0, External interrupt 1 input.</p><p>LED0, LED1, LEDC: LED serial data 0, 1, clock output.</p><p>!A15: External bus address A15 Inverting output for chip select.</p><p>T0, T1: Timer 0, Timer 1 External input.</p><p>XCS0: External bus address 4000h~7FFFh Chip select output.</p><p>DA6: Output address A6 when accessing the external bus in direct address mode.</p><p>WR, RD: External bus write signal, read signal.</p></td>
    </tr>
    <tr>
        <td>--</td><td>7</td><td>P3.1</td><td>TXD</td>
    </tr>
    <tr>
        <td>7</td><td>8</td><td>P3.2</td><td>LED0/INT0</td>
    </tr>
    <tr>
        <td>--</td><td>9</td><td>P3.3</td><td>LED1/!A15/INT1</td>
    </tr>
    <tr>
        <td>8</td><td>10</td><td>P3.4</td><td>LEDC/XCS0/T0</td>
    </tr>
    <tr>
        <td>--</td><td>11</td><td>P3.5</td><td>DA6/T1</td>
    </tr>
    <tr>
        <td>--</td><td>12</td><td>P3.6</td><td>WR</td>
    </tr>
    <tr>
        <td>--</td><td>13</td><td>P3.7</td><td>RD</td>
    </tr>
    <tr>
        <td>--</td><td>20</td><td>P4.0</td><td>LED2/A0/RXD1_</td><td rowspan="8"><p>A0~A5: Outputs the lower 6-bit address A0~A5 when accessing the external bus in direct address mode.</p><p>LED2, LED3: LED serial data 2, 3 output.</p><p>RXD1_, TNOW_/TXD1_: RXD1, TNOW/TXD1 pin mapping.</p><p>PWM3_/CAP3_: PWM3/CAP3 pin mapping.</p><p>PWM1_, PWM2_: PWM1, PWM2 pin mapping.</p><p>XI, XO: external crystal oscillator input, inverting output.</p><p>SCS_, SCK_: SPI0 Chip Select SCS, SCK pin mapping.</p></td>
    </tr>
    <tr>
        <td>--</td><td>19</td><td>P4.1</td><td>A1</td>
    </tr>
    <tr>
        <td>--</td><td>15</td><td>P4.2</td><td>PWM3_/CAP3_/A2</td>
    </tr>
    <tr>
        <td>--</td><td>14</td><td>P4.3</td><td>PWM1_/A3</td>
    </tr>
    <tr>
        <td>--</td><td>6</td><td>P4.4</td><td>LED3/TNOW_/TXD1_/A4</td>
    </tr>
    <tr>
        <td>--</td><td>5</td><td>P4.5</td><td>PWM2_/A5</td>
    </tr>
    <tr>
        <td>9</td><td>16</td><td>P4.6</td><td>XI/SCS_</td>
    </tr>
    <tr>
        <td>10</td><td>17</td><td>P4.7</td><td>X0/SCK_</td>
    </tr>
    <tr>
        <td>15</td><td>32</td><td>P5.0</td><td>DM</td><td rowspan="2">DM, DP: USB host HUB0 or D-, D+ signal terminal of USB device.</td>
    </tr>
    <tr>
        <td>14</td><td>31</td><td>P5.1</td><td>DP</td>
    </tr>
    <tr>
        <td>--</td><td>30</td><td>P5.4</td><td>HM/ALE/XB</td><td rowspan="2"><p>XB, XA: B/inverting, A/in-phase signal terminal of iRS485.</p><p>ALE: Address latch signal output in multiplexed address mode.</p><p>!A15: External bus address A15 Inverting output for chip select.</p><p>HM, HP: The USB host extends the D- and D+ signals of HUB1.</p></td>
    </tr>
    <tr>
        <td>--</td><td>29</td><td>P5.5</td><td>HP/!A15/XA</td>
    </tr>
    <tr>
        <td>6</td><td>3</td><td>P5.7</td><td>RST</td><td>External reset input with built-in pull-down resistor.</td>
    </tr>

</table>