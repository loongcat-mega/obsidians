## 1、获取音频/视频文件信息

为显示你的媒体文件细节，运行：

```text
ffmpeg -i video.mp4 -hide_banner
```
## 2、转换视频文件到不同的格式 

**复制转换**

FFmpeg 是强有力的音频和视频转换器，因此，它能在不同格式之间转换媒体文件。举个例子，要转换 mp4 文件到 avi 文件，运行：

```text
$ ffmpeg -i video.mp4 video.avi
```


类似地，你可以转换媒体文件到你选择的任何格式。

例如，为转换 YouTube flv 格式视频为 mpeg 格式，运行：

```text
$ ffmpeg -i video.flv video.mpeg
```

如果你想维持你的源视频文件的质量，使用 `-qscale 0` 参数：

```text
$ ffmpeg -i input.webm -qscale 0 output.mp4
```

为检查 FFmpeg 的支持格式的列表，运行：

```text
$ ffmpeg -formats
```