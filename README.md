node-rtsp-stream-hls
================

Convert all RTSP streams to HLS using ffmpeg [FFmpeg](https://github.com/FFmpeg/FFmpeg). Output is possible using the HTML5 video tag.

You must install [ffmpeg](https://ffmpeg.org/).


Usage:

```
$ npm install rtsp-stream-hls
```

On server: ( used express )
```
const videoStream = require('../index');
let express = require('express');
let path = require('path');
let app = express();

app.use(express.static(path.join(__dirname, 'public')));

app.get('/', function(req, res, next) {
    res.sendFile(__dirname+'/index.html');
});

const options = {
    url : 'rtsp://wowzaec2demo.streamlock.net/vod/mp4:BigBuckBunny_115k.mp4', // rtsp URL. this is sample url
    segmentFolder : __dirname + '/public/segment',  //The path is where the .m3u8 and ts files will be stored.
    ffmpegOptions : {  //ffmpeg option.
        // If you have a ffmpeg option you want to add, write it down here.
        '-hls_time': '30', // If the same option is set, it is possible to change the value.
        'ultrafast':undefined // single option other than the key-value type, it can be set by setting the value to undefined.
    },
}
const stream = new videoStream(options);
stream.start();


app.listen(3000);
    
```

On client:
```
<!DOCTYPE html>
<html>
<head>
  <title>Video Streaming</title>
</head>
<body>
<script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
<video id="video" controls preload></video>
<script>
  if(Hls.isSupported()) {
    var video = document.getElementById('video');
    var hls = new Hls();
    hls.loadSource('http://localhost:3000/segment/stream.m3u8');
    hls.attachMedia(video);
  }
</script>
</body>
</html>
```

Using ffmpeg, various options can be added. You can check the options on the [ffmpeg](https://ffmpeg.org/ffmpeg.html) official website.
(However, please note that this module is made based on HLS.)
