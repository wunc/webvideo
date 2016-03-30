# HTML5 Web Video Encoding Script

A basic shell script to encode HTML5 video.

## Requirements

- FFMPEG executable with at least the following libraries
	- libx264
	- libfaac
	- libvpx
	- libvorbis

## Syntax

`webvideo [-m] [-w] [-f] *files*`

### Options
- -m | --mp4
	- encode and output an mp4 format file
- -w | --webm
	- encode and output a webm format file
- -f | --flv
	- encode and output a flv format file
- -na | --no-audio
	- disable encoding of audio

## Background

I wrote this script mostly for myself, because I wasn't satisfied with using a GUI video encoder to encode my web videos properly.

# Video Formats

Most modern browsers support the HTML5 <video> element, but unfortunately it's not that easy. A single video file has two important components: the "container" and the "codec". Because of differing philosophical and legal opinions on the free vs licensed available container and codec formats, browser support for the different combinations is varied. Currently (as of Oct 2014), there isn't a perfect single combination of container + codec that works in all browsers. There *is* a leader, however, which is MPEG4/H.264. That's supported by the latest version of every major browser except Opera and the OSX version of Firefox. To cover those, you need WebM/VP8. Keep in mind also that not all users are geeks like us and running the latest version of their broswer on the latest OS. So, the last thing to have would be a (ugh!) Flash video fallback. And then there's mobile browsers to consider. This script gives the option to encode the 2 major formats (referred to henceforth as mp4 and webm, respectively), as that should generally cover most modern users without going overboard. More specifically, I've set:

- mp4 = H.264 baseline profile video codec, AAC audio codec, MPEG-4 container
- webm = VP8 video codec, Vorbis audio codec, WebM container

Feel free to change them to your needs. If you want to know more, there's good reading [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats), [here](http://diveintohtml5.info/video.html), [here](http://blog.zencoder.com/2013/09/13/what-formats-do-i-need-for-html5-video/), and a good support chart [here](http://caniuse.com/#feat=video).

# Streaming

Video files can be big, and you don't want the user to have to wait for the whole video to download to their computer. You want the video to start playing as soon as enough of it has been downloaded to get started. In order to do this, mp4 files need to be organized so that certain data is at the beginning of the file. This script sets the "faststart" option to make sure that encoded videos are streamable.

# Quality and Size

These are adjustable variables at the top of the script. I didn't make them script arguments because usually I like to stick to a particular quality and size for a given project, and I didn't want to overcomplicate the script. Here's my rationale for the defaults: I certainly like me some HD video, but I wanted to find a best-balance between quality and streaming speed without having to encode multiple files. I think 480p is fine for most in that regard. For 16:9 aspect ratios, that means 854x480. At that size, 800k video bitrate is sufficient to make it look decent without making the file size too large.