---
title: FFmpeg
date: 2020-11-30 15:32:00
tags:
- FFmpeg
---

#### 安装

[Centos7安装ffmpeg]: https://www.myfreax.com/how-to-install-ffmpeg-on-centos-7/

#### 命令

```
ffmpeg -i akane.mp4 -c copy -map 0 -f segment -segment_list akane.m3u8 -segment_time 10 akane%03d.ts
```



```
ffmpeg -y \
-i The_Kriss_Vector.mp4 \
-hls_time 20 \       # 将test.mp4分割成每个小段多少秒
-hls_key_info_file encrypt.keyinfo \
-hls_playlist_type vod \   # vod 是点播，表示PlayList不会变
-hls_segment_filename "playlist%d.ts" \  #  每个小段的文件名
playlist.m3u8   #  生成的m3u8文件
```

```
ffmpeg -y \
-i The_Kriss_Vector.mp4 \
-hls_time 20 \
-hls_key_info_file encrypt.keyinfo \
-hls_playlist_type event \
-hls_segment_filename "playlist%03d.ts" \
playlist.m3u8
```

```
ffmpeg -y -i akane.mp4 -hls_time 4 -hls_key_info_file enc.keyinfo -hls_segment_filename "playlist%03d.ts" playlist.m3u8
```

```
ffmpeg -y -i akane.mp4 -hls_time 4 -hls_segment_filename "playlist%03d.ts" playlist.m3u8
```

