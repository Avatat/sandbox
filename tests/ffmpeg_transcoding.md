# FFmpeg transcoding benchmarks
I made FFmpeg transcoding benchmarks to learn the basics of FFmpeg and hardware acceleration, and to check how much hardware acceleration gives us.

Currently, only Intel and NVIDIA acceleration was tested by me. I have to get a Radeon GPU to test AMD acceleration.

The reference source for every transcoding is a popular, open-source animation [Big Buck Bunny](http://bbb3d.renderfarming.net/).

## Transcoding 1080p30 (H.264) to 720p, 540p, and 480p (H.264) simultaneously
Sometimes, I had to set 478 px height, because of BBB ratio and `[libx264 @ 0x555bb8838fc0] width not divisible by 2 (853x480)` error.

### Software decoding/encoding on AMD Ryzen 5600G

<details>
<summary>Speed: 3.66x (time 2m53s), preset: medium</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -c:v h264 -i bbb_sunflower_1080p_30fps_normal.mp4 \
-c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:540 -c:v libx264 -b:v 1500k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset medium -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 27211
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
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
[libx264 @ 0x559f26295040] using SAR=1/1
[libx264 @ 0x559f26295040] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x559f26295040] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x559f26295040] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=18 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x559f26293700] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x559f2632e540] using SAR=1/1
[libx264 @ 0x559f2632e540] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x559f2632e540] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x559f2632e540] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=17 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1500 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x559f2632d340] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 960x540 [SAR 1:1 DAR 16:9], q=2-31, 1500 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x559f2627bbc0] using SAR=3824/3825
[libx264 @ 0x559f2627bbc0] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x559f2627bbc0] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x559f2627bbc0] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=15 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x559f26289f00] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 3292 fps=110 q=23.0 q=21.0 q=23.0 size=   28928kB time=00:01:49.88 bitrate=2156.5kbits/s dup=6 drop=0 speed=3.68x    
16:24:31        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:25:01        all     22,83     60,37      0,89      0,01      0,00     15,90

16:24:31      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:25:01           16      1282      5,60      3,99      4,19         0
frame= 6708 fps=113 q=26.0 q=25.0 q=27.0 size=   63232kB time=00:03:43.64 bitrate=2316.1kbits/s dup=6 drop=0 speed=3.75x    
16:25:01        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:25:31        all     21,45     60,41      0,81      0,01      0,00     17,31

16:25:01      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:25:31            9      1285     11,65      5,69      4,75         0
frame=10066 fps=112 q=28.0 q=27.0 q=28.0 size=   97024kB time=00:05:35.48 bitrate=2369.1kbits/s dup=6 drop=0 speed=3.73x    
16:25:31        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:26:01        all     22,02     61,70      0,79      0,03      0,00     15,46

16:25:31      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:26:01           10      1287     15,98      7,31      5,32         0
frame=13276 fps=111 q=29.0 q=28.0 q=30.0 size=  128768kB time=00:07:22.52 bitrate=2383.7kbits/s dup=6 drop=0 speed= 3.7x    
16:26:01        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:26:31        all     22,68     60,77      0,83      0,00      0,00     15,72

16:26:01      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:26:31           11      1285     16,97      8,34      5,73         0
frame=16376 fps=110 q=28.0 q=27.0 q=28.0 size=  156160kB time=00:09:05.72 bitrate=2344.1kbits/s dup=6 drop=0 speed=3.65x    
16:26:31        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:27:01        all     23,43     63,28      0,80      0,00      0,00     12,48

16:26:31      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:27:01            9      1284     19,52      9,72      6,27         0
[h264 @ 0x559f2652eb40] concealing 2738 DC, 2738 AC, 2738 MV errors in P frameate=2395.4kbits/s dup=6 drop=0 speed=3.63x    
[h264 @ 0x559f262803c0] Invalid NAL unit size (1048643 > 67).
[h264 @ 0x559f262803c0] Error splitting the input into NAL units.
[h264 @ 0x559f262fc580] Invalid NAL unit size (8392789 > 85).
[h264 @ 0x559f262fc580] Error splitting the input into NAL units.
[h264 @ 0x559f2640de40] Invalid NAL unit size (166 > 132).
[h264 @ 0x559f2640de40] Error splitting the input into NAL units.
[h264 @ 0x559f2642ab00] Invalid NAL unit size (268501071 > 103).
[h264 @ 0x559f2642ab00] Error splitting the input into NAL units.
[h264 @ 0x559f264478c0] reference overflow 0 > 15 or 35 > 15
[h264 @ 0x559f264478c0] decode_slice_header error
[h264 @ 0x559f264478c0] no frame!
[h264 @ 0x559f26464600] Invalid NAL unit size (-2109734821 > 83).
[h264 @ 0x559f26464600] Error splitting the input into NAL units.
[h264 @ 0x559f26481400] Invalid NAL unit size (671088719 > 79).
[h264 @ 0x559f26481400] Error splitting the input into NAL units.
[h264 @ 0x559f2649e1c0] Invalid NAL unit size (1179712 > 64).
[h264 @ 0x559f2649e1c0] Error splitting the input into NAL units.
[h264 @ 0x559f264bb080] Invalid NAL unit size (16456 > 64).
[h264 @ 0x559f264bb080] Error splitting the input into NAL units.
[h264 @ 0x559f264d7ec0] Invalid NAL unit size (2097493 > 85).
[h264 @ 0x559f264d7ec0] Error splitting the input into NAL units.
[h264 @ 0x559f264f4dc0] Invalid NAL unit size (1048661 > 87).
[h264 @ 0x559f264f4dc0] Error splitting the input into NAL units.
[h264 @ 0x559f26511c80] Invalid NAL unit size (1048671 > 95).
[h264 @ 0x559f26511c80] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 3 times
[h264 @ 0x559f2640de40] reference picture missing during reorder
[h264 @ 0x559f2640de40] Missing reference picture, default is 65686
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 7 times
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x559f26481400] concealing 4138 DC, 4138 AC, 4138 MV errors in B frameate=2365.4kbits/s dup=42 drop=0 speed=3.65x    
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x559f264bb080] Reference 2 >= 2
[h264 @ 0x559f264bb080] error while decoding MB 85 8, bytestream 3028
[h264 @ 0x559f264bb080] concealing 7164 DC, 7164 AC, 7164 MV errors in B frame
[h264 @ 0x559f264d7ec0] Invalid NAL unit size (3244 > 3212).
[h264 @ 0x559f264d7ec0] Error splitting the input into NAL units.
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
Error while decoding stream #0:0: Invalid data found when processing input
[h264 @ 0x559f2652eb40] Reference 5 >= 4 size=  179456kB time=00:10:23.00 bitrate=2359.7kbits/s dup=45 drop=0 speed=3.65x    
[h264 @ 0x559f2652eb40] error while decoding MB 71 55, bytestream 1283
[h264 @ 0x559f2652eb40] concealing 1538 DC, 1538 AC, 1538 MV errors in P frame
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x559f264bb080] concealing 2079 DC, 2079 AC, 2079 MV errors in P frame
[h264 @ 0x559f264d7ec0] non-existing PPS 5 referenced
[h264 @ 0x559f264d7ec0] decode_slice_header error
[h264 @ 0x559f264d7ec0] no frame!
[h264 @ 0x559f264f4dc0] Invalid NAL unit size (16778990 > 750).
[h264 @ 0x559f264f4dc0] Error splitting the input into NAL units.
[h264 @ 0x559f26511c80] Invalid NAL unit size (134283534 > 782).
[h264 @ 0x559f26511c80] Error splitting the input into NAL units.
[h264 @ 0x559f2652eb40] Invalid NAL unit size (1098908655 > 871).
[h264 @ 0x559f2652eb40] Error splitting the input into NAL units.
[h264 @ 0x559f262803c0] Invalid NAL unit size (49737 > 589).
[h264 @ 0x559f262803c0] Error splitting the input into NAL units.
[h264 @ 0x559f262fc580] Invalid NAL unit size (1224744219 > 5435).
[h264 @ 0x559f262fc580] Error splitting the input into NAL units.
[h264 @ 0x559f2640de40] Invalid NAL unit size (100861015 > 1111).
[h264 @ 0x559f2640de40] Error splitting the input into NAL units.
[h264 @ 0x559f2642ab00] Invalid NAL unit size (1073742484 > 661).
[h264 @ 0x559f2642ab00] Error splitting the input into NAL units.
[h264 @ 0x559f264478c0] Invalid NAL unit size (-2145385656 > 841).
[h264 @ 0x559f264478c0] Error splitting the input into NAL units.
[h264 @ 0x559f26464600] co located POCs unavailable
[h264 @ 0x559f264bb080] mmco: unref short failure
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 8 times
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
frame=19038 fps=110 q=-1.0 Lq=-1.0 q=-1.0 size=  181729kB time=00:10:34.50 bitrate=2346.3kbits/s dup=72 drop=0 speed=3.66x    
video:351446kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
[libx264 @ 0x559f26295040] frame I:168   Avg QP:15.00  size:151797
[libx264 @ 0x559f26295040] frame P:6228  Avg QP:20.19  size: 15691
[libx264 @ 0x559f26295040] frame B:12642 Avg QP:24.79  size:  2929
[libx264 @ 0x559f26295040] consecutive B-frames:  5.1% 16.9%  6.5% 71.5%
[libx264 @ 0x559f26295040] mb I  I16..4:  8.9% 63.6% 27.5%
[libx264 @ 0x559f26295040] mb P  I16..4:  1.1%  5.1%  0.9%  P16..4: 26.6% 10.7%  7.3%  0.0%  0.0%    skip:48.2%
[libx264 @ 0x559f26295040] mb B  I16..4:  0.1%  0.6%  0.2%  B16..8: 21.7%  2.2%  0.5%  direct: 1.0%  skip:73.7%  L0:40.6% L1:53.3% BI: 6.1%
[libx264 @ 0x559f26295040] final ratefactor: 20.82
[libx264 @ 0x559f26295040] 8x8 transform intra:69.5% inter:66.2%
[libx264 @ 0x559f26295040] coded y,uvDC,uvAC intra: 64.1% 68.0% 42.6% inter: 8.6% 7.9% 1.2%
[libx264 @ 0x559f26295040] i16 v,h,dc,p: 32% 25%  6% 38%
[libx264 @ 0x559f26295040] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 16% 17%  5%  8% 11%  8%  7%  7%
[libx264 @ 0x559f26295040] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 22% 17% 12%  6% 11% 11%  9%  7%  6%
[libx264 @ 0x559f26295040] i8c dc,h,v,p: 51% 22% 17% 10%
[libx264 @ 0x559f26295040] Weighted P-Frames: Y:2.7% UV:1.7%
[libx264 @ 0x559f26295040] ref P L0: 63.0% 16.1% 15.6%  5.2%  0.0%
[libx264 @ 0x559f26295040] ref B L0: 87.6%  9.7%  2.8%
[libx264 @ 0x559f26295040] ref B L1: 95.9%  4.1%
[libx264 @ 0x559f26295040] kb/s:2020.17
[libx264 @ 0x559f2632e540] frame I:168   Avg QP:14.12  size:110491
[libx264 @ 0x559f2632e540] frame P:6154  Avg QP:19.18  size: 11616
[libx264 @ 0x559f2632e540] frame B:12716 Avg QP:24.35  size:  2342
[libx264 @ 0x559f2632e540] consecutive B-frames:  6.6%  9.8%  9.2% 74.3%
[libx264 @ 0x559f2632e540] mb I  I16..4:  8.7% 49.6% 41.7%
[libx264 @ 0x559f2632e540] mb P  I16..4:  0.9%  4.3%  1.3%  P16..4: 24.3% 12.3%  9.9%  0.0%  0.0%    skip:47.0%
[libx264 @ 0x559f2632e540] mb B  I16..4:  0.1%  0.6%  0.2%  B16..8: 21.5%  3.3%  1.1%  direct: 1.8%  skip:71.5%  L0:40.5% L1:50.6% BI: 8.9%
[libx264 @ 0x559f2632e540] final ratefactor: 19.62
[libx264 @ 0x559f2632e540] 8x8 transform intra:61.3% inter:62.4%
[libx264 @ 0x559f2632e540] coded y,uvDC,uvAC intra: 69.7% 74.0% 50.5% inter: 10.3% 9.0% 1.9%
[libx264 @ 0x559f2632e540] i16 v,h,dc,p: 34% 29%  9% 29%
[libx264 @ 0x559f2632e540] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 20% 17% 17%  5%  7% 10%  7%  7%  8%
[libx264 @ 0x559f2632e540] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 16% 11%  6% 10% 10%  9%  8%  7%
[libx264 @ 0x559f2632e540] i8c dc,h,v,p: 52% 22% 17%  9%
[libx264 @ 0x559f2632e540] Weighted P-Frames: Y:4.8% UV:2.4%
[libx264 @ 0x559f2632e540] ref P L0: 61.8% 15.3% 14.5%  8.1%  0.3%
[libx264 @ 0x559f2632e540] ref B L0: 82.0% 14.2%  3.9%
[libx264 @ 0x559f2632e540] ref B L1: 91.6%  8.4%
[libx264 @ 0x559f2632e540] kb/s:1510.58
[libx264 @ 0x559f2627bbc0] frame I:169   Avg QP:16.07  size: 78110
[libx264 @ 0x559f2627bbc0] frame P:6139  Avg QP:21.09  size:  7548
[libx264 @ 0x559f2627bbc0] frame B:12730 Avg QP:26.29  size:  1592
[libx264 @ 0x559f2627bbc0] consecutive B-frames:  6.9%  8.2% 10.7% 74.2%
[libx264 @ 0x559f2627bbc0] mb I  I16..4:  8.6% 58.4% 33.0%
[libx264 @ 0x559f2627bbc0] mb P  I16..4:  0.8%  3.8%  1.1%  P16..4: 25.0% 10.5%  7.6%  0.0%  0.0%    skip:51.2%
[libx264 @ 0x559f2627bbc0] mb B  I16..4:  0.1%  0.5%  0.2%  B16..8: 22.3%  2.7%  0.8%  direct: 1.5%  skip:72.0%  L0:41.2% L1:51.6% BI: 7.2%
[libx264 @ 0x559f2627bbc0] final ratefactor: 21.37
[libx264 @ 0x559f2627bbc0] 8x8 transform intra:63.7% inter:63.5%
[libx264 @ 0x559f2627bbc0] coded y,uvDC,uvAC intra: 68.6% 72.8% 49.7% inter: 8.8% 7.6% 1.5%
[libx264 @ 0x559f2627bbc0] i16 v,h,dc,p: 31% 30%  7% 32%
[libx264 @ 0x559f2627bbc0] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 18% 18% 17%  5%  8% 11%  8%  8%  8%
[libx264 @ 0x559f2627bbc0] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 18% 20% 12%  6% 10% 10%  9%  7%  7%
[libx264 @ 0x559f2627bbc0] i8c dc,h,v,p: 50% 24% 16% 10%
[libx264 @ 0x559f2627bbc0] Weighted P-Frames: Y:3.6% UV:2.0%
[libx264 @ 0x559f2627bbc0] ref P L0: 62.9% 16.5% 12.0%  8.3%  0.3%
[libx264 @ 0x559f2627bbc0] ref B L0: 79.0% 15.2%  5.8%
[libx264 @ 0x559f2627bbc0] ref B L1: 90.9%  9.1%
[libx264 @ 0x559f2627bbc0] kb/s:1006.00

