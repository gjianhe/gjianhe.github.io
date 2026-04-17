# linux-为 APT 命令设置代理

创建一个 `proxy.conf` 文件，如下所示

```
sudo vim /etc/apt/apt.conf.d/proxy.conf
```

对于没有用户名和密码的代理服务器，如图所示进行定义。

```
Acquire {
  http::Proxy "http://192.168.137.1:10809/";
  https::Proxy "http://192.168.137.1:10809/";
}
```

对于具有用户名和登录详细信息的代理服务器：

```
Acquire {
   http::Proxy "http://username:password@proxy-IP-address:proxyport/";
   https::Proxy "http://username:password@proxy-IP-address:proxyport/";
}
```


## [参考链接](https://linux.cn/article-15815-1.html)



