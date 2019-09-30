# NVENC benchmarks
## Quadro K2200 H.264 encoding tests
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 430.26       Driver Version: 430.26       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Quadro K2200        Off  | 00000000:04:00.0 Off |                  N/A |
| 42%   42C    P0     2W /  39W |      0MiB /  4043MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
```
### Transcoding 20x 1080p30 streams to 720p:
```
# ffmpeg -vsync 0 -y -re -i bbb_sunflower_1080p_30fps_normal.mp4 -vcodec h264_nvenc -gpu 1 -vb 1500k -preset fast -s 1280x720 -an -f mp4 /dev/null
```
```
...
genre           : Animation
encoder         : Lavf58.33.100
Stream #0:0(und): Video: h264 (h264_nvenc) (Main) (avc1 / 0x31637661), yuv420p, 1280x720 [SAR 1:1 DAR 16:9], q=-1--1, 1500 kb/s, 30 fps, 15360 tbn, 30 tbc (default)
Metadata:
  creation_time   : 2013-12-16T17:44:39.000000Z
  handler_name    : GPAC ISO Video Handler
  encoder         : Lavc58.58.101 h264_nvenc
Side data:
  cpb: bitrate max/min/avg: 0/0/1500000 buffer size: 3000000 vbv_delay: N/A
frame=19036 fps= 30 q=25.0 Lsize=  112842kB time=00:10:34.56 bitrate=1456.7kbits/s speed=   1x    
video:112764kB audio:0kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: 0.068738%
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     6    52     -    36    17     9     0  2505  1045
    0     6    53     -    20    10    63     0  2505  1124
    0     7    54     -    22    10    66     0  2505  1124
    0     7    54     -    22    10    67     0  2505  1124
    0     7    54     -    22    11    66     0  2505  1124
    0     6    55     -    21    10    67     0  2505  1124
    0     7    55     -    22    10    66     0  2505  1124
    0     7    55     -    23    11    65     0  2505  1124
    0     7    55     -    22    11    67     0  2505  1124
    0     6    55     -    22    10    67     0  2505  1124
    0     7    55     -    23    11    67     0  2505  1124
    0     7    55     -    27    13    65     0  2505  1124
    0     5    55     -    20     9    64     0  2505  1124
    0     7    55     -    22    10    66     0  2505  1124
    0     7    55     -    22    10    66     0  2505  1124
    0     6    55     -    22    11    70     0  2505  1124
    0     6    55     -    23    11    69     0  2505  1124
    0     6    56     -    22    11    66     0  2505  1124
    0     7    55     -    22    11    66     0  2505  1124
    0     6    55     -    22    10    66     0  2505  1124
    0     7    55     -    24    11    67     0  2505  1124
    0     6    55     -    22    10    63     0  2505  1124
    0     1    50     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 60496280  41408 731880    0    0     6    34  226   61 17  1 82  0  0
41  0      0 59064092  41412 922968    0    0     0    84 5332 10446 84  1 15  0  0
 6  0      0 58992556  41412 922968    0    0     0    86 5007 8000 90  1  9  0  0
 7  0      0 58953088  41412 922968    0    0     0    96 5065 7189 73  1 26  0  0
 6  0      0 58944284  41412 922968    0    0     0    97 5024 7684 60  1 39  0  0
 5  0      0 58922068  41412 922968    0    0     0   104 5203 7546 70  1 29  0  0
 8  0      0 58872184  41420 922968    0    0     0   109 5033 7766 61  1 38  0  0
 7  0      0 58830488  41420 922968    0    0     0    96 5178 7226 69  1 30  0  0
 9  0      0 58804616  41420 922968    0    0     0   118 5162 7601 67  1 32  0  0
17  0      0 58767772  41420 922968    0    0     0   362 5271 8165 73  1 27  0  0
16  0      0 58766044  41420 922968    0    0     0   129 5193 7562 69  1 30  0  0
30  0      0 58744464  41420 922968    0    0     0   135 5292 7936 79  1 20  0  0
27  0      0 58740648  41420 922968    0    0     0   261 5056 7478 82  1 17  0  0
 5  0      0 58739412  41420 922968    0    0     0   141 4917 7823 75  1 24  0  0
