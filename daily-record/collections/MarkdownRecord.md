title: MarkDown_Record
date: 2022-07-02 22:30:09
categories:
- [Daily-Record,collections]
tags:
- Code
- Markdown

---

## type

`#行内代码`
*这会是 斜体 的文字*
_这会是 斜体 的文字_
**这会是 粗体 的文字**
__这会是 粗体 的文字__
_**组合** 符号_
~~划掉~~
==highlight==

<!--more-->

!
> Hello World！#quota

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium
The HTML specification
is maintained by the W3C.


## table
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

## emoji
:smile:
:fa-car:[^1]

[^1]: 默认启用表情功能

## 框框
!!! note {只适用于 markdown-it parser 而不适用于 pandoc parser。 note 缺省下是启用的。可以在插件设置里禁用此功能。}

!!! abstract  abstract == summary == tldr

!!! info  info == todo

!!! tip  tip == hint == important

!!! success  success == check == done

!!! question question == help == faq

!!! warning warning == caution == attention

!!! failure failure == fail == missing

!!! danger danger == error

!!! bug

!!! example

!!! cite quote == cite


30^th^

H~2~O 

```javascript {.class1 .class}
function add(x, y) {
  return x + y
}
```




!!! note ""
    双引号留空，取消标记

- [x] day0
- [x] day1
- [x] day2
- [ ] day3

 

---  [^2]

[^2]:三个或者更多的连字符 星号 下划线 启用分割线 


