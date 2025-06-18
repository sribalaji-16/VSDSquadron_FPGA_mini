# Task-2

## 🎯 Objective  
Implement a UART loopback mechanism where transmitted data is immediately received back, facilitating testing of UART functionality.

---

## 🧩 UART Transmitter Module (8N1)

This document explains the **8N1 UART Transmitter Module** written in Verilog. The module is designed to transmit serial data in an **8 data bits, No parity, 1 stop bit (8N1)** format. It follows a **state machine approach** to control data transmission over a UART-compatible serial interface.

### 🔍 Explanation of the 8N1 UART Tx Verilog Code:

---

### 📥 Module Breakdown

#### Inputs:
- `clk`: System clock for synchronizing transmission.  
- `txbyte[7:0]`: The 8-bit data byte to be transmitted.  
- `senddata`: A control signal that initiates transmission.  

#### Outputs:
- `tx`: The serial output line (to be connected to a receiver).  
- `txdone`: A flag that indicates transmission completion.  

#### Internal Registers:
- `state`: Holds the current FSM state.  
- `buf_tx`: A shift register that holds the byte to be transmitted.  
- `bits_sent`: A counter to track the number of transmitted bits.  
- `txbit`: Holds the current bit to be transmitted.  

---

### 🧠 Finite State Machine (FSM)

The transmission process follows a **4-state FSM**:

1. **STATE_IDLE (0)**  
   - TX line remains HIGH (idle state).  
   - Waits for `senddata` signal to start transmission.  
   - If `senddata = 1`, it loads `txbyte` into `buf_tx` and moves to `STATE_STARTTX`.

2. **STATE_STARTTX (1)**  
   - Transmits the **start bit (0)** to indicate the beginning of byte transmission.  
   - Moves to `STATE_TXING`.

3. **STATE_TXING (2)**  
   - Sends the **8 data bits** LSB-first using `buf_tx`.  
   - Shifts the buffer after each clock cycle.  
   - Once all bits are transmitted, moves to `STATE_TXDONE`.

4. **STATE_TXDONE (3)**  
   - Transmits the **stop bit (1)** to indicate the end of the byte transmission.  
   - Sets `txdone = 1` to notify that transmission is complete.  
   - Returns to `STATE_IDLE` for the next transmission.

---

## 🧱 UART Top-Level Module

This document explains the **top-level UART module** implemented in Verilog. It includes:

- External UART transmitter/receiver module (`uart_trx.v`)
- Internal oscillator
- Frequency counter
- RGB LED driver  

The module is designed for serial communication and visual indication using an LED.

---

### 📥 Module Breakdown

#### Inputs:
- `hw_clk`: External hardware clock  
- `uartrx`: UART receive pin (data input)  

#### Outputs:
- `uarttx`: UART transmit pin (data output)  
- `led_red`, `led_blue`, `led_green`: RGB LED control signals  

---

### ⚙️ Internal Oscillator

- Uses `SB_HFOSC`, an internal high-frequency oscillator (specific to Lattice FPGAs).  
- Parameter `.CLKHF_DIV("0b10")` sets the oscillator frequency division.  
- `CLKHFPU` and `CLKHFEN` are both set to `1'b1` to enable the oscillator.

---

### 🔁 UART Transmission Handling

- Implements a **loopback UART**, where received UART data (`uartrx`) is directly transmitted (`uarttx`).

---

### ⏱️ Frequency Counter

- A **28-bit counter (`frequency_counter_i`)** increments at every positive edge of `int_osc`.  
- Can be used for baud rate generation (requires additional logic).

---

### 🔦 RGB LED Driver

- Uses `SB_RGBA_DRV`, a Lattice FPGA RGB LED driver.  
- `RGB0PWM`, `RGB1PWM`, and `RGB2PWM` are PWM inputs, controlled by `uartrx`.  
- Actual LED pins (`RGB0`, `RGB1`, `RGB2`) are mapped to `led_green`, `led_blue`, and `led_red`.  
- `"0b000001"` represents a small current level to control brightness.

---

## 📊 Block Diagram (Conceptual Description)

![Blockdiagram2](https://github.com/user-attachments/assets/b76e173c-a199-49f0-b25d-9e0723aa0119)


- **Clock Signal**: An internal oscillator generates the system clock to drive the baud rate generator.
- **UART Tx & Rx**: UART Transmitter sends serial data via TX. In loopback, TX is connected directly to RX.
- **UART Receiver**: Converts serial data back to parallel. This is processed further.
- **LED Indicators**: Connected to GPIOs to show when data is transmitted or received.

---

## 🛠️ FPGA Implementation of UART Loopback

- Create files: `top.v`, `uart_trx.v`, `Makefile`, and `VSDSquadron_FM.pcf`.  
- Execute previous steps for FPGA implementation.

### No External Wiring Needed:
- If TX is routed to RX internally in the FPGA design, no wiring is needed.  
- In Verilog: `assign uartrx = uarttx;` routes TX data directly to RX.  

### USB-UART Interface:
- If the FPGA includes this, it appears as a **COM port** on your PC.  
- Use a **serial terminal** (e.g., `minicom`) to test loopback.

---
## Dump the code 

![image](https://github.com/user-attachments/assets/8ef38444-9c54-420e-b109-79f81a9a0d13)

## 🧪 Steps to Test the UART Loopback in Minicom

1. ✅ Installation

```bash
sudo apt-get install minicom 
```


2. Changing configuration settings
```bash
sudo minicom -s
```
![image](https://github.com/user-attachments/assets/e2dd0f7e-cbf9-4ca1-b597-2baebd34b42d)

3. In 'serial port setup' set 'Serial Device' as /dev/ttyUSB0 and save dfl.

![image](https://github.com/user-attachments/assets/99ac9195-eaeb-4e24-bf97-e5eaa162d5c8)

4. Press ctrl+A then Z for help on special keys

![image](https://github.com/user-attachments/assets/e809c7d6-01bc-4f1b-950f-d5408fe9ac74)

5. Press 'Enter' then ctrl+A and then press key E to enable echo. If the UART loopback is successful then the typed input will be displayed twice on the screen.

![image](https://github.com/user-attachments/assets/f17f3d5b-ce2d-4566-b149-1435fa6be954)

## Output

![image](https://github.com/user-attachments/assets/ccdef9e5-3ba2-4232-bc28-fcfd0a58e7ff)



https://github.com/user-attachments/assets/a364106b-d857-4f0d-a3b9-f77cba626dc8


