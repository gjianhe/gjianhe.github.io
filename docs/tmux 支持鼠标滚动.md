# tmux 支持鼠标滚动


刚安装了tmux。发现tmux默认不支持滚动上翻，超级不方便.

## 解决方案

### 1、通过tmux界面输入

进入tmux界面，按缀ctrl+B后，再按冒号：进入命令行模式，
输入以下命令：

> set -g mouse on

### 2、通过配置文件

通过tmux界面，有限制。对于其他新建的tmux 客户端不起作用

在用户目录的`.tmux.conf`(没有就新建)下，添加`set -g mouse on`


最后刷新文件

> tmux source-file .tmux.conf
