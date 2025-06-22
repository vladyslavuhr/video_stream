This project demonstrates how to stream a video over the web using HLS (HTTP Live Streaming) with multiple resolutions and manual quality selection via hls.js.

Step 1: Prepare the Source Video
I began by downloading a sample video file and saving it locally on my device.

To prepare it for streaming, I installed FFmpeg via Homebrew:

    brew install ffmpeg

Step 2: Encode into HLS Segments
Using FFmpeg, I transcoded the video into three different resolutions (1080p, 720p, 480p), each segmented into 2-second .ts chunks and an accompanying .m3u8 playlist:

    # 1080p
    ffmpeg -i input.mp4 -vf scale=-2:1080 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 \
    -hls_time 2 -hls_playlist_type vod -b:v 5000k -maxrate 5350k -bufsize 7500k \
    -hls_segment_filename 1080p_%03d.ts 1080p.m3u8

    # 720p
    ffmpeg -i input.mp4 -vf scale=-2:720 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 \
    -hls_time 2 -hls_playlist_type vod -b:v 2800k -maxrate 2996k -bufsize 4200k \
    -hls_segment_filename 720p_%03d.ts 720p.m3u8

    # 480p
    ffmpeg -i input.mp4 -vf scale=-2:480 -c:v libx264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 \
    -hls_time 2 -hls_playlist_type vod -b:v 1400k -maxrate 1498k -bufsize 2100k \
    -hls_segment_filename 480p_%03d.ts 480p.m3u8

Step 3: Create Master Playlist
The master.m3u8 file links the three resolution-specific playlists:

    #EXTM3U
    #EXT-X-VERSION:3
    #EXT-X-STREAM-INF:BANDWIDTH=5000000,RESOLUTION=1920x1080
    1080p.m3u8
    #EXT-X-STREAM-INF:BANDWIDTH=2800000,RESOLUTION=1280x720
    720p.m3u8
    #EXT-X-STREAM-INF:BANDWIDTH=1400000,RESOLUTION=854x480
    480p.m3u8

Step 4: Upload to GitHub Pages
I created a public GitHub repository and uploaded the following files:

    All .ts segment files
    1080p.m3u8, 720p.m3u8, and 480p.m3u8
    The master.m3u8 top-level manifest
    A custom index.html file with a built-in HLS player

Step 5: HLS.js Video Player
In the index.html file, I used the hls.js library to:

    Load the master.m3u8 manifest
    Play the video in the browser
    Add a manual quality selector dropdown

Live Demo
You can view and test the final HLS stream here:

    https://vladyslavuhr.github.io/video_stream/
