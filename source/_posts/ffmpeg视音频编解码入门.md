---
title: ffmpeg视音频编解码入门
abbrlink: 19984
date: 2018-11-09 18:27:57
categories: 工具
tags:
  - ffmpeg
---

### `FFmpeg`项目由以下几部分组成

* FFmpeg视频文件转换命令行工具,也支持经过实时电视卡抓取和编码成视频文件；
* ffserver基于HTTP、RTSP用于实时广播的多媒体服务器.也支持时间平移；
* ffplay用 SDL和FFmpeg库开发的一个简单的媒体播放器；
* libavcodec一个包含了所有FFmpeg音视频编解码器的库。为了保证最优性能和高可复用性，大多数编解码器从头开发的；
* libavformat一个包含了所有的普通音视格式的解析器和产生器的库。



使用FFMPEG作为内核视频播放器：
```
Mplayer，ffplay，射手播放器，暴风影音，KMPlayer，QQ影音...
```

使用FFMPEG作为内核的Directshow Filter：
```
ffdshow，lav filters...
```

使用FFMPEG作为内核的转码工具：
```
ffmpeg，格式工厂...
```

事实上，FFMPEG的视音频编解码功能确实太强大了，几乎囊括了现存所有的视音频编码标准，因此只要做视音频开发，几乎离不开它。

