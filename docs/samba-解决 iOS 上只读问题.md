# samba-解决 iOS 上只读问题


在使用 Debian 作为 SMB 服务器时，可能会遇到从 iOS 设备访问共享文件夹时只能读取而无法写入的问题。本文将指导您如何配置 Samba 服务以解决这一问题。

## 修改 Samba 配置文件

要允许其他设备对共享目录进行写操作，需要编辑 `/etc/samba/smb.conf` 文件，并确保 `read only` 参数被设为 `no` 或者被注释掉。

然而，iOS 设备的只读问题并非由 `read only` 参数导致。为了使 iOS 设备能够正常读写，还需要在 `[global]` 模块下添加如下行：

```
vfs objects = streams_xattr
```
此设置启用了替代数据流（ADS）的支持，这对于某些应用程序（如 Microsoft Edge 浏览器）至关重要。

## 重启 Samba 服务

完成上述配置后，请重启 Samba 服务以应用更改：

```
sudo systemctl restart smb
```


之后，尝试重新连接您的 iOS 设备到 SMB 共享，此时应该已经解决了只读限制的问题。

更多关于 `streams_xattr` VFS 模块的信息可以参考 [Samba Wiki](https://www.samba.org/samba/docs/current/man-html/vfs_streams_xattr.8.html)。

