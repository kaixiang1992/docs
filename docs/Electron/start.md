# Electron 入门

:book:[Electron桌面端](https://electronjs.org/)文档，:clock1030:`2019/05/19 10:16`

安装命令：

```javascript
npm install electron --save-dev
```

如遇安装失败，找到`.npmrc文件`，通常是在`C:\用户`路径下，使用[淘宝镜像](http://npm.taobao.org/) + [中国镜像](https://electronjs.org/docs/tutorial/installation)

```text
registry=https://registry.npm.taobao.org
ELECTRON_MIRROR="https://npm.taobao.org/mirrors/electron/"
```

配置启动命令，`npm start` 如下：

```json
"scripts": {
    "start": "electron ."
}
```