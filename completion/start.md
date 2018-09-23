# 了解自动补全

有许多方式可以在命令行指定补全。我们会讨论最主要的一种方式，这可以让你很舒服的补全。当你准备好应付更多时，查看手册页面。

我们主要讨论的语法形式基本上为：

> compctl [ -CDT ] options [command ...] 

- 补全定义以命令行上的命令`compctl`开头。
- 补全定义以你要补全的命令结尾。
- `options`列表指定所有你要补全的条目类型。（变量，选项，文件名，任务等）

你可以通过键入`compctl`查看当前所有定义。

**一些例子：**

我们来看一个例子。你有一些你经常登录的主机。许多名字很长很麻烦。你想要仅仅输入一些字符然后按`<TAB>`键来补全你的主机名。第一，我们将我们想要补全的主机名放入一个文件，每个一行，并命名为`comp_login_cmds`：

```shell
  > cat ./zsh/comp_login_cmds
  www.nebcorp.com
  www.acm.uiuc.edu
  schrof.net
  www.meat.net
  www.zsh.org
```

现在用于命令行。让我们使用一些很酷的技巧。我们会从完整的命令开始，然后分解它：

```shell
compctl -k "( ` < ./zsh/comp_login_cmds `)" telnet rlogin rsh ssh
```

现在我们看一下上面的命令：

- `comptctl`开始这个定义
- `telnet rsh rlogin ssh`是使补全起作用的命令。
- `-k`表示要补全的条目来自一个数组。使用`-k`，可以是数组的名称，也可以是双引号内的实际定义。
- 反引号`(``)`在`zsh`中运行一条命令，代替命令行的输出命令。
- 在`zsh`中，没有命令的左箭头`<`（输入重定向）在其参数上运行`cat`。因此，上面运行

```shell
cat ./zsh/comp_login_cmds 
```

并将输入打印在命令行中。

假设我们有一些主机名在文件中，现在命令行内部看起来像这样：

```shell
compctl -k "( www.nebcorp.com www.acm.uiuc.edu schrof.net www.meat.net www.zsh.org)" telnet rlogin rsh ssh
```

现在你可以输入：

```shell
> ssh www.< TAB >
```

然后`zsh`会想你展示所有以`www.`开头的条目。如果你输入：

```shell
> ssh www.a< TAB >
```

`zsh`会展示一个以`www.a`开头的条目，因此就是`www.acm.uiuc.edu`。