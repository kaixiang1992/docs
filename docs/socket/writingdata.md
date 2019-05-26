# Socket连接发送数据

使用[net.createServer](http://nodejs.cn/api/net.html#net_net_createserver_options_connectionlistener)创建TCP服务器 :clock1030:`2019/05/26 21:15`

> 监听目标文件变化，通过watcher对象+connection.wirte将文件改动事件发送给客户端

```javascript
const fs = require("fs");
const path = require("path");
const net = require("net");
const filename = process.argv[2];

if(!filename){
    throw new Error(`Error: No filename specified.`);
}

net.createServer((c) => {
    // 'connection' 监听器。
    console.log('客户端已连接');
    c.write(`Now watching "${filename}" for changes...\n`);
    const watcher = fs.watch(path.resolve(__dirname, filename), () => {
        c.write(`File changed: "${Date.now()}"\n`);
    });
    c.on('end', () => {
        console.log('客户端已断开连接');
        watcher.close();
    });
}).listen(8124, () => {
    console.log('服务器已启动');
});
```

终端窗口第一步：启动node服务，监听`target.txt文件`
```javascript
node index.js target.txt
```

终端窗口第二步： 使用[telnet命令](https://www.cnblogs.com/ylcms/p/7250129.html)，连接TCP服务器
```javascript
telnet localhost 8124
```

## 消息协议，切换JSON格式推送至客户端 {docsify-ignore}

> 使用JSON.stringify对消息进行序列化，然后使用connection.wirte发送给客户端

```javascript
const fs = require("fs");
const path = require("path");
const net = require("net");
const filename = process.argv[2];

if(!filename){
    throw new Error(`Error: No filename specified.`);
}

net.createServer((c) => {
    // 'connection' 监听器。
    console.log('客户端已连接');
    c.write(JSON.stringify({type: 'watching', file: filename}) + '\n');
    const watcher = fs.watch(path.resolve(__dirname, filename), () => {
        c.write(JSON.stringify({type: 'changed', timestamp: Date.now()}) + '\n');
    });
    c.on('end', () => {
        console.log('客户端已断开连接');
        watcher.close();
    });
}).listen(8124, () => {
    console.log('服务器已启动');
});
```