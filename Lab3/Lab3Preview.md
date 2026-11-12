Lab 3 is 
For part 1 this is my schematic: 

<img width="550" height="673" alt="image" src="https://github.com/user-attachments/assets/da195ac5-ba6a-44e7-9cc0-41cf935cc707" />
<img width="576" height="549" alt="image" src="https://github.com/user-attachments/assets/77d3436d-782a-45a5-887d-bf8ed9168fde" />

In Part 1, a 16-bit input is connected to a generic priority encoder. The encoder checks the input bits and determines the highest-numbered bit that is set to 1. It outputs the binary index of that bit.

The encoder output is sent to the hex2sseg module, which converts the 4-bit binary value into the correct seven-segment display pattern. The result is displayed on one seven-segment digit.

This part demonstrates how a priority encoder can determine which input has priority and how binary values can be displayed on an FPGA seven-segment display.

For more information about go to [Part 1 here](Lab3_Part1.md)

For part 2 this is my schematic:

In Part 2, a reusable seven-segment display driver is created using the decoder_generic and hex2sseg modules.

The decoder_generic module selects one of the eight seven-segment display positions using the active_digit input. The hex2sseg module converts the 4-bit num input into the segment pattern needed to display a hexadecimal number.

The first_sseg_driver module combines both functions:
- active_digit selects which physical digit is active.
- num determines which number is displayed.
- DP_ctrl controls the decimal point.
- AN controls the active display position.
- sseg controls the individual segments.
The test module connects the same 3-bit input to both the selected digit and the displayed number. For example, if the input is 101, digit 5 displays the number 5.

For more information about go to [Part 1 here](Lab3_Part2.md)

For part 3 this is my schematic:

<img width="1013" height="669" alt="image" src="https://github.com/user-attachments/assets/cf72c5f5-dd7f-4cff-9de1-de2e2a22b3aa" />

In Part 3, the seven-segment display driver is connected to the simple calculator developed in an earlier lab.

The calculator accepts two 4-bit inputs and a 2-bit operation selector. It performs addition, subtraction, or multiplication and produces an 8-bit binary result.

The result is processed through the following modules:
1. simple_calc calculates the result.
2. bin2bcd converts the 8-bit binary result into a 12-bit BCD value.
3. The BCD value is separated into ones, tens, and hundreds digits.
4. mux_4x1_nbit selects one of the BCD digits.
5. first_sseg_driver displays the selected digit on the seven-segment display.
The selector input chooses both the BCD digit and the physical display position:
00 → ones digit on display digit 4
01 → tens digit on display digit 5
10 → hundreds digit on display digit 6
11 → fourth selection on display digit 7

The calculator’s carry_out and overflow signals are also connected to LEDs on the FPGA board.

This part demonstrates how multiple smaller hardware modules can be combined to create a complete digital system.

For more information about go to [Part 1 here](Lab3_Part3.md)

