# Creating Physical Constraints (PCF) File  


> ## **Overview of PCF Files** :
A `Physical Constraints File (PCF)` defines the relationship between logical signals in a Verilog/HDL design and the physical pins of an FPGA. This ensures that inputs/outputs in the code are correctly connected to the target hardware. For the RGB LED controller, "the PCF file establishes how signals like LED colors and clock inputs map to the FPGA’s I/O pins".  


> ## **PCF File Format and Syntax**  
Each entry in the PCF file follows this structure:  
+Syntax
```verilog
set_io <LOGICAL_SIGNAL_NAME> <PHYSICAL_PIN_NUMBER>  
```  
+ **Logical Signal**: Name defined in the Verilog module  
+ **Physical Pin**: Hardware-specific pin number on the FPGA board 

> ## **Creating the PCF file**
```verilog
set_io  led_red	39
set_io  led_blue 40
set_io  led_green 41
set_io  hw_clk 20
set_io  testwire 17
```

> ## **Pin Mapping Configuration**  
Below are the critical pin assignments for the RGB LED controller design:  

>> ### **LED Control Pins**  
| Logical Signal | Physical Pin | Direction | Functionality Description |  
|----------------|--------------|-----------|----------------------------|  
| `led_red`      | 39           | Output    | Drives the **red** LED channel using PWM. Adjusting the duty cycle varies the red intensity, enabling color blending |  
| `led_blue`     | 40           | Output    | Controls the **blue** LED channel. Modulating this signal allows mixing with red/green to produce hues like purple or cyan. |  
| `led_green`    | 41           | Output    | Manages the **green** LED channel. Combined with red/blue, it enables colors such as yellow or teal. |  

>> ### **Clock and Debugging Pins**  
| Logical Signal | Physical Pin | Direction | Functionality Description |  
|----------------|--------------|-----------|----------------------------|  
| `hw_clk`       | 20           | Input     | Primary clock input (e.g., 12 MHz oscillator). Synchronizes the PWM generator and frequency counter. |  
| `testwire`     | 17           | Output    | Debugging signal to monitor internal oscillator stability or validate frequency counter behavior. |  


> ## **Design-Specific Notes**  

>> ### **RGB LED Operation**  
+ **Color Mixing**: By varying PWM signals on `led_red`(`set_io led_red 39`), `led_green`(`set_io led_green 41`), and `led_blue`(`set_io led_blue 40`), the RGB LED combines red, green, and blue light intensities to produce a broad color spectrum  
+ **PWM Requirements**: Each LED channel’s PWM frequency must exceed 60 Hz to avoid visible flicker  

>> ### **Clock Signal Considerations**  
+ The `hw_clk` input (pin 20) [`set_io hw_clk 20`] determines the timing resolution of the PWM signals. A higher-frequency clock enables finer brightness adjustments  
+ The `testwire` (pin 17) [`set_io testwire 17`] can be probed with an oscilloscope to verify the internal oscillator’s accuracy during testing  


> ## **Validation and Troubleshooting**  
+ **LED Validation**: Verify all three LEDs light up individually by temporarily setting their PWM duty cycles to 100%  
+ **Clock Debugging**: Use the `testwire` [`set_io testwire 17`] signal to confirm the oscillator frequency matches expectations  


>## **Summary**  
This PCF file ensures the FPGA correctly interfaces with the RGB LED and external clock. Proper pin assignments are critical for achieving accurate color output and reliable timing. Developers should cross-reference this documentation with the board’s datasheet to confirm pin numbers match the target hardware.  

