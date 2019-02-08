FFmpeg w/ decklink
==================

Static build of `ffmpeg` with h264 and decklink   


original from https://github.com/kreldjarn/ffmpeg-decklink   
Modified by : kiokahn


Install decklink driver & software   
----------------------------------   


Check driver   
------------   

    $ lspci | grep Blackmagic   
    01:00.0 Multimedia video controller: Blackmagic Design DeckLink Mini Recorder 4K   

    $ lsmod | grep blackmagic   
    blackmagic_io 1843200 1   


Install utility   
----------------------------------   

    $ sudo apt-get install autoreconf
    $ sudo apt-get install libtool
    $ sudo apt-get install git
    $ sudo apt-get install cmake
    $ sudo apt-get install nasm
    $ sudo att-get install libsdl2-dev
    $ sudo apt install python-pip
    $ ?pip install cryptography --upgrade
    $ ? sudo apt-get install python-yenc
    $ ? sudo apt-get install build-essential libssl-dev libffi-dev python-dev
    $ ? python -c "import cryptography; print dir(cryptography); print cryptography.__version__"


Build
-----

    $ ./build.sh
    # binaries can be found in ./target/bin/

NOTE: If you're going to use the h264 presets, make sure to copy them along the binaries. For ease, you can put them in your home folder like this:

    $ mkdir ~/.ffmpeg
    $ cp ./target/share/ffmpeg/*.ffpreset ~/.ffmpeg



Get device and format list   
--------------------------

Get device list :    

    $ ./bin/ffmpeg -f decklink -list_devices 1 -i dummy
    ffmpeg version 3a462e6 Copyright (c) 2000-2018 the FFmpeg developers
    built with gcc 7 (Ubuntu 7.3.0-27ubuntu1~18.04)
    configuration: --prefix=/home/mteg_vas/capture/ffmpeg-decklink/target --pkg-config-flags=--static --extra-cflags='-I/home/mteg_vas/capture/ffmpeg-decklink/target/include -I/home/mteg_vas/capture/ffmpeg-decklink/target/include/bm -std=c11' --extra-ldflags=-L/home/mteg_vas/capture/ffmpeg-decklink/target/lib --bindir=/home/mteg_vas/capture/ffmpeg-decklink/bin --enable-static --enable-decklink --enable-gpl --enable-libfdk-aac --enable-libx264 --enable-nonfree --extra-cflags=-I/home/mteg_vas/capture/Blackmagic_DeckLink_SDK_10.11.4/Linux/include/ --extra-ldflags=-L/home/mteg_vas/capture/Blackmagic_DeckLink_SDK_10.11.4/Linux/include/
    libavutil      56. 22.100 / 56. 22.100
    libavcodec     58. 35.100 / 58. 35.100
    libavformat    58. 20.100 / 58. 20.100
    libavdevice    58.  5.100 / 58.  5.100
    libavfilter     7. 40.101 /  7. 40.101
    libswscale      5.  3.100 /  5.  3.100
    libswresample   3.  3.100 /  3.  3.100
    libpostproc    55.  3.100 / 55.  3.100
    [decklink @ 0x558f190c1240] Blackmagic DeckLink input devices:
    [decklink @ 0x558f190c1240]     'DeckLink Mini Recorder 4K'
   
   
Get format list :    

    $ ./bin/ffmpeg -f decklink -list_formats 1 -i 'DeckLink Mini Recorder 4K'
    ffmpeg version 3a462e6 Copyright (c) 2000-2018 the FFmpeg developers
    built with gcc 7 (Ubuntu 7.3.0-27ubuntu1~18.04)
    configuration: --prefix=/home/mteg_vas/capture/ffmpeg-decklink/target --pkg-config-flags=--static --extra-cflags='-I/home/mteg_vas/capture/ffmpeg-decklink/target/include -I/home/mteg_vas/capture/ffmpeg-decklink/target/include/bm -std=c11' --extra-ldflags=-L/home/mteg_vas/capture/ffmpeg-decklink/target/lib --bindir=/home/mteg_vas/capture/ffmpeg-decklink/bin --enable-static --enable-decklink --enable-gpl --enable-libfdk-aac --enable-libx264 --enable-nonfree --extra-cflags=-I/home/mteg_vas/capture/Blackmagic_DeckLink_SDK_10.11.4/Linux/include/ --extra-ldflags=-L/home/mteg_vas/capture/Blackmagic_DeckLink_SDK_10.11.4/Linux/include/
    libavutil 56. 22.100 / 56. 22.100
    libavcodec 58. 35.100 / 58. 35.100
    libavformat 58. 20.100 / 58. 20.100
    libavdevice 58. 5.100 / 58. 5.100
    libavfilter 7. 40.101 / 7. 40.101
    libswscale 5. 3.100 / 5. 3.100
    libswresample 3. 3.100 / 3. 3.100
    libpostproc 55. 3.100 / 55. 3.100
    [decklink @ 0x55fa7df93240] Supported formats for 'DeckLink Mini Recorder 4K':
    format_code description
    ntsc 720x486 at 30000/1001 fps (interlaced, lower field first)
    pal 720x576 at 25000/1000 fps (interlaced, upper field first)
    23ps 1920x1080 at 24000/1001 fps
    24ps 1920x1080 at 24000/1000 fps
    Hp25 1920x1080 at 25000/1000 fps
    Hp29 1920x1080 at 30000/1001 fps
    Hp30 1920x1080 at 30000/1000 fps
    Hp50 1920x1080 at 50000/1000 fps
    Hp59 1920x1080 at 60000/1001 fps
    Hp60 1920x1080 at 60000/1000 fps
    Hi50 1920x1080 at 25000/1000 fps (interlaced, upper field first)
    Hi59 1920x1080 at 30000/1001 fps (interlaced, upper field first)
    Hi60 1920x1080 at 30000/1000 fps (interlaced, upper field first)
    hp50 1280x720 at 50000/1000 fps
    hp59 1280x720 at 60000/1001 fps
    hp60 1280x720 at 60000/1000 fps
    2d23 2048x1080 at 24000/1001 fps
    2d24 2048x1080 at 24000/1000 fps
    2d25 2048x1080 at 25000/1000 fps
    4k23 3840x2160 at 24000/1001 fps
    4k24 3840x2160 at 24000/1000 fps
    4k25 3840x2160 at 25000/1000 fps
    4k29 3840x2160 at 30000/1001 fps
    4k30 3840x2160 at 30000/1000 fps
    4d23 4096x2160 at 24000/1001 fps
    4d24 4096x2160 at 24000/1000 fps
    4d25 4096x2160 at 25000/1000 fps
    DeckLink Mini Recorder 4K: Immediate exit requested


Preview   
---------

    $ ./bin/ffplay -f decklink -video_input hdmi -duplex_mode full -raw_format argb -i 'DeckLink Mini Recorder 4K'




Capture   
---------

    $ ./bin/ffmpeg -f decklink -video_input hdmi -duplex_mode full -raw_format argb -i 'DeckLink Mini Recorder 4K' -video_size 1920x1080 -framerate 30 output.mp4

or

    $ ./bin/ffmpeg -f decklink -video_input hdmi -duplex_mode full -raw_format argb -i 'DeckLink Mini Recorder 4K' -video_size 1920x1080 -framerate 30 -c:v libx264 -b:v 4000k outputh264.mp4




Capture with preview   
--------------------

    $ ./bin/ffmpeg -y -f decklink -video_input hdmi -raw_format argb -i 'DeckLink Mini Recorder 4K' -video_size 1920x1080 -framerate 30 -c:v h264 -b:v 4000k -pix_fmt yuv420p -ac 2 -ab 96k -ar 48k -c:a aac outputh264.mp4 -flags +global_header -copytb 1 -async 1 -f mpegts tcp://127.0.0.1:1234/ | ./ffplay -i tcp://127.0.0.1:1234/?listen

or

    $ ./bin/ffmpeg -f decklink -rtbufsize 1000000k -raw_format argb -i 'DeckLink Mini Recorder 4K' -video_size 1920x1080 -framerate 30 -an -c:v libx264 -analyzeduration  5000000 -probesize 5000000 -q 0 -f h264 - | ffmpeg -f h264 -i - -an -c:v copy -f mp4 outfile_pipe.mp4 -an -c:v copy -f h264 pipe:play | ffplay -i pipe:play 




Reference   
---------

    http://manpages.ubuntu.com/manpages/bionic/man1/ffmpeg-devices.1.html   
    https://trac.ffmpeg.org/wiki/Capture/Desktop   
    https://trac.ffmpeg.org/wiki/Encode/H.264   

