# rtsp2fmp4

[English](https://github.com/yumexupanic/rtsp2fmp4) | [日本語](https://github.com/yumexupanic/rtsp2fmp4/blob/main/README_jp.md) | [简体中文](https://github.com/yumexupanic/rtsp2fmp4/blob/main/README_zh.md)

这是一个高性能的流媒体服务，让 rtsp 可以在浏览器中播放,支持 h264 和 h265。转换过程只会涉及到数据的解封装，不会进行重新转码等操作。

播放方式支持 http 和 websocket，流媒体是一个无状态无依赖的可执行文件，其中无状态服务是很重要的一个特性，这意味着你可以很方便的进行部署。

## 快速开始

下载可执行文件，或者使用 docker。

docker
```
docker run -it -d --name rtsp2fmp4 -p 6161:6161 rtsp2fmp4
```

使用
```shell
# fmp4 over http 
http://127.0.0.1:6060?url=rtsp://xxxx

# fmp4 over websocket
ws://127.0.0.1:6060?url=rtsp://xxxx
```

> 如果 rtsp 账号密码携带了特殊符号，需要 URL Encoding

这样就可以了，不需要担心断流或者网络问题导致程序退出，内部做了完善的重连等机制。

## 延迟

低延迟低方案永远不只是流媒体处理，而是流媒体+客户端配合。

流媒体本身做了优化，数据流延迟可控制在 1s 左右。如果你使用的是 http 方式直接拉流，客户端播放器没有做任何优化，
那么延迟可能会慢慢提升，播放器需要处理缓存逻辑。

在浏览器中 fmp4 over http 最多同时有6路的限制。所以推荐使用 websocket 的方案，配合 [wsfmp4.js](https://github.com/yumexupanic/wsfmp4.js) SDK，延迟可控制在 1s 左右。

## 为什么是 fmp4，而不是 flv

因为 flv.js 工作原理是把数据重新解封装为 fmp4 然后给 MSE 接口。

## Tips

也许目前来说，在浏览器播放 rtsp 方案中这是最好的方案，轻量级的流媒体服务。
rtsp 协议之所以无法像 flv.js 一样，主要是因为 rtsp 的传输协议导致。
