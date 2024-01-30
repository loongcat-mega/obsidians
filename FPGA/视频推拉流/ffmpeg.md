## Usage
```bash
-i                  指定输入文件的名字
-y：输出如果重名直接覆盖
-t duration         record or transcode "duration" seconds of audio/video
-ss time_off        set the start time offset
-r rate             set frame rate (Hz value, fraction or abbreviation)
-vn                 disable video
-an                 disable audio
-vcodec codec       force video codec ('copy' to copy stream)
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231226232725.png)


### 以拷贝形式转换
```bash
以拷贝形式转换视频
ffmpeg -i input.mp4 -vcodec copy -acodec copy output.avi
-vcodec copy：拷贝视频  
-acodec copy：拷贝音频
```

### 改变视频帧率

```bash
改变帧率
ffmpeg -i input.mp4 -r 10 -y output.mp4


```
### 提取音频

```bash
提取音频
ffmpeg  -i  video.mp4  -vn  audio.mp3
```

### 指定编码格式


```bash
-c:v libx264  指定编码格式
ffmpeg -i in.avi -c:v libx264 out.mp4  采用h264编码格式编码

nvidia显卡：
-c:v h264_nvenc    nvidiaencoder
ffmpeg -i in.avi -c:v h264_nvenc out.mp4  采用硬件加速
timeit ffmpeg -i in.avi -loglevel error -c:v h264_nvenc out.mp4 -y  屏蔽细节，只显示执行时间？？好像也能压缩视频质量
```


### preset 与 crf 控制视频质量

```bash
-preset 预设:
ffmpeg -i in.mp4 -c:v libx264 -preset xxx out.mp4

ultrafast 编码较快，但会产生较大的文件
superfast
veryfast
faster
fast
medium (默认)
slow
slower
veryslow  编码较慢 但会产生较小的文件

-crf  constant rate factor
ffmpeg -i in.mp4 -c:v libx264 -crf 22 out.mp4
用来控制图像质量 取值范围为0-51，值越大代表质量越差，0代表无损压缩，常用19-28
```

### filter 过滤器

```bash
-filter 过滤器
-vf(video filter) 指定过滤器：
-vf  "filter_name="
```
#### 缩放 
```bash
scale 缩放
ffmpeg -i in.mp4 -c:v libx264 -vf "scale=1024:560" out.mp4	缩放图像尺寸
ffmpeg -i in.mp4 -c:v libx264 -vf "scale=1024:-1" out.mp4	自动推算缩放图像尺寸
```
#### 旋转
```bash
transpose 旋转
ffmpeg -i in.mp4 -c:v libx264 -vf "scale=1024:560,transpose=1" out.mp4	缩放图像尺寸

```
#### 裁剪

```bash
crop 裁剪某一区域
ffmpeg -i in.mp4 -c:v libx264 -vf "crop=w:h:x:y" out.mp4	以(x,y)为左上起点裁剪wh的矩形区域
ffmpeg -i in.mp4 -c:v libx264 -vf "crop=iw/2:ih/2" out.mp4	裁剪宽高为原视频1/2的区域
```

#### 倍速

```bash
setpts 视频倍速 
setpts是视频滤波器通过改变每一个pts时间戳来实现倍速的效果，如下只要把PTS缩小一半就可以实现2倍速，相反的是PTS增加一倍就达到2倍慢放的效果
ffmpeg -i in.mp4 -vf "setpts=0.5*PTS" out.mp4
ffmpeg -i in.mp4 -vf "setpts=2.0*PTS" out.mp4

atempo 音频倍速
ffmpeg -i input.mkv -af "atempo=2.0" -vn output.mkv

音频视频同时倍速
ffmpeg -i input.mkv -filter_complex "[0:v]setpts=0.5*PTS[v];[0:a]atempo=2.0[a]" -map "[v]" -map "[a]" output.mkv
```


#### 调节音量

```bash
volume

ffmepg -i in.mp4 -af "volume=1.5" out.mp4
```

### 片段剪切与合并

```bash
剪切与合并

ffmpeg -i in.mp4 -c:v libx264 -ss 00:00:10  -t  00:00:10 out.mp4
ffmpeg -i in.mp4 -c:v libx264 -ss 00:00:10  -t  10 out.mp4
ffmpeg -i in.mp4 -c:v libx264 -ss 00:00:10  -to  00:00:20 out.mp4
-ss 指定剪切的起始位置
-t 指定市场
-to 指定终止位置


视频合并需要一个合并文件列表每行以 file 'file_name.mp4' 形式展示
ffmpeg -f concat -i mylist.txt -c copy out.mp4
代表把文件中的视频以拷贝而不是重新编码形式合并成一个新视频
```
### 创建缩略图

```bash
创建视频缩略图
ffmepg -i in.mp4 -vf "fps=1/10,scale=-2:720" thumbnail-%03d.jpg
fps:指定文件输出的帧率 每10秒输出一帧画面
thumbnail-%03d.jpg : 图像文件名
```

### 添加水印
```bash
添加水印
ffmpeg -i in.mp4 -i overlay.jpg -filter_complex "overlay=100:100" out.mp4
将overlay.jpg添加在(100,100)起点处
```


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231226234518.png)

### 视频倒放

```bash
ffmpeg -i in.mp4 -vf reverse -af areverse out.mp4
```

