# FFmpeg

### 什么是FFmpeg?

[^fast forward moving picture expert group 动态图像专家组]: 

- 简介：

  ```
     FFmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序,提供了音视频的编码，解码，转码，封装，解封装，流，滤镜，播放等功能。采用LGPL或GPL许可证。它提供了录制、转换以及流化音视频的完整解决方案。它几乎支持所有的音视频格式，不管是标准委员会，社区，还是公司设计的。它是高度可移植，跨平台的：可以在Linux, Mac OS X, Microsoft Windows, the BSDs, Solaris等系统上，在各种不同的编译环境，机器架构，配置下编译，运行，并通过测试
  ```

- GPL协议、LGPL协议与BSD协议的法律区别：

  ```
     简而言之，GPL协议就是一个开放源代码协议，软件的初始开发者使用了GPL协议并公开软件的源程序后，后续使用该软件源程序开发软件者亦应当根据GPL协议把自己编写的源程序进行公开。GPL协议要求的关键在于开放源程序，但并不排斥软件作者向用户收费。虽然如此，很多大公司对GPL协议还是又爱又恨，爱的是这个协议项下的软件历经众多程序员千锤百炼的修改，已经非常成熟完善，恨的是必须开放自己后续的源程序，导致竞争对手也可以根据自己修改的源程序开发竞争产品。
     正因大公司对GPL协议在商业上存在顾虑，因此，另两种协议被采用的更多，第一种是LGPL（亦称GPL V2）协议，可以翻译为更宽松的GPL协议。与GPL协议的区别为，后者如果只是对LGPL软件的程序库的程序进行调用而不是包含其源代码时，相关的源程序无需开源。调用和包含的区别类似在互联网网网页上对他人网页内容的引用：如果把他人的内容全部或部分复制到自己的网页上，就类似包含，如果只是贴一个他人网页的网址链接而不引用内容，就类似调用。有了这个协议，很多大公司就可以把很多自己后续开发内容的源程序隐藏起来。
     第二种是BSD协议（类似的还有MIT协议）。BSD协议鼓励软件的作者公开自己后续开发的源代码，但不强求。在BSD协议项下开发的软件，原始的源程序是开放源代码的，但使用者修改以后，可以自行选择发布源程序或者二进制程序（即目标程序），当然，使用者有义务把自己原来使用的源程序与BSD协议在软件对外发布时一并发布。因为比较灵活，所以BSD深受大公司的欢迎。
  ```

- FFmpeg影响范围：

  ```
  因为FFMPEG本身是开源项目，并且在LGPL/GPL协议下发布的任何人都可以自由使用，但必须严格遵守LGPL/GPL协议，其被很多开源的项目或者非开源的项目所使用：
  1. FMPEG作为内核视频播放器：
  ijkplayer，VLC，Mplayer，ffplay，暴风影音，KMPlayer，QQ影音
  2. FFMPEG作为内核的转码工具：
  ffmpeg，格式工厂
  3. FFMPEG作为内核的Directshow Filter：
  fdshow，lav filters
  4. 其他知名软件：
  微信，钉钉，Google Chrome        
  ```

### FFmpeg项目组成

FFmpeg框架的基本组成包含AVFormat、AVCodec、AVFilter、AVDevice、AVUtils、postproc、swresample、swscale  8个模块库

