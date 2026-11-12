Part 3: Calculator Connected to the Seven-Segment Display
Purpose
Part 3 connects the simple calculator from the previous lab to the seven-segment display driver.

The calculator produces an eight-bit binary result. That result is converted into three decimal BCD digits and displayed one digit at a time.

The overall signal flow is:
x, y, op_sel
      │
      ▼
simple_calc
      │
      ├── result[7:0] ──► bin2bcd
      ├── carry_out ────► LED
      └── overflow ─────► LED
                              │
                              ▼
                         BCD result
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
          hundreds           tens            ones
                              │
                              ▼
                           MUX
                              │
                              ▼
                    first_sseg_driver

Calculator inputs
The top-level module receives:
input [3:0] x
input [3:0] y
input [1:0] op_sel

The two four-bit inputs are the calculator operands.

The op_sel signal chooses the operation:
00 → addition
01 → subtraction
1x → multiplication

The calculator produces:
result
carry_out
overflow

The result is eight bits because multiplication of two four-bit numbers can require up to eight bits.
Calculator result
The calculator produces a binary result.

For example, decimal 137 is represented in binary as:
10001001

However, the seven-segment display needs separate decimal digits:
1, 3, 7

This is why the binary result is sent to bin2bcd.
Binary-to-BCD conversion
The bin2bcd module converts:
8-bit binary result

into:
12-bit BCD result

The 12-bit output is divided into three four-bit groups:
bcd_result[11:8] = hundreds digit
bcd_result[7:4]  = tens digit
bcd_result[3:0]  = ones digit

For decimal 137:
hundreds = 0001
tens     = 0011
ones     = 0111

Together:
bcd_result = 12'b0001_0011_0111

Separating the BCD digits
The code separates the digits using:
assign ones = bcd_result[3:0];
assign tens = bcd_result[7:4];
assign hundreds = bcd_result[11:8];

After this separation, each digit can be sent to a different input of the MUX.
The four-to-one MUX
The MUX receives four possible inputs:
.w0(ones)
.w1(tens)
.w2(hundreds)
.w3(4'b0000)

The MUX selection input is digit_sel.

The MUX behavior is:
digit_sel = 00 → select ones
digit_sel = 01 → select tens
digit_sel = 10 → select hundreds
digit_sel = 11 → select 0000

The selected output is connected using:
.f(selected_digit)

The output port is called f in the provided MUX module. It is not called y.
Selecting the physical display digit
The same two-bit digit_sel signal is also used to choose the physical display digit.

The code:
.active_digit({1'b1, digit_sel})

adds a leading 1 to the two-bit selector.

This creates the following mapping:
digit_sel = 00
active_digit = 100
physical digit 4 is selected

digit_sel = 01
active_digit = 101
physical digit 5 is selected

digit_sel = 10
active_digit = 110
physical digit 6 is selected

digit_sel = 11
active_digit = 111
physical digit 7 is selected

The MUX chooses the number, while active_digit chooses where the number appears.

For example, if the calculator result is 137:
digit_sel = 00 → digit 4 displays 7
digit_sel = 01 → digit 5 displays 3
digit_sel = 10 → digit 6 displays 1

Carry and overflow outputs
The calculator's status outputs are passed directly to the top-level outputs:
.carry_out(carry_out)
.overflow(overflow)

These can be connected to the FPGA LEDs through the constraint file.

carry_out indicates that an addition or subtraction generated a carry.

overflow indicates that the signed arithmetic result cannot be represented correctly using the available number of bits.
Module Hierarchy
The final structure of the lab is:
Part 1:
prior_encoder_test
├── priority_encoder_generic
└── hex2sseg

Part 2:
first_sseg_driver_test
└── first_sseg_driver
    ├── decoder_generic
    └── hex2sseg

Part 3:
simple_calc_first_sseg
├── simple_calc
│   ├── adder_subtractor
│   ├── csa_multiplier
│   └── mux_2x1_8bit
├── bin2bcd
├── mux_4x1_nbit
└── first_sseg_driver
    ├── decoder_generic
    └── hex2sseg

Important Verilog Details
Parameters
A parameter allows a module to be reused with different sizes.

For example:
priority_encoder_generic #(.N(16))

creates a 16-input encoder.
mux_4x1_nbit #(.N(4))

creates a MUX whose inputs and output are four bits wide.
Concatenation
The expression:
{1'b1, digit_sel}

joins signals together.

If:
digit_sel = 2'b01

then:
{1'b1, digit_sel} = 3'b101

Bit slicing
The expression:
bcd_result[7:4]

selects bits 7 through 4 from the 12-bit BCD result.

The three BCD slices are:
bcd_result[3:0]
bcd_result[7:4]
bcd_result[11:8]

Active-low signals
The display digit controls are active-low:
0 = on
1 = off

That is why the decoder output is inverted before connecting to AN.
Overall Purpose of Lab 3
Part 1 demonstrates how to find the highest active input and display its index.

Part 2 packages the display-selection and number-conversion logic into a reusable driver.

Part 3 combines the calculator, binary-to-BCD converter, multiplexer, and display driver into one complete FPGA system.

The main lesson is hierarchical design:
```text
small modules
     │
     ▼
combined modules
     │
     ▼
complete FPGA application
```
