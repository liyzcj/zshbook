# 更多实例

例如：

```shell
compctl -o setopt unsetopt
```

允许你在命令`setopt`和`unsetopt`后补全所有`shell`选项。

---

例如：

```shell
compctl -j -P "%" kill
```

允许你为`kill`命令补全。`-j`补全正在运行的命令，`-P arg`指定一个前缀到补全结果。