---
layout: page
title: Masters Thesis Research
description: Design of Posit Based CNN Hardware Accelerator
img: assets/img/5.jpg
importance: 1
category: RTL and Verification
related_publications: false
sidebar: left
---


This is my work and research for a masters thesis. It is a continuation of two previous thesis works presented by [<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>] and [<a href="https://repository.rit.edu/theses/10382/">Gillela, 2020</a>]. It is currently nearing completion and I am in the process of writing papers to conclude the research.

This research focuses on researching the Posit floating point system and its validity for use in quantizing CNN AI models as well as its performance in hardware. Prior work by Agbalessi and Gillela tested two CNN models as well as created a SoC hardware design implementing said models[<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>][<a href="https://repository.rit.edu/theses/10382/">Gillela, 2020</a>]. The prior research tested and validated the use of 8-bit fixed point representation over the traditional IEEE floating point[<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>].


My goals are as follows:
<div>
    <ul>
        <li>Create software model of CNN using Posits</li>
        <li>Test and make sure accuracy of Posit model is acceptable</li>
        <li>Make hardware design to perform Posit arithmetic</li>
        <li>Integrate Posit hardware design into the existing SoC design</li>
        <li>Verify the functionality of the complete SoC design with SystemVerilog testbench</li>
    </ul>
</div>


<h1>Posit Floating Point</h1>

Posits are a new form of floating points proposed by Dr. Gustafson as an upgrade to the IEEE floating point more information can be found <a href="https://posithub.org/">here</a>. The equation for Posit floating point numbers is similar to the IEEE floating point with the addition of an extra regime field.

<div style="text-align:center;">
    <p>
    -1<sup>sign</sup> <font face="Symbol">&#42;</font> (useed<sup>regime</sup>) <font face="Symbol">&#42;</font> (2<sup>exponent</sup>) <font face="Symbol">&#42;</font> (1 + mantissa)
    </p>
    <p>
    useed=2<sup>2<sup>exponent size</sup></sup>
    </p>
</div>

The regime is a new addition to the Posit formula and consists of a dynamic length of similar bits terminated by the opposite bit. An example is shown in the table below for a Posit with 4 potential bits outside of the sign bit.


<div>
<style>
table, th, td {
    border: 1px solid black;
}
</style>
    <table style="width:100%">
    <caption style="text-align:center; caption-side:top">Regime Decoding Example</caption>
        <tr style="text-align:center;">
            <th>Regime Binary</th>
            <th>0001</th>
            <th>001</th>
            <th>01</th>
            <th>10</th>
            <th>110</th>
            <th>1110</th>
            <th>1111</th>
        </tr>
        <tr style="text-align:center;">
            <th>Decoded Value</th>
            <th>-3</th>
            <th>-2</th>
            <th>-1</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
        </tr>
    </table>
</div>

<br>

<p>
The second new edition compared to the IEEE floating point is the use of the useed. A construct determined by the exponent size and grants Posits more accuracy for smaller fractional numbers as well as a larger maximum representation at the cost of less accuracy for the larger numbers.
</p>

Due to my desire to keep as much accuracy as possible, I made the decision to conduct all the software modeling and testing on Posits with an exponent size set to zero for maximum accuracy on the represented numbers.

<h1>Software</h1>

The software modeling consisted of creating C code to perform Posit arithmetic such as multiplication and addition as well as converting between floating point numbers and Posits. The CNN models that were tested consisted of a network for image classification and one for audio classification with their architectures shown in the diagrams below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Image_network.PNG" title="Image Network" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The image CNN architecture [<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>].
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Audio_network.PNG" title="Audio Network" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The audio CNN architecture [<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>].
</div>

<p>
The two CNNs were modeled using Python with arithmetic imported using my C model for Posit arithmetic. The final test validated the architectures using a 6-bit Posit representation for the inputs, although the accumulator operated in 12-bits for better performance. The results compared with the tests using 8-bit fixed point and 32-bit floating point representation is shown in the table below.
</p>

<div>
<style>
table, th, td {
    border: 1px solid black;
}
</style>
    <table style="width:100%">
    <caption style="text-align:center; caption-side:top">Software Model Testing Results</caption>
        <tr style="text-align:center;">
            <th></th>
            <th>32-bit Float</th>
            <th>8-bit Fixed</th>
            <th>6-bit Posit</th>
        </tr>
        <tr style="text-align:center;">
            <th>Audio Accuracy</th>
            <th>99.84%</th>
            <th>97.42%</th>
            <th>97.0238%</th>
        </tr>
        <tr style="text-align:center;">
            <th>Image Accuracy</th>
            <th>99.3939%</th>
            <th>96.10%</th>
            <th>94.5666%</th>
        </tr>
    </table>
</div>

<br>

<p>
The tests show great results for the 6-bit Posit compared to the 32-bit floating point and 8-bit fixed point accuracy. This test was conducted with 2500 test samples for the audio model and 3000 test samples for the image model.
</p>

<h1>Hardware</h1>

The hardware designs were created using Verilog and consisted of a 6-bit Posit multiplier and a 12-bit Posit adder. Diagrams showing the large components for the multiply and addition hardware designs are shown below.

<div class="row justify-content-md-center">
    <div class="col-8">
        {% include figure.liquid loading="eager" path="assets/img/Multiply_Diagram.png" title="Multiply Diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    6-bit Multiply Hardware Design.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Add_Diagram.png" title="Addition Diagram" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    12-bit Addition Hardware Design.
</div>


These hardware designs were verified using an exhaustive tesbench that compared all possible input combinations between the software model and the hardware design. The designs were tested in both RTL and gate level testing verifying full functionality.

The functioning hardware designs were then integrated into a configurable hardware accelerator that could run the two CNN models. The final step was the verification of the hardware accelerator through a SystemVerilog testbench that configured the hardware design and ran the same testing data as the software model did comparing the intermediate data to ensure correct functionality. A diagram of the testbench is shown below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Testbench_Diagram.png" title="Testbench for Hardware Accelerator" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    SystemVerilog Testbench for Hardware Accelerator
</div>

The hardware designs were synthesized using Synposis tools and all debugging was done with Cadence SimVision tools.

More information will be added as I finish gathering benchmarks for the hardware design to compare my new hardware design against the previous designs