10  0      0 58740616  41420 922968    0    0     0   267 5112 7827 64  1 35  0  0
 7  0      0 58714544  41420 922968    0    0     0   128 4737 7995 92  1  7  0  0
 6  0      0 58715504  41420 922968    0    0     0   160 4934 7743 78  1 21  0  0
19  0      0 58716480  41420 922968    0    0     0   461 5180 7300 68  1 32  0  0
23  0      0 58717132  41420 922968    0    0     0   530 5517 6691 83  1 16  0  0
23  0      0 58716584  41420 922988    0    0     0   173 5463 8029 91  1  8  0  0
18  0      0 58715076  41436 922988    0    0     0   180 5173 8735 92  1  7  0  0
 9  0      0 58715600  41444 922988    0    0     0   426 4943 7491 58  1 41  0  0
 0  0      0 63119916  41444 731948    0    0     0    31 1071 2962  6  1 94  0  0
 0  0      0 63121784  41444 731948    0    0     0     0  348  730  0  0 100  0  0
```
### Transcoding 11x 1080p30 streams to 720p, 540p, and 480p simultaneously:
```
# ffmpeg -y -re -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
```
```
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
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x5618700f3bc0] track 1: codec frame size is not set
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
[mp4 @ 0x561870111b40] track 1: codec frame size is not set
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
[mp4 @ 0x5618701b6440] track 1: codec frame size is not set
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
frame=19036 fps= 30 q=22.0 Lq=19.0 q=21.0 size=  180283kB time=00:10:34.73 bitrate=2326.8kbits/s speed=   1x    
video:350085kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     3    47     -     0     0     0     0  2505  1045
    0    29    62     -    99    48    98    89  2505  1124
    0    30    68     -    95    46    93    81  2505  1124
    0    30    71     -    99    49    99    84  2505  1124
    0    28    72     -    87    45    99    80  2505  1124
    0    25    73     -    79    41    96    81  2505  1124
    0    31    74     -    95    47    94    87  2505  1124
    0    31    75     -    99    49    99    86  2505  1124
    0    24    75     -    85    41    98    88  2505  1124
    0    27    76     -    99    48   100    83  2505  1124
    0    30    77     -    99    49    96    88  2505  1124
    0    21    75     -    85    45   100    70  2505  1124
    0    31    77     -    99    49   100    77  2505  1124
    0    31    77     -    99    48    98    89  2505  1124
    0    31    77     -    99    46    99    77  2505  1124
    0    27    76     -    73    37    99    77  2505  1124
    0    31    77     -    99    46    99    84  2505  1124
    0    31    77     -    99    48    99    77  2505  1124
    0    32    77     -    95    45    94    73  2505  1124
    0    32    77     -    95    46    94    75  2505  1124
    0    32    77     -    95    46    94    77  2505  1124
    0    33    76     -    94    46    92    86  2505  1124
    0     2    60     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 63095572  42008 740612    0    0     4    22   10   96 13  1 86  0  0
 1  0      0 61202684  42008 774404    0    0     0   182 5394 38755  5  5 90  0  0
 0  0      0 61189052  42008 774404    0    0     0    78 4294 22934  4  3 93  0  0
 1  0      0 61177396  42008 774404    0    0     0    80 5091 31240  4  3 92  0  0
 1  0      0 61176216  42008 774404    0    0     0    86 4356 23876  4  3 93  0  0
 2  0      0 61176000  42008 774404    0    0     0    88 4401 24240  4  3 93  0  0
 0  0      0 61163244  42016 774404    0    0     0    76 4164 21743  4  3 93  0  0
 0  0      0 61156568  42016 774404    0    0     0   216 6114 41045  4  4 91  0  0
 0  0      0 61143376  42016 774404    0    0     0   100 6300 44601  5  5 91  0  0
 0  0      0 61143764  42016 774404    0    0     0   221 6491 44747  4  5 90  0  0
 0  0      0 61143112  42016 774404    0    0     0   227 6403 45047  5  5 91  0  0
 0  0      0 61142284  42016 774404    0    0     0   232 6581 44698  4  5 91  0  0
 0  0      0 61142664  42016 774404    0    0     0   235 6491 45264  4  5 91  0  0
 0  0      0 61142372  42016 774404    0    0     0    95 6415 45893  5  5 90  0  0
 0  0      0 61140292  42016 774404    0    0     0   302 6501 45700  5  5 91  0  0
 1  0      0 61139784  42016 774404    0    0     0   243 6326 44149  5  5 91  0  0
 1  0      0 61126092  42032 775832    0    0     0   250 6797 45216  5  5 91  0  0
 0  0      0 61110592  42040 775832    0    0     0   141 6427 45949  5  5 91  0  0
 3  0      0 61109424  42184 775956    0    0     8   134 4551 25901  4  3 93  0  0
 1  0      0 61106664  42184 775956    0    0     0   138 3716 17272  4  2 93  0  0
 1  0      0 61105360  42184 775956    0    0     0    80 3347 13275  4  2 94  0  0
 2  0      0 61104480  42184 775956    0    0     0   196 3362 13346  4  2 94  0  0
 0  0      0 63082452  42184 740772    0    0     0   156 1469 6127  1  1 98  0  0
