# rtsp2fmp4

[English](https://github.com/yumexupanic/rtsp2fmp4) | [日本語](https://github.com/yumexupanic/rtsp2fmp4/blob/main/README_jp.md) | [简体中文](https://github.com/yumexupanic/rtsp2fmp4/blob/main/README_zh.md)

This is a high-performance streaming service that allows rtsp to be played in the browser, supporting h264 and h265. The conversion process only involves decapsulation of the data, not re-transcoding.

The playback method supports http and websocket. The streaming media is a stateless executable with no dependencies, and the stateless service is a very important feature, which means you can deploy it very easily.

## Quick start

Download the executable, or use docker.

docker
```shell
docker run -it -d --name rtsp2fmp4 -p 6161:6161 yumexupanic/rtsp2fmp4
```

Use
```shell
# fmp4 over http 
http://127.0.0.1:6161?url=rtsp://xxxx

# fmp4 over websocket
ws://127.0.0.1:6161?url=rtsp://xxxx
```

> If the rtsp account password carries special symbols, you need URL Encoding.

That's all, no need to worry about streaming or network problems causing the program to exit, there are internal mechanisms for reconnecting and so on.

## Latency

Low latency low program is never just streaming media processing, but streaming media + client cooperation.

The streaming media itself has been optimized, the delay of data stream can be controlled at about 1s. If you use http to pull the stream directly, and the client player is not optimized, then the latency may slowly increase.
If you are using http to pull the stream directly, and the client player is not optimized, then the latency may slowly increase, and the player needs to deal with the caching logic.

In browser, fmp4 over http has a limit of 6 channels at most. So we recommend to use websocket solution, with  [wsfmp4.js](https://github.com/yumexupanic/wsfmp4.js) SDK, the latency can be controlled around 1s.

## Why fmp4 instead of flv?

Because flv.js works by repackaging the data into fmp4 and then give it to MSE interface.

## Tips

This is probably the best solution for playing rtsp in the browser at the moment, a lightweight streaming service.
The main reason why the rtsp protocol doesn't work as well as flv.js is because of the rtsp transport protocol.
