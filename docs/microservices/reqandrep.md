# 响应网络请求(REQ/REP)

`REP模式`响应通信，准备target.txt文本，方便异步读取文件内容 :clock1030:`2019/05/30 23:20`

!> fs.readFile异步读取文件，读取成功的content内容为`Buffer类型`，请确保转为`String类型`:cry:后再进行send下发

## REP模式实现如下：{docsify-ignore}
```javascript
const fs = require("fs");
const path = require("path");
const zmq = require("zeromq");

// TODO: 创建一个reponse消息节点
const responder = zmq.socket("rep");

/**
 * @description 监听message事件，异步读取文件，在调用send方法发送数据
 * content 文件内容 Buffer类型 请务必转toString();
 * timestamp 事件戳
 * pid nodejs进程号
 */
responder.on("message", data => {
    const request = JSON.parse(data);
    fs.readFile(path.resolve(__dirname, request.path), (err, content) => {
        if(err){
            throw err;
        }
        responder.send(JSON.stringify({
            content: content.toString(),
            timestamp: Date.now(),
            pid: process.pid
        }));
    });
});

// TODO: 启动tcp服务，监听60401端口
responder.bind('tcp://*:60401', err => {
    if(err){
        throw err;
    }
    console.log('listening for zmq requesters....');
});

// TODO: 监听进程SIGINT事件，即命令行按下Ctrl+C，关闭所有连接到响应器的连接
process.on('SIGINT', () => {
    console.log('Shutting down....');
    responder.close();
});
```

!> process.on('SIGINT', callback);监听进程SIGINT事件，即命令行按下Ctrl+C

启动运行：
```javascirpt
node index.js
```

## REQ模式实现如下：{docsify-ignore}

!> 响应器处理是逐条响应，即处理完上一条请求，才会开始接受下一条消息。执行fs.readFile()方法时，Nodejs事件循环不会触发新事件。故不太适合要求较高的场景！

##### 发送`单条send`消息时，代码如下： {docsify-ignore}
```javascript
const zmq = require("zeromq");
const filename = process.argv[2];

// TODO: 创建request请求节点
const requester = zmq.socket('req');

/**
 * @description 监听message事件，接收响应消息
 * 节点每次只处理一件事情，暂无并行处理能力
 */
requester.on("message", data => {
    const response = JSON.parse(data);
    console.log('response: ', response);
});

// TODO: 连接TCP服务
requester.connect("tcp://localhost:60401");
// TODO: send发送请求(单次处理信息)
requester.send(JSON.stringify({path: filename}));
```

启动运行：
```text
node index.js target.txt
```

发送`单条send`时，message响应：
```text
response:  { content: '12313', timestamp: 1559231226958, pid: 9164 }
```

##### 发送`多条send`消息时，代码改动如下：{docsify-ignore}
```javascript
// TODO: 连接TCP服务
requester.connect("tcp://localhost:60401");
// TODO: send发送请求(单次处理信息)
// requester.send(JSON.stringify({path: filename}));

// TODO: 多次处理多个信息
for (let index = 0; index < 5; index++) {
    console.log(`sending request ${index} for ${filename}`);
    requester.send(JSON.stringify({path: filename}));
}
```

启动运行：
```text
node index.js target.txt
```

发送`多条send`时，message响应：
```text
sending request 0 for target.txt
sending request 1 for target.txt
sending request 2 for target.txt
sending request 3 for target.txt
sending request 4 for target.txt
response:  { content: '12313', timestamp: 1559231395038, pid: 9164 }
response:  { content: '12313', timestamp: 1559231395071, pid: 9164 }
response:  { content: '12313', timestamp: 1559231395093, pid: 9164 }
response:  { content: '12313', timestamp: 1559231395117, pid: 9164 }
response:  { content: '12313', timestamp: 1559231395139, pid: 9164 }
```