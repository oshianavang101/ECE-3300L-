The Goal for Part 1 is to have the binary index is displayed on a seven-segment display. We create the binary index by having the priority encoder receives several input bits and finds which input has priority.

Part 1 uses a 16-bit input, a priority encoder, and a seven-segment display.

The priority encoder examines the input vector and determines which input bit has priority. The index of the highest active input is converted into a binary number and displayed on the seven-segment display.

The main signal flow is:

```text
16-bit input
     │
     ▼
priority_encoder_generic
     │
     ▼
4-bit binary output
     │
     ▼
hex2sseg
     │
     ▼
seven-segment display
```
The priority encoder receives:
input [N-1:0] w

and produces:
output y

The parameter N determines how many input bits the encoder has.

For this part:
N = 16 (which is apart of the test not main code)

Therefore:
w = 16 input bits
y = 4 output bits

Four bits are enough because:
2^4 = 16

The active bit is bit 2, so:
y = 0010

The seven-segment display then displays the number 2.
Priority behavior
The encoder uses a loop to check the input bits:
for (k = 0; k < N; k = k + 1)

If an input bit is active:
if (w[k])
    y = k;

the output is updated with that bit's index.

Because the loop checks from the lowest index to the highest index, a higher-numbered active bit overwrites a lower-numbered active bit.

For example:
w[2] = 1
w[5] = 1

The encoder eventually outputs:
y = 5

Therefore, the highest-numbered active input has priority.
The z output
The encoder also produces:
assign z = |w;

This is a reduction OR.

It checks whether any input bit is equal to 1:
w = 0000_0000_0000_0000 → z = 0
w = 0000_0000_0000_0100 → z = 1

The z output indicates whether the input contains at least one active bit.
hex2sseg
The priority encoder output is a binary number, but a seven-segment display requires individual segment-control signals.

The hex2sseg module converts the four-bit binary value into a seven-segment pattern.

For example:
y = 4'b0101

represents the number 5. hex2sseg turns on the correct segments to display 5.

The seven segment outputs are:
sseg[0] = segment a
sseg[1] = segment b
sseg[2] = segment c
sseg[3] = segment d
sseg[4] = segment e
sseg[5] = segment f
sseg[6] = segment g

Display digit selection
The FPGA's digit-enable signals are active-low.

This means:
0 = digit enabled
1 = digit disabled

To activate display digit 0:
assign AN = 8'b11111110;

All other display digits remain disabled.

Here is the [FPGA test](https://github.com/oshianavang101/ECE-3300L-/issues/1) that I did
