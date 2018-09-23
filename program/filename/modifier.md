# 修饰符

修饰符是一个强大的机制，可以让你更改参数、文件名、和历史命令扩展返回的结果。

以下是`zsh-3.1.5`提供的修饰符列表。

---

**注意：**

带有`<H>`标记只适合历史扩展。

带有`<FP>`标记只适合文件名和参数扩展。

| 标记           | 功能                                            |
| -------------- | ----------------------------------------------- |
| `h`            | 删除尾部路径名，保留头部                        |
| `r`            | 删除形式为`.xxx`的后缀，留下基础名称            |
| `e`            | 删除后缀以外的所有内容                          |
| `t`            | 删除所有主要的路径名，留下尾部                  |
| `p` <H>        | 打印新的命令但是不执行它                        |
| `q` <H>        | 引用替代词，避免进一步替代                      |
| `x` <H>        | 像`q`一样但是在每个空白处插入单词               |
| `l`            | 将单词全部转为小写                              |
| `u`            | 将单词全部转为大写                              |
| `f` <FP>       | 重复修饰符，知道单词不再改变                    |
| `F:expr:` <FP> | 像f一样不过重复`expr`次                         |
| `w` <FP>       | 让修饰符，对字符串中的每个单词生效              |
| `W:sep:` <FP>  | 像w一样，但是单词由`sep`分隔                    |
| `s/l/r[/]`     | 将`r`用`l`代替。如果跟着一个`g`，则只匹配第一个 |
| `&`            | 重复之前的替换，像`s`一样，可以使用`g`          |

`s/l/r/`替换条件如下：

- 左侧**不是**正则表达，而是字符串
- 任何字符都可以用作分隔符代替`/`
- 反斜杠`\`引用分隔符
- 字符`&`在右侧`r`中，被左侧`l`替换
- `&`可以被反斜杠引用
- 一个空的`l`使用前一个来自`l`的字符串或来自上下文扫描`!?s`中的`s`
- 如果`r`紧跟着换行符，你可以省略最右边的分隔符
- 最右边的`?`在上下文扫描中可以省略
- 注意，所有的扩展中最后的`l`和`r`都会保留相同记录