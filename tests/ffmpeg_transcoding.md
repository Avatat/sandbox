# FFmpeg transcoding benchmarks
I made FFmpeg transcoding benchmarks to learn the basics of FFmpeg and hardware acceleration, and to check how much hardware acceleration gives us.

Currently, only Intel and NVIDIA acceleration was tested by me. I have to get a Radeon GPU to test AMD acceleration.

The reference source for every transcoding is a popular, open-source animation [Big Buck Bunny](http://bbb3d.renderfarming.net/).

## Transcoding 1080p30 (H.264) to 720p, 540p, and 480p (H.264) simultaneously
Sometimes, I had to set 478 px height, because of BBB ratio and `[libx264 @ 0x555bb8838fc0] width not divisible by 2 (853x480)` error.

### Software decoding/encoding on Intel i5-8250U

<details>
<summary>Speed: 1.53x (time 6m56s), preset: medium</summary>

```
$ time ffmpeg -y -c:v h264 -i bbb_sunflower_1080p_30fps_normal.mp4 \
-c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:540 -c:v libx264 -b:v 1500k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset medium -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_1080p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-16T17:44:39.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 3481 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], 2998 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[libx264 @ 0x55c63bdda780] using SAR=1/1
[libx264 @ 0x55c63bdda780] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55c63bdda780] profile High, level 3.1
[libx264 @ 0x55c63bdda780] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55c63bde9840] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x55c63bdde340] using SAR=1/1
[libx264 @ 0x55c63bdde340] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55c63bdde340] profile High, level 3.1
[libx264 @ 0x55c63bdde340] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1500 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55c63bddc180] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x55c63be8efc0] using SAR=3824/3825
[libx264 @ 0x55c63be8efc0] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55c63be8efc0] profile High, level 3.1
[libx264 @ 0x55c63be8efc0] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55c63bdf6bc0] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
frame=19038 fps= 46 q=-1.0 Lq=-1.0 q=-1.0 size=  181545kB time=00:10:34.50 bitrate=2343.9kbits/s dup=6 drop=0 speed=1.53x    
video:351345kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	6m55,579s
user	51m46,067s
sys	0m15,223s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      1   4645    506   9265    0    0   119   234 1050 1090 62  7 31  0  0
 0  0      1   4659    506   9250    0    0     0     0  468 1577  0  0 100  0  0
16  0      1   3903    506   9250    0    0     0     0 2729 6057 88  1 11  0  0
15  0      1   3900    506   9250    0    0     0     1 2783 5381 96  1  4  0  0
17  0      1   3896    506   9250    0    0     0     0 2922 6310 93  1  7  0  0
31  0      1   3895    506   9250    0    0     0    10 2954 6562 92  1  7  0  0
25  0      1   3893    506   9249    0    0     0     2 2829 6006 93  1  6  0  0
21  0      1   3919    506   9249    0    0     4    32 2899 5887 96  1  4  0  0
32  0      1   3917    506   9249    0    0     0    40 2766 5894 93  1  7  0  0
18  0      1   3912    507   9249    0    0     1    41 2815 5299 96  1  3  0  0
10  0      1   3909    507   9251    0    0     0    12 2940 6883 87  1 12  0  0
17  0      1   3915    507   9250    0    0     0     0 2680 5262 94  1  5  0  0
17  0      1   3914    507   9250    0    0     0     0 2764 5708 94  1  5  0  0
21  0      1   3930    507   9234    0    0     0     0 2800 4725 99  0  1  0  0
26  0      1   3929    507   9234    0    0     0     0 2853 5128 99  1  1  0  0
 0  0      1   4713    507   9234    0    0     0    17 2319 6233 75  1 24  0  0
 0  0      1   4713    507   9234    0    0     0     7   98  148  0  0 100  0  0
```
</details>

<details>
<summary>Speed: 2.75x (time 3m51s) , preset: veryfast</summary>

```
$ time ffmpeg -y -c:v h264 -i bbb_sunflower_1080p_30fps_normal.mp4 \
> -c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale=-1:540 -c:v libx264 -b:v 1500k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset veryfast -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_1080p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-16T17:44:39.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 3481 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], 2998 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[libx264 @ 0x556cd1fd9780] using SAR=1/1
[libx264 @ 0x556cd1fd9780] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x556cd1fd9780] profile High, level 3.1
[libx264 @ 0x556cd1fd9780] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=4 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x556cd1fe8840] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x556cd1fdd340] using SAR=1/1
[libx264 @ 0x556cd1fdd340] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x556cd1fdd340] profile High, level 3.1
[libx264 @ 0x556cd1fdd340] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=4 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1500 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x556cd1fdb180] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x556cd208dfc0] using SAR=3824/3825
[libx264 @ 0x556cd208dfc0] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x556cd208dfc0] profile High, level 3.1
[libx264 @ 0x556cd208dfc0] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x556cd1ff5bc0] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
frame=19038 fps= 82 q=-1.0 Lq=-1.0 q=-1.0 size=  181621kB time=00:10:34.50 bitrate=2344.9kbits/s dup=6 drop=0 speed=2.75x    
video:352198kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	3m51,172s
user	18m4,191s
sys	0m16,205s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      1   4657    507   9280    0    0   100   197  911    4 59  6 35  0  0
 0  0      1   4749    507   9228    0    0     2    15  445 1264  1  0 99  0  0
20  0      1   4209    507   9228    0    0     0     2 2504 14570 57  2 42  0  0
 1  0      1   4206    507   9228    0    0     0     0 2294 15483 51  2 47  0  0
 4  0      1   4203    507   9228    0    0     0     0 2389 14437 56  2 43  0  0
 3  0      1   4196    507   9228    0    0     1     2 2517 14720 58  2 40  0  0
 8  0      1   4193    507   9228    0    0     0    12 2382 14248 56  2 42  0  0
14  0      1   4189    507   9228    0    0     0    19 2649 14225 60  2 38  0  0
 7  0      1   4188    507   9228    0    0     0    10 2642 13704 69  2 30  0  0
 1  0      1   4732    507   9243    0    0     0    47 2016 12120 41  2 57  0  0
 0  0      1   4747    507   9228    0    0     0     8  166  403  0  0 100  0  0
```
</details>

### Intel Quick Sync Video decoding/encoding on Intel i5-8250U (UHD 620)
<details>
<summary>Speed: 5.91x (time 1m47s) , preset: medium</summary>

```
$ time ffmpeg -y -hwaccel qsv -c:v h264_qsv -i bbb_sunflower_1080p_30fps_normal.mp4 \
> -c:a copy -vf scale_qsv=-1:720 -c:v h264_qsv -b:v 2000k -preset medium -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:540 -c:v h264_qsv -b:v 1500k -preset medium -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:478 -c:v h264_qsv -b:v 1000k -preset medium -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_1080p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-16T17:44:39.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 3481 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], 2998 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x559e5da4c8c0] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x559e5da3e940] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x559e5da4b0c0] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[h264_qsv @ 0x559e5da3d2c0] A decode call did not consume any data: expect more data at input (-10)=6 drop=0 speed=5.91x    
    Last message repeated 2 times
frame=19038 fps=177 q=26.0 Lq=26.0 q=26.0 size=  175084kB time=00:10:34.50 bitrate=2260.5kbits/s dup=6 drop=0 speed=5.91x    
video:339669kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	1m47,472s
user	0m40,988s
sys	0m18,079s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      1   4732    508   9243    0    0    90   178  841   88 56  5 39  0  0
 0  0      1   4748    508   9228    0    0     0    48  297  794  1  0 99  0  0
 3  0      1   4551    508   9388    0    0     0     6 9804 17381  5  3 90  3  0
 3  0      1   4542    508   9389    0    0     0     2 11389 19995  5  4 88  3  0
 4  0      1   4534    508   9389    0    0     0     2 11257 19598  5  4 89  3  0
 0  0      1   4748    508   9228    0    0     0     5 8553 15494  3  3 91  2  0
 0  0      1   4747    508   9228    0    0     0     4  146  363  0  0 100  0  0
```
</details>

<details>
<summary>Speed: 7.65x (time 1m23s) , preset: veryfast</summary>

```
$ time ffmpeg -y -hwaccel qsv -c:v h264_qsv -i bbb_sunflower_1080p_30fps_normal.mp4 \
> -c:a copy -vf scale_qsv=-1:720 -c:v h264_qsv -b:v 2000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:540 -c:v h264_qsv -b:v 1500k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:478 -c:v h264_qsv -b:v 1000k -preset veryfast -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_1080p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-16T17:44:39.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 3481 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], 2998 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x560e2c77a8c0] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x560e2c76c940] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x560e2c7790c0] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[h264_qsv @ 0x560e2c76b2c0] A decode call did not consume any data: expect more data at input (-10)=6 drop=0 speed=7.65x    
    Last message repeated 2 times
frame=19038 fps=229 q=26.0 Lq=26.0 q=26.0 size=  175124kB time=00:10:34.50 bitrate=2261.0kbits/s dup=6 drop=0 speed=7.65x    
video:340110kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	1m23,005s
user	0m41,022s
sys	0m18,078s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      1   4724    509   9244    0    0    87   172  837  136 54  5 41  0  0
 0  0      1   4742    509   9229    0    0     0    26  496 1751  0  0 99  0  0

 4  0      1   4520    509   9395    0    0     0    51 13799 26553  7  4 87  2  0
 0  1      1   4519    509   9396    0    0     0    18 14749 28811  8  5 86  2  0
 0  0      1   4738    509   9229    0    0     0     0 12310 23814  6  4 88  2  0
 0  0      1   4740    509   9229    0    0     0     0  209  607  0  0 100  0  0
```
</details>

### NVIDIA NVDEC/NVENC decoding/encoding on Quadro K2200
The results comes from [here](nvenc.md).
<details>
<summary>Speed: 11.3x (time 56s) , preset: medium</summary>

```
$ time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
...
 Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
 Press [q] to stop, [?] for help
 [mp4 @ 0x5594172bdbc0] track 1: codec frame size is not set
 Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #0:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 4000000 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 [mp4 @ 0x5594172dbb40] track 1: codec frame size is not set
 Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #1:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 3000000 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 [mp4 @ 0x559417380440] track 1: codec frame size is not set
 Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #2:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 853x480 [SAR 2560:2559 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 2000000 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 frame=19036 fps=340 q=22.0 Lq=19.0 q=21.0 size=  180283kB time=00:10:34.73 bitrate=2326.8kbits/s speed=11.3x    
 video:350085kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

 real	0m56.111s
 user	0m19.084s
 sys	0m2.315s
```
```
# nvidia-smi dmon -i 0 -d 15
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     3    53     -     0     0     0     0  2505  1045
    0    29    64     -    96    50    99    89  2505  1124
    0    26    66     -    88    46   100    81  2505  1124
    0    31    69     -    96    48   100    82  2505  1124
    0     5    64     -     0     0    29    25  2505  1124
    0     2    57     -     0     0     0     0   405   405
```
```
# vmstat 15
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0      0 61851828  42632 741224    0    0     3    24   29   38 11  1 88  0  0
1  0      0 61643896  42632 744296    0    0     0    10 2210 6992  2  1 97  0  0
1  0      0 61643028  42640 744296    0    0     0     2 2262 7098  3  1 97  0  0
0  0      0 61636580  42640 744296    0    0     0     0 2261 7183  3  1 97  0  0
0  0      0 61848704  42640 741224    0    0     0     0 1938 6034  2  1 97  0  0
0  0      0 61848712  42640 741224    0    0     0     0  351  725  0  0 100  0  0
```
</details>

<details>
<summary>Speed: 12.5x (time 51s) , preset: fast</summary>

```
$ time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -preset fast -f mp4 720p -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -preset fast -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -preset fast -f mp4 /dev/null
...
 Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
 Press [q] to stop, [?] for help
 [mp4 @ 0x563bc6908a40] track 1: codec frame size is not set
 Output #0, mp4, to '720p':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #0:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 4000000 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 [mp4 @ 0x563bc6925bc0] track 1: codec frame size is not set
 Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #1:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 960x540 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 3000000 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 [mp4 @ 0x563bc69bcd00] track 1: codec frame size is not set
 Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #2:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 853x480 [SAR 2560:2559 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 2000000 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
 frame=19036 fps=376 q=23.0 Lq=20.0 q=22.0 size=  180309kB time=00:10:34.73 bitrate=2327.1kbits/s speed=12.5x    
 video:350097kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

 real	0m50.826s
 user	0m18.111s
 sys	0m2.280s
```
```
# nvidia-smi dmon -i 0 -d 15
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     3    53     -     0     0     0     0  2505  1045
    0    31    64     -    99    51    87    97  2505  1124
    0    31    67     -    99    51    89    92  2505  1124
    0    32    69     -    99    51    88    87  2505  1124
    0     2    61     -     0     0     3     3  2505   901
    0     2    55     -     0     0     0     0   405   405
```
```
# vmstat 15
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0      0 62243128  42656 741224    0    0     3    24   29   41 11  1 89  0  0
0  0      0 62054248  42656 744296    0    0     0  3278 2493 7958  3  1 96  0  0
1  0      0 61990576  42656 744296    0    0     0  2535 2552 8194  3  1 96  0  0
1  0      0 61931780  42672 744296    0    0     0  3744 2565 8248  3  1 96  0  0
0  0      0 62124336  42672 741224    0    0     0  2703 1394 4162  1  0 98  0  0
0  0      0 62124328  42676 741224    0    0     0    12  351  729  0  0 100  0  0
```
</details>

## Transcoding 2160p30 (H.264) to 1080p, 720p, and 480p (H.264) simultaneously

### Software decoding/encoding on Intel i5-8250U

<details>
<summary>Speed: 0.69x (time 15m16s), preset: medium</summary>

```
$ time ffmpeg -y -c:v h264 -i bbb_sunflower_2160p_30fps_normal.mp4 -c:a copy -vf scale=-1:1080 -c:v libx264 -b:v 4000k -preset medium -f mp4 /dev/null -c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset medium -f mp4 /dev/null -c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset medium -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_2160p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-18T14:43:04.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 7980 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 3840x2160 [SAR 1:1 DAR 16:9], 7498 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[libx264 @ 0x55f3587db640] using SAR=1/1
[libx264 @ 0x55f3587db640] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55f3587db640] profile High, level 4.0
[libx264 @ 0x55f3587db640] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=4000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55f3587dd040] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], q=-1--1, 4000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x55f3587dc780] using SAR=1/1
[libx264 @ 0x55f3587dc780] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55f3587dc780] profile High, level 3.1
[libx264 @ 0x55f3587dc780] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55f3587d9c00] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x55f35894ca00] using SAR=3824/3825
[libx264 @ 0x55f35894ca00] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55f35894ca00] profile High, level 3.1
[libx264 @ 0x55f35894ca00] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55f3587f6500] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
frame=19038 fps= 21 q=-1.0 Lq=-1.0 q=-1.0 size=  330888kB time=00:10:34.50 bitrate=4272.1kbits/s dup=6 drop=0 speed=0.693x    
video:540421kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	15m15,763s
user	99m34,253s
sys	0m18,758s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      1   3954    530   9984    0    0    49   108  150   42 31  4 65  0  0
 0  0      1   3970    530   9968    0    0     0     7  170  358  0  0 100  0  0
13  0      1   2699    530   9856    0    0     5     2 1908 3623 29  1 70  0  0
13  0      1   2702    530   9841    0    0     0     0 2313 3470 81  1 18  0  0
36  0      1   2689    530   9841    0    0     0     0 2365 2939 93  0  7  0  0
15  0      1   2682    530   9841    0    0     0     0 2432 3244 82  1 17  0  0
 7  0      1   2666    530   9856    0    0     0    14 2611 3975 86  1 13  0  0
 5  0      1   2677    530   9840    0    0     0     5 2167 3935 65  2 34  0  0
 7  0      1   2674    530   9840    0    0     0    11 2263 2942 83  1 16  0  0
 3  0      1   2674    530   9839    0    0     2    21 2248 3338 75  1 24  0  0
 4  0      1   2672    530   9840    0    0     9     4 2002 3193 67  1 32  0  0
20  0      1   2669    530   9842    0    0     0    41 2569 3258 84  1 15  0  0
 7  0      1   2661    530   9840    0    0    13    32 2387 3631 81  1 18  0  0
 7  0      1   2661    530   9840    0    0     0    15 2058 3130 71  1 29  0  0
 9  0      1   2660    530   9840    0    0     0    14 2322 3157 88  1 12  0  0
 8  0      1   2660    530   9840    0    0     0     2 2406 3565 84  1 15  0  0
16  0      1   2655    530   9840    0    0     0     7 2166 3182 78  1 21  0  0
15  0      1   2655    530   9840    0    0     0     1 2344 3141 88  1 12  0  0
12  0      1   2654    530   9840    0    0     0    17 2176 3002 79  1 20  0  0
17  0      1   2652    530   9840    0    0     0     6 2408 2830 94  0  5  0  0
 7  0      1   2648    530   9840    0    0     0    49 2135 3219 75  1 24  0  0
 3  0      1   2647    530   9840    0    0     0     7 1916 3351 63  1 37  0  0
 9  0      1   2647    530   9840    0    0     0     0 2278 3016 85  1 14  0  0
34  0      1   2646    530   9840    0    0     0     0 2268 2854 88  0 11  0  0
20  0      1   2647    530   9840    0    0     0     0 2147 3125 77  1 23  0  0
 4  0      1   2646    530   9840    0    0     0     0 2202 3333 76  1 23  0  0
18  0      1   2643    530   9840    0    0     0    18 2346 3120 89  1 10  0  0
16  0      1   2613    530   9840    0    0     0    41 2788 3732 98  1  1  0  0
 6  0      1   2638    530   9840    0    0     0    19 2428 3002 95  0  4  0  0
16  0      1   2637    530   9840    0    0     0    20 2530 3169 91  0  8  0  0
26  0      1   2636    530   9840    0    0     0    52 2518 3102 96  0  4  0  0
 6  0      1   2635    530   9840    0    0     0     1 2417 2931 95  0  5  0  0
 8  0      1   2613    530   9854    0    0     0     0 1692 4054 50  1 49  0  0
 0  0      1   4088    530   9854    0    0     0     0  606 1732  7  0 92  0  0
```
</details>

<details>
<summary>Speed: 1.02x (time 10m23s), preset: veryfast</summary>

```
$ time ffmpeg -y -c:v h264 -i bbb_sunflower_2160p_30fps_normal.mp4 \
> -c:a copy -vf scale=-1:1080 -c:v libx264 -b:v 4000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset veryfast -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_2160p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-18T14:43:04.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 7980 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 3840x2160 [SAR 1:1 DAR 16:9], 7498 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (libx264))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[libx264 @ 0x5574e7699640] using SAR=1/1
[libx264 @ 0x5574e7699640] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5574e7699640] profile High, level 4.0
[libx264 @ 0x5574e7699640] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=4 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=4000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5574e769b040] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], q=-1--1, 4000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x5574e769a780] using SAR=1/1
[libx264 @ 0x5574e769a780] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5574e769a780] profile High, level 3.1
[libx264 @ 0x5574e769a780] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=4 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5574e7697c00] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[libx264 @ 0x5574e780aa00] using SAR=3824/3825
[libx264 @ 0x5574e780aa00] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5574e780aa00] profile High, level 3.1
[libx264 @ 0x5574e780aa00] 264 - core 155 r2917 0a84d98 - H.264/MPEG-4 AVC codec - Copyleft 2003-2018 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5574e76b4500] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (libx264) (avc1 / 0x31637661), yuv420p, 850x478 [SAR 3824:3825 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
frame=19038 fps= 31 q=-1.0 Lq=-1.0 q=-1.0 size=  333311kB time=00:10:34.50 bitrate=4303.4kbits/s dup=6 drop=0 speed=1.02x    
video:542892kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	10m23,128s
user	38m27,655s
sys	0m17,346s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      1   4053    530   9891    0    0    44    98  158   72 34  3 63  0  0
 0  0      1   4087    530   9856    0    0     0     1  318  945  0  0 100  0  0
15  0      1   3066    530   9842    0    0     0     0 1616 5457 43  1 56  0  0
 3  0      1   3051    531   9842    0    0     0     2 1783 5380 49  1 50  0  0
 1  0      1   3046    531   9842    0    0     0     0 1664 5296 46  1 53  0  0
 3  0      1   3043    531   9842    0    0     0     4 1541 5716 40  1 59  0  0
 3  0      1   3037    531   9842    0    0     0    14 1686 5768 42  1 57  0  0
 3  0      1   3033    531   9842    0    0     0    10 1557 5748 40  1 59  0  0
 5  0      1   3031    531   9842    0    0     0     3 1801 5523 46  1 53  0  0
 1  0      1   3027    531   9842    0    0     0    58 1625 5780 42  1 57  0  0
 2  0      1   3026    531   9842    0    0     0     7 1789 5666 47  1 52  0  0
12  0      1   3021    531   9842    0    0     0     0 1643 5680 44  1 55  0  0
 2  0      1   3021    531   9842    0    0     0     0 1721 5250 49  1 50  0  0
 3  0      1   3018    531   9844    0    0    43    19 1782 5220 50  1 49  0  0
 4  0      1   3016    531   9844    0    0     0     0 1612 6193 39  1 60  0  0
 4  0      1   3014    531   9844    0    0     0     0 1732 5349 48  1 51  0  0
 4  0      1   3008    531   9851    0    0     0    13 1983 6357 49  1 50  0  0
 4  0      1   3011    531   9844    0    0     0    11 1785 5901 46  2 52  0  0
 4  0      1   3008    531   9844    0    0     0     9 1770 5329 51  1 48  0  0
 4  0      1   3006    531   9844    0    0     0    11 1901 5288 54  1 45  0  0
 2  0      1   3005    531   9844    0    0     0    41 1776 5335 51  1 48  0  0
 3  0      1   3000    531   9844    0    0     0     1 1925 5379 52  1 47  0  0
 0  0      1   4097    531   9844    0    0     0     0 1216 5460 28  1 71  0  0
 0  0      1   4098    531   9844    0    0     0     0  203  562  0  0 100  0  0
```
</details>

### Intel Quick Sync Video decoding/encoding on Intel i5-8250U (UHD 620)
<details>
<summary>Speed: 3.79x (time 2m47s) , preset: medium</summary>

```
$ time ffmpeg -y -hwaccel qsv -c:v h264_qsv -i bbb_sunflower_2160p_30fps_normal.mp4 \
> -c:a copy -vf scale_qsv=-1:1080 -c:v h264_qsv -b:v 4000k -preset medium -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:720 -c:v h264_qsv -b:v 2000k -preset medium -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:480 -c:v h264_qsv -b:v 1000k -preset medium -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_2160p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-18T14:43:04.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 7980 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 3840x2160 [SAR 1:1 DAR 16:9], 7498 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x55c0c8d570c0] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1920x1080 [SAR 1:1 DAR 16:9], q=-1--1, 4000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55c0c8d53fc0] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55c0c8ec5a80] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 853x480 [SAR 2560:2559 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[h264_qsv @ 0x55c0c8d61c40] A decode call did not consume any data: expect more data at input (-10)=6 drop=0 speed=3.79x    
    Last message repeated 2 times
frame=19038 fps=114 q=25.0 Lq=18.0 q=18.0 size=  322942kB time=00:10:34.50 bitrate=4169.5kbits/s dup=6 drop=0 speed=3.79x    
video:522663kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	2m47,475s
user	0m45,527s
sys	0m19,485s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      1   3933    529   9992    0    0    52   115  128  491 33  4 64  0  0
 0  0      1   3955    529   9971    0    0     0    76  508 1451  0  0 100  0  0
 0  5      1   3568    529  10309    0    0     2    31 7004 10973  3  3 84 10  0
 2  0      1   3567    529  10309    0    0     0    56 7479 10960  3  3 84 10  0
 0  1      1   3563    529  10309    0    0     0    15 7415 10979  3  2 85  9  0
 1  0      1   3557    529  10309    0    0     0     1 7456 11444  3  3 85  9  0
 1  1      1   3557    529  10309    0    0     0     1 7536 10896  3  3 86  8  0
 0  0      1   3965    529   9971    0    0     0     0 5060 7204  2  2 90  6  0
 0  0      1   3964    529   9971    0    0     0     0  167  372  0  0 100  0  0
```
</details>

<details>
<summary>Speed: 4.09x (time 2m35s) , preset: veryfast</summary>

```
$ time ffmpeg -y -hwaccel qsv -c:v h264_qsv -i bbb_sunflower_2160p_30fps_normal.mp4 \
> -c:a copy -vf scale_qsv=-1:1080 -c:v h264_qsv -b:v 4000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:720 -c:v h264_qsv -b:v 2000k -preset veryfast -f mp4 /dev/null \
> -c:a copy -vf scale_qsv=-1:480 -c:v h264_qsv -b:v 1000k -preset veryfast -f mp4 /dev/null
ffmpeg version 4.3.1 Copyright (c) 2000-2020 the FFmpeg developers
  built with gcc 9 (Ubuntu 9.3.0-10ubuntu2)
  configuration: --enable-libmfx --enable-nonfree --enable-libx264 --enable-libx265 --enable-gpl
  libavutil      56. 51.100 / 56. 51.100
  libavcodec     58. 91.100 / 58. 91.100
  libavformat    58. 45.100 / 58. 45.100
  libavdevice    58. 10.100 / 58. 10.100
  libavfilter     7. 85.100 /  7. 85.100
  libswscale      5.  7.100 /  5.  7.100
  libswresample   3.  7.100 /  3.  7.100
  libpostproc    55.  7.100 / 55.  7.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'bbb_sunflower_2160p_30fps_normal.mp4':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    creation_time   : 2013-12-18T14:43:04.000000Z
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    composer        : Sacha Goedegebure
  Duration: 00:10:34.53, start: 0.000000, bitrate: 7980 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p, 3840x2160 [SAR 1:1 DAR 16:9], 7498 kb/s, 30 fps, 30 tbr, 30k tbn, 60 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
    Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_qsv) -> h264 (h264_qsv))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x55a59042a0c0] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #0:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1920x1080 [SAR 1:1 DAR 16:9], q=-1--1, 4000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55a590426fc0] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #1:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55a590598a80] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.45.100
    Stream #2:0(und): Video: h264 (h264_qsv) (avc1 / 0x31637661), qsv, 853x480 [SAR 2560:2559 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.91.100 h264_qsv
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[h264_qsv @ 0x55a590434c40] A decode call did not consume any data: expect more data at input (-10)=6 drop=0 speed=4.09x    
    Last message repeated 2 times
frame=19038 fps=123 q=25.0 Lq=25.0 q=25.0 size=  322979kB time=00:10:34.50 bitrate=4170.0kbits/s dup=6 drop=0 speed=4.09x    
video:522905kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	2m35,134s
user	0m44,791s
sys	0m17,985s
```
```
$ vmstat --unit M 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      1   3941    529   9994    0    0    50   112  140   15 32  4 64  0  0
 0  0      1   3965    529   9971    0    0     0    19  249  719  0  0 100  0  0
 1  4      1   3581    529  10309    0    0     0     3 6077 9045  3  3 79 15  0
 0  4      1   3571    529  10308    0    0     0     8 6921 9498  3  2 80 14  0
 1  2      1   3558    529  10309    0    0     0     5 7275 9921  4  3 80 14  0
 0  5      1   3558    529  10305    0    0     0    46 7434 10716  5  3 76 16  0
 0  4      1   3557    529  10305    0    0     0    39 7203 9639  3  3 81 14  0
 0  0      1   3970    529   9967    0    0     0     7 2667 3878  1  2 92  5  0
 0  0      1   3969    529   9967    0    0     0    19  162  286  0  0 100  0  0
```
</details>

### NVIDIA NVDEC/NVENC decoding/encoding on Quadro K2200
The result comes from [here](nvenc.md).
<details>
<summary>Speed: 3.6x (time 2m56s) , preset: medium</summary>

```
$ time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_2160p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:1080 -c:v h264_nvenc -b:v 4000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
...
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x55f2cba9f940] track 1: codec frame size is not set
Output #0, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #0:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 1920x1080 [SAR 1:1 DAR 16:9], q=-1--1, 4000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 8000000 vbv_delay: N/A
    Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55f2cba91b40] track 1: codec frame size is not set
Output #1, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #1:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 2000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/200000 buffer size: 400000 vbv_delay: N/A
    Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
[mp4 @ 0x55f2cbbf3f00] track 1: codec frame size is not set
Output #2, mp4, to '/dev/null':
  Metadata:
    major_brand     : isom
    minor_version   : 1
    compatible_brands: isomavc1
    composer        : Sacha Goedegebure
    title           : Big Buck Bunny, Sunflower version
    artist          : Blender Foundation 2008, Janus Bager Kristensen 2013
    comment         : Creative Commons Attribution 3.0 - http://bbb3d.renderfarming.net
    genre           : Animation
    encoder         : Lavf58.33.100
    Stream #2:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), cuda, 853x480 [SAR 2560:2559 DAR 16:9], q=-1--1, 1000 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      encoder         : Lavc58.58.101 h264_nvenc
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 2000000 vbv_delay: N/A
    Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
    Side data:
      audio service type: main
frame=19036 fps=108 q=24.0 Lq=45.0 q=22.0 size=  337348kB time=00:10:34.73 bitrate=4353.9kbits/s speed= 3.6x    
video:403837kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	2m56.386s
user	0m23.817s
sys	0m4.925s
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     3    45     -     0     0     0     0  2505  1045
    0    22    58     -    65    44    52   100  2505  1124
    0    30    65     -    77    51    60   100  2505  1124
    0    26    68     -    70    48    57   100  2505  1124
    0    26    70     -    73    48    54   100  2505  1124
    0    31    72     -    84    54    59   100  2505  1124
    0     6    68     -     0     0    35    67  2505  1124
    0     2    56     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 62482368  42432 740856    0    0     3    23   26   29 12  1 87  0  0
 0  0      0 62189796  42440 743932    0    0     0     1 1474 3632  1  1 99  0  0
 0  0      0 62186992  42440 743932    0    0     0     0 1728 4170  1  0 99  0  0
 0  0      0 62186312  42440 743932    0    0     0     0 1708 4241  1  0 99  0  0
 0  0      0 62186188  42440 743932    0    0     0     0 1715 4236  1  0 98  0  0
 0  0      0 62180856  42440 743932    0    0     0     0 1716 4286  1  0 98  0  0
 0  0      0 62181236  42440 743932    0    0     0     0 1745 4421  1  0 98  0  0
 0  0      0 62468184  42440 740860    0    0     0     0  474  999  0  0 100  0  0
```
</details>