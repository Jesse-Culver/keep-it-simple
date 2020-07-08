---
title: "Example Image Post"
layout: post
author: ""
publish_date: "6/12/2020"
last_modified: ""
---

This is an example post with images.

Displaying an image is fairly straightforward

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px"}
~~~

Attributes can be [written inline thanks to Kramdown](https://kramdown.gettalong.org/syntax.html#links-and-images) so in the above we set the SVG's height and width to 256.

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px"}

Image By: <a href="https://commons.wikimedia.org/wiki/File:Calculator_Flat_Icon_Vector.svg" title="via Wikimedia Commons">Videoplasty.com</a> / <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA</a>

## Image Float Right

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px" style="float:right"}
An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px" style="float:right"}
~~~

## Image Float Left

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px" style="float:left"}
An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px" style="float:left"}
~~~

## Center Image

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:.img-center height="256px" width="256px"}

An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){: height="256px" width="256px" style="display:block; margin-left:auto; margin-right:auto"}
~~~

There is also a CSS class for centering images called "img-center".

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:.img-center height="256px" width="256px"}
~~~

## Figures

{% include image-figure.html imgURL="/assets/images/Calculator_Flat_Icon_Vector.svg" caption="A Basic Calculator" width="256px" height="256px" float="right" %}
An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

{% include image-figure.html imgURL="/assets/images/Calculator_Flat_Icon_Vector.svg" caption="A Small Basic Calculator" width="30%" height="30%" float="left" %}
Computer operating systems as far back as early Unix have included interactive calculator programs such as dc and hoc, and calculator functions are included in almost all personal digital assistant (PDA) type devices, the exceptions being a few dedicated address book and dictionary devices.

In addition to general purpose calculators, there are those designed for specific markets. For example, there are scientific calculators which include trigonometric and statistical calculations. Some calculators even have the ability to do computer algebra. Graphing calculators can be used to graph functions defined on the real line, or higher-dimensional Euclidean space. As of 2016, basic calculators cost little, but scientific and graphing models tend to cost more.

In 1986, calculators still represented an estimated 41% of the world's general-purpose hardware capacity to compute information. By 2007, this had diminished to less than 0.05%

{% raw %}
~~~liquid
{% include image-figure.html imgURL="/assets/images/Calculator_Flat_Icon_Vector.svg" caption="A Basic Calculator" width="256px" height="256px" float="right" %}
{% include image-figure.html imgURL="/assets/images/Calculator_Flat_Icon_Vector.svg" caption="A Small Basic Calculator" width="30%" height="30%" float="left" %}
~~~
{% endraw %}


## Opacity

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.7;"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.5;"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.2;"}
An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.7;"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.5;"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;opacity:0.2;"}
~~~

## Filters

All filters listed [here](https://www.w3schools.com/cssref/css3_pr_filter.asp) should work out of the box using inline styling.

![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:grayscale(50%);"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:sepia(50%);"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:blur(5px);"}
An electronic calculator is typically a portable electronic device used to perform calculations, ranging from basic arithmetic to complex mathematics.

The first solid-state electronic calculator was created in the early 1960s. Pocket-sized devices became available in the 1970s, especially after the Intel 4004, the first microprocessor, was developed by Intel for the Japanese calculator company Busicom. They later became used commonly within the petroleum industry (oil and gas).

Modern electronic calculators vary from cheap, give-away, credit-card-sized models to sturdy desktop models with built-in printers. They became popular in the mid-1970s as the incorporation of integrated circuits reduced their size and cost. By the end of that decade, prices had dropped to the point where a basic calculator was affordable to most and they became common in schools.

~~~markdown
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:grayscale(50%);"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:sepia(50%);"}
![SVG Calculator](/assets/images/Calculator_Flat_Icon_Vector.svg){:style="float:right;height:auto;max-width:25%;filter:blur(5px);"}
~~~

