# Universal Shift Register

## Overview

This project implements a **Universal Shift Register** in Verilog. The shift register supports various shift operations and parallel loading, making it versatile for different applications.

## Features

- **Shift Operations**: Supports both left and right shifts.
- **Parallel Load**: Data can be loaded from parallel inputs.
- **Clear Function**: Asynchronously clears the register.
- **Clocked Operation**: Synchronized with a clock signal.

## Modules

### Universal Shift Register

The main module implementing the Universal Shift Register:

```verilog
timescale 1ns / 1ps
module Universal_shift_reg(O , clk , clear , S , I);

input clk , clear ;
input [2 : 0] S ; 
input [3 : 0] I; 
output [3 : 0] O;
wire [3 : 0] D_temp;

Mux_8_to_1 inst1(D_temp[0] , S , O[0] , 1'b0 , O[1] , I[0] , ~O[0] , O[3] , O[1] , O[2]);
Mux_8_to_1 inst2(D_temp[1] , S , O[1] , O[0] , O[2] , I[1] , ~O[1] , O[0] , O[2] , O[3]);
Mux_8_to_1 inst3(D_temp[2] , S , O[2] , O[1] , O[3] , I[2] , ~O[2] , O[1] , O[3] , O[0]);
Mux_8_to_1 inst4(D_temp[3] , S , O[3] , O[2] , 1'b0 , I[3] , ~O[3] , O[2] , O[0] , O[1]);

D_FlipFlop D_inst1(O[0] , D_temp[0] , clk , clear);
D_FlipFlop D_inst2(O[1] , D_temp[1] , clk , clear);
D_FlipFlop D_inst3(O[2] , D_temp[2] , clk , clear);
D_FlipFlop D_inst4(O[3] , D_temp[3] , clk , clear);

endmodule
```
## Mux_8_to_1
An 8-to-1 multiplexer used to select one of eight inputs based on a 3-bit select signal.

## D_FlipFlop
A D flip-flop with asynchronous clear functionality.

### Inputs and Outputs
- **Inputs**:
  - `clk`: Clock signal for the shift register.
  - `clear`: Asynchronous clear signal.
  - `S[2:0]`: 3-bit select signal to determine the shift operation or parallel load.
  - `I[3:0]`: 4-bit parallel input data.

- **Outputs**:
  - `O[3:0]`: 4-bit output data of the shift register.

## Operations
The Universal Shift Register operates based on the select signal `S` and performs one of the following operations:

- **Parallel Load**: Loads the parallel data `I` into the register.
- **Shift Left**: Shifts all bits to the left.
- **Shift Right**: Shifts all bits to the right.


## Testbench

Write testbenches to verify the functionality of the Universal Shift Register. Testbenches should cover:
- Parallel loading
- Left and right shifts
- Clearing of the register
