DLStreamer Commands
----------------------------------

Outside docker
----------------------------
	export MODELS_PATH=/home/${USER}/intel/models


Inside docker
----------------------------
export DETECTION_MODEL=/home/dlstreamer/models/intel/person-vehicle-bike-detection-2004/FP16/person-vehicle-bike-detection-2004.xml
export DETECTION_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/person-vehicle-bike-detection-2004.json
export VEHICLE_CLASSIFICATION_MODEL=/home/dlstreamer/models/intel/vehicle-attributes-recognition-barrier-0039/FP16/vehicle-attributes-recognition-barrier-0039.xml
export VEHICLE_CLASSIFICATION_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/vehicle-attributes-recognition-barrier-0039.json
export FACE_DETECTION_MODEL=/home/dlstreamer/models/intel/face-detection-adas-0001/FP16/face-detection-adas-0001.xml
export FACE_DETECTION_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/face-detection-adas-0001.json
export AGE_GENDER_MODEL=/home/dlstreamer/models/intel/age-gender-recognition-retail-0013/FP16/age-gender-recognition-retail-0013.xml
export AGE_GENDER_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/age-gender-recognition-retail-0013.json
export EMOTIONS_MODEL=/home/dlstreamer/models/intel/emotions-recognition-retail-0003/FP16/emotions-recognition-retail-0003.xml
export EMOTIONS_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/emotions-recognition-retail-0003.json
export LANDMARKS_MODEL=/home/dlstreamer/models/intel/landmarks-regression-retail-0009/FP16/landmarks-regression-retail-0009.xml
export LANDMARKS_MODEL_PROC=/opt/intel/dlstreamer/samples/gstreamer/model_proc/intel/landmarks-regression-retail-0009.json
export VIDEO_EXAMPLE=/home/dlstreamer/models/person-bicycle-car-detection.mp4

Launch the container
----------------------------

xhost +local:root

docker run -it --rm \
  -e DISPLAY=$DISPLAY \
  -e QT_X11_NO_MITSHM=1 \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v ${MODELS_PATH}:/home/dlstreamer/models \
  --device /dev/dri \
  --device /dev/accel \
  --env ZE_ENABLE_ALT_DRIVERS=libze_intel_vpu.so \
  --env MODELS_PATH=/home/dlstreamer/models \
  --privileged \
  intel/dlstreamer:2025.0.1.2-ubuntu22
  

Launch video
-------------------------  
gst-launch-1.0 filesrc location=$VIDEO_EXAMPLE ! decodebin3 ! gvadetect model="$DETECTION_MODEL" model-proc="$DETECTION_MODEL_PROC" device=CPU ! queue ! gvawatermark ! videoconvert ! xvimagesink sync=false


gst-launch-1.0 filesrc location=https://www.youtube.com/watch?v=qPgWV8Rxemo ! decodebin3 ! gvadetect model="$DETECTION_MODEL" model-proc="$DETECTION_MODEL_PROC" device=CPU ! queue ! gvawatermark ! videoconvert ! xvimagesink sync=false

samples
-------------
./dlstreamer/samples/gstreamer/gst_launch/human_pose_estimation/human_pose_estimation.sh

Webcam launch
----------------
gst-launch-1.0 v4l2src device=/dev/video1 ! video/x-raw,width=640,height=480,format=NV12 ! decodebin3 ! gvadetect model=${FACE_DETECTION_MODEL} model_proc=${FACE_DETECTION_MODEL_PROC} device=CPU ! queue ! gvawatermark ! videoconvert ! autovideosink sync=false

convert youtube to mpx format
-----------------------------
yt-dlp -F "https://www.youtube.com/something"
yt-dlp -g -f <230> "https://www.youtube.com/watch?v=soemthign"

