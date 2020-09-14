# Janus

https://github.com/meetecho/janus-gateway


## Docker Variations

https://github.com/canyanio/janus-gateway-docker

docker pull canyan/janus-gateway:latest does not work (compiled for x86)


[Alessandro Amirante - Janus &Docker: friends or foe? a Talk about Deployment](https://www.youtube.com/watch?v=mrV2BQ95UFY)


https://groups.google.com/g/meetecho-janus/search?q=proxy 



Following Janus install instructions:

got libnice checked out version 0.1.17
installed meson https://mesonbuild.com/Quick-guide.html

didn't work

### Snap
Couldnt start http service with port 8088 even though 8088 was not in use

### docker 

fruitnanny works with Docker

### Streaming pipelines Comparison

| works           | cpu | lag                         | bandwidth | quality                                                        | command                                                                                                                                                                                                                   | second half                                                                                                        |
| --------------- | --- | --------------------------- | --------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| works           | 10  | lags remotely not in lan    | 2.5       | good                                                           | raspivid -t 0 -h 720 -w 1080 -fps 25 -hf -b 2000000 -o -                                                                                                                                                                  | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink host=0.0.0.0 port=8004          |
| no              | 15  |                             |           | Discarding outgoing empty RTP packet                           | raspivid -t 0 -h 720 -w 1080 -fps 25 -hf -b 2000000 -o -                                                                                                                                                                  | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! gdppay ! udpsink host=0.0.0.0 port=8004 |
| no              | -   |                             |           | just dies                                                      | raspivid -w 1280 -h 720 -fps 30 --b 2000000 --profile baseline --timeout 0 -o -                                                                                                                                           | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink host=127.0.0.1 port=8004        |
| no              | 10  | yes                         | 2-3       | ~0.1 fps and 0.1sec snippets of smooth video                   | raspivid -n -t 0 -w 1080 -h 720 -fps 30 -b 2000000 -o -                                                                                                                                                                   | gst-launch-1.0 -e -vvvv fdsrc ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink host=0.0.0.0 port=8004    |
| no              |     |                             |           | janus didn't send anything                                     | raspivid -o - -t 0 -hf -w 1080 -h 720 -fps 30                                                                                                                                                                             | cvlc -vvv stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8004}' :demux=h264                         |
| no              | 4   |                             | 0.5-1     | frozen till next keyframe then smooth snippet and frozen again | raspivid -t 999999 -b 200000 -o -                                                                                                                                                                                         | gst-launch-1.0 -e -vvv fdsrc ! h264parse ! rtph264pay pt=96 config-interval=1 ! udpsink host=0.0.0.0 port=8004     |
| no              |     |                             |           | just creates load                                              | gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-raw,width=1080,height=720,framerate=30/1 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=0.0.0.0 port=8004                                                       |
| yes             | 20  | no lag                      | 6-7       | good fast image                                                | gst-launch-1.0 v4l2src ! video/x-h264, width=1080, height=720, framerate=30/1 ! h264parse ! rtph264pay config-interval=1 pt=96 ! udpsink sync=false host=0.0.0.0 port=8004                                                |
| yes             | 100 | starts lagging behind       | 0.8       | gets artifacts / starts lagging behind                         | gst-launch-1.0 v4l2src ! 'video/x-raw, width=1080, height=720, framerate=30/1' ! videoconvert ! x264enc pass=qual quantizer=20 tune=zerolatency ! rtph264pay ! udpsink host=127.0.0.1 port=8004                           |
| yes             | 80  | slightly                    | 0.8       | not the greatest but okay                                      | gst-launch-1.0 -vvvv v4l2src ! 'video/x-raw, width=1080, height=720, framerate=30/1' ! videoconvert ! x264enc pass=qual quantizer=20 tune=zerolatency ! rtph264pay ! udpsink port=8004                                    |
| yes             | 35  | no lag                      | 0.1       | really low bitrate 0.01                                        | gst-launch-1.0 -v v4l2src ! video/x-raw,width=1080, height=720, framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=500 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=8004  |
| yes really well | 55  | no lag    really fast       | 2         | good image quality                                             | gst-launch-1.0 -v v4l2src ! video/x-raw,width=1080, height=720, framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=2000 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=8004 |
| best            | 45  | no lag    really fast  best | 1         | goo enough image quality                                       | gst-launch-1.0 -v v4l2src ! video/x-raw,width=1080, height=720, framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=1000 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=8004 |
| no              | 35  |                             |           | Discarding outgoing empty RTP packet                           | gst-launch-1.0 -e v4l2src do-timestamp=true ! video/x-h264,width=1080,height=720,framerate=30/1 ! h264parse ! rtph264pay config-interval=1 ! gdppay ! udpsink host=0.0.0.0 port=8004                                      |
| no              |     |                             |           | error could not open x display for reading                     | gst-launch-1.0 -v v4l2src ! video/x-raw,width=1080,height=720,framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=500 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=8004    |




