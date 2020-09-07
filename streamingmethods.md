# Streaming Methods

## what are targets?
- resolution
- latency
- bandwith
- ease of use
  - not more languages (not php)

## Methods
Comparison different Types:\
 https://flashphoner.com/how-to-broadcast-webrtc-rtsp-and-rtmp-streams-to-media-source-extensions-via-the-websocket-protocol/\
 https://www.ipcamlive.com/faqs


- get an ip camera ? for surveillance so perhaps not good latency
  - https://www.controlbooth.com/threads/low-latency-ip-cameras.45601/
- [[UV4L]]
- [[GStreamer]]
- RPI-Cam-Web-Interface
- Raspivid
- [motion](https://motion-project.github.io/motion_config.html) 
  - [high cpu usage](https://chriscarey.com/blog/2017/04/30/achieving-high-frame-rate-with-a-raspberry-pi-camera-system/comment-page-1/)
- [RemoteMe](https://remoteme.org/raspberry-pi/)
- v4l2 
- [ffmpeg](https://ffmpeg.org) 
  - https://en.wikipedia.org/wiki/FFmpeg ( what is [avconv](https://libav.org/avconv.html) last news blog 12.02.2018)
- netcat (nc ) for streaming / vlc else
## Other stuff

good collection 
https://raspberrypi.stackexchange.com/questions/7446/how-can-i-stream-h-264-video-from-the-raspberry-pi-camera-module-via-a-web-serve/7657#7657

https://raspberrypi.stackexchange.com/questions/26675/modern-way-to-stream-h-264-from-the-raspberry-cam

https://qastack.com.de/raspberrypi/7446/how-can-i-stream-h-264-video-from-the-raspberry-pi-camera-module-via-a-web-serve

info about ribbon cables and camera 
https://picamera.readthedocs.io/en/latest/fov.html

http://www.netzmafia.de/skripten/hardware/RasPi/kamera/index.html

raspivid not prefered method anymore now v4l2 is prefered src: https://qastack.com.de/raspberrypi/7446/how-can-i-stream-h-264-video-from-the-raspberry-pi-camera-module-via-a-web-serve

mpeg is veraltet -> bad compression and single images 

h264 mjpeg [dash-mpeg](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) rtsp/rtmp/rtp [m3u8](https://qastack.com.de/raspberrypi/7446/how-can-i-stream-h-264-video-from-the-raspberry-pi-camera-module-via-a-web-serve)(utf8 variante von m3u) hls difference?

hls dash mse create segments -> seconds of lag

some info 
https://flussonic.com/blog/news/html5-streaming/


h264 hardware support on raspberry pi

- webrtc? [[webRTC]]
  - turn/stun server?
- websockets

whats ffmpeg

concern that needs to be addressed
- restart stream (or close on lost connection)
  - not sure which protocol but some had problems with that (ffmpeg perhaps)
- how to display h264 (raw)
  - https://blog.ronnyvdb.net/2019/01/20/howto-stream-html5-video-h264-encoded-video-encapsulated-in-mp4-from-the-raspberry-pi-to-any-web-browser/ not sure if needed does some raspivid gstreamer stuff

boardcasting stuff
thought there was something possible with janus as a server
MedienServer
- Medooze-Media-Server für NodeJs: Compiling Fails https://openbase.io/js/medooze-media-server

balena -

lag 1-3 sec
good setup/maintenance 
fallback to mjpeg needs flag in chrome/firefox -> rly bad

Gstreamer complexity of multiplexers
https://gstreamer.freedesktop.org/data/events/gstreamer-conference/2017/Matthew Waters - GStreamer WebRTC.pdf

big problem wit surveillance solutions -> bad latency 

- Use Python Server
- Node Server
- Just other Server

|              notes              |                        qualifies                         | source                                                                                                                     |
| :-----------------------------: | :------------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------- |
|              mjpeg              |                            no                            | https://appuals.com/how-to-perform-video-streaming-using-raspberry-pi/                                                     |
|         mjpeg_streamer          |                            no                            | https://www.tomshardware.com/how-to/use-raspberry-pi-as-pc-webcam                                                          |
|              uv4l               |                          kinda                           | http://www.linux-projects.org/uv4l/                                                                                        |
|                                 |   promising 150ms latency but only one client possible   | http://www.linux-projects.org/uv4l/tutorials/custom-webapp-with-face-detection/                                            |
|                                 |           facedetection stripped down project            | https://tryolabs.com/blog/hackathon-robot-remote-work-iot-computer-vision/                                                 |
|       uv4l nc or multicat       |                                                          | http://frozen.ca/streaming-raw-h-264-from-a-raspberry-pi/                                                                  |
|                                 |                  kinda not open source                   | https://www.instructables.com/id/Raspberry-Pi-Video-Streaming/                                                             |
|                                 |                                                          | https://medium.com/home-wireless/headless-streaming-video-with-the-raspberry-pi-zero-w-and-raspberry-pi-camera-38bef1968e1 |
|                                 |                                                          | https://github.com/PietroAvolio/uv4l-webrtc-raspberry-pi                                                                   |
|             motion              |                      not realtime?                       | https://www.hackster.io/narender-singh/portable-video-streaming-camera-with-raspberry-pi-zero-w-dc22fd                     |
|                                 |                                                          | https://hada-tech.com/index.php/2020/06/07/live-stream-usb-camera-with-raspberry-pi/                                       |
|    raspivid -> cvlc -> rtsp     |                       lag i think                        | https://raspberry-projects.com/pi/pi-hardware/raspberry-pi-camera/streaming-video-using-vlc-player                         |
|                                 |                                                          | https://bitsnblobs.com/ip-camera-using-the-raspberry-pi-zero/                                                              |
|       cvlc comprehensive        |                                                          | https://stackoverflow.com/questions/49846400/raspberry-pi-use-vlc-to-stream-webcam-logitech-c920-h264-video-without-tran   |
|         ffmpeg -> rtsp          |               rtsp not browser supported?                | https://codecalamity.com/raspberry-pi-hardware-accelerated-h264-webcam-security-camera/                                    |
|           ffmpeg mpeg           |                           mpeg                           | https://phoboslab.org/log/2013/09/html5-live-video-streaming-via-websockets                                                |
|  raspivid -> ffmpeg -> youtube  |                     not what we need                     | https://www.digikey.com/en/maker/blogs/streaming-live-to-youtube-and-facebook-using-raspberry-pi-camera                    |
|                                 |                                                          | https://raspberrypi.stackexchange.com/questions/115889/best-way-to-stream-usb-camera-video-in-2020                         |
|        raspivid directly        |                           nope                           | https://wiki.marcluerssen.de/index.php?title=Raspberry_Pi/Camera_streaming                                                 |
|              v4l2               |              perhaps  not same application               | https://siytek.com/raspberry-pi-rtsp-to-home-assistant/                                                                    |
|                                 |                                                          | https://www.ics.com/blog/raspberry-pi-camera-module#.VJFhbyvF-b8                                                           |
|    pyserver webrtc remoteme     |      ?? its for rc applications so perhaps its okay      | https://remoteme.org/raspberry-pi/                                                                                         |
|    pyserver picamera  ffmpeg    |                                                          | https://nerdhut.de/2018/12/17/low-latency-and-high-fps-camera-stream-with-raspberry-pi/                                    |
| python picamera  raspivid->clvc |                          python                          | https://draeger-it.blog/raspberry-pi-camera-b01/                                                                           |
|                                 |                                                          | https://randomnerdtutorials.com/video-streaming-with-raspberry-pi-camera/                                                  |
|      raspivid    gstreamer      |                        from 2013                         | http://blog.tkjelectronics.dk/2013/06/how-to-stream-video-and-audio-from-a-raspberry-pi-with-no-latency/                   |
|                                 |       vlc player 0ms buffer 0,6s latency from 2019       | https://gist.github.com/neilyoung/8216c6cf0c7b69e25a152fde1c022a5d                                                         |
|      rpi-cam-web-interface      |                        php  mjpeg                        | https://elinux.org/RPi-Cam-Web-Interface                                                                                   |
|      medooze-media-server       |              not tested, bad documentation               | https://github.com/medooze/media-server-node                                                                               |
|          v4l2rtpserver          |      hls not supported on browsers ( apple stuff )       | https://github.com/mpromonet/v4l2rtspserver                                                                                |
|         webrtc-streamer         |       bad documentation unclear resolution config        | https://github.com/mpromonet/webrtc-streamer                                                                               |
|           balena cam            | Webrtc/mjpegfallback    costs with more than 10 projects | https://github.com/balenalabs/balena-cam                                                                                   |

https://raspberrypi.stackexchange.com/questions/27082/how-to-stream-raspivid-to-linux-and-osx-using-gstreamer-vlc-or-netcat 

some python server picamera / streams
https://picamera.readthedocs.io/en/release-1.13/api_streams.html

collection of soutions mjpeg uv4l ffmpeg 
https://projects-raspberry.com/video-streaming-on-flask-server-using-rpi/

https://raspberrypi.stackexchange.com/questions/27381/streaming-from-rpi-over-vlc

https://raspberrypi.stackexchange.com/questions/4412/streaming-h264-with-logitech-c920 

https://qastack.com.de/raspberrypi/23182/how-to-stream-video-from-raspberry-pi-camera-and-watch-it-live
https://raspberrypi.stackexchange.com/questions/23182/how-to-stream-video-from-raspberry-pi-camera-and-watch-it-live/32521#32521

something about using camera well
[Getting the most out of your V2 camera with UV4L. ](https://www.raspberrypi.org/forums/viewtopic.php?t=197585)


no idea

to read
https://www.avisec.ch/teil-11-wie-webcam-website-integrieren/

not interesting
price https://www.ipcamlive.com
https://restream.io/pricing
other commercial solutions [[comercialstreamingsolutions]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[UV4L]: uv4l "UV4L"
[GStreamer]: gstreamer "GStreamer"
[comercialstreamingsolutions]: comercialstreamingsolutions "Commercial solutions"
[//end]: # "Autogenerated link references"