Driving in Tokyo
-------------------------
gst-launch-1.0 uridecodebin uri="https://manifest.googlevideo.com/api/manifest/hls_playlist/expire/1747417995/ei/KycnaNaMAeO26dsPwI3SiQE/ip/2a02:810d:4a80:5400:10b3:8004:145:61ec/id/a8f81657c4717a6a/itag/230/source/youtube/requiressl/yes/ratebypass/yes/pfa/1/sgovp/clen%3D158761084%3Bdur%3D3052.000%3Bgir%3Dyes%3Bitag%3D134%3Blmt%3D1726471739739753/rqh/1/hls_chunk_host/rr4---sn-h0jeenle.googlevideo.com/xpc/EgVo2aDSNQ%3D%3D/met/1747396395,/mh/rM/mm/31,29/mn/sn-h0jeenle,sn-h0jelne6/ms/au,rdu/mv/m/mvi/4/pl/38/rms/au,au/initcwndbps/3736250/bui/AecWEAYUeQDTucf06x0_ok83bBaWxjuTawRQFoeBwPXofPFq1hSv02ujlT0GspZp3wEXKZ8QbiTP-wGV/spc/wk1kZn8X1_jH350VEOvhC82n9J_FxnD13Fe-kpyHVCyj_GcZksA/vprv/1/playlist_type/DVR/dover/13/txp/5432434/mt/1747395671/fvip/2/short_key/1/keepalive/yes/sparams/expire,ei,ip,id,itag,source,requiressl,ratebypass,pfa,sgovp,rqh,xpc,bui,spc,vprv,playlist_type/sig/AJfQdSswRgIhALIqrei0kdTlokZrYxZTgV5A_5BpGpaGGg0hSVhv3bX2AiEAn9YC9cmDIH0m_GzCFaPNpMVlUogIgh4wBt7NgDXFQhM%3D/lsparams/hls_chunk_host,met,mh,mm,mn,ms,mv,mvi,pl,rms,initcwndbps/lsig/ACuhMU0wRAIgTaym7ExH3OcGOeoh3W5LLVVxeBoYzNNJn4TvGx6rflkCIB4EDI5QkIZL35oCbQJlohxhJjsuEN-AZuVUfyme5kie/playlist/index.m3u8" ! decodebin3 ! gvadetect model="$DETECTION_MODEL" model-proc="$DETECTION_MODEL_PROC" device=CPU ! queue ! gvawatermark ! videoconvert ! xvimagesink sync=false

Driving in Chennai
------------------------
gst-launch-1.0 uridecodebin uri="https://manifest.googlevideo.com/api/manifest/hls_playlist/expire/1747418492/ei/HCknaPbJI4fHi9oPmqqd8QE/ip/2a02:810d:4a80:5400:10b3:8004:145:61ec/id/5b39e56197dc8af0/itag/230/source/youtube/requiressl/yes/ratebypass/yes/pfa/1/sgovp/clen%3D113504027%3Bdur%3D1550.099%3Bgir%3Dyes%3Bitag%3D134%3Blmt%3D1726835377855822/rqh/1/hls_chunk_host/rr1---sn-h0jeln7e.googlevideo.com/xpc/EgVo2aDSNQ%3D%3D/met/1747396892,/mh/-8/mm/31,29/mn/sn-h0jeln7e,sn-h0jeenle/ms/au,rdu/mv/m/mvi/1/pl/38/rms/au,au/initcwndbps/3690000/bui/AecWEAaCzi0Ml60L917I7DpwBbf1txxf3zrdzB_WhAbUazPFwTi-45IEXFXqG7oI9T20AcgaUlSagCxA/spc/wk1kZqHkSR7UKRs2sakjDzRuUZ_RHAaQgeO8MKnWjFSi_h7DxtU/vprv/1/playlist_type/DVR/dover/13/txp/6309224/mt/1747396394/fvip/1/short_key/1/keepalive/yes/sparams/expire,ei,ip,id,itag,source,requiressl,ratebypass,pfa,sgovp,rqh,xpc,bui,spc,vprv,playlist_type/sig/AJfQdSswRAIgEuENaSLE7XqhXWzcccNfCOeWK5YxyqBPuJW3lw_ipUcCIFM-BIjCAxrslhFVctZX_GZdljEmGkxCnLSAXMCp_weH/lsparams/hls_chunk_host,met,mh,mm,mn,ms,mv,mvi,pl,rms,initcwndbps/lsig/ACuhMU0wRQIgDpoi_qSvxcP8XMrb3UO95rnvo51qUs_tkPQYV2dNGMwCIQCdkvgKRPSdIXa3soHOshiBEJkcki4620BEWqiWCjOEkg%3D%3D/playlist/index.m3u8" ! decodebin3 ! gvadetect model="$DETECTION_MODEL" model-proc="$DETECTION_MODEL_PROC" device=CPU ! queue ! gvawatermark ! videoconvert ! xvimagesink sync=false


