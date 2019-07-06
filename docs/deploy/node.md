# 搭建node.js与阿里云Alinode环境

## 1.安装系统包 {docsify-ignore}
```sh
sudo apt-get update
sudo apt-get install vim openssl build-essential libssl-dev wget curl git
```

## 2.安装nodejs {docsify-ignore}

!> 选用`nvm`工具，方便升级和管理Node.js版本

```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

!> 安装完毕 nvm 以后，会有提示说，新打开一个命令行窗口操作，那么我们就老老实实把这个关掉，新开一个命令行窗口，然后重新登录进去。

继续执行安装命令，知道`node -v`显示版本`V10.14.2`

```javascript
nvm install v10.14.2
nvm use v10.14.2
nvm alias default v10.14.2
node -v
```

## 3.安装阿里云alinode {docsify-ignore}

```sh
# 安装版本管理工具 tnvm，安装过程出错参考：https://github.com/aliyun-node/tnvm
wget -O- https://raw.githubusercontent.com/aliyun-node/tnvm/master/install.sh | bash
source ~/.bashrc
# tnvm ls-remote alinode 查看需要的版本
tnvm install alinode-v3.11.4 # 安装需要的版本
tnvm use alinode-v3.11.4 # 使用需要的版本
npm install @alicloud/agenthub -g # 安装 agenthub
```

安装完毕后使用tnvm

```sh
source ~/.bashrc
```

安装完 Node 之后，NPM 也就是 Node 的包管理工具也装好了。指定使用国内的taobao镜像来下载：

```sh
npm --registry=https://registry.npm.taobao.org install -g npm
npm -v
```

增加文件监控数目

```sh
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

!> 还是之前安装 npm 时候的原因，为了保证更快、更稳定的安装速度，我们也可以采用 cnpm 来替代 npm 下载包信息：

```javascript
npm --registry=https://registry.npm.taobao.org install -g cnpm
cnpm -v
```

!> 同步最新koa依赖

```javascript
cnpm sync koa
```

安装pm2

```javascript
npm install pm2 -g
```