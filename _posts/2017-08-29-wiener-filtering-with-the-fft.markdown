---
layout: post
title:  "Optimal (Wiener) Filtering with the FFT"
date:   2017-08-29 12:00:09 +0300
categories: jekyll update
---

There are a number of tasks in numerical processing that are routinely
handled with Fourier techniques. One of these is filtering for the removal of noise
from a "corrupted" signal[^1].

Let's start with uncorrupted signal $u(t)$. We will use the following [image][airport], 512px*512px, as an example:

<table style="margin: 0px auto;">
<tr><td>
<div style="position: relative; height: 512px; width: 512px;">
  <img style="position: absolute; left: 0;     top: 0;" src="/img/5.3.02.png" />
  <!--p style="position: absolute; left: 256px; top: 0; z-index: 100; margin: 0px; padding: 0px;">$$E = mc^2$$</p-->
</div>
</td></tr>
<tr><td style="width:99%">
  Pic.1. Original image, $u(t)$
</td></tr>
</table>


First, the apparatus may not have a perfect "delta-function" response,
so that the true signal $u(t)$ is convolved with (smeared out by) some known response
function $r(t)$ to give a smeared signal $s(t)$,

\begin{equation}
  s(t) = \int_{-\infty}^\infty r(t-\tau)u(\tau)d\tau
  \label{eq:st}
\end{equation}
or
\begin{equation}
  S(f) = R(f)U(f)
  \label{eq:Sf}
\end{equation}
where $S,R,U$ are the Fourier transforms of $s,r,u$ respectively.



<!--div class="ct-chart ct-golden-section" id="chart1"></div-->
<!--div class="ct-chart ct-golden-section" id="chart2"></div-->
<!--div class="ct-chart ct-golden-section" id="chart3"></div-->

<!--script>
  // Our labels and three data series
  var data = {
    labels: ['Week1', 'Week2', 'Week3', 'Week4', 'Week5', 'Week6'],
    series: [
      [5, 4, 3, 7, 5, 10],
      [3, 2, 9, 5, 4, 6],
      [2, 1, -3, -4, -2, 0]
    ]
  };

  // We are setting a few options for our chart and override the defaults
  var options = {
    // Don't draw the line chart points
    showPoint: false,
    // Disable line smoothing
    lineSmooth: false,
    // X-Axis specific configuration
    axisX: {
      // We can disable the grid for this axis
      showGrid: false,
      // and also don't show the label
      showLabel: false
    },
    // Y-Axis specific configuration
    axisY: {
      // Lets offset the chart a bit from the labels
      offset: 60,
      // The label interpolation function enables you to modify the values
      // used for the labels on each axis. Here we are converting the
      // values into million pound.
      labelInterpolationFnc: function(value) {
        return '$' + value + 'm';
      }
    }
  };

  // Initialize a Line chart in the container with the ID chart1
  new Chartist.Line('#chart1', {
    labels: [1, 2, 3, 4],
    series: [[100, 120, 180, 200]]
  }, {fullWidth: true, chartPadding: {right: 40}});

  // Initialize a Line chart in the container with the ID chart2
  new Chartist.Bar('#chart2', {
    labels: [1, 2, 3, 4],
    series: [[5, 2, 8, 3]]
  });

  new Chartist.Line('#chart3', data, options);
</script-->

Let us use the Gaussian distribution as the response function, and,
to be more specific, use 2 dimensions $t_1$ and $t_2$ for $t$:
\begin{equation}
  r(t_1,t_2) = \frac{e^{-(t_1^2 + t_2^2)/a^2}}{a^2\pi}, \quad a=0.2
  \label{eq:rt}
\end{equation}

Use Imagemagick to perform this task:

```bash
convert 5.3.02.png -gaussian-blur 0x2 5.3.02.gaus.png
```

<table style="margin: 0px auto;">
<tr><td>
<div style="position: relative; height: 512px; width: 512px;">
  <img style="position: absolute; left: 0;     top: 0;" src="/img/5.3.02.gaus.png" />
  <!--p style="position: absolute; left: 256px; top: 0; z-index: 100; margin: 0px; padding: 0px;">$$E = mc^2$$</p-->
</div>
</td></tr>
<tr><td style="width:99%">
  Pic.2. Smeared image, $s(t)$
</td></tr>
</table>


<!--
convert -size 128x128 xc:black -fill white \
        -draw "point 64,64" -gaussian-blur 0x8 -auto-level \
        -write gaus8.png -fft -delete 1 \
        -auto-level -evaluate log 1000 gaus8_spectrum.png
-->


<!--object type="image/svg+xml" data="/img/01.svg">
  Your browser does not support SVG