```
### Transcoding (fast preset) 11x 1080p30 streams to 720p, 540p, and 480p simultaneously:
```
ffmpeg -y -re -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -preset fast -f mp4 /dev/null -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -preset fast -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -preset fast -f mp4 /dev/null
```
```
...
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x5566abf45740] track 1: codec frame size is not set
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
[mp4 @ 0x5566abf48ac0] track 1: codec frame size is not set
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
[mp4 @ 0x5566abfdf3c0] track 1: codec frame size is not set
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
frame=19036 fps= 30 q=23.0 Lq=20.0 q=22.0 size=  180309kB time=00:10:34.73 bitrate=2327.1kbits/s speed=   1x    
video:350097kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     4    62     -     0     0     0     0  2505  1045
    0    27    72     -    96    46    79    84  2505  1124
    0    26    74     -    96    46    78    82  2505  1124
    0    28    75     -    96    46    78    84  2505  1124
    0    27    76     -    95    45    77    85  2505  1124
    0    29    76     -    96    45    77    82  2505  1124
    0    28    76     -    95    44    76    84  2505  1124
    0    29    76     -    96    46    77    86  2505  1124
    0    30    77     -    96    46    78    86  2505  1124
    0    28    77     -    95    45    78    81  2505  1124
    0    26    77     -    95    46    78    81  2505  1124
    0    28    77     -    96    46    78    81  2505  1124
    0    29    77     -    95    45    78    79  2505  1124
    0    31    77     -    94    45    77    86  2505  1124
    0    30    77     -    96    45    77    73  2505  1124
    0    31    77     -    95    45    78    78  2505  1124
    0    32    77     -    95    45    78    83  2505  1124
    0    31    77     -    95    45    77    74  2505  1124
    0    31    77     -    95    45    76    73  2505  1124
    0    31    77     -    95    45    77    74  2505  1124
    0    31    77     -    97    47    77    78  2505  1124
    0    31    76     -    98    47    74    86  2505  1124
    0     2    60     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 62134704  42812 741236    0    0     3    24   34   73 10  1 89  0  0
 0  0      0 60240420  42820 775028    0    0     0    62 5656 36466  5  5 91  0  0
 0  0      0 60226872  42820 775028    0    0     0   250 3386 13317  4  2 94  0  0
 0  0      0 60214008  42820 775028    0    0     0    74 3377 13310  4  2 94  0  0
 0  0      0 60213512  42820 775028    0    0     0    80 3400 13321  4  2 94  0  0
 5  0      0 60215028  42820 775028    0    0     0    87 3399 13278  4  2 94  0  0
 0  0      0 60203012  42836 775028    0    0     0    92 3396 13308  4  2 94  0  0
 4  0      0 60196456  42844 775028    0    0     0    96 3391 13302  4  2 94  0  0
 0  0      0 60188364  42844 775028    0    0     0    79 3383 13245  4  2 94  0  0
 0  0      0 60188432  42844 775028    0    0     0    99 3389 13308  4  2 94  0  0
 2  0      0 60187408  42844 775028    0    0     0   101 3396 13298  4  2 94  0  0
 0  0      0 60185972  42844 775028    0    0     0   226 3390 13311  4  2 94  0  0
 3  0      0 60185568  42844 775028    0    0     0   229 3393 13302  4  2 94  0  0
 5  0      0 60184968  42844 775028    0    0     0   112 3388 13327  4  2 94  0  0
 0  0      0 60181436  42844 775028    0    0     0   115 3379 13304  4  2 94  0  0
 0  0      0 60179448  42852 775036    0    0     0    99 3373 13259  4  2 94  0  0
 1  0      0 60171392  42860 775036    0    0     0   366 3398 13351  4  2 94  0  0
 3  0      0 60156516  42860 775036    0    0     0   126 3389 13318  4  2 94  0  0
 1  0      0 60154932  42860 775036    0    0     0   132 3379 13300  4  2 94  0  0
 0  0      0 60155380  42860 775036    0    0     0   131 3399 13296  4  2 94  0  0
 1  0      0 60154328  42860 775036    0    0     0    64 3404 13262  4  2 94  0  0
 3  0      0 60152684  42860 775036    0    0     0    67 3391 13285  4  2 94  0  0
 0  0      0 62131132  42860 741244    0    0     0    24 1390 5593  1  1 98  0  0
 0  0      0 62131132  42860 741244    0    0     0     0  347  720  0  0 100  0  0
