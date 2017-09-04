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



Let us use the Gaussian distribution as the response function:

<div class="ct-chart ct-golden-section" id="chart1"></div>
<div class="ct-chart ct-golden-section" id="chart2"></div>
<div class="ct-chart ct-golden-section" id="chart3"></div>

<script>
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
</script>




Second, the measured signal may contain an additional component of noise $n(t)$.
So the result, currupted signal $c(t)$, contains the following two components:
\begin{equation}
  c(t) = s(t) + n(t)
  \label{eq:ct}
\end{equation}





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
[airport]: http://sipi.usc.edu/database/database.php?volume=misc&image=26#top


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
