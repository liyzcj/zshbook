# 数组参数

数组参数只是 `zsh` 中的一个包含多个条目列表的变量。

两种最简单的给数组参数赋值的方式为：

> array_name=(value1 value2 ... valueN)
>
> set -A array-name value1 value2 ... valueN

### 单个元素下标

下标用于从数组中选择一个或多个目标。`zsh` 的数组下标机制非常强大，可以说比很多编程语言都优秀。

简单的形式如下：

> some_array[expr]

这使用算数扩展将 **`expr`** 计算为整数`n。`**`n`**用来返回数组的第**`n`**个元素。

如果 `n` 为**负数**，则从数组的**最后**计算第 `n` 个元素。例如 **`ARGV[-2]`** 将返回 `ARGV` 的倒数第二个元素。

两个特殊的下标，`@` 和 `*` 返回数组的**所有**元素。

### 获取多个连续元素

下面的形式用来获取多个连续的元素：

> some_array[expr1, expr2]

这将返回数组 `some_array` 中由 `expr1` 至 `expr2` 的元素。同时，表达式的值应该是整数，负数的用法和上面一样。

实例：

```shell
  > friends=(c_hong bongus hizatch sh_izzo sh_long mufastaf)
  > echo $friends
  c_hong bongus hizatch sh_izzo sh_long mufastaf
  > echo $friends[2,-1]
  bongus hizatch sh_izzo sh_long mufastaf
  > echo $friends[-4,-2]
  hizatch sh_izzo sh_long
  > echo $friends[-4,3]
  hizatch
  > echo $friends[5,-4]
```

下标可以包含在大括号内的变量名称中。

> ${friends[2,5]} 
>
> 数组语法注释：
>
> 数组元素从1开始编号。 如果想从0开始编号（这样更直观一些），可以打开选项 KSH_ARRAYS。
>
> （注意：打开这个选项还有其他一些影响，具体请看手册）
>
> “@” 和 “*” 有一个细微的差别。
>
> > $meat[@] 返回：`"$meat[1]" "$meat[2]" ... "$meat[n]"` 
> >
> > $meat[*] 返回：`"$meat[1] $meat[2] ... $meat[n]"` 
>
> 如果设置了`KSH_ARRAYS`选项，下标**必须**包含在大括号内的数组名称中。
>
> 注意美元符号**不**包括在命令内。

### 替换部分数组

如果下标在赋值语句的**左侧**，则整个下标表示的范围都将由右侧的表达式替换。

实例：

```shell
  > veg=(lettuce carrot celery tomato onion)
  > echo $veg
  lettuce carrot celery tomato onion
  > veg[4]=(pepper radish)
  > echo $veg
  lettuce carrot celery pepper radish onion
```

### 下标字符串

这部分并非完全和数组相关，但是这很有用。

下标也可以在非数组值上使用。当它被使用时，下标指定这个变量的子字符串。（与数组中的元素列表相反）

实例：

```shell
  > pain=cluster
  > echo $pain
  cluster
  > echo $pain[3]
  u
  > echo $pain[2,5]
  lust
  > echo $pain[-6,7]
  luster
  > unset pain
```