# 模拟其他shell

`zsh`可以模拟`sh`, `ksh`或者`csh`。（`csh`并不是完全模拟）在`sh`和`ksh`模拟中表现出色。

你可以通过运行`emulate some_shell`模拟其他的`shell`。如果你加上`-R`标志，选项会被重置为它们的默认值。

你也可以创建一个名称为`csh`, `ksh` 或 `sh`的链接指向`zsh`。`zsh`会注意到它被不同的名称调用，并尽其所能表现的像你指定的`shell`。