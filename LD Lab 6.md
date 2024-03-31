# K-Maps & Don't Care

- We are given the following ``minterm`` with 4 variables. ``dc`` stands for ``don't care``.
$f = m\left(1,5,6,11,12,13,14\right) + dc\left(4\right)$

![[Lab6Ex1.png]]

- We make use of the ``don't care`` to group ``minterms`` in our advantage. The only difference is that if a ``don't care`` remains ungrouped, we don't need to group it. Functionally, in a real circuit, a ``don't care`` represents a state/value the circuit is physically unable to achieve.
- We can't define a ``minterm`` and a ``don't care`` for the same number. 

 - For the K-MAP above, the minimized function is: ``F = BD' (purple) + BC' (yellow) + A'C'D (blue) + AB'CD (green)``



- $f = m\left(1,2,6,7,8,13,14,15\right) + dc\left(0,3,5,12\right)$

![[Lab6Ex2.png]]

- ``F = A'B' (blue) + A'C (green) + AB (yellow) + AC'D' (purple)``

# Behavioral Modeling 

## The Always Block
- Even though, by default, instructions are executed in parallel in Verilog, we sometimes need to execute instructions in a sequential order. The **always** block allows us to achieve this:
```Verilog
always @(...)
begin
...
end
```

- This always block will trigger only when one of the elements in the sensitivity list changes:
```Verilog
always @(a) // this will trigger when the variable 'a' changes
always @(*) // this will trigger when anything changes
```

- A good practice is to always fill the sensitivity list with the appropriate variables. If left empty, the block will always execute, becoming a **blocking** instruction.

## 7 Segment Display - Common Anode

- Common Anode means the segments are lit up on the value ``0`` and turned off on ``1``, so the opposite of logical operators for true and false.

![[7SegmentDisplay.png]]

- We want to display HEX characters from 1 to F. We will display the characters B, D in lowercase to avoid confusions:
	- ``1, 2, 3, 4, 5, 6, 7, 8, 9, A, b, C, d, E, F``
- For example, to display the character ``0``, all segments need to be ``0`` except ``G``.

- In Verilog, we'll need:
	- An input on 4 bits to allow values from 0 to 15.
	- An output on 7 bits, because we have 7 display segments
	- To use the ``always`` block that will only update the output when the input changes
	- To use a ``case`` statement to behaviorally describe the circuit
- ``main.v``:
```Verilog
module main(input [3:0]in, output reg[6:0]out);
	// we need to use reg for the output, beause otherwise the out variable won't be stored in memory and the 'always' block won't know when to update.
	always @(in)
	begin
		case(in)
		// x'b means x bits in binary, x'h means x bits in hex
			4'h0: out = 7'b0000001; 
			4'h1: out = 7'b1001111;
			4'h2: out = 7'b0010010;
			4'h3: out = 7'b0000110;
			4'h4: out = 7'b1001100;
			4'h5: out = 7'b0110100;
			4'h6: out = 7'b0100000;
			4'h7: out = 7'b0001111;
			4'h8: out = 7'b0000000;
			4'h9: out = 7'b0001100;
			4'ha: out = 7'b0001000;
			4'hb: out = 7'b1100000;
			4'hc: out = 7'b0110001;
			4'hd: out = 7'b1000010;
			4'he: out = 7'b0110000;
			4'hf: out = 7'b0111000;
		endcase
	end
endmodule
```

- ``prj.tcl`` for running the code on the ``Intel DE10-Lite`` FPGA board:
```Bash
project_new example1 -overwrite

set_global_assignment -name FAMILY MAX10
set_global_assignment -name DEVICE 10M50DAF484C7G 

set_global_assignment -name BDF_FILE example1.bdf
set_global_assignment -name VERILOG_FILE main.v
set_global_assignment -name SDC_FILE example1.sdc

set_global_assignment -name TOP_LEVEL_ENTITY main
set_location_assignment -to clk PIN_AH10

set_location_assignment PIN_C10 -to in[0];
set_location_assignment PIN_C11 -to in[1];
set_location_assignment PIN_D12 -to in[2];
set_location_assignment PIN_C12 -to in[3];

set_location_assignment PIN_C14 -to out[6];
set_location_assignment PIN_E15 -to out[5];
set_location_assignment PIN_C15 -to out[4];
set_location_assignment PIN_C16 -to out[3];
set_location_assignment PIN_E16 -to out[2];
set_location_assignment PIN_D17 -to out[1];
set_location_assignment PIN_C17 -to out[0];


load_package flow
execute_flow -compile

project_close

```