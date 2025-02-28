---
layout: page
title: FPGA Median Filter
description: A FPGA design to filter noise from an image
img: assets/img/1.jpg
importance: 2
category: RTL and Verification
related_publications: false
---

This project is to design and implement an image filtering hardware design making use of many features of an FPGA environment including IPs, interfacing protocols, and memory.

The final desing should perform the following:
<div>
    <ul>
        <li>Receive and store an image into memory</li>
        <li>Run the stored image through a median filter</li>
        <li>Store the filtered image into memory</li>
        <li>Transmit the filtered and unfiltered image back out from memory</li>
    </ul>
</div>

<h1>FPGA</h1>

The FPGA used during the development of this design was the Nexys 4 DDR development board. The communication between the FPGA and computer was done through MatLab using a USB connection and the USB to UART bridge that the board features. The onboard DDR2 memory was utilized to store the transferred image. More details on the development board can be found on the Digilent website <a href="https://digilent.com/reference/programmable-logic/nexys-4-ddr/reference-manual">here</a>.

<h1>Xilinx IPs</h1>

The design used a total of two IPs from the Xilinx IP library. It includes an AXI protocol converter and a memory interface generator which worked together to allow the use of AXI lite communication to write to the DDR memory. This use of IPs significantly simplifies the task of storing the image into the onboard DDR memory.

<h1>AMBA AXI</h1>

Connections between IP blocks as well as the read and writes requires the use of the AXI protocol. The AXI lite protocol was used to connect my custom hardware design with the IPs. The specific control signals for writing to memory is shown in the diagram below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/AXILite.PNG" title="AXI Lite Waveform" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Waveform for AXI lite to access memory
</div>

As the AXI protocol was used for accessing memory, there are ready and valid signals for both the data and address bus to handshake. Valid and response signals are used for error checking for correct handshake function.

<h1>Hardware Design</h1>

The hardware I designed featured multiple blocks with a block diagram shown below along with a list to describe each block's functionality.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Design_Diagram.png" title="Hardware Design Diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Diagram showing design components
</div>

<div class="balance">
    <ul>
        <li>UART Module : Connects to the USB to UART bridge to receive image and commands external to FPGA</li>
        <br>
        <li>FIFO_RX Read Module : A FIFO to buffer reads when receiving data and storing into memory</li>
        <br>
        <li>FIFO_TX Write Module : A FIFO to buffer writes when writing data from memory to external device</li>
        <br>
        <li>Main Control Module : This module holds the state machine and filter design to control all the operations such as storing, filtering, and retransmitting the image</li>
        <br>
        <li>BRAM Write Module : A module implementing AXI lite to write the image into DDR2 memory</li>
        <br>
        <li>BRAM Read Module : A module implementing AXI lite to read the image from DDR2 memory</li>
        <br>
        <li>AXI Lite to AXI Full Converter : IP block that converts the AXI lite communication into AXI full required for the MIG module</li>
        <br>
        <li>Memory Interface Generator : IP block that converts the AXI full communication into DDR2 communication</li>
        <br>
        <li>DDR2 Memory : On board memory that is utilized to store image data</li>
    </ul>
</div>

With the block diagram implemented on the Nexys 4 DDR board and a MatLab script ready to handle the data and command transfers it was time to test the design.

<h1>Result</h1>

Testing the design by sending an image populated with some noise, the MatLab script was able to successfully transfer and read the image back from the FPGA with the result displayed in the figure below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/MatLab_Output.PNG" title="Test Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    MatLab output of the design test
</div>

The test figure shows the original image transferred into the FPGA, followed by the unfiltered retransmitted image, and finally the filtered image retransmitted.

The project was a success as the implemented design was able to perform all its roles correctly and I had a lot of fun getting to develop this design from the ground up.