real	2m53,453s
user	28m55,867s
sys	0m11,633s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all     22,48     61,31      0,82      0,01      0,00     15,37

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:           11      1285     13,94      7,01      5,25         0
```
</details>

<details>
<summary>Speed: 5.23x (time 2m1s), preset: veryfast</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -c:v h264 -i bbb_sunflower_1080p_30fps_normal.mp4 \
-c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale=-1:540 -c:v libx264 -b:v 1500k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset veryfast -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 28180
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
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
[libx264 @ 0x5653e2186040] using SAR=1/1
[libx264 @ 0x5653e2186040] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5653e2186040] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x5653e2186040] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=18 lookahead_threads=5 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5653e2184700] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x5653e221f540] using SAR=1/1
[libx264 @ 0x5653e221f540] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5653e221f540] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x5653e221f540] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=17 lookahead_threads=4 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1500 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5653e221e340] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 960x540 [SAR 1:1 DAR 16:9], q=2-31, 1500 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 0 vbv_delay: N/A
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x5653e216cbc0] using SAR=3824/3825
[libx264 @ 0x5653e216cbc0] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5653e216cbc0] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x5653e216cbc0] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=15 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5653e217af00] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 4725 fps=159 q=23.0 q=22.0 q=24.0 size=   43776kB time=00:02:37.40 bitrate=2278.2kbits/s dup=6 drop=0 speed= 5.3x    
16:29:11        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:29:41        all     28,30     21,57      1,07      0,03      0,00     49,03

16:29:11      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:29:41            6      1303      7,63      8,30      6,29         0
frame= 9542 fps=159 q=26.0 q=24.0 q=26.0 size=   91136kB time=00:05:18.20 bitrate=2346.2kbits/s dup=6 drop=0 speed=5.31x    
16:29:41        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:30:11        all     27,33     21,67      1,12      0,05      0,00     49,83

16:29:41      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:30:11           10      1297      6,77      8,02      6,26         0
frame=14170 fps=158 q=28.0 q=27.0 q=29.0 size=  137216kB time=00:07:52.28 bitrate=2380.1kbits/s dup=6 drop=0 speed=5.27x    
16:30:11        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:30:41        all     29,70     23,06      1,04      0,00      0,00     46,19

16:30:11      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:30:41           25      1298      8,34      8,25      6,39         0
[h264 @ 0x5653e241fb40] concealing 2738 DC, 2738 AC, 2738 MV errors in P frameate=2399.2kbits/s dup=6 drop=0 speed=5.21x    
[h264 @ 0x5653e21713c0] Invalid NAL unit size (1048643 > 67).
[h264 @ 0x5653e21713c0] Error splitting the input into NAL units.
[h264 @ 0x5653e21ed580] Invalid NAL unit size (8392789 > 85).
[h264 @ 0x5653e21ed580] Error splitting the input into NAL units.
[h264 @ 0x5653e22fee40] Invalid NAL unit size (166 > 132).
[h264 @ 0x5653e22fee40] Error splitting the input into NAL units.
[h264 @ 0x5653e231bb00] Invalid NAL unit size (268501071 > 103).
[h264 @ 0x5653e231bb00] Error splitting the input into NAL units.
[h264 @ 0x5653e23388c0] reference overflow 0 > 15 or 35 > 15
[h264 @ 0x5653e23388c0] decode_slice_header error
[h264 @ 0x5653e23388c0] no frame!
[h264 @ 0x5653e2355600] Invalid NAL unit size (-2109734821 > 83).
[h264 @ 0x5653e2355600] Error splitting the input into NAL units.
[h264 @ 0x5653e2372400] Invalid NAL unit size (671088719 > 79).
[h264 @ 0x5653e2372400] Error splitting the input into NAL units.
[h264 @ 0x5653e238f1c0] Invalid NAL unit size (1179712 > 64).
[h264 @ 0x5653e238f1c0] Error splitting the input into NAL units.
[h264 @ 0x5653e23ac080] Invalid NAL unit size (16456 > 64).
[h264 @ 0x5653e23ac080] Error splitting the input into NAL units.
[h264 @ 0x5653e23c8ec0] Invalid NAL unit size (2097493 > 85).
[h264 @ 0x5653e23c8ec0] Error splitting the input into NAL units.
[h264 @ 0x5653e23e5dc0] Invalid NAL unit size (1048661 > 87).
[h264 @ 0x5653e23e5dc0] Error splitting the input into NAL units.
[h264 @ 0x5653e2402c80] Invalid NAL unit size (1048671 > 95).
[h264 @ 0x5653e2402c80] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 3 times
[h264 @ 0x5653e22fee40] reference picture missing during reorder
[h264 @ 0x5653e22fee40] Missing reference picture, default is 65686
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 7 times
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x5653e2372400] concealing 4138 DC, 4138 AC, 4138 MV errors in B frameate=2365.4kbits/s dup=42 drop=0 speed=5.22x    
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x5653e23ac080] Reference 2 >= 2
[h264 @ 0x5653e23ac080] error while decoding MB 85 8, bytestream 3028
[h264 @ 0x5653e23ac080] concealing 7164 DC, 7164 AC, 7164 MV errors in B frame
[h264 @ 0x5653e23c8ec0] Invalid NAL unit size (3244 > 3212).
[h264 @ 0x5653e23c8ec0] Error splitting the input into NAL units.
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
Error while decoding stream #0:0: Invalid data found when processing input
[h264 @ 0x5653e241fb40] Reference 5 >= 4 size=  179712kB time=00:10:23.48 bitrate=2361.2kbits/s dup=45 drop=0 speed=5.23x    
[h264 @ 0x5653e241fb40] error while decoding MB 71 55, bytestream 1283
[h264 @ 0x5653e241fb40] concealing 1538 DC, 1538 AC, 1538 MV errors in P frame
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
[h264 @ 0x5653e23ac080] concealing 2079 DC, 2079 AC, 2079 MV errors in P frame
[h264 @ 0x5653e23c8ec0] non-existing PPS 5 referenced
[h264 @ 0x5653e23c8ec0] decode_slice_header error
[h264 @ 0x5653e23c8ec0] no frame!
[h264 @ 0x5653e23e5dc0] Invalid NAL unit size (16778990 > 750).
[h264 @ 0x5653e23e5dc0] Error splitting the input into NAL units.
[h264 @ 0x5653e2402c80] Invalid NAL unit size (134283534 > 782).
[h264 @ 0x5653e2402c80] Error splitting the input into NAL units.
[h264 @ 0x5653e241fb40] Invalid NAL unit size (1098908655 > 871).
[h264 @ 0x5653e241fb40] Error splitting the input into NAL units.
[h264 @ 0x5653e21713c0] Invalid NAL unit size (49737 > 589).
[h264 @ 0x5653e21713c0] Error splitting the input into NAL units.
[h264 @ 0x5653e21ed580] Invalid NAL unit size (1224744219 > 5435).
[h264 @ 0x5653e21ed580] Error splitting the input into NAL units.
[h264 @ 0x5653e22fee40] Invalid NAL unit size (100861015 > 1111).
[h264 @ 0x5653e22fee40] Error splitting the input into NAL units.
[h264 @ 0x5653e231bb00] Invalid NAL unit size (1073742484 > 661).
[h264 @ 0x5653e231bb00] Error splitting the input into NAL units.
[h264 @ 0x5653e23388c0] Invalid NAL unit size (-2145385656 > 841).
[h264 @ 0x5653e23388c0] Error splitting the input into NAL units.
[h264 @ 0x5653e2355600] co located POCs unavailable
[h264 @ 0x5653e23ac080] mmco: unref short failure
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 8 times
bbb_sunflower_1080p_30fps_normal.mp4: corrupt decoded frame in stream 0
frame=18794 fps=157 q=27.0 q=27.0 q=28.0 size=  179968kB time=00:10:26.36 bitrate=2353.7kbits/s dup=72 drop=0 speed=5.23x    
16:30:41        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:31:11        all     31,63     22,32      1,02      0,03      0,00     45,01

16:30:41      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:31:11            1      1304      7,40      8,03      6,38         0
frame=19038 fps=157 q=-1.0 Lq=-1.0 q=-1.0 size=  181795kB time=00:10:34.50 bitrate=2347.1kbits/s dup=72 drop=0 speed=5.23x    
video:352296kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
[libx264 @ 0x5653e2186040] frame I:167   Avg QP:15.13  size:139392
[libx264 @ 0x5653e2186040] frame P:6380  Avg QP:20.03  size: 15907
[libx264 @ 0x5653e2186040] frame B:12491 Avg QP:23.11  size:  2847
[libx264 @ 0x5653e2186040] consecutive B-frames:  5.5% 18.6%  7.0% 68.8%
[libx264 @ 0x5653e2186040] mb I  I16..4: 16.8% 17.2% 66.0%
[libx264 @ 0x5653e2186040] mb P  I16..4:  3.4%  5.5%  2.2%  P16..4: 26.3%  8.6%  5.1%  0.0%  0.0%    skip:48.9%
[libx264 @ 0x5653e2186040] mb B  I16..4:  0.4%  0.8%  0.3%  B16..8:  7.0%  2.7%  0.5%  direct: 2.8%  skip:85.6%  L0:34.6% L1:45.6% BI:19.9%
[libx264 @ 0x5653e2186040] final ratefactor: 19.57
[libx264 @ 0x5653e2186040] 8x8 transform intra:45.4% inter:32.3%
[libx264 @ 0x5653e2186040] coded y,uvDC,uvAC intra: 63.3% 58.8% 30.7% inter: 7.1% 6.1% 0.8%
[libx264 @ 0x5653e2186040] i16 v,h,dc,p: 50% 23% 13% 13%
[libx264 @ 0x5653e2186040] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 22% 18% 17%  5%  7% 14%  7%  6%  6%
[libx264 @ 0x5653e2186040] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 17% 11%  7% 10% 11%  8%  9%  8%
[libx264 @ 0x5653e2186040] i8c dc,h,v,p: 51% 21% 18%  9%
[libx264 @ 0x5653e2186040] Weighted P-Frames: Y:2.4% UV:1.5%
[libx264 @ 0x5653e2186040] kb/s:2021.04
[libx264 @ 0x5653e221f540] frame I:168   Avg QP:14.44  size: 99226
[libx264 @ 0x5653e221f540] frame P:6264  Avg QP:19.40  size: 11660
[libx264 @ 0x5653e221f540] frame B:12606 Avg QP:23.16  size:  2445
[libx264 @ 0x5653e221f540] consecutive B-frames:  6.9% 10.8% 10.5% 71.8%
[libx264 @ 0x5653e221f540] mb I  I16..4: 14.1% 12.7% 73.3%
[libx264 @ 0x5653e221f540] mb P  I16..4:  2.4%  4.2%  2.7%  P16..4: 27.6%  9.0%  6.8%  0.0%  0.0%    skip:47.3%
[libx264 @ 0x5653e221f540] mb B  I16..4:  0.4%  0.6%  0.3%  B16..8:  7.6%  3.9%  1.0%  direct: 4.7%  skip:81.5%  L0:33.0% L1:42.4% BI:24.5%
[libx264 @ 0x5653e221f540] final ratefactor: 18.68
[libx264 @ 0x5653e221f540] 8x8 transform intra:39.1% inter:31.0%
[libx264 @ 0x5653e221f540] coded y,uvDC,uvAC intra: 67.6% 64.8% 37.9% inter: 9.4% 7.5% 1.4%
[libx264 @ 0x5653e221f540] i16 v,h,dc,p: 49% 26% 14% 11%
[libx264 @ 0x5653e221f540] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 22% 19% 19%  4%  6% 13%  6%  6%  6%
[libx264 @ 0x5653e221f540] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 17% 10%  7% 10% 11%  9%  9%  8%
[libx264 @ 0x5653e221f540] i8c dc,h,v,p: 52% 22% 18%  8%
[libx264 @ 0x5653e221f540] Weighted P-Frames: Y:3.4% UV:1.7%
[libx264 @ 0x5653e221f540] kb/s:1519.50
[libx264 @ 0x5653e216cbc0] frame I:169   Avg QP:16.29  size: 70758
[libx264 @ 0x5653e216cbc0] frame P:6190  Avg QP:21.22  size:  7619
[libx264 @ 0x5653e216cbc0] frame B:12679 Avg QP:25.07  size:  1639
[libx264 @ 0x5653e216cbc0] consecutive B-frames:  7.2%  8.3% 11.4% 73.1%
[libx264 @ 0x5653e216cbc0] mb I  I16..4: 15.2% 16.8% 68.0%
[libx264 @ 0x5653e216cbc0] mb P  I16..4:  2.4%  4.1%  2.2%  P16..4: 25.6%  8.4%  5.5%  0.0%  0.0%    skip:51.7%
[libx264 @ 0x5653e216cbc0] mb B  I16..4:  0.3%  0.6%  0.3%  B16..8:  7.0%  3.2%  0.8%  direct: 3.9%  skip:84.0%  L0:33.5% L1:43.6% BI:22.9%
[libx264 @ 0x5653e216cbc0] final ratefactor: 20.19
[libx264 @ 0x5653e216cbc0] 8x8 transform intra:41.5% inter:30.5%
[libx264 @ 0x5653e216cbc0] coded y,uvDC,uvAC intra: 66.5% 63.2% 36.8% inter: 8.0% 5.8% 1.1%
[libx264 @ 0x5653e216cbc0] i16 v,h,dc,p: 47% 27% 14% 13%
[libx264 @ 0x5653e216cbc0] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 19% 21% 17%  5%  7% 13%  6%  6%  6%
[libx264 @ 0x5653e216cbc0] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 19% 18% 11%  7%  9% 10%  9%  8%  8%
[libx264 @ 0x5653e216cbc0] i8c dc,h,v,p: 50% 23% 18%  9%
[libx264 @ 0x5653e216cbc0] Weighted P-Frames: Y:2.9% UV:1.6%
[libx264 @ 0x5653e216cbc0] kb/s:1007.20

real	2m1,382s
user	12m28,275s
sys	0m12,103s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all     29,24     22,15      1,06      0,03      0,00     47,52

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:           10      1300      7,54      8,15      6,33         0
```
</details>

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
<summary>Speed: 2.75x (time 3m51s), preset: veryfast</summary>

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

