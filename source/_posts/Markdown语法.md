---
title: Markdown语法
categories: Skill
abbrlink: Markdown-Syntax
date: 2024-12-18 10:54:29
tags:
---

![](topic.jpg)

Markdown语法。

<!-- more -->

# 标题

标题可以有六级。

```
# 标题1
## 标题2
### 标题3
#### 标题4
##### 标题5
###### 标题6
```

# 区块引用

区块引用可以嵌套，也可以使用其他的Markdown语法，包括标题、列表、代码区块等。

```
> 引用
> # 标题
>
> > 嵌套引用
>
> 回到原引用
```

# 列表

## 有序列表

```
1. Bird
2. McHale
3. Parish
```

## 无序列表

```
* Red
* Green
* Blue

// 或
+ Red
+ Green
+ Blue

// 或
- Red
- Green
- Blue
```

# 分隔线

以下任意一种均可。

```
* * *
***
*****
​- - -
​---------------------------------------
```

# 超链接

## 行内式

```
This is [an example](http://example.com/ "Title") inline link.
[This link](http://example.net/) has no title attribute.
```

## 参考式

### 显性参考式链接

链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写。

```
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].
​
  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"

// 选其一即可
[1]: http://example.com/  "Optional Title Here"
[1]: http://example.com/  'Optional Title Here'
[1]: http://example.com/  (Optional Title Here)
[1]: <http://example.com/>  "Optional Title Here"
```

### 隐性链接标记

```
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].
​
  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"
```

# 强调

## 强调一行

```
*single asterisks*
_single underscores_
**double asterisks**
​__double underscores__
```

如果要在文字前后直接插入普通的星号或底线，则可以用反斜线。

```
\*this text is surrounded by literal asterisks\*
```

## 行内强调

```
un*frigging*believable
```

# 代码

```
Use the `printf()` function.
```

如果要在代码区段内插入反引号，可以用多个反引号来开启和结束代码区段。

```
``There is a literal backtick (`) here.``
```

# 图片

## 行内式

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

## 参考式

```
![Alt text][id]
[id]: url/to/image  "Optional title attribute"
```

# 折叠块

注意标签与内容间要空一行。

```
<details>
<summary>[标签]</summary>

[内容]
</details>
```

# 公式

```
// 行内公式
$公式内容$

