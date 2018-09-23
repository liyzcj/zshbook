# 初始文件


像许多的 `SHELL` 一样，`zsh` 处理许多系统和用户的初始配置文件。了解这些文件的读取顺序和导致文件被忽略的情况非常重要。否则，你的命令或者起始配置文件可能不会被 `zsh` 执行。

### 启动过程

在下面的描述中，`zsh` 默认会查找用户家目录下的配置文件。如果你希望它查找其他目录的配置文件，将参数 `ZDOTDIR` 设置为你想要的目录。

当 `zsh` 启动时，以下文件会被读取：

1. 读取 **`/etc/zshenv`**

如果在 **`/etc/zshenv`** 中未设置 **`RCS`** 选项，所有其他文件都会被跳过。

2. 读取 **`~/.zshenv`**
3. 如果 `SHELL` 为 `login shell`，读取 **`/etc/zprofile`**,然后是**`~/.zprofile`**
4. 如果 `SHELL` 为交互 `SHELL` ，读取 **`/etc/zshrc`**，然后是**`~/.zshrc`**
5. 最后，如果为 `login shell`，读取 **`/etc/zlogin`**，然后是 **`~/.zlogin`**
6. 当用户注销时，读取 **`/etc/zlogout`**，然后是 **`~/.zlogout`**

### 术语解释

- **`login shell`**：登录 `shell` 一般在登录时产生，即当你使用 `/bin/login` 或者其他接入程序时，例如 `SSH`、`rlogin`、`telnet`等。例如：

```shell
ssh HOST_NAME SOME_COMMAND
```

即启动一个登录 `shell`

- **交互 `SHELL` ：交互 `SHELL` 是一个会显示提示并允许用户使用命令交互的 `SHELL`。

### 注意

除了 `/etc` 还有**另一个**目录可以作为全局配置读取。可以再安装时确定。