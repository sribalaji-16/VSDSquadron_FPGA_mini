# Final Documentation Report

This report compiles all the outcomes and procedures from the implementation.

### Functional Overview of the Verilog Code

The Verilog module is designed to control the RGB LED using a clock-driven PWM mechanism. An internal frequency counter determines the intensity and combination of the red, green, and blue channels, allowing dynamic color transitions on the LED output.

### Pin Assignment Summary

The following pin mappings are defined in the `.pcf` file:

+ `led_red`   → Pin 39 — Drives the red color output.
+ `led_blue`  → Pin 40 — Drives the blue color output.
+ `led_green` → Pin 41 — Drives the green color output.
+ `hw_clk`    → Pin 20 — Clock signal source.
+ `testwire`  → Pin 17 — Outputs a debug or test signal.

### Integration Workflow

+ Examined the hardware datasheet to understand I/O mapping.
+ Matched the datasheet pinout with Verilog I/O and PCF mappings.
+ Connected the board through USB-C, ensuring FTDI detection.
+ Executed the following Makefile targets:
  + `make clean`
  + `make build`
  + `sudo make flash`
+ Observed the LED output pattern to verify correct programming.
+ Final LED behavior can be seen in the recorded video [here](https://github.com/user-attachments/assets/c7c5b021-d3b8-4a99-b1d9-c037566a84ae).

### Challenges & How They Were Resolved

+ **VirtualBox Extension Troubles**: Faced issues with installing the VirtualBox extension pack. Initially, the application failed to launch properly. The workaround was to open the extension file directly, which surprisingly resolved the issue.
+ **Initial Confusion**: Understanding the FPGA development workflow was overwhelming at first. Online documentation and community forums were invaluable for grasping the toolchain and board usage.
