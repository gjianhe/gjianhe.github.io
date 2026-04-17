# samba-安装及配置

## 安装软件

安装软件前先更新下：

> sudo apt update

然后再输入以下命令安装：

> sudo apt install samba -y

## 配置

配置文件在`/etc/samba/smb.conf`路径下，在配置前最好先备份下：

`sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak`

然后在文件`/etc/samba/smb.conf`末尾输入以下内容：

```shell
[share]
	browseable = No
	comment = RaspberryPi
	create mask = 0644
	path = /home/pi/data
	read only = No

```

## 检查配置

配置完成后，要检查下是否配置正确，输入命令`testparm`即可查看配置结果。

## 注册用户

- `sudo smbpasswd -a username` 添加用户
- `sudo pdbedit -L` 查看已经注册的用户