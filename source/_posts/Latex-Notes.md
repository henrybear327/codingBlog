---
title: Latex Notes
categories: Latex
tags:
  - Latex
  - Hexo
  - MathJex
date: 2016-08-20 02:02:59
---


Latex is a great way to express mathematical expressions in a clear and readable way, just like what you see in textbooks. The setup is tedious, and the usage is even messier.

The following notes are what I usually use for writing posts. Hope it can help you master Latex without the need to go through all pains that I have experienced.

<!-- more -->

# Installation

1. `npm install hexo-math --save`
2. In the folder that you need `MathJex`, execute the command `hexo math install`
3. Add the following line in the `_config.yml` file
    ```
    plugins: hexo-math
    ```

    If you have multiple plugins, add
    ```
    plugins:
    - hexo-math
    - ...
    ```

**Notice**: if you work offline, the Latex won't be rendered.

# Usage of Latex code on Hexo

## Inline

Only for very basic stuff, such as pure variables and formulas.

`$a = b + c$` will be rendered as $a = b + c$.

## Block

Render Latex code on a newline.

`$$ velocity = \frac{distance}{time} $$` will be rendered as $$ velocity = \frac{distance}{time} $$

## Rendering issue

Try this... $x_k = x_1 - [ ( a_1 + a_2 + ... + a_{k - 1} ) - (k - 1) \times avg]$. It fails because of the markdown engine rendering priority issue.

So, instead of using `$ ... $`, use
```
{% math %}
\begin{aligned}
x_k = x_1 - [ ( a_1 + a_2 + ... + a_{k - 1} ) - (k - 1) \times avg]
\end{aligned}
{% endmath %}
```

The result will be very satisfying :)

{% math %}
\begin{aligned}
x_k = x_1 - [ ( a_1 + a_2 + ... + a_{k - 1} ) - (k - 1) \times avg]
\end{aligned}
{% endmath %}

That's why I mentioned **basic stuff** in the `inline` section. One hour wasted on this issue...

# [Latex code notes](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)

## Basics

### Plain Text

Use `\text{…}` for plain text. You can use `$$` within it.

`\text{Hello World! $x^2 = y^3$}` is $\text{Hello World! $x^2 = y^3$}$

### Escape

