# 文件名通配符

**文件名生成**是当你在UNIX SHELL中指定一个`pattern`，它会将`pattern`替换为与模式匹配的所有文件的名称。下面是`pattern`一些简单的例子：

```shell
rm *
ls -l get_buf.?
```

另一个常用的术语叫做**通配符扩展**。

`Zsh` 提供了一个非常强大的文件名生成 - 它可以完成所有UNIX shell 可以做的事。要关闭文件名生成，关闭选项`GLOB`。（`unsetopt GLOB`）

### 模式由什么构成？

如果`zsh`在单词中看到以下符号，`zsh`会将这个单词当做一个文件名生成器的`pattern`。

> \* ( | < [ ? 

如果选项`EXTENDED_GLOD`打开，`zsh`还会识别以下内容：

> ^ # 

如果一个或多个文件符合这个`pattern`，`zsh`用匹配文件名的排序列表替换该单词。如果没有匹配，你可以配置`zsh`的行为：

| 配置                | 行为             |
| ------------------- | ---------------- |
| 默认                | 显示错误信息     |
| 设置`NULL_GLOD`选项 | 从命令行删除单词 |
| 设置`NOMATCH`选项   | 保留单词不变     |

### 递归匹配

`Zsh`提供了一种递归匹配的非常有用的方法。使用`**/`,然后它会递归搜索所有的目录。使用`***/`如果你想让`zsh`搜索符号链接。

例如，要在一个目录和所有子目录查找所有的`.c`, `.h`和`.o`文件，输入以下内容：

```shell
  lyric > print -l /usr/local/src/**/*.[cho]
  ...
  /usr/local/src/fvwm-2.2/modules/FvwmWinList/Mallocs.c
  /usr/local/src/fvwm-2.2/modules/FvwmWinList/Mallocs.h
  /usr/local/src/fvwm-2.2/utils/xpmroot.c
  /usr/local/src/mutt-0.95.4/_regex.h
  /usr/local/src/mutt-0.95.4/acconfig.h
```

**注意：匹配`/`和`.`**

当你使用`.`和`/`时，有一些特殊的规则。

1. `/`必须被明确的匹配。

如果你想匹配一个目录，你必须明确的将`/`写入`pattern`。

1. `.`必须明确的在一个`pattern`的**开始**或者跟在`/`**后面**。

以`.`开始的隐藏文件不会匹配通配符**除非**你在开始或者`/`后面加入`.`。

1. 没有`pattern`可以匹配`.`或者`..`。即当前目录和上层目录。

`.`和`..`永远不会再通配中出现。

如果设置了选项`GLOB_DOTS`，则规则2不适用。

[相关选项](https://www-s.acm.illinois.edu/workshops/zsh/related/file_gen.html)