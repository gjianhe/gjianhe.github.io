# linux-debian安装ifconfig

如果您尝试使用 Debian 10中的 **ifconfig** 命令获取 **IP** 或网络详细信息，则会遇到“ifconfig: command not found”错误。

> -bash: ifconfig: command not found

Debian 默认未安装 **ifconfig** 软件包。这是因为不建议使用 **ifconfig**，而推荐使用新的 **ip** 命令。现在，此 **ip** 命令负责修改或显示路由，网络设备，接口和隧道。
如果你还想使用旧的 **ifconfig** 命令，你必须重新安装它。
在 **Debian** 中安装 **ifconfig** 命令
如果尝试直接安装 ifconfig 命令，则 Debian 系统将找不到此软件包。
> root@debian:~# apt install ifconfig
> Reading package lists… Done
> Building dependency tree
> Reading state information… Done
> E: Unable to locate package ifconfig

这是因为 **ifconfig** 本身不是包。它与包含一些其他联网工具的 **net-tools** 软件包一起安装。
因此，要获取 **ifconfig**，您需要安装 **net-tools** 软件包，如下所示：

>  sudo apt install net-tools

安装后，使用ifconfig命令：

> ifconfig