### VAAPI decoding/encoding on AMD Ryzen 5600G

<details>
<summary>Speed: 4.04x (time 2m37s), preset: medium</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -hwaccel vaapi -hwaccel_output_format vaapi -hwaccel_device /dev/dri/renderD128 -i bbb_sunflower_1080p_30fps_normal.mp4 \
-c:a copy -vf scale_vaapi=w=-1:h=720 -c:v h264_vaapi -b:v 2000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=540 -c:v h264_vaapi -b:v 1500k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=478 -c:v h264_vaapi -b:v 1000k -preset medium -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 30405
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[h264_vaapi @ 0x5595995a4500] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5595995a4500] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5595995a4500] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5595995b9c00] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x5595995bb780] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5595995bb780] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5595995bb780] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5595995a3500] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 960x540 [SAR 1:1 DAR 16:9], q=2-31, 1500 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x5595995a78c0] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5595995a78c0] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5595995a78c0] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5595995a63c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 3932 fps=132 q=-0.0 q=-0.0 q=-0.0 size=   36864kB time=00:02:11.03 bitrate=2304.7kbits/s dup=6 drop=0 speed= 4.4x    
16:48:11        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:48:41        all      0,53      0,00      3,48      0,13      0,00     95,86

16:48:11      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:48:41            0      1243      1,17      4,65      5,88         0
frame= 7851 fps=132 q=-0.0 q=-0.0 q=-0.0 size=   73984kB time=00:04:21.66 bitrate=2316.2kbits/s dup=6 drop=0 speed= 4.4x    
16:48:41        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:49:11        all      0,41      0,00      3,55      0,04      0,00     96,00

