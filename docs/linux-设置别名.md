# linux-设置别名

1. 打开用户目录下的 `vim ~/.bashrc`文件
2. 设置命令别名，例如要设置`ll`为命令`ls -l`的别名，则输入`alias ll='ls -l'`即可（在`debian`系统中，该别名已经存在，只是被注释掉了，仔细查找，应该会有一行`#alias ll='ls -l'`，把前面的`#`号去掉就行了）
3. `alias tt='tmux attach -t'`
4. 执行`source ~/.bashrc`命令使`.bashrc`文件的修改立马生效，否则要重启才能生效
