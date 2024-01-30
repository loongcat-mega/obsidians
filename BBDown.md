一款命令行式哔哩哔哩下载器. Bilibili Downloader.

```bash
Description:
  BBDown是一个免费且便捷高效的哔哩哔哩下载/解析软件.

Usage:
  BBDown <url> [command] [options]

Arguments:
  <url>  视频地址 或 av|bv|BV|ep|ss

Options:
  -tv, --use-tv-api                              使用TV端解析模式
  -app, --use-app-api                            使用APP端解析模式
  
  -q, --dfn-priority <dfn-priority>              画质优先级,用逗号分隔 例: "8K 超高清, 1080P 高码率, HDR 真彩, 杜比视界"
  -info, --only-show-info                        仅解析而不进行下载
  --show-all                                     展示所有分P标题
  -ia, --interactive                             交互式选择清晰度
  -hs, --hide-streams                            不要显示所有可用音视频流
  -mt, --multi-thread                            使用多线程下载(默认开启)
  --video-only                                   仅下载视频
  --audio-only                                   仅下载音频
  --danmaku-only                                 仅下载弹幕
  --sub-only                                     仅下载字幕
  --cover-only                                   仅下载封面
  --debug                                        输出调试日志
  --skip-mux                                     跳过混流步骤
  --skip-subtitle                                跳过字幕下载
  --skip-cover                                   跳过封面下载
  --force-http                                   下载音视频时强制使用HTTP协议替换HTTPS(默认开启)
  -dd, --download-danmaku                        下载弹幕
  --skip-ai                                      跳过AI字幕下载(默认开启)
  --video-ascending                              视频升序(最小体积优先)
  --audio-ascending                              音频升序(最小体积优先)
  -p, --select-page <select-page>                选择指定分p或分p范围: (-p 8 或 -p 1,2 或 -p 3-5 或 -p ALL 或 -p LAST)
  --language <language>                          设置混流的音频语言(代码), 如chi, jpn等
  -ua, --user-agent <user-agent>                 指定user-agent, 否则使用随机user-agent
  -c, --cookie <cookie>                          设置字符串cookie用以下载网页接口的会员内容
  -token, --access-token <access-token>          设置access_token用以下载TV/APP接口的会员内容
  --aria2c-args <aria2c-args>                    调用aria2c的附加参数(默认参数包含"-x16 -s16 -j16 -k 5M", 使用时注意字符串转义)
  --work-dir <work-dir>                          设置程序的工作目录
  --delay-per-page <delay-per-page>              设置下载合集分P之间的下载间隔时间(单位: 秒, 默认无间隔)
  --version                                      Show version information
  -?, -h, --help                                 Show help and usage information


Commands:
  login    通过APP扫描二维码以登录您的WEB账号
  logintv  通过APP扫描二维码以登录您的TV账号
  
```
