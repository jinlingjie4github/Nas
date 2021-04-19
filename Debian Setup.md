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
