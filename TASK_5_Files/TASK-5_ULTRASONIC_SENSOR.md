
# Task 5: Real-Time Sensor Data Acquisition and Transmission System

The code file were linked in this [link](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/tree/main/Task_5%266_Code_Files) given.   

## What this project does
This theme interfaces with the sensors to receive the data sensed, and sends this data to the FPGA. Then with the GPIO pins, it transmits the output results to an external device (example: buzzer). In this project, we will make a touchless bell using the VSDSquadronFM board and with a sensor named HC-SR04 ultrasonic sensor.

## Required Software and Hardware Components

### Hardware
- VSDSquadronFM FPGA board
- HC-SR04 ultrasonic sensor
- A buzzer

### Software
- Virtual Ubuntu software (for programming)
- Docklight

## Step 1: Analysis of the Existing Verilog Code
The existing Verilog code can be accessed [here](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/top%205%20.v). The top module integrates an ultrasonic sensor to measure distance, transmits the measured distance over UART, and controls RGB LEDs based on the distance.

### Module Declaration
The module has several input and output ports:

**Outputs**
- `led_red`, `led_blue`, `led_green`: Control the RGB LEDs.
- `uarttx`: UART transmission pin.
- `trig`: Trigger output for the ultrasonic sensor.
- `buzzer`: Buzzer signal.

**Inputs**
- `uartx`: UART receiver pin.
- `hw_clk`: Hardware clock input.
- `echo`: Echo signal from the ultrasonic sensor.

### Internal Oscillator and Counter
Uses a high-frequency oscillator (`SB_HFOSC`) to generate a 12 MHz clock signal (`int_osc`). And generates a 9600 Hz clock (`clk_9600`) using a counter (`cntr_9600`) to divide the 12MHz clock.

### Ultrasonic Sensor Signals
Declares signals for distance measurement and sensor control:
- `distanceRAW`, `distance_cm`: Raw and processed distance values.
- `sensor_ready`: Indicates if the sensor is ready.
- `measure`: Control signal for measurement.
- `buzzer_signal`: Signal to control the buzzer.

### Finite State Machine to Print `distance_cm` as ASCII
Implements an FSM to convert the distance measurement (`distance_cm`) into ASCII characters and transmit via UART:

**States:**
- `IDLE`: Waits for sensor readiness.
- `DIGIT_4` to `DIGIT_0`: Converts each digit of the distance value to ASCII.
- `END_CR`: Sends carriage return (CR).
- `SEND_LF`: Sends line feed (LF).
- `DONE`: Completes the transmission.

The FSM ensures that the distance is converted to ASCII and sent over UART, and the LEDs provide a visual indication of proximity.

## Analysis of the Ultrasonic Sensor Module (`ultra_sonic_sensor.v`)
The Verilog code for the HC-SR04 module can be accessed [here](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/ulta_sonic_sensor.v).

### Module Declaration
**Inputs:**
- `clk`: System clock input.
- `measure`: Signal to start the measurement.
- `state`: Current state of the finite state machine (FSM).

**Outputs:**
- `ready`: Indicates if the module is ready for a new measurement.
- `echo`: Echo signal from the ultrasonic sensor.
- `trig`: Trigger signal to the ultrasonic sensor.
- `distanceRAW`: Raw distance measurement in clock cycles.
- `distance_cm`: Converted distance in centimeters.
- `buzzer_signal`: Signal to activate a buzzer if the distance is less than or equal to 5 cm.

### State Definitions
- `IDLE`: Waiting for a measurement pulse.
- `TRIGGER`: Sending the trigger pulse.
- `WAIT`: Waiting for the echo signal.
- `COUNTECHO`: Counting the duration of the echo signal.

### Finite State Machine (FSM)
- The FSM transitions between states based on the measure signal and the echo signal.
- In the `TRIGGER` state, it generates a trigger pulse.
- In the `WAIT` state, it waits for the echo signal to go high.
- In the `COUNTECHO` state, it counts the duration of the echo signal.

### Distance Measurements
- The `distanceRAW` counter increments while the echo signal is high.
- The raw distance is then converted to centimeters using the formula:  
  `distance_cm = (distanceRAW * 34300) / (2 * 12000000)`.
- If the distance is less than or equal to 5 cm, the `buzzer_signal` is activated.

## Refresher Module
The `refresher50ms` module generates a measurement pulse every 50 milliseconds.

## Step 2: Block Diagram of the Pin Connection and Processing Flow
_Alt text_

### Internal Processing Flow
_Alt text_

## Step 3: Implementation on the FM Board
Make sure you have copied the following files: [`top.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/top%205%20.v), [`ultra_sonic_sensor.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/ulta_sonic_sensor.v), [`uart_trx.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/ulta_sonic_sensor.v), [`Makefile`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/Makefile%205.txt), and [`VSDSquadronFM.pcf`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/VSDSquadronFM%205.pcf). Place them all in a directory named [`touchless_Switch`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/ulta_sonic_sensor.v) inside the `VSDSquadron_FM` folder.

### To implement the code on FM, follow these steps:
1. Go to Ubuntu software and open the terminal. Ensure the FM is connected by typing `lsusb`.
2. Navigate to the folder using `cd <folder name>`.
3. Type `make build` to build the binaries.
4. Type `sudo make flash` to program the board.

Now you have successfully implemented the code on the FM.

## Step 4: Testing and Verification
Follow the steps to test using Docklight:

- Set the baud rate to 9600 and 1 stop bit.
- Ensure the correct COM port is selected from the Device Manager.
- Test with objects: Place an object above the sensor. The buzzer will beep when the object is within a field of 5 cm.

## Step 5: Final Documentation
The project has three Verilog files including [`top.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/top%205%20.v). This module integrates sensor data acquisition, signal processing, and communication for an FPGA-based system. The Verilog file [`ultra_sound_sensor.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/ulta_sonic_sensor.v) sets up the HC-SR04 sensor to measure distance and trigger a buzzer if the distance is below a specified threshold.

### Ultrasonic Sensor Pin Connections

![Blockdiagram5](https://github.com/user-attachments/assets/ee45d613-7ad4-4b70-887f-cb72d307e8e4)

 
### Internal Processing Diagram

![circuit diagram 3](https://github.com/user-attachments/assets/1f56546d-d685-4f3b-86b2-2d88dd6099cd)


### Run the following steps to implement the codes:
1. Open the terminal in Ubuntu and ensure FM is connected with `lsusb`.
2. Navigate to the project folder: `cd <folder name>`.
3. Build the binaries: `make build`.
4. Flash the board: `sudo make flash`.

### Test using Docklight by following these steps:
- Set the baud rate to 9600 and 1 stop bit.
- Check the connected COM port in Device Manager.
- Place an object above the sensor. The buzzer will beep when within a 5 cm range.
