# 目录栈

`Zsh` 有一个内置的目录栈。这个工具可以帮助你在整个 `zsh` 会话中管理许多不同的工作。

### 使用方法

使用 **`pushd`** 命令将目录放置到目录堆栈中。使用 **`popd`** 将目录从堆栈中移除。可以使用 **`dirs`** 命令查看或者重建堆栈。

要查看堆栈的内容，只需要简单的输入以下指令（没有参数也可以但是输出不易读）：

```shell
  > dirs -v
  0       /etc/mail
  1       /var
  2       /tmp
  3       /usr/local
  4       /usr
  5       ~
  >
```

如果你要重建堆栈，使用 **`dirs`** 后面跟目录名称列表。`zsh` 会执行以下步骤：

- 删除堆栈的所有目录
- 从最右边开始将所有参数放入堆栈
- 将 `$PWD` 放入堆栈最上面

```shell
  > echo $PWD
  /tmp
  > dirs /usr/local/ews /usr/local /usr /var /var/log $HOME
  > dirs -v
  0       /tmp
  1       /usr/local/ews
  2       /usr/local
  3       /usr
  4       /var
  5       /var/log
  6       ~
  >
```

### `popd`

`popd` 从堆栈中删除条目，并 `cd` 到最新的顶层目录。

行为

- 没有参数时，从堆栈中删除当前顶层目录项，并将`cd`移到新的顶层目录。 
- `popd` 可以带一个参数 `+n` 或 `-n` ，`n`是一个整数。
  - `+n`，会删除从顶部起第`n`个元素
  - `-n`，会删除从底部起第`n`个元素
- 计数从零开始
- 如果你打开选项 `PUSHD_MINUS`，`+` 和 `-` 的意义会相反。

```shell
  > dirs -v
  0       /tmp
  1       /usr/local/ews
  2       /usr/local
  3       /usr
  4       /var
  5       /var/log
  6       ~
  > setopt PUSHD_MINUS
  > popd -2
  > echo $PWD
  /tmp
  > dirs -v
  0       /tmp
  1       /usr/local/ews
  2       /usr
  3       /var
  4       /var/log
  5       ~
  > popd
  > echo $PWD
  /usr/local/ews
  > dirs -v
  0       /usr/local/ews
  1       /usr
  2       /var
  3       /var/log
  4       ~
  >
```

### `pushd`

`pushd` 修改当前目录到一个你指定的参数，并将 `$OLDPWD` 的内容入栈。

- 第一种形式：`pushd [arg]`

修改当前目录到 `arg`。如果 `arg` 没有指定，跳转到第二个元素。

```shell
  > pwd
  /etc
  > dirs -v
  0       /etc
  1       /usr/local/ews
  > pushd mail
  > dirs -v
  0       /etc/mail
  1       /etc
  2       /usr/local/ews
```

**注意**：如果选项 `PUSHD_TO_HOME` 打开，或者只有一个元素在堆栈内，`pushd` 更改当前目录到 `$HOME`。

如果 `arg` 是 `-`，`pushd` 修改目录到 `$OLDPWD`。如果 `arg` 在当前目录找不到，并且 `arg` 不包含 `/`，`zsh` 会查找参数 `cdpath` 的每一个条目。如果选项 `CDABLE_VARS` 打开，并且变量名 `arg` 存在并包含一个完整路径，会将 `arg` 的值作为目录。

如果选项 `PUSHD_SILENT` 没有打开，目录堆栈会在 `pushd` 命令后被打印。

- 第二种形式：`pushd old new`

`pushd` 使用 `new` 替换 `old` ， 并尝试切换到这个新的目录。（和 `cd` 的第二种形式相似）

- 第三种形式：`pushd ｛+ | -｝n`

使 `pushd` 通过旋转堆栈来更改当前目录。参数 `n` 在 `popd` 中已经描述。

```shell
  > setopt PUSHD_MINUS
  > dirs -v
  0       /etc/mail
  1       /usr/local
  2       /usr
  3       ~
  > pushd -2
  /usr
  > dirs -v
  0       /usr
  1       ~
  2       /etc/mail
  3       /usr/local
```

### 文件名扩展和目录堆栈

`Zsh`提供了一种在日常工作中快速引用目录堆栈的好方法。 以下说明直接从手册页获取： 

>  后跟一个数字的`〜`被目录堆栈中的那个位置替换。 `〜0`相当于`〜+`，`〜1`是堆栈的顶部。`〜+`后面跟着一个数字被堆栈中那个位置替换。`〜+0`等于`〜+`，`〜+1`是堆栈的顶部。`〜-`后面跟着一个数字被堆栈中从底部开始的那个位置替换。`〜-0`是堆栈的底部。选项 `PUSHD_MINUS` 交换 `+` 和 `-` 的影响。