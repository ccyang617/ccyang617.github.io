---
layout: project
type: project
image: img/fpga1.JPG
title: "FPGA Hardware Implementation"
date: 2023
published: true
labels:
  - SystemVerilog
  - FPGA
  - Hardware Design
summary: "Implementing a combinational circuit design on a Basys3 FPGA"
---

In this project, my lab partners and I implemented a combinational logic circuit on a Basys3 FPGA using the SystemVerilog Hardware Description Language and the Xilinx Vivado software. The task of the lab was to help familiarize us with FPGAs and how download our programs into the hardware using the software. We were in charge of creating modules for three separate circuit components: Registers, Multiplexers, and Arithmentic Logic Units and connecting them combinationally to create the desired circuit. 

It was important to ensure the correct inputs and outputs had wires connecting each other so that values can be passed to the next module. Continuous assignment was necessary to ensure that registers would update when the corresponding switches would change from case to case. Continuous assignment also provides near instant results without delay.

This project allowed me to refresh my skills in SystemVerilog and practice designing circuits and better understand how circuits rely on Hardware Description Language to commmunicate between Engineers and Hardware. The Hardware is capable of many things and it is up to the humans on the other side to unlock their usefulness with communication through common language.

Here you can see examples of the modules for the registers, multiplexer, and arithmetic logic unit:
<div class="text-center p-4">
  <img width="314px" src="../img/fpga_reg.JPG" class="img-thumbnail" >
  <img width="388px" src="../img/fpga_mux.JPG" class="img-thumbnail" >
  <img width=""320px" src="../img/fpga_selalu.JPG" class="img-thumbnail" >
</div>