16:48:41      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:49:11            0      1240      0,91      4,25      5,70         0
frame=11896 fps=133 q=-0.0 q=-0.0 q=-0.0 size=  112128kB time=00:06:36.50 bitrate=2316.7kbits/s dup=6 drop=0 speed=4.42x    
16:49:11        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:49:41        all      0,43      0,00      3,52      0,06      0,00     96,00

16:49:11      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:49:41            0      1243      0,55      3,85      5,52         0
frame=15917 fps=133 q=-0.0 q=-0.0 q=-0.0 size=  150528kB time=00:08:50.53 bitrate=2324.3kbits/s dup=6 drop=0 speed=4.42x    
16:49:41        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:50:11        all      0,43      0,00      3,60      0,07      0,00     95,91

16:49:41      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:50:11            1      1243      0,60      3,54      5,37         0
frame=18224 fps=122 q=-0.0 q=-0.0 q=-0.0 size=  187648kB time=00:10:07.64 bitrate=2529.8kbits/s dup=6 drop=0 speed=4.06x    
16:50:11        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:50:41        all      0,41      0,00      2,59      0,03      0,00     96,96

16:50:11      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:50:41            0      1242      0,50      3,23      5,20         0
[h264 @ 0x559599865740] Invalid NAL unit size (1048643 > 67).=00:10:12.92 bitrate=2525.1kbits/s dup=6 drop=0 speed=4.04x    
[h264 @ 0x559599865740] Error splitting the input into NAL units.
[h264 @ 0x559599830280] Invalid NAL unit size (8392789 > 85).
[h264 @ 0x559599830280] Error splitting the input into NAL units.
[h264 @ 0x55959984d180] Invalid NAL unit size (166 > 132).
[h264 @ 0x55959984d180] Error splitting the input into NAL units.
[h264 @ 0x5595998c4d80] Invalid NAL unit size (268501071 > 103).
[h264 @ 0x5595998c4d80] Error splitting the input into NAL units.
[h264 @ 0x5595998e1b40] reference overflow 0 > 15 or 35 > 15
[h264 @ 0x5595998e1b40] decode_slice_header error
[h264 @ 0x5595998e1b40] no frame!
[h264 @ 0x5595998fe900] Invalid NAL unit size (-2109734821 > 83).
[h264 @ 0x5595998fe900] Error splitting the input into NAL units.
[h264 @ 0x55959991b800] Invalid NAL unit size (671088719 > 79).
[h264 @ 0x55959991b800] Error splitting the input into NAL units.
[h264 @ 0x559599938700] Invalid NAL unit size (1179712 > 64).
[h264 @ 0x559599938700] Error splitting the input into NAL units.
[h264 @ 0x559599955600] Invalid NAL unit size (16456 > 64).
[h264 @ 0x559599955600] Error splitting the input into NAL units.
[h264 @ 0x559599972500] Invalid NAL unit size (2097493 > 85).
[h264 @ 0x559599972500] Error splitting the input into NAL units.
[h264 @ 0x55959998f3c0] Invalid NAL unit size (1048661 > 87).
[h264 @ 0x55959998f3c0] Error splitting the input into NAL units.
[h264 @ 0x5595999ac2c0] Invalid NAL unit size (1048671 > 95).
[h264 @ 0x5595999ac2c0] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 3 times
[h264 @ 0x55959984d180] reference picture missing during reorder
[h264 @ 0x55959984d180] Missing reference picture, default is 65686
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 7 times
[h264 @ 0x559599972500] Invalid NAL unit size (3244 > 3212).e=00:10:19.80 bitrate=2520.8kbits/s dup=42 drop=0 speed=4.03x    
[h264 @ 0x559599972500] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing inputbitrate=2518.4kbits/s dup=42 drop=0 speed=4.03x    
[h264 @ 0x559599972500] non-existing PPS 5 referenced4kB time=00:10:24.44 bitrate=2515.4kbits/s dup=45 drop=0 speed=4.03x    
[h264 @ 0x559599972500] decode_slice_header error
[h264 @ 0x559599972500] no frame!
[h264 @ 0x55959998f3c0] Invalid NAL unit size (16778990 > 750).
[h264 @ 0x55959998f3c0] Error splitting the input into NAL units.
[h264 @ 0x5595999ac2c0] Invalid NAL unit size (134283534 > 782).
[h264 @ 0x5595999ac2c0] Error splitting the input into NAL units.
[h264 @ 0x5595999c91c0] Invalid NAL unit size (1098908655 > 871).
[h264 @ 0x5595999c91c0] Error splitting the input into NAL units.
[h264 @ 0x559599865740] Invalid NAL unit size (49737 > 589).
[h264 @ 0x559599865740] Error splitting the input into NAL units.
[h264 @ 0x559599830280] Invalid NAL unit size (1224744219 > 5435).
[h264 @ 0x559599830280] Error splitting the input into NAL units.
[h264 @ 0x55959984d180] Invalid NAL unit size (100861015 > 1111).
[h264 @ 0x55959984d180] Error splitting the input into NAL units.
[h264 @ 0x5595998c4d80] Invalid NAL unit size (1073742484 > 661).
[h264 @ 0x5595998c4d80] Error splitting the input into NAL units.
[h264 @ 0x5595998e1b40] Invalid NAL unit size (-2145385656 > 841).
[h264 @ 0x5595998e1b40] Error splitting the input into NAL units.
[h264 @ 0x5595998fe900] co located POCs unavailable
[h264 @ 0x559599955600] mmco: unref short failure
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 8 times
frame=19038 fps=121 q=-0.0 Lq=-0.0 q=-0.0 size=  194994kB time=00:10:34.56 bitrate=2517.3kbits/s dup=72 drop=0 speed=4.04x    
video:374987kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	2m37,330s
user	0m6,630s
sys	0m5,108s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all      0,44      0,00      3,35      0,07      0,00     96,15

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:            0      1242      0,75      3,90      5,53         0
```
</details>

<details>
<summary>Speed: 3.97x (time 2m40s), preset: veryfast</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -hwaccel vaapi -hwaccel_output_format vaapi -hwaccel_device /dev/dri/renderD128 -i bbb_sunflower_1080p_30fps_normal.mp4 \
-c:a copy -vf scale_vaapi=w=-1:h=720 -c:v h264_vaapi -b:v 2000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=540 -c:v h264_vaapi -b:v 1500k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=478 -c:v h264_vaapi -b:v 1000k -preset veryfast -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 32346
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[h264_vaapi @ 0x5616c1b29500] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5616c1b29500] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5616c1b29500] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5616c1b3ec00] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x5616c1b40780] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5616c1b40780] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5616c1b40780] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5616c1b28500] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 960x540 [SAR 1:1 DAR 16:9], q=2-31, 1500 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x5616c1b2c8c0] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x5616c1b2c8c0] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x5616c1b2c8c0] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x5616c1b2b3c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:39.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-16T17:44:42.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 3926 fps=132 q=-0.0 q=-0.0 q=-0.0 size=   36864kB time=00:02:11.00 bitrate=2305.1kbits/s dup=6 drop=0 speed= 4.4x    
16:52:24        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:52:54        all      0,47      0,00      3,51      0,18      0,00     95,83

16:52:24      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:52:54            0      1253      0,68      2,30      4,59         0
frame= 7834 fps=132 q=-0.0 q=-0.0 q=-0.0 size=   73728kB time=00:04:21.10 bitrate=2313.2kbits/s dup=6 drop=0 speed=4.39x    
16:52:54        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:53:24        all      0,47      0,00      3,57      0,27      0,00     95,69

16:52:54      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:53:24            0      1252      0,76      2,16      4,47         0
frame=11883 fps=133 q=-0.0 q=-0.0 q=-0.0 size=  112128kB time=00:06:36.06 bitrate=2319.2kbits/s dup=6 drop=0 speed=4.42x    
16:53:24        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:53:54        all      0,45      0,00      3,46      0,04      0,00     96,05

16:53:24      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:53:54            1      1251      0,71      2,01      4,35         0
frame=15897 fps=133 q=-0.0 q=-0.0 q=-0.0 size=  150272kB time=00:08:49.88 bitrate=2323.2kbits/s dup=6 drop=0 speed=4.42x    
16:53:54        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:54:24        all      0,44      0,00      3,56      0,04      0,00     95,96

16:53:54      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:54:24            0      1251      0,64      1,87      4,23         0
frame=18217 fps=122 q=-0.0 q=-0.0 q=-0.0 size=  187392kB time=00:10:07.20 bitrate=2528.2kbits/s dup=6 drop=0 speed=4.05x    
16:54:24        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:54:54        all      0,37      0,00      2,58      0,03      0,00     97,01

16:54:24      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:54:54            0      1246      0,53      1,72      4,10         0
[h264 @ 0x5616c1db7480] Invalid NAL unit size (1048643 > 67).=00:10:12.56 bitrate=2526.6kbits/s dup=6 drop=0 speed=4.03x    
[h264 @ 0x5616c1db7480] Error splitting the input into NAL units.
[h264 @ 0x5616c1e0fa80] Invalid NAL unit size (8392789 > 85).
[h264 @ 0x5616c1e0fa80] Error splitting the input into NAL units.
[h264 @ 0x5616c1e2c9c0] Invalid NAL unit size (166 > 132).
[h264 @ 0x5616c1e2c9c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1e498c0] Invalid NAL unit size (268501071 > 103).
[h264 @ 0x5616c1e498c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1e667c0] reference overflow 0 > 15 or 35 > 15
[h264 @ 0x5616c1e667c0] decode_slice_header error
[h264 @ 0x5616c1e667c0] no frame!
[h264 @ 0x5616c1e836c0] Invalid NAL unit size (-2109734821 > 83).
[h264 @ 0x5616c1e836c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1ea05c0] Invalid NAL unit size (671088719 > 79).
[h264 @ 0x5616c1ea05c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1ebd4c0] Invalid NAL unit size (1179712 > 64).
[h264 @ 0x5616c1ebd4c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1eda380] Invalid NAL unit size (16456 > 64).
[h264 @ 0x5616c1eda380] Error splitting the input into NAL units.
[h264 @ 0x5616c1ef7280] Invalid NAL unit size (2097493 > 85).
[h264 @ 0x5616c1ef7280] Error splitting the input into NAL units.
[h264 @ 0x5616c1f14180] Invalid NAL unit size (1048661 > 87).
[h264 @ 0x5616c1f14180] Error splitting the input into NAL units.
[h264 @ 0x5616c1f31080] Invalid NAL unit size (1048671 > 95).
[h264 @ 0x5616c1f31080] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 3 times
[h264 @ 0x5616c1e2c9c0] reference picture missing during reorder
[h264 @ 0x5616c1e2c9c0] Missing reference picture, default is 65686
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 7 times
[h264 @ 0x5616c1ef7280] Invalid NAL unit size (3244 > 3212).e=00:10:21.08 bitrate=2518.9kbits/s dup=42 drop=0 speed=4.01x    
[h264 @ 0x5616c1ef7280] Error splitting the input into NAL units.
Error while decoding stream #0:0: Invalid data found when processing input
[h264 @ 0x5616c1ef7280] non-existing PPS 5 referenced4kB time=00:10:23.60 bitrate=2518.9kbits/s dup=45 drop=0 speed=   4x    
[h264 @ 0x5616c1ef7280] decode_slice_header error
[h264 @ 0x5616c1ef7280] no frame!
[h264 @ 0x5616c1f14180] Invalid NAL unit size (16778990 > 750).
[h264 @ 0x5616c1f14180] Error splitting the input into NAL units.
[h264 @ 0x5616c1f31080] Invalid NAL unit size (134283534 > 782).
[h264 @ 0x5616c1f31080] Error splitting the input into NAL units.
[h264 @ 0x5616c1f4dec0] Invalid NAL unit size (1098908655 > 871).
[h264 @ 0x5616c1f4dec0] Error splitting the input into NAL units.
[h264 @ 0x5616c1db7480] Invalid NAL unit size (49737 > 589).
[h264 @ 0x5616c1db7480] Error splitting the input into NAL units.
[h264 @ 0x5616c1e0fa80] Invalid NAL unit size (1224744219 > 5435).
[h264 @ 0x5616c1e0fa80] Error splitting the input into NAL units.
[h264 @ 0x5616c1e2c9c0] Invalid NAL unit size (100861015 > 1111).
[h264 @ 0x5616c1e2c9c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1e498c0] Invalid NAL unit size (1073742484 > 661).
[h264 @ 0x5616c1e498c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1e667c0] Invalid NAL unit size (-2145385656 > 841).
[h264 @ 0x5616c1e667c0] Error splitting the input into NAL units.
[h264 @ 0x5616c1e836c0] co located POCs unavailable
[h264 @ 0x5616c1eda380] mmco: unref short failure
Error while decoding stream #0:0: Invalid data found when processing input
    Last message repeated 8 times
frame=19038 fps=119 q=-0.0 Lq=-0.0 q=-0.0 size=  195004kB time=00:10:34.56 bitrate=2517.4kbits/s dup=72 drop=0 speed=3.97x    
video:375005kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	2m40,038s
user	0m6,691s
sys	0m5,173s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all      0,44      0,00      3,34      0,11      0,00     96,11

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:            0      1251      0,66      2,01      4,35         0
```
</details>

