# 发布和订阅消息 

使用zeromq搭建发布者PUB/订阅者SUB服务 :clock1030:`2019/05/29 23:58`

!> 实现功能microservices-pub`fs.watch`监听`process.argv[2]`传入文件名对应文件改变，文件改变即发布改变通知

microservices-pub发布者实现：
```javascript
const path = require("path");
const fs = require("fs");
const zmq = require("zeromq");
const filename = process.argv[2];

if(!filename){
    throw new Error("a file to watch must be specified!");
}

// TODO: 创建一个消息发布节点
const publisher = zmq.socket('pub');

/**
 * @description watch监听改变，回调使用发布者send
 */
fs.watch(path.resolve(__dirname, filename), () => {
    publisher.send(JSON.stringify({
        type: 'changed',
        file: filename,
        timestamp: Date.now()
    }));
});

// TODO: 启动pub服务，监听60400端口
publisher.bind('tcp://*:60400', err => {
    if(err){
        throw new Error(err);
    }
    console.log('Listening for zmq subsribers...');
});
```

启动发布者服务`(target.txt => process.argv[2])`：
```text
node index.js target.text
```

microservices-sub订阅者实现：
```javascript
const zmq = require("zeromq");

// TODO: 创建订阅subscriber节点
const subscriber = zmq.socket('sub');

/**
 * @description 接收发布者所有信息
 * 接收特定类型信息，传入一个字符串作为前缀过滤
 */
subscriber.subscribe('');

/**
 * @description 接收发布者publisher信息，触发message事件
 * data类型为buffer
 */
subscriber.on('message', data => {
    const message = JSON.parse(data);
    let { file, timestamp } = message;
    console.log(`File "${file}" changed at ${new Date(timestamp)}`)
});

// TODO: 建立连接
subscriber.connect('tcp://localhost:60400');
```

启动订阅者服务：
```text
node index.js
```