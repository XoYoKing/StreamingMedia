https://ffmpeg.org/download.html

---

http://download.csdn.net/download/qprevf/9694322
http://download.csdn.net/download/xiaoxiaospace/9577051

------------------
http://blog.chinaunix.net/uid-9688646-id-3399113.html

ffmpeg和ffserver配合使用可以实现实时的流媒体服务。

一、理解
里边主要有如下四个东西，搞清楚他们之间的关系就差不多明白了。
1. ffmpeg

2. ffserver

3. ffserver.conf

4. feed1.ffm

 
1. ffmpeg，负责媒体文件的transcode工作，把你服务器上的源媒体文件转换成要发送出去的流媒体文件。

2. ffserver，负责响应客户端的流媒体请求，把流媒体数据发送给客户端。

3. ffserver.conf，ffserver启动时的配置文件，在这个文件中主要是对网络协议，缓存文件feed1.ffm（见下述）和要发送的流媒体文件的格式参数做具体的设定。

4. feed1.ffm，可以看成是一个流媒体数据的缓存文件，ffmpeg把转码好的数据发送给ffserver，如果没有客户端连接请求，ffserver把数据缓存到该文件中。


二、http的建立流程
1. 配置ffserver.conf文件（初次接触可以参考ffmpeg源码中的doc/ffserver.conf，里边有详细的注释）
如下写一个示例
Port 10535
RTSPPort 5454
BindAddress 0.0.0.0、
MaxHTTPConnections 2000
MaxClients 1000
MaxBandwidth 1000
CustomLog -
NoDaemon

#实时流数据配置(参考源码ffmpeg/test/下的ffserver.conf)
<Feed feed1.ffm>
File /tmp/feed1.ffm
FileMaxSize 1M
ACL allow 127.0.0.1
</Feed>

<Stream test.avi>
Feed feed1.ffm
Format avi
#
BitExact
DctFastint
IdctSimple
VideoFrameRate 10
VideoSize 352x288
VideoBitRate 100
VideoGopSize 30
NoAudio

PreRoll 10
StartSendOnKey
MaxTime 100

</Stream>

#已经存在的文件而非实时流
 
<Stream test.flv>
File "/project/apps/ffserver/test.flv"
Format flv
</Stream>


2、如何实现播放
（1）实时流用http传输
如果传输硬盘上的文件，则：
ffserver -f myfile/ffmpeg0.8.9/ffserver.conf & ffmpeg -i inputfile(输入文件) http://localhost:10535/feed1.ffm
如何传输摄像头捕获的实时流，则：
ffserver -f myfile/ffmpeg0.8.9/ffserver.conf & ffmpeg -f video4linux2 -framerate 30 -i /dev/video0 http://127.0.0.1:8090/feed1.ffm

启动ffserver和ffmpeg。ffserver先于ffmpeg启动，它在启动的时候需要加参数-f指定其配置文件。ffserver启动后，feed1.ffm就会被创建，这时如果你打开feed1.ffm看看，会发现feed1.ffm开始的部分已经写入了内 容，你可以找到关键字ffm以及向客户端传送流的配置信息，在feed1.ffm做缓冲用的时候，这些信息是不会被覆盖掉的，就把它们理解为 feed1.ffm文件的头吧。

ffserver启动后，ffmpeg启动，它启动时加的一个关键参数就是“http://ip:10535/feed1.ffm”，其中ip是运行 ffserver主机的ip，如果ffmpeg和ffserver都在同一系统中运行的话，用localhost也行。ffmpeg启动后会与 ffserver建立一个连接(短暂的连接)，通过这第一次的连接，ffmpeg从ffserver那里获取了向客户端输出流的配置，并把这些配置作为自 己编码输出的配置，然后ffmpeg断开了这次连接，再次与ffserver建立连接(长久的连接)，利用这个连接ffmpeg会把编码后的数据发送给 ffserver。

如果你观察ffserver端的输出就会发现这段时间会出现两次HTTP的200，这就是两次连接的过程。

ffmpeg从摄像头获取数据后，按照输出流的编码方式编码，然后发送给ffserver，ffserver收到ffmpeg的数据后，如果网络上 没有播放的请求，就把数据写入feed1.ffm中缓存，写入时把数据加上些头信息然后分块，每块4096B（每块也有结构），当feed1.ffm的大 小到了ffserver.conf中规定的大小后，就会从文件开始（跳过头）写入，覆盖旧的数据。直到网络上有播放的请求，ffserver从 feed1.ffm中读取数据，发送给客户端。

(2)本地文件用http传输
ffserver -f /etc/ffserver.conf
用命令启动ffserver，然后用ffplay http://ip:port/test.flv,或者在vlc中输入以上网址也可实现播放。

(3)本地文件用rtsp传输
ffserver -f /etc/ffserver.conf
用命令启动ffserver，然后用ffplay rtsp://ip:port/rtsp.mpg，或者在vlc中输入以上网址也可实现播放。
备注：在做测试的时候，用rtsp不能传输flv文件。