### Intel Quick Sync Video decoding/encoding on Intel i5-8250U (UHD 620)
<details>
<summary>Speed: 5.91x (time 1m47s), preset: medium</summary>

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
<summary>Speed: 7.65x (time 1m23s), preset: veryfast</summary>

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
<summary>Speed: 11.3x (time 56s), preset: medium</summary>

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
<summary>Speed: 12.5x (time 51s), preset: fast</summary>

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

### Software decoding/encoding on AMD Ryzen 5600G

<details>
<summary>Speed: 1.53x (time 6m56s), preset: medium</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -c:v h264 -i bbb_sunflower_2160p_30fps_normal.mp4 \
-c:a copy -vf scale=-1:1080 -c:v libx264 -b:v 4000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset medium -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 28734
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
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
[libx264 @ 0x55e865393040] using SAR=1/1
[libx264 @ 0x55e865393040] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55e865393040] profile High, level 4.0, 4:2:0, 8-bit
[libx264 @ 0x55e865393040] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=18 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=4000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55e865391800] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1920x1080 [SAR 1:1 DAR 16:9], q=2-31, 4000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x55e865387a00] using SAR=1/1
[libx264 @ 0x55e865387a00] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55e865387a00] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x55e865387a00] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=18 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55e865386840] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x55e865379700] using SAR=3824/3825
[libx264 @ 0x55e865379700] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x55e865379700] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x55e865379700] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=7 psy=1 psy_rd=1.00:0.00 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=-2 threads=15 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=40 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x55e8654cf980] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 1358 fps= 46 q=30.0 q=30.0 q=30.0 size=   24320kB time=00:00:45.08 bitrate=4418.7kbits/s dup=6 drop=0 speed=1.52x    
16:32:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:33:07        all     26,14     42,36      0,64      0,01      0,00     30,85

16:32:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:33:07            9      1298      4,71      6,35      5,93         0
frame= 2680 fps= 45 q=28.0 q=28.0 q=28.0 size=   46592kB time=00:01:29.24 bitrate=4276.6kbits/s dup=6 drop=0 speed= 1.5x    
16:33:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:33:37        all     25,14     46,80      0,53      0,02      0,00     27,52

16:33:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:33:37            3      1292      6,95      6,73      6,07         0
frame= 4113 fps= 46 q=25.0 q=25.0 q=25.0 size=   69888kB time=00:02:17.24 bitrate=4171.4kbits/s dup=6 drop=0 speed=1.53x    
16:33:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:34:07        all     22,68     39,28      0,49      0,05      0,00     37,50

16:33:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:34:07            4      1292      6,76      6,70      6,08         0
frame= 5562 fps= 46 q=24.0 q=24.0 q=25.0 size=   94720kB time=00:03:05.24 bitrate=4188.7kbits/s dup=6 drop=0 speed=1.55x    
16:34:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:34:37        all     22,45     37,96      0,51      0,00      0,00     39,07

16:34:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:34:37            8      1289      7,59      6,89      6,16         0
frame= 6923 fps= 46 q=26.0 q=26.0 q=26.0 size=  120832kB time=00:03:50.84 bitrate=4287.9kbits/s dup=6 drop=0 speed=1.54x    
16:34:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:35:07        all     23,81     43,22      0,55      0,00      0,00     32,43

16:34:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:35:07           44      1296      8,24      7,15      6,27         0
frame= 8306 fps= 46 q=26.0 q=26.0 q=26.0 size=  146176kB time=00:04:36.92 bitrate=4324.1kbits/s dup=6 drop=0 speed=1.54x    
16:35:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:35:37        all     24,22     44,92      0,50      0,03      0,00     30,33

16:35:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:35:37            9      1298      8,04      7,22      6,33         0
frame= 9686 fps= 46 q=27.0 q=28.0 q=28.0 size=  173056kB time=00:05:23.00 bitrate=4389.0kbits/s dup=6 drop=0 speed=1.54x    
16:35:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:36:07        all     24,44     44,09      0,59      0,00      0,00     30,87

16:35:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:36:07           20      1292      9,18      7,55      6,46         0
frame=11001 fps= 46 q=29.0 q=29.0 q=30.0 size=  199168kB time=00:06:06.68 bitrate=4449.5kbits/s dup=6 drop=0 speed=1.53x    
16:36:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:36:37        all     25,82     49,65      0,53      0,01      0,00     23,99

16:36:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:36:37           13      1290      8,91      7,65      6,53         0
frame=12463 fps= 46 q=26.0 q=26.0 q=27.0 size=  217600kB time=00:06:55.64 bitrate=4288.7kbits/s dup=6 drop=0 speed=1.54x    
16:36:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:37:07        all     23,72     34,84      0,48      0,01      0,00     40,95

16:36:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:37:07            8      1291      7,89      7,55      6,54         0
frame=13782 fps= 46 q=29.0 q=29.0 q=30.0 size=  248064kB time=00:07:39.32 bitrate=4424.2kbits/s dup=6 drop=0 speed=1.53x    
16:37:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:37:37        all     25,60     47,77      0,52      0,00      0,00     26,11

16:37:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:37:37           13      1294      8,54      7,75      6,64         0
frame=15188 fps= 46 q=27.0 q=27.0 q=27.0 size=  267264kB time=00:08:26.36 bitrate=4323.8kbits/s dup=6 drop=0 speed=1.54x    
16:37:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:38:07        all     25,72     40,45      0,52      0,01      0,00     33,31

16:37:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:38:07           12      1295      8,97      7,96      6,74         0
frame=16424 fps= 46 q=28.0 q=28.0 q=29.0 size=  291584kB time=00:09:07.64 bitrate=4361.7kbits/s dup=6 drop=0 speed=1.52x    
16:38:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:38:37        all     28,19     52,59      0,54      0,00      0,00     18,68

