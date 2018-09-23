# 限制模式

有时候，我们希望严格限制UNIX账户的访问权限和特权。一旦有人想在`linux`上为他的朋友建立一个来宾账户。但是，所有者想要阻止用户窥探系统并预先禁止任何恶意活动。`zsh`通过限制模式提供了这个功能。

### 限制模式的好处

当`zsh`运行在限制模式时，用户不能够：

- 通过`set +r`或者`unsetopt RESTRICTED`关闭限制模式
- 指定一个**任何**地方包含`/`的命令名字
- 通过`cd`命令改变目录
- 使用`hash`指定命令路径名
- 使用`exec`内建命令将`shell`替换为另一个
- 将输出重定向到文件
- 使用明确给定的包含`/`的路径名指定要被加载的模块
- 使用`jobs -Z`来重写`shell`进程的参数和环境空间
- 使用`ARGVO`参数为外部命令重写`argv[0]`
- 修改或关闭一下选项：`PATH`, `MODULE_PATH`, `SHELL`, `HISTFILE`, `HISTSIZE`, `GID`, `EGID`, `UID`, `EUID`, `USERNAME`, `LD_LIBRARY_PATH`, `LD_AOUT_LIBRARY_PATH`, `LD_PRELOAD`, `LD_AOUT_PRELOAD`

上面这些限制并不能让一个账户“安全”。你应该为限制`shell`谨慎的配置初始启动配置文件。

- 你应该在初始配置文件中设置`PATH`来制定一个安全命令文件夹供用户使用
- 各种`zsh`内建命令可以在初始配置中关闭：

> disable COMMAND

`COMMAND`是要关闭的内建命令。

### 打开限制模式

有三种方式可以打开限制模式。前两个在启动时，而最后一个可以是任何时候。

1. 将`-r`命令选项用于`zsh`。
2. 以`r`开头的命令调用`zsh`。

一个简单的方法是建立一个指向`zsh`的软链接称为`rzsh`：

```shell
  lyric[251]: ln -s ./zsh rzsh
  lyric[252]: ./rzsh
  lyric[1]: cd
  cd: restricted
  zsh: exit 1
  lyric[2]:
```

注意你仍然可以模拟其他`shell`。在`r`被删除后，下一个字母用于确定仿真。例如，`rksh`会让`zsh`模拟`ksh`，并运行在限制模式。

3. 打开选项`RESTRICTED`。