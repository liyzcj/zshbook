# 下标

如果你觉得下标还不够强大，`zsh` 提供的一些标志来修改选择数组或字符串一部分的行为。

### 语法

每个标志都是一个字符。一些标志需要参数；它们的形式是 `F:arg:`，`F`是一个单字母标志，`arg` 是标志的参数。

你要指定的一个或多个标志**必须**包含在括号内。它可以再方括号后立即出现。如果范围被指定了（`2`个由逗号分隔的下标），你可以在逗号后立即使用另一个标志。

> ${VAR[(X)expr1,(Y)expr2] 

其中`X`是一组标志，`Y`可以是另一组标志。

### 各种标志

- 标志：`w`

告诉`zsh`用**单词**指定一个标量，而不是单个字符。

实例：

```shell
  > VAR="this is a big long sentence"
  > echo $VAR[(w)5]
  long
  > echo $VAR[2,(w)4]
  his is a big
  > echo $VAR[(w)2,15]
  is a big l
  > echo $VAR[(w)5,17]
  lon
  > echo $VAR[(w)-3,-4]
  big long sente
  > echo $VAR[(w)-5,(w)-2]
  is a big long
```

- 标志：`s:string:`(和`w`一起使用)

如果你想使用除了空格以外的字符来分隔标量，可以使用`s`指定。

实例：

```shell
  > AR="this/is/a/long/sentence"
  > echo $AR[(ws:/:)4]
  long
```

- 标志`p`

有时候你需要分隔奇怪的或不可打印的字符。如果你在`s:string:`之前加入`p`，你可以使用所有的`print `转义序列。(`print`是`zsh`内建命令)。

用一个复杂的例子来详细说明：

```shell
  > # The < BEEP > marks where the computer beeps
  > AR="this\C-gis a long sentence"
  > echo $AR
  this\C-gis a long sentence
  > print $AR
  this< BEEP >is a long sentence
  > # Don't forget to escape the backslash for the control character!
  > echo $AR[(wps:\\C-g:)1]
  this
```

- 标志：`f`

使用`f`标志如果你想以行为单位指定下标。这是`(wps: \n:)`的缩写。

实例：

```shell
  > AR=`head /etc/passwd`
  > echo $AR[(f)2]
  sumikeh:x:0:3::/:/usr/bin/zsh
```

- 标志：`r`

这是一个有趣并且有用的标志。当你指定标志`r`时，`zsh`会将下标表达式作为一个`pattern`而不是一个整数。

它返回的东西取决于你指定的变量类型。

| 变量类型 | 返回结果                          |
| -------- | --------------------------------- |
| 一个数组 | 匹配`pattern`的第一个**元素**     |
| 一个标量 | 匹配`pattern`的第一个**子字符串** |

一个带有`w`标志的标量会返回匹配的第一个**单词**。

- 标志：`R`

这个标志和`r`很像，但是它返回的是**最后一个**匹配的条目。

- 标志：`i`

这个标志和`r`相似，不过返回的是**第一个**匹配条目的**索引**。

- 标志：`I`

这个标志和`i`相似，不过返回的是**最后一个**匹配条目的**索引**。

- 标志：`n:expr:`

`n`是标志`r`, `R`, `i`和`I`中的一个。

`expr`是一个整数。（或者一个表达式，它的值是整数！）我们称这个值为`x`，`zsh`会返回第`x`个匹配的值（或倒数第`x`个）。