+++
title = "Yosys to FPGA"
date = 2013-10-29
aliases = [
	"2013/10/29/yosys-to-fpga.html"
]
+++

YoSys can output synthesised designs targeted for Xilinx primitives.
So lets have a go!
<!--more-->

This tutorial is targeted for the Zynq ZEDBOARD development kit. (zedboard.org).
Porting to other Xilinx kits is as simple as modifying the user-constraints file
and changing the part-number.

# example.v

{{< highlight verilog >}}
module top(clk, ctrl, led_7, led_6, led_5, led_4, led_3, led_2, led_1, led_0);

input clk, ctrl;
output led_7, led_6, led_5, led_4;
output led_3, led_2, led_1, led_0;

reg [31:0] counter;

always @(posedge clk)
	   counter <= counter + (ctrl ? 4 : 1);

assign {led_7, led_6, led_5, led_4, led_3, led_2, led_1, led_0} = counter >> 24;

endmodule
{{< / highlight >}}

The module is simple, it takes a clk signal and outputs leds at various divisions of
the clock frequency. (A binary counter).

The ctrl signal can be used to speed up the clk counter increment by 4x.

# Synthesis script

{{< highlight bash >}}
#!/bin/bash
set -ex

XILINX_DIR=/opt/Xilinx/14.7/ISE_DS/ISE
XILINX_PART=xc7z020clg484-1

yosys - <<- EOT
	read_verilog example.v
	synth_xilinx -edif synth.edif
EOT

$XILINX_DIR/bin/lin64/edif2ngd -a synth.edif synth.ngo
$XILINX_DIR/bin/lin64/ngdbuild -p $XILINX_PART -uc example.ucf synth.ngo synth.ngd
$XILINX_DIR/bin/lin64/map -p $XILINX_PART -w -o mapped.ncd synth.ngd constraints.pcf
$XILINX_DIR/bin/lin64/par -w mapped.ncd placed.ncd constraints.pcf
$XILINX_DIR/bin/lin64/bitgen -w placed.ncd example.bit constraints.pcf

$XILINX_DIR/bin/lin64/promgen -w -b -p bin -o example.bin -u 0 example.bit -data_width 32
{{< / highlight >}}

The script executes YoSys, and gets it to synthesise our simple counter example.
The next commands do the following:

1. Convert the synthesised output from yosys (in edif format) into the native xilinx format.
2. { part of above } ??
3. Map the design to the targetted part's technology.
4. Place and route the design. (Hopefully we can use torc for this soon).
5. Generate a bitstream binary file.

6. This is an optional stage which byte-swaps the bit file so it can be piped into the Zynq devcfg device easily.


# User Constraints file

{{< highlight text >}}
NET "clk" TNM_NET = clk;
TIMESPEC TS_clk = PERIOD "clk" 50 MHz HIGH 50%;

NET "clk" LOC = Y9		   | IOSTANDARD=LVCMOS33;  # "GCLK"
NET "ctrl" LOC = P16		   | IOSTANDARD=LVCMOS18;  # "BTNC"

NET "led_0" LOC = T22 | IOSTANDARD=LVCMOS33;  # "LD0"
NET "led_1" LOC = T21 | IOSTANDARD=LVCMOS33;  # "LD1"
NET "led_2" LOC = U22 | IOSTANDARD=LVCMOS33;  # "LD2"
NET "led_3" LOC = U21 | IOSTANDARD=LVCMOS33;  # "LD3"
NET "led_4" LOC = V22 | IOSTANDARD=LVCMOS33;  # "LD4"
NET "led_5" LOC = W22 | IOSTANDARD=LVCMOS33;  # "LD5"
NET "led_6" LOC = U19 | IOSTANDARD=LVCMOS33;  # "LD6"
NET "led_7" LOC = U14 | IOSTANDARD=LVCMOS33;  # "LD7"
{{< / highlight >}}


# Synthesis

Simply run:

    bash example.sh

# Download to zedboard

You can use the xilinx tools to download the bitstream via JTAG....
Or even better, just place a pre-built bootthunder binary on your sd-card and load the bitstream from the command line.


BOOT.BIN