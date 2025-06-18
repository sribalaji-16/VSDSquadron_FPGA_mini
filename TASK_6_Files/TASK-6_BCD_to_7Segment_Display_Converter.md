# Task6: BCD to 7-segment Display Converter
The code file were linked in this [link](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/tree/main/Task_5%266_Code_Files) given.  
## What this project does

A BCD to 7-segment converter, also known as a BCD to 7-segment decoder, is a circuit that converts a binary-coded decimal (BCD) input into a signal that can drive a 7-segment display. The 7-segment display is a common electronic display device used to show decimal digits. This project is used to implement Verilog code in the VSDSquadronFM to display the decimal digits in the 7-segment-display.

## Required Software and Hardware Components

### Hardware
- VSDSquadronFM
- A common cathode 7-segment-display
- Breadboard
- Male to male and male to female jumper wires
- 3V battery

### Software
- Only the virtual Ubuntu software

## Step 1: Analysis of the Existing Verilog Code

This is the Verilog code for a BCD (Binary Coded Decimal) to 7 segment display converter. [code top.v](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/Top%206.v)

### Verilog Code (`top.v`)

```verilog
module top(
    bcd,
    seg
);

input [3:0] bcd;
output [6:0] seg;
reg [6:0] seg;

always @(bcd)
begin
    case (bcd) 
        0 : seg = 7'b0000001;
        1 : seg = 7'b1001111;
        2 : seg = 7'b0010010;
        3 : seg = 7'b0000110;
        4 : seg = 7'b1001100;
        5 : seg = 7'b0100100;
        6 : seg = 7'b0100000;
        7 : seg = 7'b0001111;
        8 : seg = 7'b0000000;
        9 : seg = 7'b0000100;
        default : seg = 7'b1111111; 
    endcase
end

endmodule
```

### Module

```verilog
module top(
    bcd,
    seg
);
```

- `module`: A keyword in Verilog used to define a module (basic building block in Verilog).
- `top`: The name of the module.
- `(bcd, seg)`: The input and output ports of the module. Here, `bcd` is the input, and `seg` is the output.

### Inputs and Outputs

```verilog
input [3:0] bcd;
output [6:0] seg;
reg [6:0] seg;
```

- `input [3:0] bcd`: Declares `bcd` as a 4-bit input representing a Binary Coded Decimal (BCD) digit (values 0–9). `[3:0]` specifies that `bcd` is 4 bits wide.
- `output [6:0] seg`: Declares `seg` as a 7-bit output representing the 7 segments (a, b, c, d, e, f, g) of a 7-segment display.
- `reg [6:0] seg`: Declares `seg` as a 7-bit register type. Since `seg` is assigned inside an always block, it must be declared as a reg.

### Always Block

```verilog
always @(bcd)
```

- `always`: A keyword in Verilog used to define a procedural block that executes whenever a specified event occurs.
- `@(bcd)`: Specifies that the always block will execute whenever there is any change in the `bcd` input.

### Case Statement

```verilog
case (bcd) //case statement
```

- `case (bcd)`: A case construct is used to define multiple conditions. Here, the value of `bcd` determines which block of code will execute.

Each line assigns a 7-bit binary value to `seg` for a specific `bcd` value. These binary values control which of the 7 segments (a, b, c, d, e, f, g) of a 7-segment display are turned ON (0) or OFF (1).

Example:

```verilog
0 : seg = 7'b0000001;
```

- `0`: Condition when `bcd` is 0.
- `seg = 7'b0000001`: Assigns the binary value `0000001` to `seg`.

Explanation:
- `7'b`: Indicates a 7-bit binary value.
- `0000001`: Turns ON segments a, b, c, d, e, f (to display "0") and turns OFF segment g.

### Default Case

```verilog
default : seg = 7'b1111111; 
```

- `default`: Specifies the action to take when `bcd` does not match any of the defined conditions (i.e., values other than 0–9).
- `seg = 7'b1111111`: Turns OFF all segments of the 7-segment display.

This code is typically used in digital circuits where numeric data needs to be displayed on a 7-segment display, such as digital clocks, counters, and calculators.

## Step 2: Analysis of the PCF File

```
set_io bcd[0] 42
set_io bcd[1] 43
set_io bcd[2] 44
set_io bcd[3] 45
set_io seg[0] 46
set_io seg[1] 13
set_io seg[2] 48
set_io seg[3] 3
set_io seg[4] 4
set_io seg[5] 12
set_io seg[6] 26
```

This is the PCF file for the project. There are 4 input pins (`bcd[0]`, `bcd[1]`, `bcd[2]`, `bcd[3]`) and seven output pins (`seg[0]` to `seg[6]`). You can set the GPIO pins yourself.

## Step 3: Block and Circuit Diagrams

- **Block diagram**

![Blockdiagram6](https://github.com/user-attachments/assets/1de52bc8-56fa-4e34-8bfa-8b64e47f9421)

- **Circuit diagram**

![circuit diagram 4](https://github.com/user-attachments/assets/e141f65c-521e-4782-bb66-1e6171d8ded4)

(*Add appropriate images or descriptions for these diagrams if needed.*)

## Step 4: Implementing in the VSDSquadronFM

Make sure you have copied the following files: [`top.v`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/Top%206.v), [`Makefile`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/Makefile%206.txt), and the [`PCF`]() file. Place all these in the folder created inside the `VSDSquadron_FM` directory named [`7_segement`](https://github.com/sribalaji-16/VSDSquadron_FPGA_mini/blob/main/Task_5%266_Code_Files/Top%206.v).

### To implement the code on FM, follow these steps:

1. Go to software Ubuntu and open the terminal. Ensure that the FM is connected by typing:
   ```bash
   lsusb
   ```

2. Navigate to the folder:
   ```bash
   cd <folder name>
   ```

3. Build the binaries:
   ```bash
   make build
   ```

4. Program the board:
   ```bash
   sudo make flash
   ```

You have now successfully implemented the code in the FM.

## Step 5: Testing and Verification

Connect the GPIO pins to the input pins of the 7-segment display as follows:

| Pins of the 7 Segment Display | GPIO Pins |
|------------------------------|------------|
| a                            | 46         |
| b                            | 13         |
| c                            | 48         |
| d                            | 3          |
| e                            | 4          |
| f                            | 12         |
| g                            | 26         |

You can place four DIP switches or four pushbuttons on the input side to control the decimal digits presented on the 7-segment display. If you are getting the decimal numbers, you have successfully completed this project.
![image](https://github.com/user-attachments/assets/81ba4a98-76c6-49e4-9c61-977543ea395f)

