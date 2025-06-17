# TASK-1

## Objective
To understand and document the provided Verilog code for LED blinking, create the necessary PCF file, and integrate the design with the VSDSquadron FPGA Mini board using the provided datasheet. (install tools as explained in datasheet)

---

## 1. Understanding the Verilog Code

The Verilog implementation is available in the GitHub repository at LINK. This repository includes various projects designed for the VSDSquadron_FM board, leveraging open-source FPGA tools for development. It showcases how to drive RGB LEDs using an internal oscillator and a counter-based approach to generate the required control signals.

### Purpose of the Module:
The top module is designed to control an RGB LED using an internal high-frequency oscillator (SB_HFOSC). It features:

- A frequency counter driven by the internal oscillator.
- A test signal output based on the frequency counter.
- An RGB LED driver instantiated with configurable current parameters.

### Description of Internal Logic and Oscillator

#### Internal Oscillator
- The module uses the SB_HFOSC primitive to generate an internal clock signal.
- The parameter `CLKHF_DIV = "0b10"` determines the clock division factor.
- The oscillator is always enabled using `CLKHFPU` and `CLKHFEN`, both set to `1'b1`.

#### Frequency Counter
- A 28-bit counter (`frequency_counter_i`) increments at each rising edge of `int_osc`.
- The test signal output (`testwire`) is driven by bit 5 of the frequency counter.

### Functionality of the RGB LED Driver

#### RGB LED Driver
This primitive is used to control the RGB LED.

- The driver enables the LED output via the `RGBLEDEN` input.
- Each color channel (Red, Green, Blue) is controlled by the corresponding PWM input signals (`RGB0PWM`, `RGB1PWM`, `RGB2PWM`).

In this configuration:

- Red (`RGB0PWM`) = 0 (off)
- Green (`RGB1PWM`) = 0 (off)
- Blue (`RGB2PWM`) = 1 (on)

The output is connected to hardware pins `led_red`, `led_green`, and `led_blue`.

The current for each channel is set using `defparam` statements, limiting it to a small predefined value (`0b000001`).

### Summary
The module generates an internal clock using `SB_HFOSC`. A 28-bit counter increments on each clock cycle, with bit 5 controlling a test output. The RGB LED is controlled via `SB_RGBA_DRV`, with only the blue LED turned on in this implementation.

---

## 2. Creating the PCF File

### Pin Mapping and Functionality
To ensure proper functionality, the pin assignments specified in the PCF (Physical Constraints File) must be verified against the VSDSquadron FPGA Mini board datasheet. Below is an analysis of the assigned pins and their roles in the design.

The following table summarizes the pin assignments:

| Signal Name | Assigned Pin | Purpose                     |
|-------------|---------------|------------------------------|
| led_red     | 39            | Controls the red LED         |
| led_blue    | 40            | Controls the blue LED        |
| led_green   | 41            | Controls the green LED       |
| hw_clk      | 20            | Input clock signal           |
| testwire    | 17            | Debugging/General-purpose signal |

These pin mappings ensure the correct hardware connections for controlling the RGB LED and monitoring test signals on the FPGA board. By ensuring the correct mapping, the module can drive the RGB LED and monitor internal operations effectively.

---

## 3. Integrating with the VSDSquadron FPGA Mini Board

Install the required softwares and and setup the Linux environment as specified in the respective datasheet.

To navigate through project directories in a UNIX environment, use the following commands:

```sh
cd
cd VSDSquadron_FM
cd blink_led
```
To make sure that the board is connected to the Oracle Virtual Machine. Perform below steps:

Connect the board to your PC through USB-C.

On the Virtual Machine, click on "Devices → USB → FTDI Single RS232-HS [0900]".

To confirm if the board is connected to the USB, type the lsusb command in the terminal. It should show a line stating "Future Technology Devices International".

To program in the VSDSquadron FPGA Mini (FM) board, follow these steps:
```sh
make clean     # To clean up previous builds.
make build     # To build the binaries for the FPGA board.
sudo make flash  # To flash the code to the external SRAM.
```
## 4. Output

![image]()

//video add

The verilog file and pcf file required to blink the RGB led are RGB_blink.v & VSD_FM.pcf respectively.

Summary of the above verilog file:
The module uses an internal oscillator (SB_HFOSC) to generate a clock signal.
A 28-bit counter runs on this clock to create a time-dependent pattern.
The RGB LED driver (SB_RGBA_DRV) receives PWM signals derived from specific bits of the counter.
The LEDs blink in a specific pattern, alternating between red, green, and blue.

This is a simple FPGA-based blinking RGB LED circuit that can be used for debugging, status indication, or decorative lighting.

