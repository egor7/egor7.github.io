---
layout: post
title:  "Optimal (Wiener) Filtering with the FFT"
date:   2017-08-29 12:00:09 +0300
categories: jekyll update
---
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
