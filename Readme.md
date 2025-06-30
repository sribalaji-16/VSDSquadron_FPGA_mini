
# VSDSquadron FPGA Mini Research Project

The VSDSquadron FPGA Mini (FM) board is an affordable, compact tool for prototyping and embedded system development. With powerful ICE40UP5K FPGA, onboard programming, versatile GPIO access, SPI flash, and integrated power regulation, it enables efficient design, testing, and deployment, making it ideal for developers, hobbyists, and educators exploring FPGA applications.

![image](https://github.com/user-attachments/assets/ae8e7a0b-ef4d-4b00-932d-e335220666ad)

## Architecture Diagram

The iCE40 UltraPlus family architecture contains an array of Programmable Logic Blocks (PLB), two Oscillator Generators, two user configurable I2C controllers, two user configurable SPI controllers, blocks of sysMEM™ Embedded Block RAM (EBR) and Single Port RAM (SPRAM) surrounded by Programmable I/O (PIO). the block shows the diagram of the iCE40UP5K device. 

![image](https://github.com/user-attachments/assets/934c230b-03ab-4b99-b03b-50eb66384c49)


## Block Diagram

The VSDSquadron FPGA Mini (FM) board features the Lattice ICE40UP5K FPGA with the following capabilities:

![Blockdiagram1](https://github.com/user-attachments/assets/adfdf761-374a-461b-8051-586612d992c7)

- 48-lead QFN package
- 5.3K LUTs for flexible logic design
- 1Mb SPRAM and 120Kb DPRAM for efficient memory usage
- Onboard FTDI FT232H USB-to-SPI interface for programming and communication
- All 32 FPGA GPIO accessible for rapid prototyping
- Integrated 4MB SPI flash for configuration and data storage
- RGB LED for user-defined signaling
- Onboard 3.3V and 1.2V power regulators, with the ability to supply 3.3V externally

Further details about this FPGA can be known from its [datasheet](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_1_Files/iCE40%20UltraPlus%20Family%20Data%20Sheet.pdf).