</object-->


<!--svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1000" height="500">
  <circle cx="100" cy="100" r="99" fill="yellow" stroke="red" />
  <circle cx="100" cy="100" r="3" fill="blue" />
  <foreignObject x="100" y="100" width="100" height="100">
    <div xmlns="http://www.w3.org/1999/xhtml" style="font-family:Times; font-size:15px">
    \(\displaystyle{x+1\over y-1}\)
    </div>
  </foreignObject>
</svg-->


<!--svg xmlns="http://www.w3.org/2000/svg" xmlns:inkspace="http://www.inkscape.org/namespaces/inkscape" xmlns:xlink="http://www.w3.org/1999/xlink" width="800" height="600">
  <defs id="defs_block">
    <filter height="1.504" id="filter_blur" inkspace:collect="always" width="1.1575" x="-0.07875" y="-0.252">
      <feGaussianBlur id="feGaussianBlur3780" inkspace:collect="always" stdDeviation="4.2" />
    </filter>
  </defs>
  <title>blockdiag</title>
  <rect fill="rgb(0,0,0)" height="40" stroke="rgb(0,0,0)" style="filter:url(#filter_blur);opacity:0.7;fill-opacity:1" width="128" x="67" y="46" />
  <rect fill="rgb(0,0,0)" height="40" stroke="rgb(0,0,0)" style="filter:url(#filter_blur);opacity:0.7;fill-opacity:1" width="128" x="259" y="46" />
  <rect fill="rgb(0,0,0)" height="40" stroke="rgb(0,0,0)" style="filter:url(#filter_blur);opacity:0.7;fill-opacity:1" width="128" x="451" y="46" />
  <rect fill="rgb(0,0,0)" height="40" stroke="rgb(0,0,0)" style="filter:url(#filter_blur);opacity:0.7;fill-opacity:1" width="128" x="451" y="126" />
  <rect fill="rgb(0,0,0)" height="40" stroke="rgb(0,0,0)" style="filter:url(#filter_blur);opacity:0.7;fill-opacity:1" width="128" x="643" y="126" />
  <rect fill="rgb(255,255,255)" height="40" stroke="rgb(0,0,0)" width="128" x="64" y="40" />
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="110" y="65">plain text</text>
  <rect fill="rgb(255,255,255)" height="40" stroke="rgb(0,0,0)" width="128" x="256" y="40" />
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="320" y="65">\( e^{-sT} \)</text>
  <rect fill="rgb(255,255,255)" height="40" stroke="rgb(0,0,0)" width="128" x="448" y="40" />
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="512" y="65">R\( \frac{3}{s^2+1} \)</text>
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="470" y="95">aligned at the right</text>
  <rect fill="rgb(255,255,255)" height="40" stroke="rgb(0,0,0)" width="128" x="448" y="120" />
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="512" y="145">L\( H(s) \)</text>
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="470" y="175">aligned at the left</text>
  <rect fill="rgb(255,255,255)" height="40" stroke="rgb(0,0,0)" width="128" x="640" y="120" />
  <text fill="rgb(0,0,0)" font-family="sansserif" font-size="11" font-style="normal" font-weight="normal" x="704" y="145">$ K_p + \left( 1 + {1 \over T s} \right) $</text>
  <path d="M 192 60 L 248 60" fill="none" stroke="rgb(0,0,0)" />
  <polygon fill="rgb(0,0,0)" points="255,60 248,56 248,64 255,60" stroke="rgb(0,0,0)" />
  <path d="M 384 60 L 440 60" fill="none" stroke="rgb(0,0,0)" />
  <polygon fill="rgb(0,0,0)" points="447,60 440,56 440,64 447,60" stroke="rgb(0,0,0)" />
  <path d="M 384 60 L 416 60" fill="none" stroke="rgb(0,0,0)" />
  <path d="M 416 60 L 416 140" fill="none" stroke="rgb(0,0,0)" />
  <path d="M 416 140 L 440 140" fill="none" stroke="rgb(0,0,0)" />
  <polygon fill="rgb(0,0,0)" points="447,140 440,136 440,144 447,140" stroke="rgb(0,0,0)" />
  <path d="M 576 140 L 632 140" fill="none" stroke="rgb(0,0,0)" />
  <polygon fill="rgb(0,0,0)" points="639,140 632,136 632,144 639,140" stroke="rgb(0,0,0)" />
  <path d="M 768 140 L 784 140" fill="none" stroke="rgb(0,0,0)" />
  <path d="M 784 140 L 784 25" fill="none" stroke="rgb(0,0,0)" />
  <path d="M 320 25 L 784 25" fill="none" stroke="rgb(0,0,0)" />
  <path d="M 320 25 L 320 32" fill="none" stroke="rgb(0,0,0)" />
  <polygon fill="rgb(0,0,0)" points="320,39 316,32 324,32 320,39" stroke="rgb(0,0,0)" />
