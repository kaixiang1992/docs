# 配置test_dev权限账号与无密码登录

1.将服务器上所有的包和源都check和更新一遍，确保服务器处在一个崭新的状态
```sh
sudo apt-get update
```

## 创建新账号 {docsify-ignore}
!> 创建一个新账号，然后给它分派一定的权限，最后再赋予它无密码登录的能力，这样这个新账号就不用每次登录服务器的时候都得输入一遍密码了。

```sh
sudo adduser test_dev
```
```sh
Adding user `test_dev' ...
Adding new group `test_dev' (1000) ...
Adding new user `test_dev' (1000) with group `test_dev' ...
Creating home directory `/home/test_dev' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for test_dev
Enter the new value, or press ENTER for the default
	Full Name []: Scott
	Room Number []: 419
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
```

## 授权用户以sudo的方式调用系统命令 {docsify-ignore}

!> 以test_dev用户示例
```sh
gpasswd -a test_dev sudo
```
```sh
Adding user wangxiao to group sudo
```

## 赋予账号更高权限 {docsify-ignore}

!> 打开权限配置文件
```sh
sudo visudo
```
!> 执行sudo visudo 命令，默认是使用nano编辑器打开visudo文件执行 sudo visudo 命令，往里面加入test_dev ALL=(ALL:ALL) ALL

* 上下键，上下移动光标对代码进行编辑

```sh
Defaults        env_reset
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

root        ALL=(ALL:ALL) ALL
rn_manager    ALL=(ALL:ALL) ALL   

%admin      ALL=(ALL) ALL
%sudo       ALL=(ALL:ALL) ALL
```

* 编辑后按下`ctrl+X`，然后按下`shitf+Y`，最后`回车`

## 重启ssh服务 {docsify-ignore}

!> 测试创建账号的登录，保险起见，重启ssh服务

```sh
service ssh restart
```