https://ffmpeg.org/download.html

---

http://download.csdn.net/download/qprevf/9694322
http://download.csdn.net/download/xiaoxiaospace/9577051

------------------
http://blog.chinaunix.net/uid-9688646-id-3399113.html

ffmpeg��ffserver���ʹ�ÿ���ʵ��ʵʱ����ý�����

һ�����
�����Ҫ�������ĸ����������������֮��Ĺ�ϵ�Ͳ�������ˡ�
1. ffmpeg

2. ffserver

3. ffserver.conf

4. feed1.ffm

 
1. ffmpeg������ý���ļ���transcode����������������ϵ�Դý���ļ�ת����Ҫ���ͳ�ȥ����ý���ļ���

2. ffserver��������Ӧ�ͻ��˵���ý�����󣬰���ý�����ݷ��͸��ͻ��ˡ�

3. ffserver.conf��ffserver����ʱ�������ļ���������ļ�����Ҫ�Ƕ�����Э�飬�����ļ�feed1.ffm������������Ҫ���͵���ý���ļ��ĸ�ʽ������������趨��

4. feed1.ffm�����Կ�����һ����ý�����ݵĻ����ļ���ffmpeg��ת��õ����ݷ��͸�ffserver�����û�пͻ�����������ffserver�����ݻ��浽���ļ��С�


����http�Ľ�������
1. ����ffserver.conf�ļ������νӴ����Բο�ffmpegԴ���е�doc/ffserver.conf���������ϸ��ע�ͣ�
����дһ��ʾ��
Port 10535
RTSPPort 5454
BindAddress 0.0.0.0��
MaxHTTPConnections 2000
MaxClients 1000
MaxBandwidth 1000
CustomLog -
NoDaemon

#ʵʱ����������(�ο�Դ��ffmpeg/test/�µ�ffserver.conf)
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

#�Ѿ����ڵ��ļ�����ʵʱ��
 
<Stream test.flv>
File "/project/apps/ffserver/test.flv"
Format flv
</Stream>


2�����ʵ�ֲ���
��1��ʵʱ����http����
�������Ӳ���ϵ��ļ�����
ffserver -f myfile/ffmpeg0.8.9/ffserver.conf & ffmpeg -i inputfile(�����ļ�) http://localhost:10535/feed1.ffm
��δ�������ͷ�����ʵʱ������
ffserver -f myfile/ffmpeg0.8.9/ffserver.conf & ffmpeg -f video4linux2 -framerate 30 -i /dev/video0 http://127.0.0.1:8090/feed1.ffm

����ffserver��ffmpeg��ffserver����ffmpeg����������������ʱ����Ҫ�Ӳ���-fָ���������ļ���ffserver������feed1.ffm�ͻᱻ��������ʱ������feed1.ffm�������ᷢ��feed1.ffm��ʼ�Ĳ����Ѿ�д������ �ݣ�������ҵ��ؼ���ffm�Լ���ͻ��˴�������������Ϣ����feed1.ffm�������õ�ʱ����Щ��Ϣ�ǲ��ᱻ���ǵ��ģ��Ͱ��������Ϊ feed1.ffm�ļ���ͷ�ɡ�

ffserver������ffmpeg������������ʱ�ӵ�һ���ؼ��������ǡ�http://ip:10535/feed1.ffm��������ip������ ffserver������ip�����ffmpeg��ffserver����ͬһϵͳ�����еĻ�����localhostҲ�С�ffmpeg��������� ffserver����һ������(���ݵ�����)��ͨ�����һ�ε����ӣ�ffmpeg��ffserver�����ȡ����ͻ�������������ã�������Щ������Ϊ�� ��������������ã�Ȼ��ffmpeg�Ͽ���������ӣ��ٴ���ffserver��������(���õ�����)�������������ffmpeg��ѱ��������ݷ��͸� ffserver��

�����۲�ffserver�˵�����ͻᷢ�����ʱ����������HTTP��200��������������ӵĹ��̡�

ffmpeg������ͷ��ȡ���ݺ󣬰���������ı��뷽ʽ���룬Ȼ���͸�ffserver��ffserver�յ�ffmpeg�����ݺ���������� û�в��ŵ����󣬾Ͱ�����д��feed1.ffm�л��棬д��ʱ�����ݼ���Щͷ��ϢȻ��ֿ飬ÿ��4096B��ÿ��Ҳ�нṹ������feed1.ffm�Ĵ� С����ffserver.conf�й涨�Ĵ�С�󣬾ͻ���ļ���ʼ������ͷ��д�룬���Ǿɵ����ݡ�ֱ���������в��ŵ�����ffserver�� feed1.ffm�ж�ȡ���ݣ����͸��ͻ��ˡ�

(2)�����ļ���http����
ffserver -f /etc/ffserver.conf
����������ffserver��Ȼ����ffplay http://ip:port/test.flv,������vlc������������ַҲ��ʵ�ֲ��š�

(3)�����ļ���rtsp����
ffserver -f /etc/ffserver.conf
����������ffserver��Ȼ����ffplay rtsp://ip:port/rtsp.mpg��������vlc������������ַҲ��ʵ�ֲ��š�
��ע���������Ե�ʱ����rtsp���ܴ���flv�ļ���