</svg-->
<!--script type="text/javascript">new Svg_MathJax().install();</script-->


Second, the measured signal may contain an additional component of noise $n(t)$.
So the result, currupted signal $c(t)$, contains the following two components:
\begin{equation}
  c(t) = s(t) + n(t)
  \label{eq:ct}
\end{equation}


Add a [Perlin][perlin_wiki] noise by running a Perlin-noise [generator][perlin_gen]:
```bash
perlin 512x512 -n poisson -a 2 noise.png
```
<table style="margin: 0px auto;">
<tr><td>
<div style="position: relative; height: 512px; width: 512px;">
  <img style="position: absolute; left: 0;     top: 0;" src="/img/noise.png" />
  <!--p style="position: absolute; left: 256px; top: 0; z-index: 100; margin: 0px; padding: 0px;">$$E = mc^2$$</p-->
</div>
</td></tr>
<tr><td style="width:99%">
  Pic.3. Noise, $n(t)$
</td></tr>
</table>

To get the result use the Imagemagick's special [composition method][im_compose]:
```bash
convert noise.png -normalize  +level 30,70% noise.30-70.png
convert 5.3.02.gaus.png noise.30-70.png -compose Mathematics -define compose:args='0,1,1,-0.5' -composite 5.3.02.corrupt.png
```
<table style="margin: 0px auto;">
<tr><td>
<div style="position: relative; height: 512px; width: 512px;">
  <img style="position: absolute; left: 0;     top: 0;" src="/img/5.3.02.corrupt.png" />
  <!--p style="position: absolute; left: 256px; top: 0; z-index: 100; margin: 0px; padding: 0px;">$$E = mc^2$$</p-->
</div>
</td></tr>
<tr><td style="width:99%">
  Pic.4. Corrupted image, $c(t) = s(t) + n(t)$
</td></tr>
</table>


Bibliography
============

[^1]: Press, William H., Teukolsky, Saul A., Vetterling, William T. & Flannery, Brian P. (1992). *Numerical Recipes in C (2Nd Ed.): The Art of Scientific Computing.* Cambridge University Press
{% comment %}
@book{Press:1992:NRC:148286,
 author = {Press, William H. and Teukolsky, Saul A. and Vetterling, William T. and Flannery, Brian P.},
 title = {Numerical Recipes in C (2Nd Ed.): The Art of Scientific Computing},
 year = {1992},
 isbn = {0-521-43108-5},
 publisher = {Cambridge University Press},
 address = {New York, NY, USA},
}
{% endcomment %}


{% comment %}
![image alt <>](/img/5.3.02.png)
$$
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\
\hline
1 & 0.24 & 1 & 125 \\
2 & -1 & 189 & -8 \\
3 & -20 & 2000 & 1+10i
\end{array}
$$
<div style="display:none">
\(
  \def\<#1>{\left<#1\right>}
  \newcommand{\CC}{\mathbf{C}}
\)
</div>
See $$\<x,y,z>$$
equations:
$$
\begin{equation}
  E = mc^2 + 5
  \label{eq:eins}
\end{equation}
$$
eq with no numbers, opposite to \eqref{eq:eins}:
$$
\begin{equation*}
  e^{\pi i} + 1 = 0
\end{equation*}
$$
more links:
In equation \eqref{eq:sample}, we find the value of an interesting integral:
\begin{equation}
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
  \label{eq:sample}
\end{equation}
latex snippet 1:
When $$a \ne 0$$, there are two solutions to \\(ax^2 + bx + c = 0\\) and they are $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
latex snippet 2:
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$
latex snippet 3:
* See bold-faced R: $$\RR$$
* Some bold text: $$\bold{bbb}$$
* Abs values: $$\Abs{aaa}{bbb} = 5$$
* Abs: $$\left\lvert aaa \right\rvert_{\text{bbb}}$$
code snippet:
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
see [Jekyll docs][jekyll-docs] for additional info
[jekyll-docs]: https://jekyllrb.com/docs/home
{% endcomment %}


[airport]: http://sipi.usc.edu/database/database.php?volume=misc&image=26#top
[perlin_wiki]: https://en.wikipedia.org/wiki/Perlin_noise
[perlin_gen]: http://fmwconcepts.com/imagemagick/perlin/index.php
[im_compose]: https://www.imagemagick.org/Usage/compose/#special
