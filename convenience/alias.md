# 别名

### 什么是别名

在大多数 `SHELL` 中，别名是用户**自定义**的命令的替代名称。它们经常被用来缩短命令长度，或者代表更常用的命令。例如，某用户可以定义 `myprocs` 来运行 `ps -fu $LOGNAME`。

`Zsh` 由以下方式定义别名。它有一个非常灵活和强大的系统来定义、使用和操作别名。

### 别名是怎么工作的？

检查输入中的每个标记以确定是否为它定义了别名。如果定义了别名，这段字符会在以下情况被它定义的命令替换：

- 这个别名在命令的某个位置
- 别名是全局性
- 该行的上一个单词是一个以空格结尾的别名

### 使用别名

`Zsh` 内建函数 `alias` 的语法如下

> alias [ -gmrL ]  \[name [=value] ...]

`-r` 参数表示 `alias` 命令为**常规模式**。 `-g` 参数表示 `alias` 为**全局模式**。若不加 `-r` 或者 `-g` 则对这两种类型都有效。

如果只写某个值，`zsh` 将打印名称和它的定义。若没有任何参数，`alias` 打印所有定义的 `alias`。

使用 -L 参数，别名的会剪切复制到你的启动脚本中。

要定义一个或多个别名，只需要输入：

> alias **name1=value1 name2=value2 ... nameN=valueN**

对于每个相应的名称，`zsh` 将使用该值定义一个别名。

### 全局别名

即使你的别名不再命令中的第一个单词，`zsh`也允许你使用别名。这种别名称为 **全局别名**。

例如，假设你对你的 `.procmailrc` 文件做了许多工作。你运行了许多工具，例如 `emacs`，`cp`，`less` 等等。你不想为你的每个命令定义一个别名。这将会执行以下命令：

```shell
alias wprc="wc -l ~/.procmailrc"
alias cprc="cp ~/.procmailrc ~/.procmailrc.safe"
alias eprc="emacs ~/.procmailrc"
```

相反，你可以使用文件的全局别名。要定义一个全局别名，使用 `-g` 参数。

```shell
alias -g prc=~/.procmailrc
```

现在你可以简单的在任何地方查看你的 `.procmailrc` 文件：

```shell
lyric > emacs -nw prc
```

看[这里](https://www-s.acm.illinois.edu/workshops/zsh/related/messy_global.html)，如果你对一个不是那么强大的别名感兴趣。

### 列出匹配模式的别名

你可以让` zsh` 打印出那些符合给定模式的别名，而不是所有的。你可以添加参数 `-m`到 `alias` 命令中。`pattern` 与文件名生成模式完全相同。事实上，你需要在 `pattern` 两边增加单引号以防止 `zsh` 将它当做文件名匹配模式。

### 禁用和删除别名

你可以使用以下命令删除别名`foo` ： `unalias foo`

你也可以**临时**禁用别名`foo` ： `disable -a foo`

也可以重新激活它： `enable -a foo`

最后，你可能会有一个别名例如 `ftp` ，它实际上运行了一个不同的程序。如果你想要使用真正的 `ftp` 程序，在命令中使用 `=ftp` 或者 `\ftp`。