// 独行公式
$$公式内容$$
```

## 常用符号

绝对值为`||`。

|             符号             |       含义       |
|------------------------------|------------------|
| **上/下标与组合**            |                  |
| ^                            | 上标             |
| _                            | 下标             |
| {}                           | 组合             |
| **汉字、字体与格式**         |                  |
| \mbox{}                      | 汉字             |
| \displaystyle                | 字体             |
| \underline{}                 | 下划线           |
| \tag{数字}                   | 标签             |
| \overbrace{算式}             | 上大括号         |
| \underbrace{算式}            | 下大括号         |
| \stacrel{上位符号}{基位符号} | 上位符号         |
| **占位符**                   |                  |
| \qquad                       | 两个quad空格     |
| \quad                        | quad空格         |
| \                            | 大空格           |
| \:                           | 中空格           |
| \,                           | 小空格           |
| `\!`                         | 紧贴             |
| **定界符与组合**             |                  |
| ()                           | 小括号（小）     |
| \big(\big)                   | 小括号（中）     |
| \Big(\Big)                   | 小括号（大）     |
| \bigg(\bigg)                 | 小括号（超大）   |
| \Bigg(\Bigg)                 | 小括号（特大）   |
| `[]`                         | 中括号           |
| `\{ \}`                      | 大括号           |
| \left \right                 | 自适应括号       |
| {上位公式 \choose 下位公式}  | 组合公式         |
| {上位公式 \atop 下位公式}    | 组合公式         |
| **运算符**                   |                  |
| +                            | 加               |
| -                            | 减               |
| \pm                          | 加减             |
| \mp                          | 减加             |
| \times                       | 乘               |
| \cdot                        | 点乘             |
| \ast                         | 星乘             |
| \div                         | 除               |
| /                            | 斜法             |
| \frac{分子}{分母}            | 分式             |
| {分子} \over {分母}          | 分式             |
| \overline{算式}              | 平均数           |
| \sqrt                        | 开平方           |
| `\sqrt[开方数]{被开方数}`    | 开方             |
| \log                         | 对数             |
| \lim                         | 极限             |
| \displaystyle \lim           | 极限             |
| \sum                         | 求和             |
| \displaystyle \sum           | 求和             |
| \int                         | 积分             |
| \displaystyle \int           | 积分             |
| \partial                     | 微分             |
| **逻辑运算**                 |                  |
| =                            | 等于             |
| >                            | 大于             |
| <                            | 小于             |
| \geq                         | 大于等于         |
| \leq                         | 小于等于         |
| \neq                         | 不等于           |
| \ngeq                        | 不大于等于       |
| \not\geq                     | 不大于等于       |
| \nleq                        | 不小于等于       |
| \not\leq                     | 不小于等于       |
| \approx                      | 约等于           |
| \equiv                       | 恒等于           |
| **集合**                     |                  |
| \in                          | 属于             |
| \notin                       | 不属于           |
| \not\in                      | 不属于           |
| \subset                      | 子集             |
| \supset                      | 子集             |
| \subseteq                    | 真子集           |
| \supseteq                    | 真子集           |
| \subsetneq                   | 非真子集         |
| \supsetneq                   | 非真子集         |
| \not\subset                  | 非子集           |
| \not\supset                  | 非子集           |
| \cup                         | 并集             |
| \cap                         | 交集             |
| \setminus                    | 差集             |
| \bigodot                     | 同或             |
| \bigotimes                   | 同与             |
| \mathbb{R}                   | 实数集           |
| \mathbb{Z}                   | 整数集           |
| \emptyset                    | 空集             |
| **数学符号**                 |                  |
| \infty                       | 无穷             |
| \imath                       | 虚数             |
| \jmath                       | 虚数             |
| \hat{a}                      |                  |
| \check{a}                    |                  |
| \breve{a}                    |                  |
| \tilde{a}                    |                  |
| \bar{a}                      |                  |
| \vec{a}                      | 矢量             |
| \acute{a}                    |                  |
| \grave{a}                    |                  |
| \mathring{a}                 |                  |
| \dot{a}                      | 一阶导数         |
| \ddot{a}                     | 二阶导数         |
| \uparrow                     | 上箭头           |
| \Uparrow                     | 上箭头           |
| \downarrow                   | 下箭头           |
| \Downarrow                   | 下箭头           |
| \leftarrow                   | 左箭头           |
| \Leftarrow                   | 左箭头           |
| \rightarrow                  | 右箭头           |
| \Rightarrow                  | 右箭头           |
| \ldots                       | 底端对齐的省略号 |
| \cdots                       | 中线对齐的省略号 |
| \vdots                       | 竖直对齐的省略号 |
| \ddots                       | 斜对齐的省略号   |
| **希腊字母**                 |                  |
| A                            | A                |
| \alhpa                       | α                |
| B                            | B                |
| \beta                        | β                |
| \Gamma                       | Γ                |
| \gamma                       | γ                |
| \Delta                       | Δ                |
| \delta                       | δ                |
| E                            | E                |
| \epsilon                     | ϵ                |
| Z                            | Z                |
| \zeta                        | ζ                |
| H                            | H                |
| \eta                         | η                |
| \Theta                       | Θ                |
| \theta                       | θ                |
| I                            | I                |
| \iota                        | ι                |
| K                            | K                |
| \kappa                       | κ                |
| \Lambda                      | Λ                |
| \lambda                      | λ                |
| M                            | M                |
| \mu                          | μ                |
| N                            | N                |
| \nu                          | ν                |
| \Xi                          | Ξ                |
| \xi                          | ξ                |
| O                            | O                |
| \omicron                     | ο                |
| \Pi                          | Π                |
| \pi                          | π                |
| P                            | P                |
| \rho                         | ρ                |
| \Sigma                       | Σ                |
| \sigma                       | σ                |
| T                            | T                |
| \tau                         | τ                |
| \Upsilon                     | Υ                |
| \upsilon                     | υ                |
| \Phi                         | Φ                |
| \phi                         | ϕ                |
| X                            | X                |
| \chi                         | χ                |
| \Psi                         | Ψ                |
| \psi                         | ψ                |
| \v                           | Ω                |
| \omega                       | ω                |

## 矩阵表示

`&`用于分隔，`\\`用于换行。

```
\begin{matrix}
1 & 2 \\
3 & 4
\end{matrix}
```

## 分段函数

`&`用于分隔，`\\`用于换行。

```
函数名=
\begin{cases}
公式1 & 条件1 \\
公式2 & 条件2 \\
公式3 & 条件3
\end{cases}
```

# 参考教程

## Markdown 语法说明 (简体中文版)

```
https://www.appinn.com/markdown/
```

## Markdown 分段函数写法

```
https://blog.csdn.net/Hey___Man/article/details/79272137
```

## Markdown数学公式语法

```
https://www.jianshu.com/p/e74eb43960a1
```
