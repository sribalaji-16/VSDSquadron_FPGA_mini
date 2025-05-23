# ***Documentation of Task 1**  

---

> ### **Module Purpose**

Controls an RGB LED using a hardware oscillator and internal counter logic. Provides static color output and a test signal for debugging.

---

> ### **Internal Logic & Oscillator**
1. #### **Internal Oscillator**:  
   - Uses `SB_HFOSC` primitive (Lattice Semiconductor-specific) to generate a high-frequency clock (`int_osc`).  
   - Configured with a divider (`0b10`) to reduce the oscillator frequency.  

2. #### **Counter Logic**:  
   - A 28-bit counter (`frequency_counter_i`) increments on every rising edge of `int_osc`.  
   - Bit 5 of the counter drives `testwire`, creating a lower-frequency test signal (observable externally).  

---

> ### **RGB LED Driver**
1. #### **Primitive Instantiation**  
   - Uses `SB_RGBA_DRV` (Lattice RGB driver primitive) to interface with the RGB LED hardware.  
   - **PWM Control**:  
     - `RGB0PWM` (red) is enabled (`1'b1`), while `RGB1PWM` (green) and `RGB2PWM` (blue) are disabled (`1'b0`).  
     - **Result**: Only the red LED is active in this configuration.  

2. #### **Current Configuration** 
   - `RGB0_CURRENT`, `RGB1_CURRENT`, `RGB2_CURRENT` set to `0b000001` (minimum drive strength for all colors).  
   - **Output Mapping**:  
     - `RGB0` → `led_red` (hardware connection for red LED).  
     - `RGB1` → `led_green`, `RGB2` → `led_blue` (disabled in this design).  

---

> ### **Key Relationships**:  
- `hw_clk` (external clock) is unused; the design relies on the internal oscillator (`int_osc`).  
- The counter (`frequency_counter_i`) drives `testwire` but does not affect LED behavior (LEDs are statically configured).  
- LED outputs are directly tied to the RGB driver’s PWM enable signals, with only `led_red` active.  

---

> ### **Summary**:  

This module demonstrates a basic `RGB LED control` system using an `internal oscillator` and static PWM settings. The red LED remains constantly lit, while `testwire` provides a debug signal derived from a counter clocked by the oscillator. The design can be extended to enable dynamic color changes by modifying the PWM signals and counter logic.
