# Win 内网转发

```bat
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=3000 connectaddress=192.168.137.3 connectport=3000

netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=3000
```



`netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=3000 connectaddress=192.168.137.3 connectport=3000`表示监听所有ip的3000端口转发给192.168.137.3:3000端口