16:38:07      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:38:37            6      1295     13,09      9,12      7,17         0
frame=17728 fps= 45 q=29.0 q=30.0 q=31.0 size=  318720kB time=00:09:50.84 bitrate=4419.0kbits/s dup=6 drop=0 speed=1.52x    
16:38:37        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:39:07        all     28,16     47,50      0,56      0,00      0,00     23,78

16:38:37      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:39:07           11      1295     11,09      9,00      7,20         0
frame=19038 fps= 46 q=-1.0 Lq=-1.0 q=-1.0 size=  334349kB time=00:10:34.50 bitrate=4316.8kbits/s dup=6 drop=0 speed=1.53x    
video:543933kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
[libx264 @ 0x55e865393040] frame I:169   Avg QP:14.59  size:280428
[libx264 @ 0x55e865393040] frame P:6060  Avg QP:19.59  size: 31056
[libx264 @ 0x55e865393040] frame B:12809 Avg QP:24.06  size:  6326
[libx264 @ 0x55e865393040] consecutive B-frames:  5.5%  9.6% 14.0% 70.8%
[libx264 @ 0x55e865393040] mb I  I16..4: 10.6% 57.6% 31.8%
[libx264 @ 0x55e865393040] mb P  I16..4:  1.5%  6.6%  1.2%  P16..4: 23.4%  9.8%  6.7%  0.0%  0.0%    skip:50.7%
[libx264 @ 0x55e865393040] mb B  I16..4:  0.2%  0.8%  0.2%  B16..8: 21.1%  2.3%  0.5%  direct: 1.1%  skip:73.9%  L0:42.5% L1:52.2% BI: 5.3%
[libx264 @ 0x55e865393040] final ratefactor: 20.64
[libx264 @ 0x55e865393040] 8x8 transform intra:67.1% inter:72.5%
[libx264 @ 0x55e865393040] coded y,uvDC,uvAC intra: 58.2% 63.8% 33.6% inter: 7.6% 7.4% 0.9%
[libx264 @ 0x55e865393040] i16 v,h,dc,p: 33% 24%  7% 35%
[libx264 @ 0x55e865393040] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 24% 16% 19%  5%  7% 10%  6%  7%  6%
[libx264 @ 0x55e865393040] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 25% 16% 12%  6% 10% 10%  8%  7%  6%
[libx264 @ 0x55e865393040] i8c dc,h,v,p: 54% 21% 17%  8%
[libx264 @ 0x55e865393040] Weighted P-Frames: Y:4.2% UV:2.0%
[libx264 @ 0x55e865393040] ref P L0: 66.0% 16.4% 12.4%  5.2%  0.1%
[libx264 @ 0x55e865393040] ref B L0: 86.5% 11.1%  2.4%
[libx264 @ 0x55e865393040] ref B L1: 96.1%  3.9%
[libx264 @ 0x55e865393040] kb/s:3991.39
[libx264 @ 0x55e865387a00] frame I:169   Avg QP:15.30  size:151309
[libx264 @ 0x55e865387a00] frame P:6237  Avg QP:20.56  size: 15648
[libx264 @ 0x55e865387a00] frame B:12632 Avg QP:25.29  size:  2958
[libx264 @ 0x55e865387a00] consecutive B-frames:  5.0% 17.6%  5.8% 71.6%
[libx264 @ 0x55e865387a00] mb I  I16..4:  8.7% 63.5% 27.8%
[libx264 @ 0x55e865387a00] mb P  I16..4:  1.0%  4.8%  0.9%  P16..4: 26.5% 10.4%  7.2%  0.0%  0.0%    skip:49.3%
[libx264 @ 0x55e865387a00] mb B  I16..4:  0.1%  0.6%  0.1%  B16..8: 21.9%  2.2%  0.5%  direct: 1.0%  skip:73.6%  L0:40.0% L1:54.2% BI: 5.8%
[libx264 @ 0x55e865387a00] final ratefactor: 21.13
[libx264 @ 0x55e865387a00] 8x8 transform intra:68.9% inter:66.4%
[libx264 @ 0x55e865387a00] coded y,uvDC,uvAC intra: 65.1% 69.5% 43.8% inter: 8.6% 7.7% 1.4%
[libx264 @ 0x55e865387a00] i16 v,h,dc,p: 33% 26%  6% 35%
[libx264 @ 0x55e865387a00] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 20% 16% 17%  5%  8% 11%  8%  8%  8%
[libx264 @ 0x55e865387a00] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 16% 12%  6% 11% 11%  9%  8%  7%
[libx264 @ 0x55e865387a00] i8c dc,h,v,p: 51% 22% 17% 10%
[libx264 @ 0x55e865387a00] Weighted P-Frames: Y:2.8% UV:1.9%
[libx264 @ 0x55e865387a00] ref P L0: 62.9% 16.3% 15.2%  5.6%  0.0%
[libx264 @ 0x55e865387a00] ref B L0: 87.1% 10.0%  3.0%
[libx264 @ 0x55e865387a00] ref B L1: 95.6%  4.4%
[libx264 @ 0x55e865387a00] kb/s:2023.79
[libx264 @ 0x55e865379700] frame I:169   Avg QP:16.18  size: 78173
[libx264 @ 0x55e865379700] frame P:6101  Avg QP:21.28  size:  7579
[libx264 @ 0x55e865379700] frame B:12768 Avg QP:26.53  size:  1596
[libx264 @ 0x55e865379700] consecutive B-frames:  6.9%  7.6% 10.3% 75.2%
[libx264 @ 0x55e865379700] mb I  I16..4:  8.7% 58.9% 32.4%
[libx264 @ 0x55e865379700] mb P  I16..4:  0.7%  3.6%  1.0%  P16..4: 25.3% 10.4%  7.6%  0.0%  0.0%    skip:51.3%
[libx264 @ 0x55e865379700] mb B  I16..4:  0.1%  0.5%  0.2%  B16..8: 22.6%  2.6%  0.8%  direct: 1.5%  skip:71.8%  L0:41.1% L1:51.8% BI: 7.1%
[libx264 @ 0x55e865379700] final ratefactor: 21.46
[libx264 @ 0x55e865379700] 8x8 transform intra:63.7% inter:63.6%
[libx264 @ 0x55e865379700] coded y,uvDC,uvAC intra: 69.5% 74.1% 51.1% inter: 8.8% 7.6% 1.7%
[libx264 @ 0x55e865379700] i16 v,h,dc,p: 31% 31%  7% 31%
[libx264 @ 0x55e865379700] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 18% 18% 16%  6%  8% 11%  8%  8%  9%
[libx264 @ 0x55e865379700] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 18% 20% 12%  6% 10% 10%  9%  7%  7%
[libx264 @ 0x55e865379700] i8c dc,h,v,p: 49% 24% 16% 11%
[libx264 @ 0x55e865379700] Weighted P-Frames: Y:3.7% UV:2.2%
[libx264 @ 0x55e865379700] ref P L0: 62.3% 16.8% 11.8%  8.7%  0.3%
[libx264 @ 0x55e865379700] ref B L0: 78.7% 15.5%  5.9%
[libx264 @ 0x55e865379700] ref B L1: 90.8%  9.2%
[libx264 @ 0x55e865379700] kb/s:1006.38