![img](https://upload-images.jianshu.io/upload_images/5810867-19297a539d20b492.png)

- libavformat：

  ```
  用于各种音视频封装格式的生成和解析，包括获取解码所需信息以生成解码上下文结构和读取音视频帧等功能，包含demuxers和muxer库
  ```

- libavcodec：

  ```
  用于各种类型声音/图像编解码。该库是音视频编解码核心，实现了市面上可见的绝大部分解码器的功能， libavcodec 库被其他各大解码器 ffdshow， Mplayer 等所包含或应用。但是有一些Codec是具备自己的License的，FFmpeg是不会默认添加像libx264、FDK-AAC、lame等库的，但是FFmpeg就像一个平台一样，可以将其他的第三方的Codec以插件的方式添加进来，然后为开发者提供统一的接口
  ```

- libavdevice: 

  ```
  输入输出设备库，比如，需要编译出播放声音或者视频的工具ffplay，就需要确保该模块是打开的，同时也需要libSDL的预先编译，因为该设备模块播放声音与播放视频使用的都是libSDL库
  ```

- libavfilter:

  ```
  音视频滤镜库，该模块提供了包括音频特效和视频特效的处理，如宽高比 裁剪 格式化 非格式化 伸缩。在使用FFmpeg的API进行编解码的过程中，直接使用该模块为音视频数据做特效处理是非常方便同时也非常高效的一种方式
  ```

- libavutil：

  ```
  包含一些公共的工具函数的使用库，包括算数运算 字符操作
  ```

- libswscale：

  ```
  原始视频格式转换） 用于视频场景比例缩放、色彩映射转换；图像颜色空间或格式转换，如 rgb565、rgb888 等与 yuv420 等之间转换
  ```

- libswresample:

  ```
  该模块可用于音频重采样，可以对数字音频进行声道数、数据格式、采样率等多种基本信息的转换
  ```

- libpostproc：

  ```
  该模块可用于进行后期处理，当我们使用AVFilter的时 候需要打开该模块的开关，因为Filter中会使用到该模块的一些基础函数
  ```

  编译完成以后主要生成了三个应用程序

- ffmpeg：

  ```
  是一个命令行工具，可用于格式转换、解码或电视卡即时编码等
  ```

- ffsever：

  ```
  一个HTTP 多媒体即时广播串流服务器
  ```

- ffplay：

  ```
  是一个简单的播放器，使用ffmpeg库解析和解码，通过SDL显示
  ```
  
- ffprobe

  ```
  主要用来查看多媒体文件的信息
  ```



### FFmpeg处理流程

容器(Container)
容器就是一种文件格式，比如flv，mkv等。包含下面5种流以及文件头信息

流(Stream)
是一种视频数据信息的传输方式，5种流：音频，视频，字幕，附件，数据

帧(Frame)
帧代表一幅静止的图像，分为I帧，P帧，B帧

```
I帧

I帧又称帧内编码帧，是一种自带全部信息的独立帧，无需参考其他图像便可独立进行解码，即全部为帧内编码。可以简单理解为一张静态画面。视频序列中的第一个帧始终都是I帧，因为它是关键帧。如果传输过程中I真丢失，画面最直接的影响就是会卡顿，因为后面的帧都无法正确解码，只能等待下一个GOP。

IDR帧

即时解码刷新，其实就是I帧，不过他是第一个I帧，或者是强制I帧，它的作用就是立即刷新，使错误不至于传播，从IDR开始，重新算一个新的序列开始编码。IDR会导致DPB（参考序列表）清空，而I帧不会，IDR帧一定是I帧，但是I帧不一定。一个图像序列中可以有很多I帧，一个I帧后的图像可以引用I帧之间的图像做运动参考，但是对于IDR帧来说，IDR帧后的图像不能引用IDR之前的帧内容，因为从IDR帧相当于重新开始。

P帧

P帧又称帧间预测编码帧，需要参考前面的I帧才能进行编码。表示的是当前帧画面与前一帧（前一帧可能是I帧也可能是P帧）的差别。解码时需要用之前缓存的画面叠加上本帧定义的差别(采用了预测编码)，生成最终画面。与I帧相比，P帧通常占用更少的数据位，但不足是，由于P帧对前面的P和I参考帧有着复杂的依耐性，因此对传输错误非常敏感，所以如果P帧丢失，画面会出现马赛克现象，因为前向参考帧错误，补齐的并不是真正运动变化后的数据。

B帧

B帧又称双向预测编码帧，也就是B帧记录的是本帧与前后帧的差别。也就是说要解码B帧，不仅要取得之前的缓存画面，还要解码之后的画面，通过前后画面的与本帧数据的叠加取得最终的画面。B帧压缩率高，但是对解码性能要求较高。如果图像中没有B帧，解码顺序和显示顺序相同；如果视频中含有B帧，解码顺序和现实序列不同，解码输出显示前需要进行图像重排列。目前接触到一般都是 I + P。
```

编解码器(Codec)
是对视频进行压缩或者解压缩，CODEC =COde （编码）+ DECode（解码）

复用/解复用(mux/demux)
把不同的流按照某种容器的规则放入容器，这种行为叫做复用（mux）
把不同的流从某种容器中解析出来，这种行为叫做解复用(demux)

<img src="https://upload-images.jianshu.io/upload_images/11591878-810021da4c347e09.png?imageMogr2/auto-orient/strip|imageView2/2/w/1067/format/webp" alt="img"  />

```
1、FFmpeg程序把-i参数指定的若干文件内容读入到内存，按照输入的参数或者程序默认的参数来处理并且把结果写入到若干的文件中。输入和输出文件可以是计算机文件、管道、网络流、捕获设备等
2、FFmpeg用libavformat包调用解复用器（demuxers）来读取输入文件中被编码的数据包(packets)，如果有多个输入文件，FFmpeg以有效输入流的最小时间戳来同步
3、然后解码器（decoder）从已编码的数据包中产生未被压缩的帧（frame），在那之后调用可选的过滤器
4、这些帧被传递到编码器，编码器会产生新的编码包
5、把新的编码包传递给复用器(muxer)处理并且把结果写入到输出文件中
```

### FFmpeg 实际运用

```java
//视频分片
ffmpeg -i xxx.mp4 -f segment -segment_time 60 -segment_format mpegts -segment_list /home/xxx/video_name.m3u8 -c copy -bsf:v h264_mp4toannexb -map 0 /home/xxx/course-%04d.ts
    
ffmpeg -i D:\video-demo\demo.mp4 -f segment -segment_time 10 -segment_format mpegts -segment_list playlist.m3u8 -c copy -bsf:v h264_mp4toannexb -map 0 demo-%04d.ts
// 去掉视频中的音频
ffmpeg -i input.mp4 -vcodec copy -an output.mp4
// -an: 去掉音频；-vcodec:视频选项，一般后面加copy表示拷贝

// 提取视频中的音频 （格式需要和Duration: Stream #0:1(und): Audio:***,格式(***)一样）
ffmpeg -i input.mp4 -acodec copy -vn output.mp3
// -vn: 去掉视频；-acodec: 音频选项， 一般后面加copy表示拷贝

// 音视频合成
ffmpeg -y –i input.mp4 –i input.mp3 –vcodec copy –acodec copy output.mp4
// -y 覆盖输出文件

//剪切视频
ffmpeg -ss 0:1:30 -t 0:0:20 -i input.mp4 -vcodec copy -acodec copy output.mp4
// -ss 开始时间; -t 持续时间

// 视频截图
ffmpeg –i test.mp4 –f image2 -t 0.001 -s 1280x720 image-%3d.jpg
// -s 设置分辨率; -f 强迫采用格式fmt;

// 视频分解为图片
ffmpeg –i test.mp4 –r 1 –f image2 image-%3d.jpg
// -r 指定截屏频率

// 将图片合成视频
ffmpeg -f image2 -i image%d.jpg output.mp4

//视频拼接
ffmpeg -f concat -i filelist.txt -c copy output.mp4

// 将视频转为gif
ffmpeg -i input.mp4 -ss 0:0:30 -t 10 -s 320x240 -pix_fmt rgb24 output.gif
// -pix_fmt 指定编码

// 将视频前30帧转为gif
ffmpeg -i input.mp4 -vframes 30 -f gif output.gif

// 旋转视频
ffmpeg -i input.mp4 -vf rotate=PI/2 output.mp4

// 缩放视频
ffmpeg -i input.mp4 -vf scale=iw/2:-1 output.mp4
// iw 是输入的宽度， iw/2就是一半;-1 为保持宽高比

//视频变速
ffmpeg -i input.mp4 -filter:v setpts=0.5*PTS output.mp4

//音频变速
ffmpeg -i input.mp3 -filter:a atempo=2.0 output.mp3

//音视频同时变速，但是音视频为互倒关系
ffmpeg -i input.mp4 -filter_complex "[0:v]setpts=0.5*PTS[v];[0:a]atempo=2.0[a]" -map "[v]" -map "[a]" output.mp4

// 视频添加水印
ffmpeg -i input.mp4 -i logo.jpg -filter_complex [0:v][1:v]overlay=main_w-overlay_w-10:main_h-overlay_h-10[out] -map [out] -map 0:a -codec:a copy output.mp4
// main_w-overlay_w-10 视频的宽度-水印的宽度-水印边距；

//添加字幕
ffmpeg -i 0.mp4 -vf subtitles=0.srt 1.mp4
```

#### 视频加密方案

想达到的目的：将一个mp4视频文件切割为多个ts片段，并在切割过程中对每一个片段使用 AES-128 加密，最后生成一个m3u8的视频索引文件；

电脑环境 Fedora,已经安装了最新的ffmpeg；

如果要加密，首先准备好一下两个东西：

加密用的 key

```
openssl rand  16 > enc.key （生成一个enc.key文件）
```

另一个是 iv

```
openssl rand -hex 16  （生成一段字符串，记下来）
```

新建一个文件 enc.keyinfo 内容格式如下：

```Yml
Key URI  # enc.key的路径，使用http形式
Path to key file  # enc.key文件
IV  #  上面生成的iv
```

例子：

```
http://localhost/video/enc.key
enc.key
48c674428c1e719751565ad00fe24243
```

```yml
ffmpeg -y 
-i test.mp4 
-hls_time 12    #将test.mp4分割成每个小段多少秒
-hls_key_info_file enc.keyinfo 
-hls_playlist_type vod    #vod 是点播，表示PlayList不会变
-hls_segment_filename "file%d.ts"   #每个小段的文件名
playlist.m3u8   #生成的m3u8文件

ffmpeg -y -i D:\video-demo\demo.mp4 -hls_time 10 -hls_key_info_file D:\video-demo\enc.keyinfo -hls_playlist_type vod -hls_segment_filename "demo-%d.ts" playlist.m3u8
```

FFmpeg java工具类

```java
package com.zkcm.upload.util;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class MediaUtil {

    //ffmpeg安装目录
    public   static  String FFMPEG_PATH ="D:\\soft\\ffmpge\\ffmpeg.exe";

    //设置图片大小
    private final static String IMG_SIZE = "1920x1080";

    /**
     * 视频截图方法（windows）
     */
    public static boolean ffmpegToImage(String videoPath,String imagePath,int timePoint){
        List<String> commands = new java.util.ArrayList<String>();
        FFMPEG_PATH= FFMPEG_PATH.replace("%20", " ");
        commands.add(FFMPEG_PATH);
        commands.add("-ss");
        //这个参数是设置截取视频多少秒时的画面
        commands.add(timePoint+"");
        commands.add("-i");
        commands.add(videoPath);
        commands.add("-y");
        commands.add("-f");
        commands.add("image2");
        commands.add("-t");
        commands.add("0.001");
        commands.add("-s");
        //这个参数是设置截取图片的大小
        commands.add(IMG_SIZE);
        commands.add(imagePath);
        try {
            ProcessBuilder builder = new ProcessBuilder();
            builder.command(commands);
            builder.start();
            System.out.println("截取成功:"+imagePath);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * @Description 文件是否能被ffmpeg解析
     */
    public static int checkFileType(String fileName) {
        String type = fileName.substring(fileName.lastIndexOf(".") + 1, fileName.length())
                .toLowerCase();
        if (type.equals("avi")) {
            return 0;
        }  else if (type.equals("mov")) {
            return 0;
        } else if (type.equals("mp4")) {
            return 0;
        }  else if (type.equals("flv")) {
            return 0;
        }
        else if (type.equals("png")) {
            return 1;
        } else if (type.equals("jpg")) {
            return 1;
        } else if (type.equals("jpeg")) {
            return 1;
        }
        return 9;
    }


    /**
     * @Description 获取视频时长
     */
    static int getVideoTime(String video_path) {
        List<String> commands = new java.util.ArrayList<String>();
        commands.add(FFMPEG_PATH);
        commands.add("-i");
        commands.add(video_path);
        try {
            ProcessBuilder builder = new ProcessBuilder();
            builder.command(commands);
            final Process p = builder.start();

            //从输入流中读取视频信息
            BufferedReader br = new BufferedReader(new InputStreamReader(p.getErrorStream()));
            StringBuffer sb = new StringBuffer();
            String line = "";
            while ((line = br.readLine()) != null) {
                sb.append(line);
            }
            br.close();

            //从视频信息中解析时长
            String regexDuration = "Duration: (.*?), start: (.*?), bitrate: (\\d*) kb\\/s";
            Pattern pattern = Pattern.compile(regexDuration);
            Matcher m = pattern.matcher(sb.toString());
            if (m.find()) {
                int time = getTimelen(m.group(1));
                System.out.println(video_path+",视频时长："+time+", 开始时间："+m.group(2)+",比特率："+m.group(3)+"kb/s");
                return time;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        return 0;
    }

    //格式:"00:00:10.68"
    private static int getTimelen(String timelen){
        int min=0;
        String strs[] = timelen.split(":");
        if (strs[0].compareTo("0") > 0) {
            //秒
            min+=Integer.valueOf(strs[0])*60*60;
        }
        if(strs[1].compareTo("0")>0){
            min+=Integer.valueOf(strs[1])*60;
        }
        if(strs[2].compareTo("0")>0){
            min+=Math.round(Float.valueOf(strs[2]));
        }
        return min;
    }

    /**
     * 秒转化成 hh:mm:ss
     * @param duration
     * @return
     */
    public static String convertInt2Date(long duration){
        Calendar cal = Calendar.getInstance();
        cal.set(Calendar.HOUR_OF_DAY, 0);
        cal.set(Calendar.MINUTE, 0);
        cal.set(Calendar.SECOND, 0);
        cal.set(Calendar.MILLISECOND, 0);

        SimpleDateFormat formatter = new SimpleDateFormat("HH:mm:ss");
        return formatter.format(cal.getTimeInMillis() + duration * 1000);
    }

    /**
     * 视频抽取音频文件
     * @param videoPath
     * @param type
     * @param audioPath
     * @return
     */
    public static boolean ffmpegToAudio(String videoPath,String type, String audioPath){
        List<String> commands = new java.util.ArrayList<String>();
        FFMPEG_PATH= FFMPEG_PATH.replace("%20", " ");
        commands.add(FFMPEG_PATH);
        commands.add("-i");
        commands.add(videoPath);
        commands.add("-f");
        commands.add(type);
        commands.add("-vn");
        commands.add("-y");
        commands.add("-acodec");
        if("wav".equals(type)){
            commands.add("pcm_s16le");
        }else if("mp3".equals(type)){
            commands.add("mp3");
        }
        commands.add("-ar");
        commands.add("16000");
        commands.add("-ac");
        commands.add("1");
        commands.add(audioPath);
        try {
            ProcessBuilder builder = new ProcessBuilder();
            builder.command(commands);
            Process p = builder.start();
            System.out.println("抽离成功:"+audioPath);

            // 1. start
            // 保存ffmpeg的输出结果流
            BufferedReader buf = null;
            String line = null;

            buf = new BufferedReader(new InputStreamReader(p.getInputStream()));

            StringBuffer sb = new StringBuffer();
            while ((line = buf.readLine()) != null) {
                System.out.println(line);
                sb.append(line);
                continue;
            }
            // 这里线程阻塞，将等待外部转换进程运行成功运行结束后，才往下执行
            p.waitFor();
            // 1. end
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * wav 转 mp3
     * @param wavPath
     * @param mp3Path
     * @return
     */
    //wav转mp3命令：ffmpeg -i test.wav -f mp3 -acodec libmp3lame -y wav2mp3.mp3
    public static boolean ffmpegOfwavTomp3(String wavPath, String mp3Path){
        List<String> commands = new java.util.ArrayList<String>();
        FFMPEG_PATH= FFMPEG_PATH.replace("%20", " ");
        commands.add(FFMPEG_PATH);
        commands.add("-i");
        commands.add(wavPath);
        commands.add("-f");
        commands.add("mp3");
        commands.add("-acodec");
        commands.add("libmp3lame");
        commands.add("-y");
        commands.add(mp3Path);
        try {
            ProcessBuilder builder = new ProcessBuilder();
            builder.command(commands);
            Process p = builder.start();
            System.out.println("转换成功:"+mp3Path);

            // 1. start
            // 保存ffmpeg的输出结果流
            BufferedReader buf = null;
            String line = null;

            buf = new BufferedReader(new InputStreamReader(p.getInputStream()));

            StringBuffer sb = new StringBuffer();
            while ((line = buf.readLine()) != null) {
                System.out.println(line);
                sb.append(line);
                continue;
            }
            // 这里线程阻塞，将等待外部转换进程运行成功运行结束后，才往下执行
            p.waitFor();
            // 1. end
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    public static void main(String[] args) {
//        // 视频文件
//        String videoRealPath1 = "D:\\5.MOV";
//        // 截图的路径（输出路径）
//        String imageRealPath1 = "D:\\31.jpg";
//        String imageRealPath2 = "D:\\32.jpg";
//        if(checkFileType(videoRealPath1) ==0){
//        	 ffmpegToImage(videoRealPath1,imageRealPath1,10);
//        	 ffmpegToImage(videoRealPath1,imageRealPath2,11);
//        }
//		String videoRealPath = "D:\\5.MOV";
//		String audioRealPath = "D:\\HHH\\5.wav";
//		String audioType = "wav";
//		ffmpegToAudio(videoRealPath,audioType,audioRealPath);

        String wavPath = "D:\\videoDemo\\test.wav";
        String mp3Path = "D:\\videoDemo\\test.mp3";
        ffmpegOfwavTomp3(wavPath,mp3Path);
    }
}
```

##### 基于域名防盗链处理

```
这是最通行的做法，从技术实现上也是最为简洁和容易使用，但是缺点也是比较明显的，只适合于网站页面模式使用，一旦脱离的浏览器，就会受到限制。同时从技术上的破解相对也是容易的。
```

##### 基于连接令牌（Token）接入，通过带入加密的Token进行接入访问

```
这种做法，是针对每个视频源都设置指定的加密Token，使用者只有获得Token，才容许授权接入，适合于网站页面，客户端，pc，手机使用，实施相对简单，但是由于Token的相对固定，一旦Token被他人知晓，也会面临内容源被盗取的风险。
```

##### 基于发布令牌（Token）加密，结合授权服务接入访问

```
发布令牌授权，是在原有Token授权模式上，做了更进一步的加强，Token每时每刻都在不断随机变化，使用者必须连接到授权服务器，每次接入的时候，都需要从授权服务器获取最新的Token，才容许接入。这个就类似银行的个人usb安全密码器，在登录网上银行的时候，需要获取随机登录密码，才可以登录网银。 在这种情况下，使用者如果无法连接到授权服务器，几乎是不可能得到连接令牌，从而杜绝的被非法使用的可能性。这种模式同样适用于页面，客户端，pc，手机各种接入模式。同时实施相对简单
```

