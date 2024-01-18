---
{"dg-publish":true,"permalink":"/adv/test-format/","title":"Test format","tags":["basis"],"noteIcon":""}
---


>
## Math
> [!INFO] INFO
> 
> Test 
> $$\int_{a}^{b} x^2 dx$$
> Inline $a$
> 

> [!NOTE] 
> 
> DG: Supported, But can't be in the same line, must start another line
> 
> Obsidian: Supported.
> 
> Hugo: Supported.

---
$$
\sum^N_{k=1} \color{Lavender}{k^2}
$$
$$
\sum^N_{k=1} \color{green}{k^2}
$$
---
> [!NOTE]
> 
>  Hugo: the color not supported. `Lavender`, Ok with `green`
>  
>  Obsidian: Supported.
> 
>  DG: Supported. [GitHub - oleeskild/digitalgarden](https://github.com/oleeskild/digitalgarden)

---

* Should display error or not rendered

$$
\left\{\begin{aligned}
3x + 5y +  z \\\\
7x - 2y + 4z \\\\
-6x + 3y + 2z
\end{aligned}\right.
$$
$$
\left\lbrace\begin{aligned}
3x + 5y +  z \\\\
7x - 2y + 4z \\\\
-6x + 3y + 2z
\end{aligned}\right.
$$

> [!WARNING]  
> 
> Hugo: Not supported.  `\left\{`, Can use `\left(` or `\left\lbrace`

---

$$
  \begin{array}{c|cccc}
  \text{min} & 0 & 1 & 2 & 3\\
  \hline
  0 & 0 & 0 & 0 & 0\\
  1 & 0 & 1 & 1 & 1\\
  2 & 0 & 1 & 2 & 2\\
  3 & 0 & 1 & 2 & 3
  \end{array}
$$
---

$$
% outer vertical array of arrays 外层垂直表格
\begin{array}{c}
    % inner horizontal array of arrays 内层水平表格
    \begin{array}{cc}
        % inner array of minimum values 内层"最小值"数组
        \begin{array}{c|cccc}
        \text{min} & 0 & 1 & 2 & 3\\
        \hline
        0 & 0 & 0 & 0 & 0\\
        1 & 0 & 1 & 1 & 1\\
        2 & 0 & 1 & 2 & 2\\
        3 & 0 & 1 & 2 & 3
        \end{array}
    &
        % inner array of maximum values 内层"最大值"数组
        \begin{array}{c|cccc}
        \text{max}&0&1&2&3\\
        \hline
        0 & 0 & 1 & 2 & 3\\
        1 & 1 & 1 & 2 & 3\\
        2 & 2 & 2 & 2 & 3\\
        3 & 3 & 3 & 3 & 3
        \end{array}
    \end{array}
    % 内层第一行表格组结束
    \\
    % inner array of delta values 内层第二行Delta值数组
        \begin{array}{c|cccc}
        \Delta&0&1&2&3\\
        \hline
        0 & 0 & 1 & 2 & 3\\
        1 & 1 & 0 & 1 & 2\\
        2 & 2 & 1 & 0 & 1\\
        3 & 3 & 2 & 1 & 0
        \end{array}
        % 内层第二行表格组结束
\end{array}
$$



> [!WARNING] Please use `\\` instead of `\\\\`

## Code 

```python
import os
print("A message")
```


## Other

> [!FAQ]- Closed by default
> 
> Folding/Collapsable callout


> [!NOTE]+ Open by default
> 
> Folding/Collapsable callout
> 
> A new line is required between the label and the content to be used correctly in Hugo

## MathJax / LaTex

[MathJax basic tutorial and quick reference](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)

Inline $\sqrt{\mathstrut a} - \sqrt{\mathstrut b}$ formula

* Should display error

$$
f\left(
   \left[ 
     \frac{
       1+\left\{x,y\right\}
     }{
       \left(
          \frac{x}{y}+\frac{y}{x}
       \right)
       \left(u+1\right)
     }+a
   \right]^{3/2}
\right)
$$

$$
f\left(
   \left[ 
     \frac{1+\left\lbrace x,y\right\rbrace }{\left(\frac{x}{y}+\frac{y}{x}\right)
\left(u+1\right)}
+a
   \right]^{3/2}
\right)
$$
> [!WARNING] Please use `\left\(` or `\left\lbrace` instead of `\left\{`


---

$$0.414213562373095048\approx6\*16^{-1}+a\*16^{-2}+0\*16^{-3}+\cdots$$

* Should not displayed

$$0.414213562373095048\approx6*16^{-1}+a*16^{-2}+0*16^{-3}+\cdots$$

$$0.414213562373095048\approx6\ast16^{-1}+a\ast16^{-2}+0\ast16^{-3}+\cdots$$

> [!WARNING] Please use `\ast` instead of `\*`, when there have many `***` in the formula, the formula will not render.
> 
> Hugo:  Supported. 
> 
> DG: some error. `\*`
> 
> Obsidian: same as DG `\*`

---

$$
\boldsymbol{x}_{i+1}+\boldsymbol{x}
$$

---

* Should not displayed

$$\boldsymbol{x}_{i+1}+\boldsymbol{x}_{i+2}=\boldsymbol{x}_{i+3}$$

$$\boldsymbol{x}\_{i+1}+\boldsymbol{x}\_{i+2}=\boldsymbol{x}\_{i+3}$$

---
> [!WARNING] You must replace `_` to `\_`
> 
> Hugo: The real reason is `_` , Otherwise this formula will not render.

---

## Misc


<video id="video" controls="" preload="none" poster="视频图片地址"> 
<source id="mp4" src="https://server1.xyzzyxwz.top:12030/res/a.mp4" type="video/mp4">
</video>

<audio id="audio" controls="" preload="none"> <source id="mp3" src="https://server1.xyzzyxwz.top:12030/res/a.mp3">
</audio>​
---

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


## Symblos

$$
\begin{cases}
\sqsubseteq  \\
{\displaystyle \color {Periwinkle}{\text{Periwinkle}}} \\
{\color{Blue}x^2}+{\color{Brown}2x} - {\color{OliveGreen}1} \\
{\Vvdash \nvdash \nVdash \nvDash \nVDash} \\
{\or \lor \vee, \curlyvee, \bigvee} \\
\blacktriangle, \blacktriangledown, \blacktriangleleft,  \blacktriangleright \\
\sim, \nsim, \backsim, \thicksim, \simeq, \backsimeq, \eqsim, \cong, \ncong \\
\cup, \Cup, \sqcup, \bigcup, \bigsqcup, \uplus, \biguplus \\
\infty, \aleph, \complement, \backepsilon, \eth, \Finv, \hbar \\
\Z \\
\yen \\
\Zeta \\
\zeta \\
\xcancel \\
\widehat \\
\varinjlim \\
\end{cases}
$$

>[!WARNING] When space  ` `  shown in this formula, that will not render.

---
$$f(n) =
\begin{cases} 
n/2,  & \text{if }n\text{ is even} \\\\
3n+1, & \text{if }n\text{ is odd}
\end{cases}$$

---

$$
\begin{array}{cc}
\mathrm{Bad} & \mathrm{Better} \\
\hline \\
e^{i\frac{\pi}2} \quad e^{\frac{i\pi}2}& e^{i\pi/2} \\
\int_{-\frac\pi2}^\frac\pi2 \sin x\,dx & \int_{-\pi/2}^{\pi/2}\sin x\,dx \\
\end{array}
$$
---
$$\cancelto{\cancelto{\cancelto{x^{2+x}}{\cancelto{x^2}{x}+4}}4}0$$

$$
\begin{array}{|rc|}
\hline
\verb+\color{black}{text}+ & \color{black}{text} \\
\verb+\color{gray}{text}+ & \color{gray}{text} \\
\verb+\color{silver}{text}+ & \color{silver}{text} \\
\verb+\color{white}{text}+ & \color{white}{text} \\
\hline
\verb+\color{maroon}{text}+ & \color{maroon}{text} \\
\verb+\color{red}{text}+ & \color{red}{text} \\
\verb+\color{yellow}{text}+ & \color{yellow}{text} \\
\verb+\color{lime}{text}+ & \color{lime}{text} \\
\verb+\color{olive}{text}+ & \color{olive}{text} \\
\verb+\color{green}{text}+ & \color{green}{text} \\
\verb+\color{teal}{text}+ & \color{teal}{text} \\
\verb+\color{aqua}{text}+ & \color{aqua}{text} \\
\verb+\color{blue}{text}+ & \color{blue}{text} \\
\verb+\color{navy}{text}+ & \color{navy}{text} \\
\verb+\color{purple}{text}+ & \color{purple}{text} \\ 
\verb+\color{fuchsia}{text}+ & \color{magenta}{text} \\
\hline
\end{array}
$$

---

* Do not understand why a new page can be rendered, or delete the space and line feed can be rendered

$$
\left\langle  
q 
\middle\|
  \frac{\frac{x}{y}}{\frac{u}{v}}
\middle| 
p 
\right\rangle
$$

---

<div style="color: green">
$$
\left\langle  
  q
\middle\|
  \frac{\frac{x}{y}}{\frac{u}{v}}
\middle| 
   p 
\right\rangle
$$
</div>

> [!INFO]
> 
> Hugo: Support on MathJax, And use `mathj:true`. But sometimes not displayed. Or use `<div></div>`


<h4>After testing for half a day, I found that the default rendering is the best</h4>