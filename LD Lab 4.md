## Function optimization: 
- `A^1 is NOT A`
$$\begin{aligned}F=ABC^{1}D^{1}+ABC^{1}D+AB^{1}C^{1}D+ABCD+AB^{1}CD+ABCD^{1}+AB^{1}CD^{1},\\
F=ABC^{1}\left( D^{1}+D\right) +ACD\left( B+B^{1}\right) +ACD^{1}\left( B^{1}+B\right) +AB^{1}C^{1}D,\\
F=ABC^{1}+ACD+ACD^{1}+AB^{1}C^{1}D,\\
F=AC\left( D+D^{1}\right) +AC^{1}\left( B+B^{1}D\right) ,\\
F=AC+AC^{1}\left( B+D\right) ,\\
F=AC+ABC^{1}+AC^{1}D,\\
F=A\left( C+C^{1}D\right) +ABC^{1},\\
F=AC+AD+ABC^{1},\\
F=A\left( C+C^{1}B\right) +AD\Rightarrow F=AB+AC+AD\end{aligned}$$

- Implementation example of a parent module that wraps around two child modules ( instances of the same module 'C' ):
 ![[Interconnected_Modules.png]]
```Verilog
module circuit(input [1:0]a, [1:0]b, input s, output [1:0]x, output s_out);
	// input/output [1:0] means a input/output on two bits which can be accessed like an array ([0], [1])
	// when we declare 's', it is a best practice to write 'input' again, so the interpreter won't think it's on two bits

	wire s1; // the wire between the two circuits
	C celula1(a[0], b[0], s, x[0], s1);
	// 'C' is the module, and 'celula1' is the name of the declared module

	C celula2(a[1], b[1], s1, x[1], s_out);
	// 's_out' is the final 's'
endmodule
```

## Ripple-Carry Adder (4 bits)

![[Ripple_Carry_Adder_4bits.png]]

- The first 'Full-Adder' can be replaced with a 'Half-Adder'
- For running the code on the FPGA, access 'Additional-Resources' > 'Altera CMD Line Script Example'
- FPGA's name is ``Intel DE10-Lite``. You can find its PIN layout online.

- ``fulladder.v``
```Verilog
module fulladder(input ai, input bi, input ci, output si, output ci1);
	assign si = (~ai&(bi^ci))|(ai&~(bi^ci));
	assign ci1 = (bi&(ai^ci))|(ai&ci);
endmodule
```

- ``circuit.v``
```Verilog
circuit(input[3:0]a, [3:0]b, input c, output [3:0]s, output s_out);
	wire s1;
	fulladder fa1(a[0],b[0], c, s[0], s1);
	wire s2;
	fulladder fa2(a[1],b[1], s1, s[1], s2);
	wire s3;
	fulladder fa3(a[2],b[2], s2, s[2], s3);
	fulladder fa4(a[3],b[3], s3, s[3], s_out);
endmodule
```

- ``circuit_tb.v``
```Verilog
module circuit_tb;
	reg [3:0]a;
	reg [3:0]b;
	reg c;
	
	wire [3:0]s;
	wire s_out;
	
	circuit uut(
		.a(a),
		.b(b),
		.c(c),
		.s(s),
		.s_out(s_out)
	);

	initial begin
		a = 0;
		b = 0;
		c = 0;
		#100;
	end

	always begin
		#25 a = a + 1'b1;
		#50 b = b + 1'b1;
		#75 c = ~c;
	end
endmodule
```

- For running the module on the FPGA:

- ``prj.tcl``
```TCL
set_global_assignment -name FAMILY MAX10
set_global_assignment -name DEVICE 10M50DAF484C7G 

set_global_assignment -name BDF_FILE example1.bdf
set_global_assignment -name VERILOG_FILE circuit.v
set_global_assignment -name SDC_FILE example1.sdc

set_global_assignment -name TOP_LEVEL_ENTITY circuit
set_location_assignment -to clk PIN_AH10

set_location_assignment PIN_C10 -to a[0]   ;
set_location_assignment PIN_C11 -to a[1]  ;
set_location_assignment PIN_D12 -to a[2]  ;
set_location_assignment PIN_C12 -to a[3]  ;

set_location_assignment PIN_A12 -to b[0]  ;
set_location_assignment PIN_B12 -to b[1]  ;
set_location_assignment PIN_A13 -to b[2]  ;
set_location_assignment PIN_A14 -to b[3]  ;

set_location_assignment PIN_B14 -to c  ;

set_location_assignment PIN_A8 -to s[0]  ;
set_location_assignment PIN_A9 -to s[1]  ;
set_location_assignment PIN_A10 -to s[2]  ;
set_location_assignment PIN_B10 -to s[3]  ;

set_location_assignment PIN_D13 -to s_out  ;

load_package flow
execute_flow -compile

project_close
```

- ``placuta.v`` (both modules need to be in the same file)
```Verilog
module fulladder(input ai, input bi, input ci, output si, output ci1);
	assign si = (~ai&(bi^ci))|(ai&~(bi^ci));
	assign ci1 = (bi&(ai^ci))|(ai&ci);
endmodule

module circuit(input [3:0]a, input [3:0]b, input c, output [3:0]s, output s_out);
	wire s1;
	fulladder fa1(a[0],b[0], c, s[0], s1);
	wire s2;
	fulladder fa2(a[1],b[1], s1, s[1], s2);
	wire s3;
	fulladder fa3(a[2],b[2], s2, s[2], s3);
	fulladder fa4(a[3],b[3], s3, s[3], s_out);
endmodule
```
