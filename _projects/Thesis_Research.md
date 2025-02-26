---
layout: page
title: Masters Thesis Research
description: Design of Posit Based CNN Hardware Accelerator
img: assets/img/12.jpg
importance: 1
category: work
related_publications: false
---

This is my work and research for a masters thesis. It is a continuation of two previous thesis works presented by [<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>] and [<a href="https://repository.rit.edu/theses/10382/">Gillela, 2020</a>]. It is currently nearing completion and I am in the process of writing papers to conclude the research.

This research focuses on researching the Posit floating point system and its validity for use in quantizing CNN AI models as well as its performance in hardware. Prior work by Agbalessi and Gillela tested two CNN models as well as created a SoC hardware design implementing the models[<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>] and [<a href="https://repository.rit.edu/theses/10382/">Gillela, 2020</a>]. The tests conducted validated the use of 8-bit fixed point representation rather than traditional IEEE floating point[<a href="https://repository.rit.edu/theses/11244/">Agbalessi, 2022</a>].


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


<h1>Posits</h1>

Posits are a new form of floating points proposed by Dr. Gustafson as an upgrade to the IEEE floating point more information can be found <a href="https://posithub.org/">here</a>. The equation for Posit floating point numbers is similar to the IEEE floating point with the addition of an extra regime field.

<div style="text-align:center;">
    <p>
    -1<sup>sign</sup> <font face="Symbol">&#42;</font> (useed<sup>regime</sup>) <font face="Symbol">&#42;</font> (2<sup>exponent</sup>) <font face="Symbol">&#42;</font> (1 + mantissa)
    </p>
    <p>
    useed=2<sup>2<sup>exponent size</sup></sup>
    </p>
</div>


<div>
<style>
table, th, td {
    border: 1px solid black;
}
</style>
    <table style="width:100%">
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


<h1>Software</h1>

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite ChristieAgbalessi %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
