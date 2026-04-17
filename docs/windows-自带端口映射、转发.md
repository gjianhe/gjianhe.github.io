# windows-自带端口映射、转发


## 1.查看已添加端口映射

```
netsh interface portproxy show all
或者
netsh interface portproxy show v4tov4
或者
netsh interface portproxy show v4tov6
或者
netsh interface portproxy show v6tov4
或者
netsh interface portproxy show v6tov6
```

## 2.添加端口映射转发

```cmd
netsh interface portproxy add v4tov4 listenaddress=10.10.10.234 listenport=64822 connectaddress=172.168.128.68 connectport=22
```

## 3.删除端口映射转发

```cmd
netsh interface portproxy delete v4tov4 listenaddress=10.10.10.234 listenport=64822
```

## 4. 常用
listenaddress=0.0.0.0 代表监听所有ip，注意记得在防火墙开放该端口

```cmd
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=3000 connectaddress=192.168.56.103 connectport=3000

netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=3000
```