[此项目的参与指南](docs/CONTRIBUTING.md)
![This is an image](https://myoctocat.com/assets/images/base-octocat.svg)
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b


- Item 1
- Item 2
  - Item 2a
  - Item 2b


<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://user-images.githubusercontent.com/25423296/163456776-7f95b81a-f1ed-45f7-b7ab-8fa810d529fa.png">
  <source media="(prefers-color-scheme: light)" srcset="https://user-images.githubusercontent.com/25423296/163456779-a8556205-d0a5-45e2-ac17-42d089e3c3f8.png">
  <img alt="Shows an illustrated sun in light color mode and a moon with stars in dark color mode." src="https://user-images.githubusercontent.com/25423296/163456779-a8556205-d0a5-45e2-ac17-42d089e3c3f8.png">
</picture>

!!! note 可以通过将 HTML <picture> 元素与 prefers-color-scheme 媒体功能结合使用来指定在 Markdown 中显示图像的主题。 我们区分浅色和深色模式，因此有两个选项可用。 您可以使用这些选项显示针对深色或浅色背景优化的图像。 这对于透明的 PNG 图像特别有用,代码为浅色主题显示一个太阳图像，为深色主题显示一个月亮


[](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)


!!! note 通过在 Markdown 字符前面输入 \，可告诉 GitHub 忽略（或规避）Markdown 格式。

将 \*our-new-project\* 重命名为 \*our-old-project\*。


## MathType
要在文本中包含内联的数学表达式，请使用美元符号 $ 分隔表达式.要将美元符号显示为与数学表达式相同的行中的字符，需要对非分隔符 $ 进行转义，以确保该行正确呈现。

在数学表达式中，在显式 $之前添加 \ 符号。此表达式使用 `\$` 来显示美元符号：$\sqrt{\$4}$\


!!! example 使用 `$` 分隔符来显示内联数学：$\sqrt{3x-1}+(1+x)^2$

要将数学表达式添加为块，请开始一个新行，并使用两个美元符号 $$ 分隔表达式。

!!! example **The Cauchy-Schwarz Inequality**
    $$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$


!!! note 使用 \left 和 \right 命令作为 () ，[] 以及	的前缀，可以显示大括号

### 常用符号
写法  |	符号  |	备注 |写法  |	符号  |	备注
------|------|-----|------|-------|-----
\sin(x)|	$sin(x)$|	正弦函数|\log(x)|	$log(x)$|	对数函数
\sum_{i=0}|	$∑_n^i=0$|	累加和|\prod_{i=0}^n	|$∏_n^i=0$|	累积乘
\displaystyle	|∑i=0n|	块显示|90^{\circ}|	$90^{\circ}$	|度数
\ldots|	1 … 2	|底部省略号 |\cdot|+ ⋯ +	|中部省略号
\int_a^b|	∫ba	|积分符号|\vec{a}|	$\vec{a}$ |	矢量a
\lim	|lim	|极限函数|\to|	→	|箭头
\uparrow|	↑	|上箭头|\Uparrow|	⇑	|双上箭头
\partial y|	∂y|	导数/偏导|\infty|	∞	|无穷
\Pi	|Πi=0	|累乘|\sqrt{x}|	$\sqrt{x}$	|求平方根
\overline{a+b}	|$\overline{a+b}$|上划线|\underline{a+b}|	$\underline{a+b}$|下划线
\overbrace{a+b}	|$\overbrace{a+b}$|上括号|\underbrace{a+b}	|$\underbrace{a+b}$	|下括号
\pm{a}{b}	|±ab|	正负号| \mp{a}{b}|	∓ab	|负正号
\times|	×	|乘法|\cdot	|⋅	|点乘
\ast|	∗	|星乘|\div|	÷	|除法|
\frac{1}{5}	|$\frac{1}{5}$	|\not|	$\not$ |	非
\leq|	≤	|小于等于 |\geq	| ≥ |	大于等于
\neq    |	≠	|不等于 |\nleq|	 $\nleq$|	不小于等于
\ngeq	  |$\neq$|不大于等于 |\sim	|∼|	相关符号
\approx	|≈   |	约等于| \equiv|	≡	|常等于/横等于
\bigodot|	⨀	|加运算符|\bigotimes|	⨂	|乘运算符
\in|	∈|	属于|  \notin|	∉	|不属于
\subset|	⊂	|真子集|\not |\subset|	⊄|	非子集
\subseteq|	⊆	|子集|\supset	|⊃	|超集
\supseteq	|⊇	|超集|
\cup|	∪|	并集|\cap|	∩|	交集
\mathbb{R}|	R	|实数集|\emptyset|	∅	|空集

###  希腊符号
写法|	符号| 写法|	符号
---|---|---|---
\alpha|	α|\beta	|β
\gamma|	γ|\Gamma|	Γ
\theta|	θ|\Theta|	Θ
\delta|	δ|\Delta|	Δ
\triangledown	|▽|\epsilon|	ϵ
\zeta|	ζ|\eta|	η
\kappa|	κ|\lambda|	λ
\mu|	μ|\nu	|ν
\xi|	ξ|\pi	|π
\sigma|	σ|\tau|	τ
\upsilon|	υ|\phi|	ϕ|
\omega|	ω




### 多值函数
使用 cases 块表达式，每行 \\结尾，每个元素 & 分隔。

$$
p(x) = 
\begin{cases}
  p, & x = 1 \\
  1 - p, & x = 0
\end{cases}
$$

$$
\begin{align}
  f(x) = a + b \\
       = c + d  \\
\end{align}
$$

### 矩阵
$$
\begin{matrix}
  1 & 0 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 1 \\
\end{matrix}
$$

$$
\begin{pmatrix}
 1& 1\\
 1& 1
\end{pmatrix}
$$

$$
\begin{bmatrix}
 1& 1\\
 1& 1
\end{bmatrix}
$$

$$
\begin{Bmatrix}
 1& 1\\
 1& 1
\end{Bmatrix}
$$

$$
\begin{Vmatrix}
 1& 1\\
 1& 1
\end{Vmatrix}
$$

$$
\begin{vmatrix}
 1& 1\\
 1& 1
\end{vmatrix}
$$

### 标记
在数学表达式之外，但在同一行上，在显式 $ 周围使用 span 标记。

要 <span>$</span>100 分成两半，我们计算 $100/2$

!!! note <details> 块中的任何 Markdown 都将折叠，直到读者单击展开详细信息。 在 块中，使用 <summary> 标记在右侧创建一个标签。

<details><summary>CLICK HERE</summary>

<p>

#### We can hide anything, even code!

```ruby
   puts "Hello World"
```

</p>

</details>
默认情况下，Markdown 将折叠。

Here is a simple flow chart:

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```


要在围栏代码块中显示三重倒引号，请将其包在四个倒引号内。
