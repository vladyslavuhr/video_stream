# video_stream

I began by downloading a sample video file and saved on my device.

To prepare the video for streaming, I installed FFmpeg via Homebrew:
  brew install ffmpeg

Using FFmpeg, I transcoded the input video into 3 versions at different resolutions, each segmented into 2-second .ts chunks and corresponding .m3u8 playlists:
  ffmpeg -i input.mp4 -vf scale=-2:1080 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 -hls_time 2 -hls_playlist_type vod -b:v 5000k -maxrate 5350k -bufsize 7500k -hls_segment_filename 1080p_%03d.ts 1080p.m3u8
  ffmpeg -i input.mp4 -vf scale=-2:720 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 -hls_time 2 -hls_playlist_type vod -b:v 2800k -maxrate 2996k -bufsize 4200k -hls_segment_filename 720p_%03d.ts 720p.m3u8
  ffmpeg -i input.mp4 -vf scale=-2:480 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 -hls_time 2 -hls_playlist_type vod -b:v 1400k -maxrate 1498k -bufsize 2100k -hls_segment_filename 480p_%03d.ts 480p.m3u8

This top-level manifest links the three variant playlists:
  #EXTM3U
  #EXT-X-VERSION:3
  #EXT-X-STREAM-INF:BANDWIDTH=5000000,RESOLUTION=1920x1080
  1080p.m3u8
  #EXT-X-STREAM-INF:BANDWIDTH=2800000,RESOLUTION=1280x720
  720p.m3u8
  #EXT-X-STREAM-INF:BANDWIDTH=1400000,RESOLUTION=854x480
  480p.m3u8

I created a new repository and uploaded:
  All .ts segment files
  The 480p.m3u8, 720p.m3u8, and 1080p.m3u8 playlists
  The master.m3u8 file
  A custom index.html player
I hosted it using GitHub Pages.

In the index.html file, I included the HLS.js library and wrote JavaScript code to:
  Load master.m3u8
  Allow manual quality switching (via dropdown)

The video is playable here via GitHub Pages:
  https://vladyslavuhr.github.io/video_stream/
