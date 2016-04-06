FFmpeg w/ decklink
==================

Static build of `ffmpeg` with h264 and decklink


Build
-----

    $ ./build.sh
    # binaries can be found in ./target/bin/

NOTE: If you're going to use the h264 presets, make sure to copy them along the binaries. For ease, you can put them in your home folder like this:

    $ mkdir ~/.ffmpeg
    $ cp ./target/share/ffmpeg/*.ffpreset ~/.ffmpeg

