- **FPGA** boards, **Verilog HDL**
```Verilog
module <module_name>(<port_type_1><port_name_1>, ...);
	// without '<>'
endmodule
```
- The module name should be the same as the file name (convention). It facilitates easier file recognition.

- Port types: ``input``, ``output``, ``inout``
- Local variables: ``wire``, ``reg``

- ``Vsim`` (Linux simulator for the ``verilog`` described circuits) - ``Questa Intel Starter FPGA edition``

- ``and_gate.v`` (Project>Create New File - File type should be Verilog)
```Verilog
// output is always first
// 'input a' is equivalent to 'input [0:1]a'
// so, by default it will be on one bit
module and_gate(output o, input a, input b);
	assign o=a&b; // or and(o,a,b);
	// where 'and' is a primitive in verilog
endmodule
```

- ``and_gate_tb.v`` (``tb`` is a convention and it stands for ``test bench``)
```Verilog
module and_gate_tb;
	// inputs
	// we use reg to assign values to variables
	reg a;
	reg b;

	// outputs
	// ?wire links to variable?
	wire o;

	// 'main' is the name of the first module
	// 'uut' stands for unit under test
	and_gate uut(
		.o(o), 
		.a(a),
		.b(b)
	);
	// .o references to 'main' module 'o', 
	// which we link to the 'o' declared here

	initial begin
		a = 0;
		b = 0;
		// we wait 100 nanoseconds to make
		// sure the operations above execute
		#100;
	end
	
	always begin
		#20 a = ~a;
		#30 b = ~b;
	end

endmodule

```

- To run the simulation:
	- Library > Work > ``and_gate_tb`` > Simulate
	- Right click on ``and_gate_tb`` inside ``sim view``, and click ``add wave``
	- Besides ``100ns``, there's a Run button.
		- You can change the time frame to ``1000ns`` to see a wider simulation
	- To quit the simulation type ``quit -sim`` inside the ``questa (or nsim)`` console.

- The ``Quartus`` software is for running the ``verilog`` described behavior on a FPGA board. 