[官网](http://ffmpeg.org/)

[下载地址](https://ffmpeg.zeranoe.com/builds/)

>下载完成后把 `bin` 添加到全局变量

1 ffmpeg.exe
ffmpeg是用于转码的应用程序。

一个简单的转码命令可以这样写：

将input.avi转码成output.ts，并设置视频的码率为640kbps
```bash
ffmpeg -i input.avi -b:v 640k output.ts
```

2 ffplay.exe
ffplay是用于播放的应用程序。

一个简单的播放命令可以这样写：

播放test.avi
```bash
ffplay test.avi
```

3. 使用命令 ffmpeg -i http://...m3u8 -c copy out.mkv 将视频流下载并合并成 out.mkv。

### `ffmpeg` 的一些参数
基本使用方式：ffmpeg [[options][`-i’ input_file]] {[options] output_file} 

a) 通用选项 
```
ffmpeg -h 帮助，查看所有参数说明
-i filename 输入文件 
-b bitrate 设置比特率，缺省200kb/s 
-ss position 搜索到指定的时间 [-]hh:mm:ss[.xxx]的格式也支持。使用-ss参数的作用，可以从指定时间点开始转换任务，-ss后的时间单位为秒 
-title string 设置标题（比如PSP中显示影片的标题） 
-author string 设置作者 
-r fps 设置帧频 缺省25 
-s size 设置帧大小 格式为W*H 缺省160*128.也可以直接使用简写
-ab bitrate 设置音频码率 
-ar freq 设置音频采样率 
```


## ffmepg常用的方法

### 1.提取音频
```
ffmpeg -i test.mp4 -acodec copy -vn output.aac 

# 上面的命令，默认mp4的audio codec是aac,如果不是，可以都转为最常见的aac。 
ffmpeg -i test.mp4 -acodec aac -vn output.aac
```

### 2.视频剪切
```
# 下面的命令，可以从时间为00:00:15开始，截取5秒钟的视频。 
ffmpeg -ss 00:00:15 -t 00:00:05 -i input.mp4 -vcodec copy -acodec copy output.mp4 
# -ss表示开始切割的时间，-t表示要切多少。上面就是从15秒开始，切5秒钟出来。
```

### 3.码率控制
码率控制对于在线视频比较重要。因为在线视频需要考虑其能提供的带宽。

那么，什么是码率？很简单： 
> bitrate = file size / duration 
> 比如一个文件20.8M，时长1分钟，那么，码率就是： 
> biterate = 20.8M bit/60s = 20.8*1024*1024*8 bit/60s= 2831Kbps 
> 一般音频的码率只有固定几种，比如是128Kbps， 
> 那么，video的就是 
> video biterate = 2831Kbps -128Kbps = 2703Kbps

**那么ffmpeg如何控制码率？**

ffmpg控制码率有3种选择，`-minrate -b:v -maxrate`  
`-b:v`主要是控制平均码率。 
比如一个视频源的码率太高了，有10Mbps，文件太大，想把文件弄小一点，但是又不破坏分辨率。 
```
ffmpeg -i input.mp4 -b:v 2000k output.mp4 
```
上面把码率从原码率转成2Mbps码率，这样其实也间接让文件变小了。目测接近一半。 
不过，ffmpeg官方wiki比较建议，设置b:v时，同时加上 `-bufsize`。 

`-bufsize` 用于设置码率控制缓冲器的大小，设置的好处是，让整体的码率更趋近于希望的值，减少波动。（简单来说，比如1 2的平均值是1.5， 1.49 1.51 也是1.5, 当然是第二种比较好） 
```
ffmpeg -i input.mp4 -b:v 2000k -bufsize 2000k output.mp4
```

`-minrate` 就简单了，在线视频有时候，希望码率波动，不要超过一个阈值，可以设置maxrate。 
```
ffmpeg -i input.mp4 -b:v 2000k -bufsize 2000k -maxrate 2500k output.mp4
```

### 4.视频编码格式转换
比如一个视频的编码是MPEG4，想用H264编码，咋办？ 
```
ffmpeg -i input.mp4 -vcodec h264 output.mp4 
# 相反也一样 
ffmpeg -i input.mp4 -vcodec mpeg4 output.mp4
```

### 5.视频压缩
将输入的1920x1080缩小到960x540输出:
```
ffmpeg -i input.mp4 -vf scale=960:540 output.mp4 
# ps: 如果540不写，写成-1，即scale=960:-1, 那也是可以的，ffmpeg会通知缩放滤镜在输出时保持原始的宽高比。
```

### 6.为视频添加logo
想要贴到一个视频上，那可以用如下命令： 
```
./ffmpeg -i input.mp4 -i iQIYI_logo.png -filter_complex overlay output.mp4 
```
结果如下所示： 
![001](19984/001.jpg)

要贴到其他地方？看下面：
``` 
右上角： 
./ffmpeg -i input.mp4 -i logo.png -filter_complex overlay=W-w output.mp4 
左下角： 
./ffmpeg -i input.mp4 -i logo.png -filter_complex overlay=0:H-h output.mp4 
右下角： 
./ffmpeg -i input.mp4 -i logo.png -filter_complex overlay=W-w:H-h output.mp4
```

### 7.去掉视频的logo
```
语法：
-vf delogo=x:y:w:h[:t[:show]] 
x:y 离左上角的坐标 
w:h logo的宽和高 
t: 矩形边缘的厚度默认值4 
show：若设置为1有一个绿色的矩形，默认值0。
```
```
ffmpeg -i input.mp4 -vf delogo=0:0:220:90:100:1 output.mp4 
```
结果如下所示： 
![002](19984/002.jpg)

### 8.截取视频图像
```
ffmpeg -i input.mp4 -r 1 -q:v 2 -f image2 pic-%03d.jpeg 
# -r 表示每一秒几帧 
# -q:v表示存储jpeg的图像质量，一般2是高质量。 
```
如此，ffmpeg会把input.mp4，每隔一秒，存一张图片下来。假设有60s，那会有60张。


可以设置开始的时间，和你想要截取的时间。 
```
ffmpeg -i input.mp4 -ss 00:00:20 -t 10 -r 1 -q:v 2 -f image2 pic-%03d.jpeg 
# -ss 表示开始时间 
# -t 表示共要多少时间。 
```
如此，ffmpeg会从input.mp4的第20s时间开始，往下10s，即20~30s这10秒钟之间，每隔1s就抓一帧，总共会抓10帧。