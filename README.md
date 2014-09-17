zbcclivestream
==============

Collection of scripts that power the internal livestream at ZBCC.

Install
-------

- QuickTime Broadcaster

- Darwin Streaming Server(http://dss.macosforge.org)

- homebrew

- ffmpeg

Startup Script
--------------
`sleep 10`
`cd /Library/WebServer/Documents/video/ && ./start_livestream.sh &`
`osascript -e 'tell application "Terminal" to set miniaturized of first window to true'`
`sleep 10`
`osascript -e 'tell application "QuickTime Broadcaster" to set miniaturized of first window to true'`

start_livestream.sh
-------------------
`rm -f *.ts`
`rm -f play.m3u8`
`broadcasterctl quit`
`sleep 4 `
`open /Applications/QuickTime\ Broadcaster.app`
`sleep 4`
`broadcasterctl -f /Library/WebServer/Documents/video/livestream.qtbr -a LS -v LS -n LS -t av start`
`sleep 2`
`ffmpeg -i /Library/WebServer/Documents/video/livestream.sdp -c:v libx264 -s 960x540 -aspect 16:9  -c:a copy -r 23.97 -flags -global_header -f segment -segment_time 2 -segment_list play.m3u8 -segment_list_flags +live -segment_list_size 2 -segment_format mpegts -segment_list_type hls stream%05d.ts`