Special characters used for MathJax interpreting can be escaped using the `\` character
* `\$` is $\$$
* `\{` is $\{$
* `\_` is $\_$

### Spacing

`This is a book` becomes $This is a book$. So we need to specify the spacing by using:

* `\,` for thin space
* `\;` for normal space

`This\, is\; a\; book` becomes $This\, is\; a\; book$

### Superscripts and Subscripts

Use `^` for superscripts and `_` for subscripts. e.g. `x_i^2` is $x_i^2$

*Notice*: without `{}`, the `^` and `_` only apply to the next character only.

### Group

Superscripts, subscripts, and other operations apply only to the next "group" (use `{}` for grouping). For example, to represent 10 to the 10th power, you shouldn't write...

`10^10` $10^10$

You should write...

`10^{10}` $10^{10}$

### Fraction

1. `\frac ab` is $\frac ab$
2. `\frac{a+1}{b+1}` is $\frac{a+1}{b+1}$
3. `{a+1\over b+1}` is ${a+1\over b+1}$

## Special Letters and Symbols

### Greek Letters

* `alpha \beta \delta \Delta \gamma \Gamma \omega \Omega` is $\alpha \beta \delta \Delta \gamma \Gamma \omega \Omega$
* `\epsilon \varepsilon \phi \varphi` is $\epsilon \varepsilon \phi \varphi$

### Special Symbol

* `\lt \gt \le \ge \neq` is $\lt \gt \le \ge \neq$
* `\times \div \pm \mp \cdot` is $\times \div \pm \mp \cdot$
* `\cup \cap \setminus \subset \subseteq \subsetneq \supset \in \notin \emptyset \varnothing` is $\cup \cap \setminus \subset \subseteq \subsetneq \supset \in \notin \emptyset \varnothing$
* `\to \rightarrow \leftarrow \Rightarrow \Leftarrow \mapsto` is $\to \rightarrow \leftarrow \Rightarrow \Leftarrow \mapsto$
* `\land \lor \lnot \forall \exists \top \bot \vdash \vDash` is $\land \lor \lnot \forall \exists \top \bot \vdash \vDash$
* `\star \ast \oplus \circ \bullet` is $\star \ast \oplus \circ \bullet$
* `\approx \sim \simeq \cong \equiv \prec \lhd` is $\approx \sim \simeq \cong \equiv \prec \lhd$
* `a\equiv b\pmod n` is $a\equiv b\pmod n$
* `\ldots` for $a1, a2, \ldots, an$, and `\cdots` for $a1 + a2 + \cdots + an$
* `\hat x \bar x \overline x \vec x \overrightarrow {xy}` is $\hat x\; \bar x\; \overline x\; \vec x\; \overrightarrow {xy}$

### Special Function: trigonometry, limit, sqrt, sum

* Trigonometry
    * `\sin x` is $\sin x$
    * `\cos x` is $\cos x$
    * `\tan x` is $\tan x$
* Limits
    * `\lim_{x\to 0}` is {% math %}\lim_{x \to 0}{% endmath %}
* Square root
    * `\sqrt{\frac xy}` is $\sqrt{\frac xy}$
    * `\sqrt[3]{\frac xy}` is $\sqrt[3]{\frac xy}$
* Summation
    * `\sum_{i=0}^n i^2 = \frac{n(n+1)(2n+1)}{6}` is {% math %}\sum_{i=0}^n i^2 = \frac{n(n+1)(2n+1)}{6}{% endmath %}
    * For multiline subscripts
    ```
    \sum_{\substack{
        0\le i\lt n\\
        0\le j\lt m}
    }
    i^2+j^3
    ```
    {% math %}
    \sum_{\substack{
        0\le i\lt n\\
        0\le j\lt m}
    }
    i^2+j^3
    {% endmath %}

## Equations and Functions

### System of Equations

```
\begin{cases}
a_1x+b_1y+c_1z=d_1 \\
a_2x+b_2y+c_2z=d_2 \\
a_3x+b_3y+c_3z=d_3
\end{cases}
```

{% math %}
\begin{cases}
a_1x+b_1y+c_1z=d_1 \\
a_2x+b_2y+c_2z=d_2 \\
a_3x+b_3y+c_3z=d_3
\end{cases}
{% endmath %}

### Tagging Equations

```
{% math %}
\begin{align*}
   a &= p^2 - q^2 &&\text{Equation note 1} \tag 1\\
   b &= 2pq  &&\text{Equation note 2} \tag 2\\
   c &= p^2 + q^2 &&\text{Equation note 3} \tag 3\\
\end{align*}
{% endmath %}
```

{% math %}
\begin{align*}
   a &= p^2 - q^2 &&\text{Equation note 1} \tag 1\\
   b &= 2pq  &&\text{Equation note 2} \tag 2\\
   c &= p^2 + q^2 &&\text{Equation note 3} \tag 3\\
\end{align*}
{% endmath %}

[P.S.](http://tex.stackexchange.com/questions/159723/what-does-a-double-ampersand-mean-in-latex) `&` is the cell separator in tabulars and similar constructions. A single `&` means “go to the next cell of the alignment”, so `&&` means “the next cell is empty, go to the following one”.

Check out [here](https://cdn.mathjax.org/mathjax/latest/test/sample-eqnum.html) for notes on aligning.

### Pairwise Functions

```
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
```

{% math %}
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
{% endmath %}

## Matrix

Basic version.

```
{% math %}
\begin{aligned}
        \begin{matrix}
        a & b \\
        0 & 1 \\
        \end{matrix}
\end{aligned}
{% endmath %}
```

{% math %}
\begin{aligned}
        \begin{matrix}
        a & b \\
        0 & 1 \\
        \end{matrix}
\end{aligned}
{% endmath %}

Bracket matrix with [exponent](http://tex.stackexchange.com/questions/114369/pmatrix-and-superscript) associated with it.

```
{% math %}
\begin{aligned}
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}^{n}
\end{aligned}
{% endmath %}
```

{% math %}
\begin{aligned}
        \begin{bmatrix}
        a & b \\
        0 & 1 \\
        \end{bmatrix}^{n}
\end{aligned}
{% endmath %}

Two more kinds...

```
{% math %}
\begin{aligned}
        \begin{pmatrix}
        a & b \\
        0 & 1 \\
        \end{pmatrix}^{n}
\end{aligned}
{% endmath %}
```

{% math %}
\begin{aligned}
        \begin{pmatrix}
        a & b \\
        0 & 1 \\
        \end{pmatrix}^{n}
\end{aligned}
{% endmath %}

```
{% math %}
\begin{aligned}
        \begin{vmatrix}
        a & b \\
        0 & 1 \\
        \end{vmatrix}^{n}
\end{aligned}
{% endmath %}
```

{% math %}
\begin{aligned}
        \begin{vmatrix}
        a & b \\
        0 & 1 \\
        \end{vmatrix}^{n}
\end{aligned}
{% endmath %}
