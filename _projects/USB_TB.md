---
layout: page
title: UVM Verification of USB
description: A UVM testbench designed to verify a USB RTL design
img: assets/img/3.jpg
importance: 2
category: RTL and Verification
related_publications: false
---

The USB protocol is used throughout the world for communication between computers and devices. This project aims to create a UVM testbench capable of testing a RTL design implementing USB compliant hardware.

<h1>USB</h1>

The USB protocol is diviced into many layers going from hardware such as packet control to software specifications for device operation. I only care about the hardware specifications although a good overview of the USB standard can be found <a href="https://www.beyondlogic.org/usbnutshell/usb1.shtml">here</a>.

USB consists of several endpoints where each endpoint correspond to a section of memory. Endpoints are configurable for three functions: IN, OUT, and CONTROL where IN is for sending data from device to host, OUT is for reciving data from host, and CONTROL is used for configuration or sharing device data between device and host.

My testbench will aim to use constrained randomization to configure several endpoints to prepare for packet transfers back and forth as well as checking for correct CRC function in the hardware for error checking. More information about CRC for USB can be found <a href="https://www.usb.org/sites/default/files/crcdes.pdf">here</a>.

A few extra things that are present in the USB design I am testing is the existince of a DMA feature which I will need to include in the testbench.

<h1>UVM Testbench</h1>

The testbench I designed includes two agents, one to represent the host connection and a second to represent the device side. A diagram showcasing the testbench design is shown below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/USB_Testbench.png" title="USB Testbench Diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Diagram of UVM Testbench for USB.
</div>


This testbench features two agents which each individually handle controlling their side of the USB data transfers, one for the host side and another for the device side. Sequencers generate randomized data for configuration and data transfers between the two agents through the USB DUT.

I made great use of UVM subscribers to transfer data around the testbench as well as enable a way to control the actions in the drivers and monitors to allow for synchronization between the two agents.

This project was fun and I was able to learn and get a lot of experience about the USB protocol and how to utilize UVM for verification.