Part 2 combines the digit-selection logic and the number-to-segment conversion into one reusable module called:
first_sseg_driver

Instead of manually connecting a decoder and hex2sseg every time, the driver combines them into one block.

The main signal flow is:
active_digit ──► decoder_generic ──► AN
num ───────────► hex2sseg ─────────► sseg
DP_ctrl ───────────────────────────► DP

Inputs and outputs
The driver receives:
active_digit
num
DP_ctrl

The driver produces:
sseg
AN
DP

active_digit
There are eight display positions, so three bits are required:
000 → digit 0
001 → digit 1
010 → digit 2
011 → digit 3
100 → digit 4
101 → digit 5
110 → digit 6
111 → digit 7

num
This is the four-bit number that should be displayed.

For example:
num = 4'b0011

means the display should show 3.
DP_ctrl
This controls the decimal point.

The decimal point is also active-low on the FPGA board, so the signal is inverted before being sent to the physical output.
decoder_generic
The decoder is instantiated with:
decoder_generic #(.N(3))

Since:
2^3 = 8

the decoder has eight outputs, one for each display digit.

If:
active_digit = 3'b101

then the decoder produces:
decoded_digit[5] = 1

This means digit 5 is selected.
Why AN is inverted
The decoder output is active-high:
decoded_digit[5] = 1 → digit 5 selected

However, the FPGA's physical AN signals are active-low:
AN[5] = 0 → digit 5 turns on

Therefore, the driver uses:
assign AN[i] = ~decoded_digit[i];

The inversion changes the active-high decoder signal into the active-low signal required by the board.
Seven-segment conversion
The number is sent to hex2sseg:
```text
hex2sseg number_decoder (
    .hex(num),
    .sseg(sseg)
);
```
This converts the four-bit number into the seven segment signals needed to display it.
Part 2 test module
The test module uses a three-bit input X.

It connects X to the selected display digit:
.active_digit(X)

It also converts X into a four-bit number:
.num({1'b0, X})

The expression:
{1'b0, X}

adds a leading zero to the three-bit input.

For example:
X = 3'b101
{1'b0, X} = 4'b0101

Therefore:
digit 5 displays number 5

This test demonstrates that the same input can control both:
1. Which physical digit is active
2. Which number is displayed

Here is the [FPGA test for Part 2](https://github.com/oshianavang101/ECE-3300L-/issues/1) that I did
