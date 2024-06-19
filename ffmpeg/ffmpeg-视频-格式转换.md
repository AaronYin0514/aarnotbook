# ffmpeg-视频

## ffmpeg-视频格式转换

```shell
# 把mp4文件转换为flv文件
ffmpeg -i input.mp4 output.flv

# 无损视频格式转换
ffmpeg -i input.mp4 -vcodec copy -acodec copy output.flv

# 把webm文件转换为
ffmpeg -i input.webm -s 1920x1080 -pix_fmt yuv420p -vcodec libx264 -preset medium -profile:v high -level:v 4.1 -crf 23 -acodec aac -ar 44100 -ac 2 -b:a 128k output.mp4
```

- 【-vcodec libx264】v是video的缩写，vcodec是用来设置视频编码器的，libx264是h264的编码器，通用稳定，也可以写-vcodec h264、-vcodec mpeg4，根据你的需求自己指定，可以通过ffmpeg -codecs进行查看编码器  
- 【-vcodec copy】采用视频编码器进行copy操作，即复制原视频的编码  
- 【-s 1920x1080】缩放视频尺寸的，这里设置的是1920宽1080高  
- 【-pix_fmt yuv420p】pix_fmt是pixel format，用来设置的是视频的颜色空间，具体的颜色空间很多，可以用ffmpeg -pix_fmts查看  
- 【-preset medium】这个perset是编码器预设（设置编码器性能），可以设置编码算法的精度，精度越高编码速度越慢，CPU占用率越多。一共有十个参数可选：ultrafast superfast veryfast faster fast medium slow veryslow placebo，如果我们不写的话那默认就是medium，一般情况下我们在录制视频的时候选择veryfast，这么一来编码器不会占用太多的cpu资源，其他软件也能正常运行，缺点就是生成的文件会比较大，这是用存储空间换取电脑性能。在压制视频的时候一般采用veryslow，牺牲一点时间，获取对参数的精准控制。  
- 【-profile:v high】-profile:v指定视频编码器的配置，这里设置的参数为high。这个配置主要是和压缩比有关，实时通信领域用baseline，流媒体就采用main，超清视频用high
- 【-level:v 4.1】对视频编码器配置的具体规范和限制，这里参数为4.1。压缩比和画质就像鱼和熊掌不可兼得，我们要根据不同的使用场景，在二者间做出权衡，从1到5.2我们应该怎么选？一般情况下，1080p的视频就用4.1  
- 【-crf 23】crf（constant rate factor恒定速率因子模式）用来设置码率控制模式的，每一帧的画面都按照要求的视频质量去获取他所需要的比特数，画质均衡，但是它无法精准的控制码率，也无法控制最终生成文件的大小，适用于对画质有要求，文件大小无关紧要的场景。这里设置的是23，这个23是视频的质量。总的参数0…18…23…28…51，如果不写默认值就是23，数值越小质量越高，0就是无损的画质  
- 【-r 30】用于设置视频帧率的，这里设置的是每秒30帧  
- 【-acodec aac】设置音频编码器，这里设置的是aac视频编码器  
- 【-ar 44100】设置音频采样率，这里设置的是44100  
- 【-ac 2】设置音频的声道，2是双声道立体声  
- 【-b:a 128k】设置音频比特率的，同-ab 128k，这里设置的是128k

## 获取视频信息

```shell
ffprobe -of json -show_streams -select_streams v -i test.mp4
```