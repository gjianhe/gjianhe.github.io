# git-解决git status命令不能显示中文问题

## 现象

`git status`查看有改动但未提交的文件时总只显示数字串，显示不出中文文件名。

## 原因

在默认设置下，中文文件名在工作区状态输出，中文名不能正确显示，而是显示为八进制的字符编码。

## 解决方法

将git配置文件 `core.quotepath`项设置为`false`。`quotepath`表示引用路径，加上`--global`表示全局配置

`git bash`终端输入命令：

```bash
git config --global core.quotepath false
```