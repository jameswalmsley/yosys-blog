---
layout: article
title: Simple Gates
next_section: combinational/simple-gates
permalink: /verilog/combinational/simple-gates/
section_path: verilog
---

We'll start by implementing some logic gates as verilog modules. These are the basis
of all combinational (sometimes called combinatorial) logic that make up all digital
circuits.

# Verilog Modules

You can think of a module as defining a block box which has a number of input signals,
and at least 1 or more output signals. Within the black box some function is performed,
the implementation of which is not necessarily known to the "user".

{{< highlight verilog >}}
module and_gate(a, b, q);
    input  a, b;
    output q;

    assign q = (a & b);
endmodule
{{< /highlight>}}

{{< note "Black-boxes (modules) with no outputs.">}}
	No matter how complex a module might be, if it provides no outputs then it matches an
	equivalent module that contains no logic, and does nothing. All good synthesis tools will
	optimise such constructs away.
{{< /note>}}

Now we have a description of a digital circuit we can synthesise it.

Synthesis is the process of taking a (relatively) high-level description of a digital circuit
and expressing it in a low-level (usually gate-level) form.

{{< highlight verilog >}}
read_verilog and_gate.v
show
{{< /highlight>}}

Which of produces the following circuit in YoSys.

![YoSys and gate](/verilog/combinational/simple-gates/and_gate.png)

An exciting result, nothing has happened! We have an exact representation in YoSys of our original description.
Well no surprises there, as gates are usually the building blocks of such circuits.

{{< note "Are logic gates fundamental?">}}

	Logic gates are themselves built from transistors. However it is rarely necessary to look at gates at such a low-level.
	In ASIC design it may be useful to get the synthesis tool to map our logic gates to transistor circuits available to us.

	These fundamental building blocks used in different technologies are referred to as cells.
{{< /note>}}

Thats not the full story, we haven't actually synthesised our design. Load the design in YoSys once more and
run the following commands:

{{< highlight verilog >}}
read_verilog and_gate.v
techmap
opt
show
{{< /highlight>}}

This loads the design, maps the design to our default technology (logic gates) and then optimises the design.
The result is subtley different.

![YoSys synthesised and gate](/verilog/combinational/simple-gates/and_gate_synth.png)

Note the name of the gate, its now $_AND_ instead of $and. $and is a behavioural representation of the AND boolean
function, and $_AND_ is a fundamental AND gate.

# Closing the Gate

Ok such simple gates are kind of boring, nothing special will happen, but for completion here are some
basic gates in verilog:
{{< highlight verilog >}}
module not_gate(a, q);
    input  a;
    output q;

    assign q = ~a;
endmodule

module and_gate(a, b, q);
    input  a, b;
    output q;

    assign q = (a & b);
endmodule

module or_gate(a, b, q);
    input  a, b;
    output q;
    assign q = (a | b);
endmodule

module xor_gate(a, b, q);
    input a, b;
    output q;
    assign q = (a ^ b);
endmodule
{{< /highlight>}}

Now we've learnt some fundamentals aspects of YoSys and implementing logic gates in verilog, lets move on
to some more complex combinational logic functions.

{{< note "Gate Primitives in Verilog">}}
	Our verilog examples are actually behavioral descriptions. After running the YoSys techmap and opt commands
	on these designs, run the write_verilog command and compare the verilog output.

	We shall cover verilog primitive instantiation in a later tutorial.
{{< /note>}}