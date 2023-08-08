<div align="center">
<img src="./assets/header.svg">

<ul>
<summary style="list-style-type: none;"><h1 style="display: inline-block;">webcamize</h1></summary>
<summary style="list-style-type: none;"><h2 style="display: inline-block;">Use any camera as a webcam!</h2></summary>
</ul>

<!-- <strong>
<samp>

[English](readme.md) · [简体中文](localized/readme.zh-CN.md) · [繁體中文](localized/readme.zh-TW.md) · [한국어](localized/readme.ko.md)

</samp>
</strong> -->

<img alt="Awesomeness badge" src="https://img.shields.io/badge/video-lots-orchid?style=flat-square">
<img alt="License" src="https://img.shields.io/github/license/weebney/webcamize?style=flat-square">
<img alt="GitHub top language" src="https://img.shields.io/github/languages/top/weebney/webcamize?style=flat-square">

<img alt="Awesomeness badge" src="https://img.shields.io/badge/awesome-extremely-limegreen?style=flat-square">


<br>
</div>

Webcamize allows you to use [basically any camera](http://www.gphoto.org/proj/libgphoto2/support.php) as a webcam on Linux—your DSLR, mirrorless, camcorder, point-and-shoot, or even your smartphone/tablet. It also gets many webcams that don't work out of the box on Linux up and running in a flash.
<div align="center" width="33%">
<br>

<img src="./assets/demo.gif" style="width:75%;">
</div>
<br>
<div align="center" width="33%">

There's literally only three steps...

**1​. Plug in some camera**

**2​. Run the `webcamize` command**

**3​. Now your camera is a webcam!**
</div>

It's really that easy! Webcamize is a tiny bash script that coordinates [gphoto2](http://gphoto.org/) and [ffmpeg](https://www.ffmpeg.org/) to capture video from any camera and output it to a live video device, ready to be used as a webcam. Whether it's for a Zoom meeting, a live streaming event, or a virtual conference, webcamize bridges the gap between high-end photography equipment and everyday tech usability. 

<!-- -->

<div align="center">
<br>

## Using webcamize

<img src="./assets/usage.svg">

</div>

<br>

Webcamize is designed to be as easy to use as possible. Just plug in your camera, then run the script:

```console
$ webcamize
webcamize: Starting camera on /dev/video0
```

In the vast majority of cases, that's all you'll need to do. You might be asked to enter your password for `modprobe` to enable the video device.

### Enabling Webcamize on Startup

First, [install webcamize](#installation)! Then, you can make webcamize run by default on startup with a systemd unit file included in this repository. Start by downloading the unit file into your unit files folder:

```console
$ curl -sSo /etc/systemd/system/webcamize.service https://raw.githubusercontent.com/weebney/webcamize/v1.1.2/webcamize.service > /dev/null && echo 'Successfully downloaded!'
Successfully downloaded!
```

Then, just enable it!

```console
$ systemctl enable webcamize
Created symlink /etc/systemd/system/multi-user.target.wants/webcamize.service → /etc/systemd/system/webcamize.service.
```

Webcamize should now run in the background automatically when you start up your PC.

### Advanced Usage

```console
$ webcamize --help
Usage: webcamize [OPTIONS...]
        -v, --version                   Print version info and quit
        -c, --camera NAME               Specify a gphoto2 camera to use; autodetects by default
        -d, --device NUMBER             Specify the /dev/video device number to use (default: 0)
        -g, --gphoto-args ARGS          Pass arguments to gphoto2 (default: "autofocusdrive=1")
        -f, --ffmpeg-args ARGS          Pass arguments to ffmpeg (default: "-vcodec rawvideo -pix_fmt yuv420p -threads 0")
        -h, --help                      Show this help message
```

#### Choosing a Different Camera

If you have multiple cameras gphoto2 compatible connected and available, you can specify the camera you want to use with the `--camera` flag. You can list the available cameras by running the following command:

```console
$ gphoto2 --summary | grep Model:
Model: Canon EOS 80D
Model: Sony Alpha-A7r III 
```

Once you know the name of the camera you want to use, just pass it to the `--camera` flag:

```console
$ webcamize --camera "Sony Alpha-A7r III"
webcamize: Starting Sony Alpha-A7r III on /dev/video0
```


#### Setting a Different Video Device

Want to use your camera as a webcam on `/dev/video4` instead of the default `/dev/video0`? Easy-peasy! Just set the `--device` flag:

```console
$ webcamize --device 4
webcamize: Starting camera on /dev/video4
```

#### Multi-Camera Setup

Alright mister show biz, here's how you can do a multiple camera setup; just chain together the `--device` and `--camera` flags to route multiple cameras into multiple video devices.

```console
$ webcamize --device 4 --camera "Canon EOS 80D" &
webcamize: Starting Canon EOS 80D on /dev/video4
$ webcamize --device 3 --camera "Sony Alpha-A7r III" &
webcamize: Starting Sony Alpha-A7r III on /dev/video3
```

#### Custom Arguments for gphoto2/ffmpeg

Unless you're really familiar with gphoto2/ffmpeg, it's inadvisable to pass custom arguments with the `--gphoto-args` and `--ffmpeg-args` flags; all these flags do is pass arguments into their respective command. As an example, here's the default flags for ffmpeg but edited to have a different thread count:

```console
$ webcamize --ffmpeg-args "-vcodec rawvideo -pix_fmt yuv420p -threads 2"
webcamize: Starting camera on /dev/video0
```

<!-- -->

<div align="center">
<br>

## Installation

<img src="./assets/installation.svg">

</div>

### Manual Installation

Webcamize is super easy to install—it only has a few additional dependencies that you should make sure are installed before beginning:

- [gphoto2](http://gphoto.org/)
- [ffmpeg](https://www.ffmpeg.org/)

#### Installation Instructions

> **Warning**
>
> You will probably have to run the below commands with `sudo`! Remember to double check what commands are doing if you're copying them from the internet, **especially** if they want you to use root privileges!
>

To get started, download the script to `/usr/local/bin/` (or somewhere else on your `$PATH`)

```console
$ curl -sSo /usr/local/bin/webcamize https://raw.githubusercontent.com/weebney/webcamize/v1.1.2/webcamize > /dev/null && echo 'Successfully downloaded!'
Successfully downloaded!
```

Then, make the script executable with `chmod`

```console
$ chmod a+x -v /usr/local/bin/webcamize
mode of '/usr/local/bin/webcamize' changed from 0644 (rw-r--r--) to 0755 (rwxr-xr-x)
```

**That's all; you're ready to go!** 🎉🎉

Give it a quick test just to make sure it's working:

```console
$ webcamize -v
webcamize: 1.1.2
```

... and here's everything again as a one-liner to reward those patient few who read all the way through the instructions before starting!

```console
$ sudo bash -c "curl -sSo /usr/local/bin/webcamize https://raw.githubusercontent.com/weebney/webcamize/v1.1.2/webcamize && chmod a+x /usr/local/bin/webcamize" && echo -e '\n\e[32mSuccessfully installed webcamize!\e[0m'
Successfully installed webcamize!
```
<!--
### Package Managers

Webcamize is brand new and probably not available via your distribution's package manager (yet)—if you want to support the project by maintaining a package for webcamize, I'd be eternally grateful!

#### Arch Linux (AUR)

Webcamize is available from the Arch User Repository as `webcamize`

```console
$ yay -S webcamize
```
 -->

<!-- -->

<div align="center">
<br>

## Issues & Contributing

<img src="./assets/issues.svg">

</div>

<br>

This project follows the following philosophy:

- If this project is not helping you, then there is a bug
- If you are having a bad time using this project, then there is a bug
- If the documentation is confusing, then the documentation is buggy
- If there is a bug in this project, then we can work together to fix it.

If you get stumped, find a bug, have a bad time, or have a suggestion, please open an issue in the [issue tracker](https://github.com/weebney/webcamize/issues/)—noobs welcome!

### Contributing

Webcamize has only a few contribution rules to keep the project's growing at a sustainable rate:

- Please squash your commits before submitting a pull request.
- Pull requests should be split into multiple pull requests where possible.



<div align="center">
<br>

## Cheers!

<img src="./assets/cheers.svg">

</div>

<br>

This is just a little script I cooked up one afternoon—the software it depends on is where the real magic happens. I know it can be corny, but I do feel it's important that we remind ourselves that we stand on the shoulders of giants. With that being said, a big thanks goes out from me to:

- [Michael Niedermayer](https://github.com/michaelni) and all other contributors to [ffmpeg](https://github.com/FFmpeg/FFmpeg/graphs/contributors) for their incredible work on the absolute behemoth and marvel of software that is ffmpeg.
- [Marcus Meissner](https://github.com/msmeissn),  [Hans Niedermann](https://github.com/ndim), and the other contributors to [gphoto2](https://github.com/gphoto/gphoto2) and [libgphoto2](https://github.com/gphoto/libgphoto2). This project is pretty small and obscure compared to how powerful and well constructed it is. Definitely check it out!
- [You](https://en.wikipedia.org/wiki/You_(Time_Person_of_the_Year)), the reader! Thank you for using, supporting, and contributing to webcamize; without you, this project would not be possible.

<br>

![GitHub Stars Over Time](https://starchart.cc/weebney/webcamize.svg)

![](././assets/nightynight.svg)
