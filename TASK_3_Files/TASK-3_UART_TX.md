
# Task-3

## 🎯 Objective  
Develop a UART transmitter module capable of sending serial data from the FPGA to an external device.

---

## 🧾 Introduction 
The top module code , pcf file and other file are in this [link](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/tree/main/TASK_3_Files/uart_tx).  
The **8N1 UART Transmitter Module** is a Verilog-based design that implements a simple Universal Asynchronous Receiver Transmitter (UART) transmitter with:

- 8-bit data frame  
- No parity  
- 1 stop bit (8N1)  

This [module](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/uart_trx.v) is responsible for **transmitting a byte of data serially** through a `tx` (transmit) wire, following the standard UART protocol. Below is the detailed description of the UART transmission module ([`uart_tx.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/uart_trx.v)) in Verilog HDL.

---

## 🔌 Port Descriptions

| Signal   | Direction | Description                          |
|----------|-----------|--------------------------------------|
| `clk`    | Input     | System clock signal                  |
| `txbyte` | Input     | 8-bit data to be transmitted         |
| `senddata` | Input   | Start transmission when high         |
| `txdone` | Output    | Goes high when transmission completes|
| `tx`     | Output    | Serial data output                   |

---

## ⚙️ Parameters and State Variables

The design uses **four states** to control the UART transmission process:

```verilog
parameter STATE_IDLE    = 8'd0;
parameter STATE_STARTTX = 8'd1;
parameter STATE_TXING   = 8'd2;
parameter STATE_TXDONE  = 8'd3;
```

---

### 🧠 Registers Used

- `state` : Stores the current state of transmission  
- `buf_tx` : Temporary buffer to hold the byte being transmitted  
- `bits_sent` : Counter for the number of bits sent  
- `txbit` : Holds the current output bit value  
- `txdone` : Flag indicating the end of transmission  

---

## 🔄 Operation of the Module

The transmission process follows these steps:

1. **IDLE State**:  
   System remains in `STATE_IDLE` while waiting for `senddata` to go high.

2. **Start Bit Transmission**:  
   When `senddata` is received, the state transitions to `STATE_STARTTX`, where the **start bit (0)** is sent.

3. **Data Bit Transmission**:  
   Moves to `STATE_TXING`, where each bit of the `txbyte` is transmitted **serially, LSB first**.

4. **Stop Bit Transmission**:  
   After all 8 data bits are sent, a **stop bit (1)** is transmitted.

5. **Completion**:  
   The `txdone` flag is set high, and the state returns to `STATE_IDLE`.

---

## 🧱 Block Diagram and Circuit Diagram  

![Blockdiagram3](https://github.com/user-attachments/assets/755eb982-588b-43b2-a54e-397796d49a93)

![circuit diagram](https://github.com/user-attachments/assets/fcfefb6c-9e3b-46ac-b488-644403e5187d)

---

## 🛠️ FPGA Implementation and Verification of UART Tx

1. Create the following files in a directory:
   - [`top.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/top.v)  
   - [`uart_tx.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/uart_trx.v)  
   - [`Makefile`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/Makefile)
   - [`VSDSquadronFM.pcf`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/TASK_3_Files/uart_tx/VSDSquadronFM.pcf)  

2. Execute the previous steps for **FPGA implementation**.

3. The FPGA board includes a **built-in USB-UART bridge**, so it will automatically appear as a **COM port** on the PC.

4. The TX data can be monitored using a serial terminal like **PuTTY**.

---

### 💻 Communication Setup Overview

- The FPGA **continuously transmits** the character `'D'` in an infinite loop.
- This is a **transmit-only** design.
- Pressing keys on the keyboard has no effect.
- The design does **not include a receive function**, so the FPGA does not process or respond to any incoming data from the PC.

---

## 🔧 Installation & Setup of PuTTY

### Dump the code

![image](https://github.com/user-attachments/assets/0d02a748-235c-409b-b6b2-c2becb3b48f5)

### 📦 Installation
- Download the appropriate MSI (Windows Installer) package based on system architecture.
- Install PuTTY and launch the application.

### ⚙️ Configuration in PuTTY
- Select **"Serial"** as the connection type.  _(In this case, it was assigned to **dev/ttyUSB0**.)_
- Enter the detected  port in PuTTY and click **"Open"** to start the session.

> **Important:**  
Before running PuTTY, the FPGA must be **physically connected** to the computer via USB cable.

---

## ✅ Final Output

https://github.com/user-attachments/assets/369a9f83-4852-4682-9b83-1c6f2cbf3e66

If everything is set up correctly, the character `'D'` should **continuously appear in PuTTY**, confirming **successful transmission**.