real	6m55,677s
user	56m30,294s
sys	0m14,637s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all     25,08     43,95      0,53      0,01      0,00     30,42

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:           12      1294      8,46      7,51      6,47         0
```
</details>

<details>
<summary>Speed: 1.96x (time 5m24s), preset: veryfast</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -c:v h264 -i bbb_sunflower_2160p_30fps_normal.mp4 \
-c:a copy -vf scale=-1:1080 -c:v libx264 -b:v 4000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale=-1:720 -c:v libx264 -b:v 2000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale=-1:478 -c:v libx264 -b:v 1000k -preset veryfast -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 29597
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
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
[libx264 @ 0x5625aee69040] using SAR=1/1
[libx264 @ 0x5625aee69040] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5625aee69040] profile High, level 4.0, 4:2:0, 8-bit
[libx264 @ 0x5625aee69040] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=18 lookahead_threads=6 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=4000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5625aee67800] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1920x1080 [SAR 1:1 DAR 16:9], q=2-31, 4000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/4000000 buffer size: 0 vbv_delay: N/A
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x5625aee5da00] using SAR=1/1
[libx264 @ 0x5625aee5da00] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5625aee5da00] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x5625aee5da00] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=18 lookahead_threads=5 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=2000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5625aee5c840] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/2000000 buffer size: 0 vbv_delay: N/A
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[libx264 @ 0x5625aee4f700] using SAR=3824/3825
[libx264 @ 0x5625aee4f700] using cpu capabilities: MMX2 SSE2Fast SSSE3 SSE4.2 AVX FMA3 BMI2 AVX2
[libx264 @ 0x5625aee4f700] profile High, level 3.1, 4:2:0, 8-bit
[libx264 @ 0x5625aee4f700] 264 - core 163 r3060 5db6aa6 - H.264/MPEG-4 AVC codec - Copyleft 2003-2021 - http://www.videolan.org/x264.html - options: cabac=1 ref=1 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=2 psy=1 psy_rd=1.00:0.00 mixed_ref=0 me_range=16 chroma_me=1 trellis=0 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=15 lookahead_threads=3 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=3 b_pyramid=2 b_adapt=1 b_bias=0 direct=1 weightb=1 open_gop=0 weightp=1 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc_lookahead=10 rc=abr mbtree=1 bitrate=1000 ratetol=1.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00
[mp4 @ 0x5625aefa5980] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (avc1 / 0x31637661), yuv420p(tv, progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 libx264
    Side data:
      cpb: bitrate max/min/avg: 0/0/1000000 buffer size: 0 vbv_delay: N/A
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 1728 fps= 58 q=25.0 q=26.0 q=27.0 size=   29696kB time=00:00:57.56 bitrate=4225.8kbits/s dup=6 drop=0 speed=1.94x    
16:41:19        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:41:49        all     29,84     13,80      0,68      0,02      0,00     55,66

16:41:19      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:41:49           12      1302      4,55      6,65      6,60         0
frame= 3537 fps= 59 q=22.0 q=23.0 q=23.0 size=   59136kB time=00:01:58.04 bitrate=4103.8kbits/s dup=6 drop=0 speed=1.98x    
16:41:49        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:42:19        all     25,53     14,57      0,66      0,02      0,00     59,22

16:41:49      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:42:19            4      1300      4,39      6,42      6,52         0
frame= 5380 fps= 60 q=22.0 q=22.0 q=23.0 size=   91904kB time=00:02:59.48 bitrate=4194.6kbits/s dup=6 drop=0 speed=   2x    
16:42:19        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:42:49        all     24,93     13,25      0,60      0,05      0,00     61,17

16:42:19      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:42:49            9      1299      5,82      6,54      6,56         0
frame= 7161 fps= 60 q=25.0 q=25.0 q=25.0 size=  126464kB time=00:03:58.52 bitrate=4343.3kbits/s dup=6 drop=0 speed=   2x    
16:42:49        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:43:19        all     26,07     14,54      0,65      0,00      0,00     58,74

16:42:49      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:43:19            8      1297      5,91      6,49      6,54         0
frame= 8997 fps= 60 q=25.0 q=25.0 q=25.0 size=  158720kB time=00:04:59.96 bitrate=4334.6kbits/s dup=6 drop=0 speed=   2x    
16:43:19        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:43:49        all     26,84     14,72      0,68      0,00      0,00     57,76

16:43:19      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:43:49            6      1299      6,54      6,61      6,58         0
frame=10723 fps= 60 q=27.0 q=27.0 q=28.0 size=  193792kB time=00:05:57.56 bitrate=4439.8kbits/s dup=6 drop=0 speed=1.99x    
16:43:49        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:44:19        all     29,20     16,41      0,65      0,00      0,00     53,75

16:43:49      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:44:19            5      1297      5,91      6,43      6,52         0
frame=12511 fps= 60 q=25.0 q=25.0 q=25.0 size=  219136kB time=00:06:57.08 bitrate=4304.0kbits/s dup=6 drop=0 speed=1.99x    
16:44:19        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:44:49        all     26,35     12,40      0,58      0,00      0,00     60,67

16:44:19      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:44:49            2      1298      6,26      6,48      6,53         0
frame=14231 fps= 59 q=28.0 q=28.0 q=29.0 size=  257792kB time=00:07:54.20 bitrate=4453.4kbits/s dup=6 drop=0 speed=1.98x    
16:44:49        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:45:19        all     29,94     15,97      0,70      0,01      0,00     53,37

16:44:49      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:45:19           10      1295      7,43      6,78      6,63         0
frame=15967 fps= 59 q=26.0 q=26.0 q=27.0 size=  283136kB time=00:08:52.28 bitrate=4357.5kbits/s dup=6 drop=0 speed=1.97x    
16:45:19        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:45:49        all     30,09     14,99      0,66      0,01      0,00     54,25

16:45:19      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:45:49            5      1296      7,03      6,76      6,63         0
frame=17601 fps= 59 q=28.0 q=29.0 q=29.0 size=  316416kB time=00:09:46.56 bitrate=4419.1kbits/s dup=6 drop=0 speed=1.96x    
16:45:49        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:46:19        all     32,25     16,42      0,70      0,02      0,00     50,60

16:45:49      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:46:19            3      1300      6,74      6,72      6,62         0
frame=19038 fps= 59 q=-1.0 Lq=-1.0 q=-1.0 size=  334393kB time=00:10:34.50 bitrate=4317.3kbits/s dup=6 drop=0 speed=1.96x    
video:544054kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
[libx264 @ 0x5625aee69040] frame I:168   Avg QP:14.59  size:258928
[libx264 @ 0x5625aee69040] frame P:6314  Avg QP:19.30  size: 31082
[libx264 @ 0x5625aee69040] frame B:12556 Avg QP:22.29  size:  6125
[libx264 @ 0x5625aee69040] consecutive B-frames:  6.3% 12.9% 13.2% 67.6%
[libx264 @ 0x5625aee69040] mb I  I16..4: 18.9% 18.5% 62.6%
[libx264 @ 0x5625aee69040] mb P  I16..4:  4.0%  7.7%  2.3%  P16..4: 22.6%  8.2%  4.8%  0.0%  0.0%    skip:50.4%
[libx264 @ 0x5625aee69040] mb B  I16..4:  0.6%  1.1%  0.3%  B16..8:  7.1%  2.8%  0.5%  direct: 3.0%  skip:84.6%  L0:36.7% L1:46.1% BI:17.3%
[libx264 @ 0x5625aee69040] final ratefactor: 19.38
[libx264 @ 0x5625aee69040] 8x8 transform intra:50.6% inter:34.9%
[libx264 @ 0x5625aee69040] coded y,uvDC,uvAC intra: 59.7% 55.7% 24.5% inter: 5.9% 5.8% 0.7%
[libx264 @ 0x5625aee69040] i16 v,h,dc,p: 51% 22% 15% 12%
[libx264 @ 0x5625aee69040] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 24% 17% 18%  4%  6% 13%  6%  6%  5%
[libx264 @ 0x5625aee69040] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 23% 16%  9%  7% 10% 11%  8%  9%  7%
[libx264 @ 0x5625aee69040] i8c dc,h,v,p: 53% 21% 19%  7%
[libx264 @ 0x5625aee69040] Weighted P-Frames: Y:3.1% UV:1.5%
[libx264 @ 0x5625aee69040] kb/s:3991.96
[libx264 @ 0x5625aee5da00] frame I:168   Avg QP:15.44  size:138673
[libx264 @ 0x5625aee5da00] frame P:6369  Avg QP:20.35  size: 15934
[libx264 @ 0x5625aee5da00] frame B:12501 Avg QP:23.60  size:  2857
[libx264 @ 0x5625aee5da00] consecutive B-frames:  5.6% 18.3%  6.6% 69.5%
[libx264 @ 0x5625aee5da00] mb I  I16..4: 17.3% 17.7% 65.0%
[libx264 @ 0x5625aee5da00] mb P  I16..4:  3.3%  5.3%  2.1%  P16..4: 26.2%  8.4%  4.9%  0.0%  0.0%    skip:49.7%
[libx264 @ 0x5625aee5da00] mb B  I16..4:  0.4%  0.8%  0.3%  B16..8:  6.7%  2.6%  0.5%  direct: 2.8%  skip:85.9%  L0:33.7% L1:45.4% BI:20.9%
[libx264 @ 0x5625aee5da00] final ratefactor: 19.87
[libx264 @ 0x5625aee5da00] 8x8 transform intra:45.3% inter:32.9%
[libx264 @ 0x5625aee5da00] coded y,uvDC,uvAC intra: 64.3% 59.7% 31.3% inter: 7.1% 5.9% 0.9%
[libx264 @ 0x5625aee5da00] i16 v,h,dc,p: 49% 23% 14% 14%
[libx264 @ 0x5625aee5da00] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 20% 16% 17%  5%  7% 15%  7%  6%  6%
[libx264 @ 0x5625aee5da00] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 21% 16% 11%  7% 10% 11%  8%  9%  8%
[libx264 @ 0x5625aee5da00] i8c dc,h,v,p: 51% 21% 19%  9%
[libx264 @ 0x5625aee5da00] Weighted P-Frames: Y:2.3% UV:1.6%
[libx264 @ 0x5625aee5da00] kb/s:2023.26
[libx264 @ 0x5625aee4f700] frame I:170   Avg QP:16.44  size: 70250
[libx264 @ 0x5625aee4f700] frame P:6182  Avg QP:21.42  size:  7653
[libx264 @ 0x5625aee4f700] frame B:12686 Avg QP:25.29  size:  1632
[libx264 @ 0x5625aee4f700] consecutive B-frames:  7.2%  7.9% 11.6% 73.3%
[libx264 @ 0x5625aee4f700] mb I  I16..4: 15.5% 17.5% 67.0%
[libx264 @ 0x5625aee4f700] mb P  I16..4:  2.3%  4.1%  2.2%  P16..4: 25.7%  8.3%  5.5%  0.0%  0.0%    skip:51.9%
[libx264 @ 0x5625aee4f700] mb B  I16..4:  0.3%  0.6%  0.3%  B16..8:  6.8%  3.1%  0.7%  direct: 3.9%  skip:84.3%  L0:33.4% L1:43.3% BI:23.4%
[libx264 @ 0x5625aee4f700] final ratefactor: 20.30
[libx264 @ 0x5625aee4f700] 8x8 transform intra:42.2% inter:31.4%
[libx264 @ 0x5625aee4f700] coded y,uvDC,uvAC intra: 67.2% 64.2% 37.4% inter: 8.0% 5.7% 1.1%
[libx264 @ 0x5625aee4f700] i16 v,h,dc,p: 46% 27% 14% 13%
[libx264 @ 0x5625aee4f700] i8 v,h,dc,ddl,ddr,vr,hd,vl,hu: 19% 20% 17%  5%  7% 13%  6%  6%  7%
[libx264 @ 0x5625aee4f700] i4 v,h,dc,ddl,ddr,vr,hd,vl,hu: 19% 18% 11%  7%  9% 10%  9%  9%  8%
[libx264 @ 0x5625aee4f700] i8c dc,h,v,p: 49% 23% 18%  9%
[libx264 @ 0x5625aee4f700] Weighted P-Frames: Y:3.0% UV:1.6%
[libx264 @ 0x5625aee4f700] kb/s:1007.90

real	5m23,689s
user	27m25,175s
sys	0m16,861s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all     28,11     14,71      0,66      0,01      0,00     56,52

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:            6      1298      6,06      6,59      6,57         0
```
</details>

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

### VAAPI decoding/encoding on AMD Ryzen 5600G

