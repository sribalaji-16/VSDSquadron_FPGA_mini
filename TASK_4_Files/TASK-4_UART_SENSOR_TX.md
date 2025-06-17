
# Task-4

## Objective  
Implement a UART transmitter that sends data based on sensor inputs, enabling the FPGA to communicate real-time sensor data to an external device.

---

## Overview  
This document explains the Verilog implementation of a UART (Universal Asynchronous Receiver Transmitter) Transmitter module with the 8N1 configuration `uart_trx.v`. This is a transmit-only module, meaning it sends serial data over a single `tx` line. The module includes logic for:

- Start bit generation  
- 8-bit data transmission (LSB first)  
- Stop bit generation  
- State machine-based control for sequential UART behavior  

---

## Port Descriptions

| Port Name | Direction | Description |
|-----------|-----------|-------------|
| clk       | Input     | Clock signal for synchronous state machine operation |
| txbyte    | Input     | 8-bit data to be transmitted |
| senddata  | Input     | Trigger signal to start transmission |
| txdone    | Output    | Indicates completion of transmission |
| tx        | Output    | UART transmit line |

---

## Internal Parameters and Registers

### Parameters (FSM States):

```verilog
parameter STATE_IDLE=8'd0;
parameter STATE_STARTTX=8'd1;
parameter STATE_TXING=8'd2;
parameter STATE_TXDONE=8'd3;
```

These define the finite state machine (FSM) states for tracking transmission progress.

### Registers:

- `state`: Current FSM state  
- `buf_tx`: Buffer to hold a copy of txbyte for bit-wise shifting  
- `bits_sent`: Counter for number of bits transmitted  
- `txbit`: Internal signal connected to tx, drives actual serial line  
- `txdone`: Indicates when transmission is complete  

---

## Functional Behavior

### Idle State (STATE_IDLE)

- UART line (tx) remains HIGH.  
- If `senddata` is HIGH, the module transitions to `STATE_STARTTX`, stores `txbyte` into `buf_tx`.

### Start Bit (STATE_STARTTX)

- Transmits the start bit (0).  
- Proceeds to `STATE_TXING`.

### Transmit Data Bits (STATE_TXING)

- Serially shifts and sends 8 bits (LSB first) from `buf_tx`.  
- Each bit is assigned to `txbit`.  
- After 8 bits are sent, transitions to stop bit phase.

### Stop Bit (within STATE_TXING completion)

- Sends a HIGH signal (1) as stop bit.  
- Resets bit counter and moves to `STATE_TXDONE`.

### Transmission Done (STATE_TXDONE)

- Sets `txdone = 1` to signal transmission completion.  
- FSM returns to `STATE_IDLE`.

---

## Sensor Data Acquisition & UART Transmission

- **Data Source**: There's no real sensor — a 28-bit counter (`frequency_counter_i`) is used to simulate sensor data by incrementing on each clock cycle.  
- **Triggering UART**: Bit 24 of the counter toggles slowly and is used as a trigger (`senddata`) to initiate UART transmission.  
- **UART Output**: A constant character `'D'` is transmitted every time `senddata` goes high using the `uart_tx_8n1` module.  
- **Clocking**: A 9600 baud clock is derived from the 12 MHz internal oscillator.  
- **Visualization**: The RGB LED is connected to the UART RX pin, likely for visual debugging.  

> **Note**:  
> To transmit real sensor data, you would need to:
> - Replace the counter with actual sensor input  
> - Store the data in `txbyte`  
> - Trigger `senddata` based on sampling conditions  

---

## Block Diagram  
*(Not included in text)*

---

## Circuit Diagram  
*(Not included in text)*

---

## FPGA Implementation and Verification

- Create the `top.v`, `uart_trx.v`, `Makefile`, and `VSDSquadronFM.pcf` files in a directory and execute the previous steps for FPGA implementation.  
- The FPGA board includes a built-in USB-UART bridge so it will automatically appear as a COM port on a PC.  
- The TX data can be monitored using a serial terminal like PuTTY.  
- This task demonstrates how to establish communication between an FPGA and a computer using a USB-C cable via the UART protocol.  
- The FPGA continuously transmits the character `'D'` in an infinite loop.  

> - Since this implementation is transmit-only, pressing keys on the keyboard will have no effect on the output.  
> - The design does not include a receive function, meaning the FPGA does not process or respond to any data sent from the PC.

To verify the UART communication, PuTTY need to be installed which is a free and open-source terminal emulator. Hence, follow the previous steps mentioned in **Task-3**.

---

## Final Output
