# Integrating with the VSDSquadron FPGA Mini Board

>## Requirements

1. **Install Dependencies**
   Install the open-source toolchain for Lattice iCE40 FPGAs:

   ```bash
   sudo apt update
   sudo apt install -y yosys nextpnr-ice40 icestorm dfu-util make git
   ```

   These tools include:

   * `yosys`: synthesizer
   * `nextpnr-ice40`: place and route tool
   * `icepack`, `iceprog`, and `dfu-util`: flashing tools

2. **Clone the Repository**
   Clone the LED example that contains the `Makefile`:

   ```bash
   git clone https://github.com/thesourcerer8/VSDSquadron_FM.git
   cd VSDSquadron_FM/led_blue
   ```

>## Connecting the FPGA Board

1. Use a **USB-C cable** to connect the FPGA board to your computer.
2. Verify the **FTDI interface**(USB-to-serial communication bridge) is recognized:

   ```bash
   ls /dev/ttyUSB*
   ```

   If nothing appears, verify the cable and drivers. Use `dmesg | grep FTDI` to check detection.

>## Reviewing Datasheet and Project Files

* **Datasheet**: Review to understand pin functions, power requirements, and FTDI setup. View the datasheet pdf by clicking upon [Datasheet](https://github.com/sribalaji-16/VSD_FPGA/blob/main/Task_1_Files/iCE40%20UltraPlus%20Family%20Data%20Sheet.pdf)
* **PCF File**: Maps Verilog signals to physical pins. Example:

  ```pcf
  set_io led_blue A5
  ```
* **Verilog Code**: Confirm top module signals match the PCF file.

>## Building and Flashing the Design

From within the `led_blue` directory:

1. **Clean Previous Builds**

   ```bash
   make clean
   ```

2. **Build the Bitstream**

   ```bash
   make build
   ```

   This step runs synthesis, place-and-route, and generates a `.bin` file for flashing.

3. **Flash the FPGA**
   Connect the board and execute:

   ```bash
   sudo make flash
   ```

   This typically uses `iceprog` via FTDI.

>## Confirming Successful Programming

* Observe the **RGB LED** on the board.
* The `led_blue` design should blink the blue LED.


>## Troubleshooting Tips

* If `iceprog` doesn't detect the board:

  ```bash
  dmesg | tail
  ```

  Look for USB connection errors.

* You might need to press the **reset** or **boot** button before flashing.

* Manually flash with:

  ```bash
  sudo iceprog -S your_file.bin
  ```

