# 创建读/写流

使用[process.stdout](http://nodejs.cn/api/process.html#process_process_stdout)标准输出数据  :clock1030:`2019/05/26 14:54`
```javascript
const fs = require("fs");
const path = require("path");
const filename = process.argv[2];

/**
 * @description 必须指定读取文件
 */
if(!filename){
    throw new Error("a file to watch must be specified!");
}

/**
 * @description 使用文件流把数据传送到标准输出
 * 1213344
 */
fs.createReadStream(path.resolve(filename)).pipe(process.stdout);
```

使用`EventEmitter`监听事件
```javascript
const fs = require("fs");
const path = require("path");
const filename = process.argv[2];

/**
 * @description 必须指定读取文件
 */
if(!filename){
    throw new Error("a file to watch must be specified!");
}

/**
 * @description 监听文件流的data事件实现效果
 */
fs.createReadStream(path.resolve(filename)).on('data', (chunk) => {
    process.stdout.write('\n');
    process.stdout.write(chunk);
}).on('error', (e) => {
    process.stderr.write(`Error: ${e.message}\n`);
});
```
