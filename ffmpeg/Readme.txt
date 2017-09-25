https://ffmpeg.org/download.html

http://ffmpeg.zeranoe.com/builds/

¡¾ÍÆÁ÷¡¿
D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f dshow -i video="screen-capture-recorder":audio="FrontMic (Realtek High Definiti"  -f flv rtmp://push.ybxin.net/tuyunwx/abc

D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f dshow -i video="Avidemux":audio="FrontMic (Realtek High Definiti"  -f flv rtmp://push.ybxin.net/tuyunwx/abc

D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f avfoundation -i "1" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://push.ybxin.net/tuyunwx/abc

D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f gdigrab -video_size 600x480 -offset_x 100 -offset_y 60 -t 20 -r 15 -i desktop -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://push.ybxin.net/tuyunwx/abc

D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f gdigrab -video_size 600x480 -offset_x 100 -offset_y 60 -t 20 -r 15 -i desktop -vcodec -f flv rtmp://push.ybxin.net/tuyunwx/abc

D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f gdigrab -video_size 600x480 -offset_x 100 -offset_y 60 -t 20 -r 15 -i desktop -vcodec libx264 x264.mp4


D:\GitHub\WT\Windows\ffmpeg\ffmpeg.exe -f gdigrab -video_size 600x480 -offset_x 100 -offset_y 60 -t 20 -r 150 -i desktop -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://push.ybxin.net/tuyunwx/abc

G:\ffmpeg\ffmpeg\ffmpeg.exe -f gdigrab -video_size 600x480 -offset_x 100 -offset_y 60 -t 20 -r 150 -i desktop -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://push.ybxin.net/tuyunwx/abc

http://play.ybxin.net/tuyunwx/abc.m3u8