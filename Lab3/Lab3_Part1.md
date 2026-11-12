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

Here is the [FPGA test](https://github.com/oshianavang101/ECE-3300L-/issues/1) that I did
