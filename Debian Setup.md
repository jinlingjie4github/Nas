# Debian Init

## 将当前用户加入超级用户组 add to root
当前的Debian版本默认创建的用户是不能开启root权限的。需要手动添加
1）进入超级用户模式 也就是输入"su",系统会让你输入超级用户密码，输入密码后就进入了超级用户模式。（当然，你也可以直接用root用） 
2）添加文件的写权限 输入命令"chmod u+w /etc/sudoers"
3）编辑sudoers文件 如“nano /etc/sudoers”
   找到这行："root ALL=(ALL) ALL",在下面添加"xxx ALL=(ALL) ALL"(这里的xxx是你的用户名)
   然后保存退出
4）撤销文件的写权限 也就是输入命令"chmod u-w /etc/sudoers"
