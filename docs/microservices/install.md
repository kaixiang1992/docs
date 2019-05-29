# 安装zeromq模块 

使用:book:[zeromq模块](http://zeromq.github.io/zeromq.js/) :clock1030:`2019/05/29 22:43`

第一步，windows用户，请先运行下列命令，安装Python 2.7
```text
npm install -g windows-build-tools 
```

第二步，安装zeromq
```text
npm install zeromq
```

第三步，查看zeromq是否安装成功，成功返回版本，例：`4.2.2`：
```text
node -p -e "require('zeromq').version"
```