```

### Transcoding (Low Latency High Performance) 9x 1080p30 streams to 720p, 540p, and 480p simultaneously:
```
ffmpeg -y -re -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -preset llhp -f mp4 /dev/null -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -preset llhp -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -preset llhp -f mp4 /dev/null
```
```
...
Stream mapping:
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
  Stream #0:0 -> #2:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #2:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x556798e4e740] track 1: codec frame size is not set
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
[mp4 @ 0x556798e51ac0] track 1: codec frame size is not set
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
[mp4 @ 0x556798ee83c0] track 1: codec frame size is not set
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
frame=19036 fps= 30 q=22.0 Lq=20.0 q=23.0 size=  179970kB time=00:10:34.73 bitrate=2322.7kbits/s speed=   1x    
video:349624kB audio:74314kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     3    54     -     0     0     0     0  2505  1045
    0    23    66     -    82    41    93    71  2505  1124
    0    24    69     -    83    42    92    66  2505  1124
    0    23    70     -    83    40    93    66  2505  1124
    0    25    71     -    83    41    93    69  2505  1124
    0    27    72     -    83    41    92    68  2505  1124
    0    23    72     -    84    41    92    71  2505  1124
    0    26    73     -    84    42    92    70  2505  1124
    0    25    73     -    82    41    92    71  2505  1124
    0    30    73     -    84    42    93    67  2505  1124
    0    28    73     -    85    41    92    68  2505  1124
    0    30    73     -    84    40    93    66  2505  1124
    0    31    73     -    82    40    93    63  2505  1124
    0    30    73     -    82    40    91    70  2505  1124
    0    31    73     -    84    39    93    62  2505  1124
    0    28    73     -    83    42    94    64  2505  1124
    0    28    73     -    82    41    93    68  2505  1124
    0    31    73     -    83    41    95    61  2505  1124
    0    30    73     -    82    40    94    59  2505  1124
    0    31    73     -    84    41    94    61  2505  1124
    0    31    73     -    84    42    95    63  2505  1124
    0    31    73     -    83    41    91    71  2505  1124
    0     2    59     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 62132084  43172 741260    0    0     3    21   44   24  9  1 90  0  0
 0  0      0 60524240  43172 768912    0    0     0   184 4607 32430  4  3 92  0  0
 1  0      0 60519372  43180 768912    0    0     0    69 3586 11738  3  1 95  0  0
 2  0      0 60507856  43196 768912    0    0     0   193 3578 11708  3  1 95  0  0
 0  0      0 60505732  43208 768912    0    0     0   317 3606 11786  4  2 95  0  0
 0  0      0 60506508  43208 768912    0    0     0    76 3584 11708  3  1 95  0  0
 0  0      0 60495916  43208 768912    0    0     0    81 3589 11754  3  1 95  0  0
 0  0      0 60492048  43216 768916    0    0     0   208 3592 11771  3  1 95  0  0
 0  0      0 60484580  43224 768916    0    0     0   217 3590 11767  3  1 95  0  0
 0  0      0 60483732  43224 768916    0    0     0    87 3593 11733  3  1 95  0  0
 0  0      0 60482212  43224 768916    0    0     0    75 3579 11747  3  1 95  0  0
 0  0      0 60482656  43224 768916    0    0     0    93 3592 11733  3  1 95  0  0
 0  0      0 60482296  43232 768916    0    0     0    99 3598 11719  4  1 95  0  0
 0  0      0 60481444  43232 768916    0    0     0   104 3592 11729  4  1 95  0  0
 0  0      0 60479960  43232 768916    0    0     0   104 3585 11743  3  1 95  0  0
 0  0      0 60479492  43232 768916    0    0     0   103 3612 11818  4  1 95  0  0
 0  0      0 60459568  43232 768916    0    0     0   109 3605 11715  4  2 95  0  0
 0  0      0 60459692  43232 768916    0    0     0    94 3577 11688  3  1 95  0  0
 0  0      0 60458668  43232 768916    0    0     0   114 3593 11756  3  1 95  0  0
 0  0      0 60456460  43232 768916    0    0     0   118 3596 11728  4  1 95  0  0
 0  0      0 60456900  43232 768916    0    0     0   196 3599 11801  4  1 95  0  0
 0  0      0 60455968  43232 768916    0    0     0    64 3598 11729  4  1 95  0  0
 0  0      0 62139112  43240 741272    0    0     0    22 1277 4769  1  1 98  0  0
 0  0      0 62139120  43240 741272    0    0     0     1  351  728  0  0 100  0  0
