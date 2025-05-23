```verilog
module top (  
  // outputs  
  output wire led_red  , // Red  
  output wire led_blue , // Blue  
  output wire led_green , // Green  
  input wire hw_clk,  // Hardware Oscillator, not the internal oscillator  
  output wire testwire  
);
```

> this module declares the output ports ,where the hw_clock is the external oscillator input

```verilog
wire int_osc;  
reg [27:0] frequency_counter_i;  

assign testwire = frequency_counter_i[5];
```
> int_osc is an internal oscillator ,frequency_counter_i is a 28-bit counter and testwire outputs bit 5 of the counter which changes periodically
```verilog
always @(posedge int_osc) begin  
  frequency_counter_i <= frequency_counter_i + 1'b1;  
end
```

>This is an always block, which runs every time int_osc (internal clock) rises this increments the counter on each clock pulse.
```verilog
SB_HFOSC #(.CLKHF_DIV ("0b10")) u_SB_HFOSC (   
  .CLKHFPU(1'b1),   
  .CLKHFEN(1'b1),   
  .CLKHF(int_osc)  
);  
```

> SB_HFOSC is an internal oscillator primitive ,.CLKHFPU(1'b1) this powers up the oscillator,.CLKHFEN(1'b1) this enables the oscillator and .CLKHF(int_osc) this Connects the oscillator output to int_osc

```verilog
SB_RGBA_DRV RGB_DRIVER (  
  .RGBLEDEN(1'b1),  
  .RGB0PWM (1'b0), // red  
  .RGB1PWM (1'b0), // green  
  .RGB2PWM (1'b1), // blue  
  .CURREN  (1'b1),  
  .RGB0    (led_red),   
  .RGB1    (led_green),  
  .RGB2    (led_blue)  
);  
```

> here this .RGBLEDEN(1'b1) this enables the RGB LED driver and power the led if it is kept high(1) in its respective port

```verilog
defparam RGB_DRIVER.RGB0_CURRENT = "0b000001";  
defparam RGB_DRIVER.RGB1_CURRENT = "0b000001";  
defparam RGB_DRIVER.RGB2_CURRENT = "0b000001";  
```
> Sets the brightness of each LED and 0b000001 represents a low current setting, making the LED dim

