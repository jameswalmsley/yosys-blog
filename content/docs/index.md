+++
layout = "docs"
title = "Install"
aliases = [
    "/docs/home/"
]
+++

This is the documentation section of the learning Verilog with YoSys blog.
I aim to add documentation about the features of YoSys I've used as we go along.

## So what is exactly is YoSys

YoSys is a complete HDL sysnthesis and optimisation tool.

## Quick-build guide

For the impatient, here's how to get YoSys built and installed.

{{< highlight bash >}}
~ $ git clone https://github.com/cliffordwolf/yosys.git
~ $ cd yosys
~/yosys/ $ make config-release
~/yosys/ $ make
~/yosys/ $ sudo make install
{{< /highlight >}}

## Quick-build - Project IceStorm

{{< highlight bash >}}
~ $ git clone git clone https://github.com/cliffordwolf/icestorm.git icestorm
~/icestorm/ $ cd icestorm
~/icestorm/ $ make -j8
~/icestorm/ $ sudo make install
{{< /highlight >}}

## Quick-build - Arachne-PNR (Place and Route tool for Ice FPGA's used in tutorials).

{{< highlight bash >}}
~ $ git clone https://github.com/cseed/arachne-pnr.git arachne-pnr
~ $ cd arachne-pnr
~/arachne-pnr/ $ make -j8
~/arachne-pnr/ $ sudo make install
{{< /highlight >}}

## WizTips™, Notes, and Warnings

Throughout this site, there are a number of small and handy pieces of information
that can be useful while using YoSys. Some make your life easier,
some are interesting, and some keep you safe! Here's what to look out for:

{{< wiztip "WizTips™ help you get more from YoSys">}}
  These are tips that will help you become a YoSys Wizard!
{{< /wiztip>}}

{{< note "Notes are handy pieces of information" >}}
  These often give you useful background information, on a YoSys feature.
{{< /note>}}

{{< warning "Warnings help you stay safe!" >}}
  Be aware of these messages if you wish to avoid certain death.
{{< /warning>}}

If you come across anything along the way that we haven’t covered, or if you
know of a tip you think others would find handy, please [file an
issue](https://github.com/jameswalmsley/yosys-blog/issues/new) and we’ll see about
including it on this site.

