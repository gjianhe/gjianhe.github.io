# ssh-免密登录

## 1. 创建SSH Key

> ssh-keygen -t rsa -C “youremail@example.com”

`-t` 指定生成密钥的类型，默认 RSA  
`-C` 提供一个新注释，比如邮箱

把邮件地址换成自己的邮件地址，然后一路回车，使用默认值即可。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是`SSH Key`的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

## 2. 上传公钥

> ssh-copy-id user@remote_host \[-p 22\]

或者

手动把公钥`id_rsa.pub`文件内容复制到服务器 `~./.ssh/`文件夹下的 `authorized_keys`文件中

## 3. 参考

1.  [创建密钥](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)
2.  [ssh免密登陆](https://www.cnblogs.com/luckforefforts/p/13797705.html)