```
### One-shot transcoding 1080p30 movie to 720p, 540p and 480p simultaneously:
```
 time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
```
```
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
### One-shot transcoding (fast preset) 1080p30 movie to 720p, 540p and 480p simultaneously:
```
 time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_1080p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -preset fast -f mp4 720p -c:a copy -vf scale_npp=-1:540 -c:v h264_nvenc -b:v 1500k -preset fast -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -preset fast -f mp4 /dev/null
```
```
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
0  0      0 62124328  42676 741224    0    0     0     0  349  729  0  0 100  0  0
```

### Transcoding 3x 2160p30 streams to 1080p, 720p, and 480p simultaneously:
```
ffmpeg -y -re -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_2160p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:1080 -c:v h264_nvenc -b:v 4000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
```

```
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
  Stream #0:0 -> #0:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #0:1 (copy)
  Stream #0:0 -> #1:0 (h264 (h264_cuvid) -> h264 (h264_nvenc))
  Stream #0:2 -> #1:1 (copy)
Press [q] to stop, [?] for help
[mp4 @ 0x558af4cd7600] track 1: codec frame size is not set
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
[mp4 @ 0x558af4cf36c0] track 1: codec frame size is not set
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
frame=19036 fps= 30 q=24.0 Lq=45.0 size=  337348kB time=00:10:34.73 bitrate=4353.9kbits/s speed=   1x    
video:325918kB audio:49542kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: unknown
```
```
# nvidia-smi dmon -i 0 -d 30
# gpu   pwr gtemp mtemp    sm   mem   enc   dec  mclk  pclk
# Idx     W     C     C     %     %     %     %   MHz   MHz
    0     4    59     -     0     0     0     0  2505  1045
    0    30    65     -    51    35    39    88  2505  1124
    0    30    66     -    52    34    39    80  2505  1124
    0    32    67     -    52    34    41    78  2505  1124
    0    31    67     -    51    33    41    84  2505  1124
    0    30    68     -    51    34    38    85  2505  1124
    0    32    68     -    51    34    39    89  2505  1124
    0    30    68     -    52    35    42    84  2505  1124
    0    31    68     -    49    34    40    90  2505  1124
    0    30    68     -    52    34    41    83  2505  1124
    0    31    68     -    52    33    37    89  2505  1124
    0    33    68     -    52    34    45    76  2505  1124
    0    32    68     -    49    34    42    77  2505  1124
    0    29    68     -    52    34    40    88  2505  1124
    0    29    68     -    51    34    42    84  2505  1124
    0    26    68     -    52    32    40    83  2505  1124
    0    27    68     -    52    34    39    81  2505  1124
    0    24    68     -    52    34    38    77  2505  1124
    0    32    68     -    52    33    38    75  2505  1124
    0    21    68     -    51    34    39    75  2505  1124
    0    19    68     -    52    35    38    75  2505  1124
    0    12    68     -    52    34    37    91  2505  1124
    0     2    57     -     0     0     0     0   405   405
```
```
# vmstat 30
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 62458204  42324 740860    0    0     4    23   24   23 12  1 87  0  0
 0  0      0 61736100  42324 750076    0    0     0    58 1352 3806  1  0 99  0  0
 0  0      0 61734076  42324 750076    0    0     0   171 1248 3167  1  0 99  0  0
 1  0      0 61734436  42324 750076    0    0     0   185 1255 3212  1  0 99  0  0
 0  0      0 61732748  42324 750076    0    0     0   182 1249 3205  1  0 99  0  0
 0  0      0 61734640  42324 750076    0    0     0    63 1250 3184  1  0 99  0  0
 0  0      0 61714472  42340 750168    0    0     0    66 1298 3255  1  0 99  0  0
 4  0      0 61714264  42348 750168    0    0     0    73 1257 3224  1  0 99  0  0
 1  0      0 61716048  42348 750168    0    0     0    70 1262 3214  1  0 99  0  0
 1  0      0 61715748  42348 750168    0    0     0    57 1249 3177  1  0 99  0  0
 0  0      0 61717268  42348 750168    0    0     0   190 1262 3245  1  0 99  0  0
 0  0      0 61718624  42348 750168    0    0     0   190 1269 3258  1  0 99  0  0
 0  0      0 61718152  42348 750168    0    0     0    68 1257 3224  1  0 99  0  0
 0  0      0 61717936  42348 750096    0    0     0    72 1264 3242  1  0 99  0  0
 0  0      0 61718088  42356 750100    0    0     0   256 1260 3250  1  0 99  0  0
 0  0      0 61719072  42360 750100    0    0     0    78 1270 3244  1  0 99  0  0
 0  0      0 61717708  42368 750048    0    0     0    63 1253 3187  1  0 99  0  0
 1  0      0 61719648  42368 750048    0    0     0   315 1265 3272  1  0 99  0  0
 0  0      0 61718796  42368 750048    0    0     0    77 1262 3228  1  0 99  0  0
 0  0      0 61719864  42368 750048    0    0     0    76 1264 3238  1  0 99  0  0
 0  0      0 61719920  42368 750048    0    0     0    77 1272 3254  1  0 99  0  0
 0  0      0 61720980  42376 750048    0    0     0    70 1266 3237  1  0 99  0  0
 0  0      0 62467460  42376 740832    0    0     0    19  633 1694  0  0 99  0  0
```
### One-shot transcoding 2160p30 movie to 1080p, 720p and 480p simultaneously:
```
 time ffmpeg -y -vsync 0 -hwaccel_device 1 -hwaccel cuvid -c:v h264_cuvid -i bbb_sunflower_2160p_30fps_normal.mp4 -c:a copy -vf scale_npp=-1:1080 -c:v h264_nvenc -b:v 4000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:720 -c:v h264_nvenc -b:v 2000k -f mp4 /dev/null -c:a copy -vf scale_npp=-1:480 -c:v h264_nvenc -b:v 1000k -f mp4 /dev/null
```
```
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
