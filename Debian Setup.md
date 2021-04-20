# Debian Init

## 将当前用户加入超级用户组 add to root
当前的Debian版本默认创建的用户是不能开启root权限的。需要手动添加
1）进入超级用户模式 也就是输入"su",系统会让你输入超级用户密码，输入密码后就进入了超级用户模式。（当然，你也可以直接用root用） 
2）添加文件的写权限 输入命令"chmod u+w /etc/sudoers"
3）编辑sudoers文件 如“nano /etc/sudoers”
   找到这行："root ALL=(ALL) ALL",在下面添加"xxx ALL=(ALL) ALL"(这里的xxx是你的用户名)
   然后保存退出
4）撤销文件的写权限 也就是输入命令"chmod u-w /etc/sudoers"

## 开启SSH
1）安装
OpenSSh 分为客户端openssh-client 与服务端 openssh-server.一般情况下，我们的linux系统都会自带 client端的。
最简单的方法下载：
apt-get install openssh-client
apt-get install openssh-server

安装完成以后,可以通过以下命令看到它们运行的进程:
ps -e |grep ssh

2）配置
ssh的配置文件为：/etc/ssh/sshd_config
网上一个中文的注释文件（来自：http://xujpxm.blog.51cto.com/8614409/1717862）
带 # 的表示注释掉了

3）启动服务
方法一：
/etc/init.d/ssh restart
方法二：
service ssh restart
通过 service ssh status 可以查看服务的状态

4）登录与退出
输入： ssh 用户名@IP地址 然后会提示你输入密码， 就OK了
如果退出的话，输入： exit

## 开启Samba服务
1） 安装Samba
apt-get install samba

2) 创建一个共享目录
创建一个用于共享的目录，比如在/home目录下创建一个share目录
mkdir -p /home/share
同时给这个目录777权限 chmod -R 777 /home/share
这样子创建的目录是所有人都有访问权限的

3）设置samba参数
修改samba的config文件，创建一个共享目录，可以任意访问
nano /etc/samba/smb.conf
在文件最后加入
```
[disk1]  # 显示的名称
path = /srv/smbshare/disk1  #文件在服务器上的路径
writable = yes
guest ok = yes
write list = username
validusers = username
display charset = UTF-8
unix charset = UTF-8
dos charset = cp936
```
4）重启samba服务
service smbd restart  #重启 
systemctl enable nmbd.service  #设置开机服务

5）samba一些操作
smbpasswd -a username   #将用户加入samba用户组（linux的用户）
pdbedit -w -L #列出现有的Samba用户列表
smbclient ：访问所有共享资源
smbstatus： 列出当前所有的samba连接状态
smbpasswd：修改samba用户口令、增加samba用户。
Nmblookup：用于查询主机的NetBIOS名，并将其映射为IP地址
Testparam： 用于检查配置文件中的参数设置是否正确

```
[共享名称]:共享中看到的共享目录名
comment = 共享的描述. 
path = 共享目录路径(可以用%u、%m这样的宏来代替路径如:/home/share/%u) 
browseable = yes/no指定该共享是否在“网上邻居”中可见。
writable = yes/no指定该共享路径是否可写。
read only = yes/no设置共享目录为只读(注意设置不要与writable有冲突) 
available = yes/no指定该共享资源是否可用。
admin users = bobyuan，jane指定该共享的管理员,用户验证方式为“security=share”时，此项无效。 
valid users = bobyuan，jane允许访问该共享的用户或组-“@+组名” 
invalid users = 禁止访问该共享的用户与组(同上) 
write list = 允许写入该共享的用户
public = yes/no共享是否允许guest账户访问。 
guest ok = yes/no意义同“public”。
create mask = 0700指定用户通过Samba在该共享目录中创建文件的默认权限。0600代表创建文件的权限为rw-------
directory mask = 0700指定用户通过Samba在该共享目录中创建目录的默认权限。0600代表创建目录的权限为rwx---
```
