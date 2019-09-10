# 使用注意

先push到/data/local/tmp，然后再使用

 ./ffmpeg -re -i test.mp4 -c:v libx264 -c:a copy -vf "drawtext=fontfile=/system/fonts/DroidSans.ttf:text='hello':x=100:y=100:fontsize=24:fontcolor=yellow:shadowy=2" -f flv rtmp://192.168.2.117:1935/stream/example