<details>
<summary>Speed: 2.92x (time 3m37s), preset: medium</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -hwaccel vaapi -hwaccel_output_format vaapi -hwaccel_device /dev/dri/renderD128 -i bbb_sunflower_2160p_30fps_normal.mp4 \
-c:a copy -vf scale_vaapi=w=-1:h=1080 -c:v h264_vaapi -b:v 4000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=720 -c:v h264_vaapi -b:v 2000k -preset medium -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=478 -c:v h264_vaapi -b:v 1000k -preset medium -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 33540
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[h264_vaapi @ 0x55ca06385d80] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55ca06385d80] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55ca06385d80] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55ca0635e6c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1920x1080 [SAR 1:1 DAR 16:9], q=2-31, 4000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x55ca0637c100] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55ca0637c100] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55ca0637c100] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55ca0637a8c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x55ca064c4f40] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55ca064c4f40] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55ca064c4f40] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55ca064c34c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 2553 fps= 87 q=-0.0 q=-0.0 q=-0.0 size=   44800kB time=00:01:25.06 bitrate=4314.3kbits/s dup=6 drop=0 speed=2.89x    
16:57:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:57:43        all      0,45      0,00      3,21      0,04      0,00     96,30

16:57:13      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:57:43            0      1248      0,26      1,07      3,45         0
frame= 5243 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=   92160kB time=00:02:54.73 bitrate=4320.7kbits/s dup=6 drop=0 speed=2.92x    
16:57:43        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:58:13        all      0,42      0,00      3,22      0,06      0,00     96,29

16:57:43      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:58:13            0      1244      0,55      1,06      3,37         0
frame= 7856 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  137984kB time=00:04:22.04 bitrate=4313.6kbits/s dup=6 drop=0 speed=2.93x    
16:58:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:58:43        all      0,40      0,00      3,32      0,13      0,00     96,16

16:58:13      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:58:43            1      1242      0,46      0,99      3,27         0
frame=10477 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  184576kB time=00:05:49.40 bitrate=4327.5kbits/s dup=6 drop=0 speed=2.93x    
16:58:43        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:59:13        all      0,38      0,00      3,22      0,04      0,00     96,35

16:58:43      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:59:13            1      1245      0,83      1,03      3,21         0
frame=13130 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  232448kB time=00:07:17.72 bitrate=4350.2kbits/s dup=6 drop=0 speed=2.92x    
16:59:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
16:59:43        all      0,46      0,00      3,30      0,05      0,00     96,20

16:59:13      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
16:59:43            0      1245      0,75      0,99      3,12         0
frame=15754 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  278272kB time=00:08:45.10 bitrate=4341.3kbits/s dup=6 drop=0 speed=2.92x    
16:59:43        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:00:13        all      0,43      0,00      3,25      0,05      0,00     96,27

16:59:43      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:00:13            1      1248      0,59      0,93      3,03         0
frame=18358 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  358912kB time=00:10:11.96 bitrate=4804.5kbits/s dup=6 drop=0 speed=2.92x    
17:00:13        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:00:43        all      0,47      0,00      3,23      0,06      0,00     96,24

17:00:13      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:00:43            0      1248      0,61      0,90      2,96         0
frame=19038 fps= 88 q=-0.0 Lq=-0.0 q=-0.0 size=  370229kB time=00:10:34.56 bitrate=4779.5kbits/s dup=6 drop=0 speed=2.92x    
video:608249kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	3m37,106s
user	0m7,770s
sys	0m5,618s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all      0,43      0,00      3,25      0,06      0,00     96,26

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:            0      1246      0,58      1,00      3,20         0
```
</details>

<details>
<summary>Speed: 2.93x (time 3m37s), preset: veryfast</summary>

```
$ sar -u -q 30 & time \
ffmpeg -y -hwaccel vaapi -hwaccel_output_format vaapi -hwaccel_device /dev/dri/renderD128 -i bbb_sunflower_2160p_30fps_normal.mp4 \
-c:a copy -vf scale_vaapi=w=-1:h=1080 -c:v h264_vaapi -b:v 4000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=720 -c:v h264_vaapi -b:v 2000k -preset veryfast -f mp4 /dev/null \
-c:a copy -vf scale_vaapi=w=-1:h=478 -c:v h264_vaapi -b:v 1000k -preset veryfast -f mp4 /dev/null \
; pkill -SIGINT sar
[1] 35685
Linux 5.15.0-41-generic (bzieba-desktop) 	12.07.2022 	_x86_64_	(12 CPU)
ffmpeg version 4.4.2-0ubuntu0.22.04.1 Copyright (c) 2000-2021 the FFmpeg developers
  built with gcc 11 (Ubuntu 11.2.0-19ubuntu1)
  configuration: --prefix=/usr --extra-version=0ubuntu0.22.04.1 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --arch=amd64 --enable-gpl --disable-stripping --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzimg --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-pocketsphinx --enable-librsvg --enable-libmfx --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared
  libavutil      56. 70.100 / 56. 70.100
  libavcodec     58.134.100 / 58.134.100
  libavformat    58. 76.100 / 58. 76.100
  libavdevice    58. 13.100 / 58. 13.100
  libavfilter     7.110.100 /  7.110.100
  libswscale      5.  9.100 /  5.  9.100
  libswresample   3.  9.100 /  3.  9.100
  libpostproc    55.  9.100 / 55.  9.100
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
      vendor_id       : [0][0][0][0]
  Stream #0:1(und): Audio: mp3 (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 160 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
  Stream #0:2(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (native) -> h264 (h264_vaapi))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[h264_vaapi @ 0x55af88212d80] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55af88212d80] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55af88212d80] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55af881eb6c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1920x1080 [SAR 1:1 DAR 16:9], q=2-31, 4000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #0:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x55af88209100] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55af88209100] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55af88209100] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55af882078c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #1:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 1280x720 [SAR 1:1 DAR 16:9], q=2-31, 2000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #1:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
[h264_vaapi @ 0x55af88351f40] Driver does not support some wanted packed headers (wanted 0xd, found 0).
[h264_vaapi @ 0x55af88351f40] Driver does not support packed sequence headers, but a global header is requested.
[h264_vaapi @ 0x55af88351f40] No global header will be written: this may result in a stream which is not usable for some purposes (e.g. not muxable to some containers).
[mp4 @ 0x55af883504c0] track 1: codec frame size is not set
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
    encoder         : Lavf58.76.100
  Stream #2:0(und): Video: h264 (High) (avc1 / 0x31637661), vaapi_vld(progressive), 850x478 [SAR 3824:3825 DAR 16:9], q=2-31, 1000 kb/s, 30 fps, 15360 tbn (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:04.000000Z
      handler_name    : GPAC ISO Video Handler
      vendor_id       : [0][0][0][0]
      encoder         : Lavc58.134.100 h264_vaapi
  Stream #2:1(und): Audio: ac3 (ac-3 / 0x332D6361), 48000 Hz, 5.1(side), fltp, 320 kb/s (default)
    Metadata:
      creation_time   : 2013-12-18T14:43:06.000000Z
      handler_name    : GPAC ISO Audio Handler
      vendor_id       : [0][0][0][0]
    Side data:
      audio service type: main
frame= 2604 fps= 87 q=-0.0 q=-0.0 q=-0.0 size=   45568kB time=00:01:26.84 bitrate=4298.2kbits/s dup=6 drop=0 speed= 2.9x    
17:11:36        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:12:06        all      0,42      0,00      3,27      0,09      0,00     96,22

17:11:36      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:12:06            1      1244      0,29      0,22      1,45         0
frame= 5252 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=   92160kB time=00:02:55.16 bitrate=4310.0kbits/s dup=6 drop=0 speed=2.93x    
17:12:06        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:12:36        all      0,41      0,00      3,21      0,05      0,00     96,32

17:12:06      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:12:36            2      1241      0,23      0,22      1,41         0
frame= 7876 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  138496kB time=00:04:22.52 bitrate=4321.7kbits/s dup=6 drop=0 speed=2.93x    
17:12:36        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:13:06        all      0,43      0,00      3,23      0,05      0,00     96,29

17:12:36      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:13:06            2      1239      0,35      0,24      1,38         0
frame=10540 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  185600kB time=00:05:51.32 bitrate=4327.7kbits/s dup=6 drop=0 speed=2.93x    
17:13:06        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:13:36        all      0,45      0,00      3,31      0,03      0,00     96,21

17:13:06      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:13:36            0      1243      0,35      0,25      1,34         0
frame=13151 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  232704kB time=00:07:18.33 bitrate=4349.0kbits/s dup=6 drop=0 speed=2.93x    
17:13:36        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:14:06        all      0,42      0,00      3,25      0,04      0,00     96,29

17:13:36      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:14:06            1      1241      0,48      0,29      1,32         0
frame=15779 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  278784kB time=00:08:46.04 bitrate=4341.4kbits/s dup=6 drop=0 speed=2.93x    
17:14:06        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:14:36        all      0,44      0,00      3,23      0,04      0,00     96,29

17:14:06      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:14:36            1      1241      0,44      0,30      1,29         0
frame=18431 fps= 88 q=-0.0 q=-0.0 q=-0.0 size=  360192kB time=00:10:14.36 bitrate=4802.8kbits/s dup=6 drop=0 speed=2.93x    
17:14:36        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:15:06        all      0,53      0,00      3,17      0,04      0,00     96,26

17:14:36      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
17:15:06            0      1243      0,52      0,33      1,27         0
frame=19038 fps= 88 q=-0.0 Lq=-0.0 q=-0.0 size=  370247kB time=00:10:34.56 bitrate=4779.7kbits/s dup=6 drop=0 speed=2.93x    
video:608258kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown

real	3m36,688s
user	0m7,811s
sys	0m5,561s


Average:        CPU     %user     %nice   %system   %iowait    %steal     %idle
Average:        all      0,44      0,00      3,24      0,05      0,00     96,27

Average:      runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
Average:            1      1242      0,38      0,26      1,35         0
```
</details>

### Intel Quick Sync Video decoding/encoding on Intel i5-8250U (UHD 620)
<details>
<summary>Speed: 3.79x (time 2m47s), preset: medium</summary>

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
<summary>Speed: 4.09x (time 2m35s), preset: veryfast</summary>

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
<summary>Speed: 3.6x (time 2m56s), preset: medium</summary>

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