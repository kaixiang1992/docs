# 文件系统事件循环

:link:[demo地址](https://github.com/kaixiang1992/node-dev-practice)以watching-开头文件夹 使用fs.watch监听文件变化  :clock1030:`2019/05/25 22:40`

新建index.js，如下代码：
```javascript
const fs = require("fs");
const path = require("path");
/**
 * @description process.argv[2]
 * 接收使用命令行参数 
 * [2]为命令行下标第二个参数
 */
const filename = process.argv[2];
console.log(filename);

/**
 * @description 必须指定观察文件
 */
if(!filename){
    throw new Error("a file to watch must be specified!");
}

fs.watch(path.resolve(__dirname, filename), () => {
    console.log(`File ${filename} changed!`);
});

console.log(`Now watching ${filename} for changes....`);
```

使用，例新建target.txt
```javascript
node index.js target.txt
```

node.js运行模式：

* 正常加载代码执行，命令行输出Now watching
* 由于调用fs.watch，node.js不会退出
* fs模块监听目标文件变化
* 目标文件发生变化时，执行回调函数
* 继续监听，node.js不会退出
