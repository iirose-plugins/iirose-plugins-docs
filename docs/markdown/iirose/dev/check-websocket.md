# IIROSE 开发方法

## 监听ws消息

安装浏览器js脚本：[IIROSE WebSocket Logger.js](https://github.com/iirose-plugins/koishi-plugin-adapter-iirose/blob/main/scripts/IIROSE%20WebSocket%20Logger.js)

然后打开 https://iirose.com/ 页面

按下`F12`，查看`console`

即可看到各种输出的ws消息了。

## 发送ws消息

在`console`输入
```js
send(">#")
```
然后回车。

你就可以看到
```log
■■■ IIROSE Websocket ■■■ 收到消息: >1158"583.04694194009"0.50349476851476"0"40.981
```
