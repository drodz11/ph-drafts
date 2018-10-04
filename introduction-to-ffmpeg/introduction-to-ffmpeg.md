---
title: |
    Introduction to Audiovisual Transcoding, Editing, and Analysis with FFmpeg
authors:
- David Rodriguez
date: 2018-08-03
reviewers:
layout: lesson
---

# Introduction
The Digital Humanities, as a discipline, have historically focused almost exclusively on the analysis of textual sources through computational methods (Hockey, 2004). However, there is growing interest in the field around using computational methods for the analysis of audiovisual cultural heritage materials as indicated by the creation of the [Alliance of Digital Humanities Organizations Special Interest Group: Audiovisual Materials in the Digital Humanities](https://avindhsig.wordpress.com/) and [the rise in submissions related to audiovisual topics at the global ADHO conference](https://figshare.com/articles/AV_in_DH_State_of_the_Field/5680114) over the past few years. Newer investigations, such as [Distant Viewing TV](https://distantviewing.org/), also indicate a shift in the field toward projects concerned with using computational techniques to expand the scope of materials digital humanists can explore. As Erik Champion states, "The DH audience is not always literature-focused or interested in traditional forms of literacy" and applying digital methodologies to the study of audiovisual culture is an exciting and emerging facet of Digital Humanities (Champion, 2017). There are many valuable, free, and open-source tools and resources available to those interested in working with audiovisual materials (for example, the Programming Historian tutorial [Editing Audio with Audacity](/en/lessons/editing-audio-with-audacity)) and this tutorial will introduce another: FFmpeg.

[FFmpeg](https://www.ffmpeg.org/about.html) is "the leading multimedia framework able to decode, encode, transcode, mux, demux, stream, filter and play pretty much anything that humans and machines have created." Many common software applications and websites use FFmpeg to handle reading and writing audiovisual files, including VLC, Google Chrome, YouTube, [and many more.](https://trac.ffmpeg.org/wiki/Projects) In addition to being a software and web-developer tool, FFmpeg can be used at the command-line to perform many common, complex, and important tasks related to audiovisual file management, alteration, and analysis. These kinds of processes, such as editing, re-encoding, or extracting metadata from files, usually require access to other software (such as a non-linear video editor like Adobe Premire or Final Cut Pro), but FFmpeg allows a user to operate on audiovisual files directly without the use of third-party software or interfaces. As such, knowledge of the framework empowers users to manipulate audiovisual materials to meet their needs with a free, open-source solution that carries much of the functionality of expensive audio and video editing software. This tutorial will provide an introduction to reading and writing FFmpeg commands and walkthrough a use cases for how the framework can be used in Digital Humanities scholarship (specifically, how FFmpeg can be used to extract, analyze, and visualize color data from an archival video source).

## Learning Objectives
* Learn how to install FFmpeg on your computer or use a demo version in your web browser
* Understand the basic structure and syntax of FFmpeg commands
* Learn several useful commands such as:
  * "Re-wrapping" (change file container) & Transcoding (re-encode files)
  * Demuxing (seperating audio and video tracks)
  * Trimming/Editing files
  * File playback with FFplay
  * Creating vectorscopes for color data visualization
  * Generating color data reports with FFprobe
* Use color data created with FFprobe to create visualizations
* Introduce outside resources for further exploration and experimentation

## Prerequisites
Before starting this tutorial, you should be comfortable with locating and using your computer's [Terminal](https://en.wikipedia.org/wiki/Terminal_(macOS)) or other command-line interface, as this is where you will be entering and executing FFmpeg commands. If you need instruction on how to access and work at the command-line, I recommend the Program Historian's [Bash tutorial](/en/lessons/intro-to-bash) for Mac and Linux users or the [Windows PowerShell tutorial](/en/lessons/intro-to-powershell#quick-reference). Additionally, a basic understanding of audiovisual [codecs](https://en.wikipedia.org/wiki/Codec) and [containers](https://en.wikipedia.org/wiki/Digital_container_format) will also be useful to understanding what FFmpeg does and how it works. We will provide some additional information and discuss codecs and containers in a bit more detail in the Command Examples section of this tutorial.

# Installing FFmpeg
Installing FFmpeg can be the most difficult part of using FFmpeg. Thankfully, there are some helpful guides and resources available for installing the framework based on your operating system.

> * **Note**: New versions of FFmpeg are released approximately every 6 months. To keep track of these updates, follow FFmpeg on [Twitter](https://twitter.com/FFmpeg) or through its website. New versions of FFmpeg usually contain features such as new and updated filters, codec compatibilities, and bug fixes. The syntax of FFmpeg commands do not change with these updates and old capabilities are rarely removed. To get an idea of what kinds of features come with these updates, you can scroll through previous update announcements in the [News](https://www.ffmpeg.org/index.html#news) section of the FFmpeg website.

## For Mac OS Users
The simplest option is to use a package manager such as [Homebrew](https://brew.sh/)
to install FFmpeg and ensure it remains in the most up-to-date version. Homebrew is also useful in ensuring that your computer has the necessary dependencies installed to ensure FFMpeg runs properly. To complete this kind of installation, follow these steps:
* Install Homebrew following the instructions in the above link
* Run `brew install ffmpeg` in your Terminal to initiate a basic installation.
    * **Note**: Generally, it is recommended to install FFMpeg with additional features than what is included in the basic installation. Including additional options will provide access to more of FFmpeg's tools and functionalities. Reto Kromer's [Apple installation guide](https://avpres.net/FFmpeg/install_Apple.html) provides a good set of additional options:
    `brew install ffmpeg --with-sdl2 --with-freetype --with-openjpeg --with-x265 --with-rubberband --with-tesseract`
    * For an explanation of these additional options, refer to [Ashley Blewer's FFmpeg guide](https://training.ashleyblewer.com/presentations/ffmpeg.html#10)
* After installing, it is best practice to update Homebrew and FFmpeg to ensure all dependencies and features are most up-to-date by running:
  `brew update && brew upgrade ffmpeg`
* For more installation options for Mac OS, see the [Mac OS FFmpeg Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide/macOS)

## For Windows Users
Windows users can use the package manager [Chocolately](https://chocolatey.org/) to install and maintain FFmpeg. Reto Kromer's [Windows instllation guide](https://avpres.net/FFmpeg/install_Windows.html) provides all the necessary information to use Chocolately or to install the software from a build.

## For Linux Users
[Linuxbrew](http://linuxbrew.sh/), a program similar to Homebrew, can be used to
install and maintain FFmpeg in Linux. Reto Kromer also provides a helpful [Linux installation guide](https://avpres.net/FFmpeg/install_Linux.html)
that closely resembles the Mac OS installation.

## Other Installation Resources

* [Download Packages](https://www.ffmpeg.org/download.html)
  * FFmpeg allows access to binary files and source code directly through its website, enabling users to build the framework without a package manager. It is likely that only advanced users will want to follow this option.
* [FFmpeg Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide)
  * The FFmpeg Wiki page also provides a compendium of guides and strategies for building FFmpeg on your computer.

## Testing the Installation
* To ensure FFmpeg is installed properly, run:
`ffmpeg -version`
* If you see a long output of information, the installation was successful! It
should look similar to this:

`ffmpeg version 4.0.1 Copyright (c) 2000-2018 the FFmpeg developers
built with Apple LLVM version 9.1.0 (clang-902.0.39.1)
configuration: --prefix=/usr/local/Cellar/ffmpeg/4.0.1 --enable-shared --enable-pthreads --enable-version3 --enable-hardcoded-tables --enable-avresample --cc=clang --host-cflags= --host-ldflags= --enable-gpl --enable-ffplay --enable-libfreetype --enable-libmp3lame --enable-librubberband --enable-libtesseract --enable-libx264 --enable-libx265 --enable-libxvid --enable-opencl --enable-videotoolbox --disable-lzma --enable-libopenjpeg --disable-decoder=jpeg2000 --extra-cflags=-I/usr/local/Cellar/openjpeg/2.3.0/include/openjpeg-2.3
libavutil      56. 14.100 / 56. 14.100
libavcodec     58. 18.100 / 58. 18.100
libavformat    58. 12.100 / 58. 12.100
libavdevice    58.  3.100 / 58.  3.100
libavfilter     7. 16.100 /  7. 16.100
libavresample   4.  0.  0 /  4.  0.  0
libswscale      5.  1.100 /  5.  1.100
libswresample   3.  1.100 /  3.  1.100
libpostproc    55.  1.100 / 55.  1.100`

* If you see something like `-bash: ffmpeg: command not found` then something has
gone wrong.
  * Note: If you are using a package manager it is unlikely that you will encounter this error message. However, if there is a problem after installing with a package manager, it is likely the issue is with the package manager itself as opposed to FFmpeg. Consult the Troubleshooting sections for [Homebrew](https://docs.brew.sh/Troubleshooting), [Chocolatey](https://chocolatey.org/docs/troubleshooting), or [Linuxbrew](http://linuxbrew.sh/) to ensure the package manager is functioning properly on your computer. If you are attempting to install without a package manager and see this error message, cross-reference your method with the FFmpeg Compilation Guide provided above.

## Using FFmpeg in a web browser (without installing)
If you do not want to install FFmpeg on your computer but would like to become familiar with using it at the command-line, Brian Grinstead's [videoconverter.js](https://bgrins.github.io/videoconverter.js/demo/) provides a way to run FFmpeg commands and learn its basic functions in the web-browser of your choice.
  **Note**: This resource runs on an older version of FFmpeg and may not contain all the features of the most recent version.

# Basic Structure and Syntax of FFmpeg commands
Basic FFmepg commands consist of four elements:

`[Command Prompt] [Input File] [Flags/Actions] [Output File]`

* A command prompt will begin every ffmpeg command. Depending on the use, this
prompt will either be `ffmpeg` (changing files), `ffprobe` (gathering metadata from files), or `ffplay` (playback of files).
* Input files are the files being read, edited, or examined. The output file is
the new file created by the command or the report generated by an `ffprobe` command.
* Flags and actions are the things you are telling FFmpeg to do the input files.
Most commands will contain multiple flags and actions of various complexity.

Written generically, a basic FFmpeg command looks like this:

`ffmpeg -i /filepath/input_file.ext -flag some_action /filepath/output_file.ext`

Next, we will look at some examples of several different commands that use this structure and syntax. These commands will also demonstrate some of FFmpeg's most basic, useful functions and allow us to become more familiar with how digital audiovisual files are constructed.

# Getting Started
For this tutorial, we will be taking an archival film called *[Destination Earth]*(https://archive.org/details/4050_Destination_Earth_01_47_33_28) as our object of study. This film has been made available by the [Prelinger Archives](https://en.wikipedia.org/wiki/Prelinger_Archives) collection on the [Internet Archive](https://archive.org/). Released in 1956, this film is a prime example of pro-capitalist, Cold War-era propaganda produced by the [American Petroleum Institute](https://en.wikipedia.org/wiki/American_Petroleum_Institute) and [John Sutherland Productions](https://en.wikipedia.org/wiki/John_Sutherland_(producer). Utilizing the [Technicolor](https://en.wikipedia.org/wiki/Technicolor) process, this science-fiction animated short tells a story of a Martian society living under an oppressive, Communist-like government and their efforts to improve their industrial methods. They send an emissary to Earth who discovers the key to this is oil refining and free-enterprise. We will be using this video to introduce some of the basic functionalities of FFmpeg and analyzing its color properties in relation to its propagandist rhetoric.

{Image - Destination Earth title card}

For this tutorial, you will need to:
* Navigate to the *[Destination Earth]*(https://archive.org/details/4050_Destination_Earth_01_47_33_28) page on IA
* Download two video files: the "MPEG4" (file extension `.m4v`) and "OGG" (file extension `.ogv`) versions of the film (see image below)
* Save these two video files in the same directory somewhere on your computer. Save them with the file names `destEarth` followed by its extension.

{Image File_Selection}

# Preliminary Command Examples

## Viewing Basic Metadata with FFprobe
Before we begin manipulating our `destEarth` files, let's use FFmpeg to examine some basic information about the file itself using a simple `ffprobe` command. This will help illuminate how digital audiovisual files are constructed and provide a foundation for the rest of the tutorial. Navigate to the file's directory and execute:

`ffprobe destEarth.ogv`

You will see the file's basic technical metadata printed in the `stdout`:

{% include figure.html filename="ffprobe_ogg.png" caption="The output of a basic `ffprobe` command with destEarth.ogv" %}

Run another `ffprobe` command, this time with the `.m4v` file:

`ffprobe destEarth.m4v`

Again you'll see the basic technical metadata printed to the `stdout`:

{% include figure.html filename="ffprobe_mp4.png" caption="The output of a basic `ffprobe` command with destEarth.m4v" %}

The `Input #0` line of the reports identifies the **container** as [ogg](https://en.wikipedia.org/wiki/Ogg) and [mp4](https://en.wikipedia.org/wiki/MPEG-4_Part_14), respectively. You'll also notice that the report for the `.m4v` file contains multiple containers on the `Input #0` line like `mov` and `m4a`. It isn't necessary to get too far into the details for the purposes of this tutorial, but be aware that the `mp4` and  `mov` containers come in many "flavors" and different file extensions but are all very similar in their technical construction, and as such you may see them grouped together in technical metadata. Similarly, the `ogg` file has the extension `.ogv`, a "flavor" or variant of the `ogg` format.

Containers (also called "wrappers") provide the file with structure for its various streams. Different containers (other common ones include `.mkv`, `.avi`, and `.flv`) have different features and compatibilities with various software. We will examine how and why you might want to change a file's container in the next command.

The lines `Stream #0:0` and `Stream #0:1` provide information about the file's streams (i.e. the content you see on screen and hear through your speakers) and identify the **codec** of each stream as well. Codecs specify how information is encoded/compressed (written and stored) and decoded/decompressed (played back).

The `.ogv` file's video stream (`Stream #0:0`) uses the [theora](https://en.wikipedia.org/wiki/Theora) codec while the audio stream (`Stream #0:1`) uses the [vorbis](https://en.wikipedia.org/wiki/Vorbis) codec. If we look at the `.m4v` file's video stream (`Stream #0:0`) we can see it uses the [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) codec while the audio stream (`Stream #0:1`) uses the [aac](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) codec.

Codecs, to a much greater extent than containers, determine an audiovisual file's quality and compatibility with different software and platforms (other common codecs include `DNxHD` and `ProRes` for video and `mp3` and `FLAC` for audio). We will examine how and why you might want to change a file's codec in the next command as well.

Now that we know more about the technical make-up of our file, we can begin exploring the transformative features and functionalities of FFmpeg (we will use `ffprobe` again later in the tutorial to conduct more advanced color metadata extraction).

## Changing Containers and Codecs (Re-Wrap and Transcode)
Depending on your operating system, you may have one or more media players installed. For the purposes of demonstration, let's see what happens if you try to open `destEarth.ogv` using the QuickTime media player that comes with Mac OSX:

{% include figure.html filename="QT_fail.png" caption="Proprietary media players such as QuickTime are often limited in the kinds of files they can work with" %}

One option when faced with such a message is to simply use another media player. [VLC](https://www.videolan.org/vlc/index.html), which is built with FFmpeg, is an excellent open-source alternative) but simply "using another software" may not always be a viable solution (and you may not always have another version of a file to work with, either). Many popular video editors such as Adobe Premiere, Final Cut Pro, and DaVinci Resolve all have their own limitations on the kinds of formats they are compatible with. Further, different web-platforms and hosting/streaming sites such as Vimeo have [their own requirements as well.](https://vimeo.com/help/compression) As such, it is important to be able to re-wrap and transcode your files to meet the various specifications for playback, editing, and digital publication.

> **Note**: For a complete list of codecs and containers supported by your installation of FFmpeg, run `ffmpeg -codecs` and `ffmpeg -formats`, respectively, to see the list printed to your `stdout`.

As an exercise in learning basic FFmpeg syntax and learning how to transcode between formats, we will begin with our `destEarth.ogv` file and write a new file with video encoded to `H.264`, audio to `AAC`, and wrapped in an `.mp4` container, a very common and highly-portable combination of codecs and container that is practically identical the `.m4v` file we originally downloaded. Here is the command you will execute along with an explanation of each part of the syntax:

`ffmpeg -i destEarth.ogv -c:v libx264 -c:a aac destEarth_transcoded.mp4`

* `ffmpeg` = starts the command
* `-i destEarth.ogv` = specifies the input file
* `-c:v libx264` = copy the video stream to the H.264 codec
* `-c:a aac` = copy the audio stream to the AAC codec
* `destEarth_transcoded.mp4` = specifies the output file. Note this is where the new container type is specified

If you execute this command as it is written and in the same directory as `destEarth.ogv`, you will see a new file called `destEarth_transcoded.mp4` appear in the directory. If you are operaing in Mac OSX, you will also be able to play this new file with QuickTime. A full exploration of codecs, containers, compadibility, and file extension conventions is beyond the scope of this tutorial, however this preliminary set of examples should give those less familiar with how digital audiovisual files are constructed a baseline set of knowledge that will enable them to complete the rest of the tutorial.

## Creating Excerpts & Demuxing Audio & Video
Now that we have a better understanding of streams, codecs, and containers, lets look at ways FFmpeg can help us work with video materials at a more granular level. For this tutorial, we will examine two discrete sections of *Destination Earth* to compare how color is used. We will create and prepare these excerpts for analysis using a command that performs two different functions:

* First, the command will create two excerpts from `destEarth.m4v`
* Second, the command will remove ("demux") the audio components (`Stream #0:1`) from these excerpts
  * We are removing the audio in the interest in promoting good practice in saving storage space (the audio information is not necessary for color analysis) and will likely be useful if you hope to this is kind of analysis at larger scales. More on how to do this near the end of the tutorial.

The first excerpt we will be making is a sequence near the beginning of the film depicting the difficult conditions and downtrodden life of the Martian society. The following command specifies start and end points of the excerpts in addition to telling FFmpeg to retain all information in each stream without transcoding anything.

`ffmpeg -i destEarth.m4v -ss 00:01:00 -to 00:04:35 -c:v copy -an destEarth_Mars_video.mp4`

* `ffmpeg` = starts the command
* `-i destEarth.m4v` = specifies the input file
* `-ss 00:01:00` = sets start point at 1 minute from start of file
* `-to 00:04:45` = sets end point to 4 minutes and 35 seconds from start of file
* `-c:v copy` = copy the video stream directly, without re-encoding
* `-an` = tells FFmpeg to ignore audio stream when writing the output file.
* `destEarth_Mars.mp4` = specifies the output file

We will now run a similar command to create an "Earth" excerpt. This portion of the film has a similar sequence depicting the wonders of life on Earth and the richness of its society thanks to free-enterprise capitalism and the use of oil and petroleum products:

`ffmpeg -i destEarth.m4v -ss 00:07:30 -to 00:11:05 -c copy -an destEarth_Earth_video.mp4`

You should now have two new files in your directory called `destEarth_Mars_video.mp4` and `destEarth_Earth_video.mp4`. You can test one or both files (or any of the other files in the directory) using the `ffplay` feature of FFmpeg as well. Simply run:

`ffplay destEarth_Mars_video.mp4`

and/or

`ffplay destEarth_Earth_video.mp4`

You will see a window open and the video will begin at the specified start point, play through once, and then close. You should also notice that the videos created with the last two commands do not have a soundtrack.

  >**Note**:`FFplay` is a very versatile media player that comes with a number of [options](https://ffmpeg.org/ffplay.html#Options) for customizing playback. For example, adding `-loop 0` to the command will loop playback indefinitely.

We have now created our two excerpts for analysis. In the next part of the tutorial, we will examine and extract color data from the video files and then use [plot.ly](https://plot.ly/#/) to create simple visualizations from this information.

## Color Data Analysis
The use of [digital tools to analyze color information](https://filmcolors.org/2018/03/08/vian/) in motion pictures is another emerging facet of DH scholarship that overlaps with traditional film studies. The [FilmColors](https://filmcolors.org/) project, in particular, at the University of Zurich, interrogates the critical intersection of film's "formal aesthetic features to [the] semantic, historical, and technological aspects" of its production, reception, and dissemination through the use of digital analysis and annotation tools (Flueckiger, 2017). Although there is no standardized method for this kind of investigation at the time of this writing, the `ffprobe` command offered here is a powerful tool for extracting such quantitative information for use in computational analysis.

### Vectorscopes

### Color Data Extraction with FFprobe
At the beginning of this tutorial, we used an `ffprobe` command to view our file's basic metadata printed to the `stdout`. In these next examples, we'll use `ffprobe` to extract color data from our video excerpts and output this information to `.csv` files. Within our `ffprobe` command, we are going to use the `signalstats` filter to create `.csv` reports of median color [hue](https://en.wikipedia.org/wiki/Hue) information for each frame in the video stream of `destEarth_Mars_video.mp4` and `destEarth_Earth_video.mp4`, respectively.

`ffprobe -f lavfi -i movie=destEarth_Mars_video.mp4,signalstats -show_entries frame=pkt_pts_time:frame_tags=lavfi.signalstats.HUEMED -print_format csv > destEarth_Mars_hue.csv`

* `ffprobe` = starts the command
* `-f lavfi` = specifies the [libavfilter](https://ffmpeg.org/ffmpeg-devices.html#lavfi) virtual input device as the chosen format. This is necessary when using `signalstats` and many filters in more complex FFmpeg commands.
* `-i movie=destEarth_Mars_video.mp4` = name of input file
* `,signalstats` = specifies use of the `signalstats` filter with the input file
* `-show_entries` = sets list of entries that will be shown in the report. These are specified by the next options
* `frame=pkt_pts_time` = specifies showing each frame with its corresponding `pkt_pts_time`, creating a unique entry for each frame of video.
* `:frame_tags=lavfi.signalstats.HUEMED` = creates a tag for each frame that contains the median hue value
* `-print_format csv` = specifies the format of the metadata report
* `> destEarth_Mars_hue.csv` = writes a new `.csv` file containing the metadata report. The file extension should match the format specified by the `print_format` flag.

Next, run the same command for the "Earth" excerpt:

`ffprobe -f lavfi -i movie=destEarth_Earth.mp4,signalstats -show_entries frame=pkt_pts_time:frame_tags=lavfi.signalstats.HUEMED -print_format csv > destEarth_Earth_hue.csv`

> **Note**: For more information about the `signalstats` filter and the various metrics that can be extracted from video streams, refer to the FFmpeg's [Filters Documentation](https://ffmpeg.org/ffmpeg-filters.html#signalstats-1).

You should now have two `.csv` files in your directory. If you open these in a text editor or spreadsheet program, you will see three columns of data:

{Image csv head}

Going from left to right, the first two columns give us information about where we are in the video. The decimal numbers represent fractions of a second that also roughly correspond to the video's time-base of 30fps. As such, each row in our `.csv` corresponds to one frame of video. The third column carries a whole number between 0-360, and this value represents the median hue for that frame of video. This numerical data the underlying quantitative data of the vectorscope's visualization.

## Visualize Audio and Video Information (Create vectorscopes and waveforms)
Data visualization is a concept familiar to digital humanists. For years, sound and video professionals have also relied on data visualization to work with audio-visual content. These types of visualizations include [vectorscopes](https://en.wikipedia.org/wiki/Vectorscope#Video) (to visualize video color information) and [waveform patterns](https://en.wikipedia.org/wiki/Waveform) (to visualize audio signal data). Although this kind of data visualization is not the kind traditionally created by DH scholarship, FFmpeg includes a number of tools and libraries that can be used to visualize sound and image information that can potentially expand the field and open new lines of critical inquiry.

This section will provide commands for creating a few different types of visualizations with examples of the intended result. Additionally, these commands are more complex than the previous examples in this tutorial and provide further insight into creating complex options for FFmpeg commands.

### Vectorscope (Video)
Our first example will build on our `ffplay bigbuckbunny.webm` command to include a vectorscope as part of the playback image. To play the video accompanied by a vectorscope:

`ffplay bigbuckbunny.webm -vf "split=2[m][v], [v]vectorscope=b=0.7:m=color3:g=green[v],[m][v]overlay=x=W-w:y=H-h"`

* `ffplay` = starts the command
* `bigbuckbunny.webm` = path and name of input file
* `-vf` = creates a [filter-graph](https://trac.ffmpeg.org/wiki/FilteringGuide) to use for the streams
* `"` = quotation mark to start the filter-graph. Information inside the quotation marks will specify the parameters of the vectorscope's appearance and position.
* `split=2[m][v]` = splits the input into two identical outputs called `[m]` and `[v]`
* `,` = comma signifies another parameter is coming
* `[v]vectorscope=b=0.7:m=color3:g=green[v]` = assigns the `[v]` output the vectorscope filter
* `[m][v]overlay=x=W-w:y=H-h` = overlays the vectorscope on top of the video image in a certain location (in this case, in the lower right corner of the frame)
* `"` = ends the filter-graph

{% include figure.html filename="vector_bbb.png" caption="A sample video frame with a vectorscope" %}

As previously discussed, this `ffplay` command will playback the video one time and then close the window. You can add a `-loop` option, but it is likely that you'll want to create a new file with the vectorscope included in the image for later analysis and investigation. To accomplish this, we need to change the command prompt to `ffmpeg` and specify the parameters of the output file. Our new command looks like this:

`ffmpeg -i bigbuckbunny.webm -vf "split=2[m][v], [v]vectorscope=b=0.7:m=color3:g=green[v],[m][v]overlay=x=W-w:y=H-h" -c:v libx264 -c:a copy bbb_vectorscope.mp4`

Note the slight but important changes in syntax:
  * We have added an `-i` flag because it is an `ffmpeg` command
  * We have specified the output video codec as [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) with the flag `-c:v libx264` and are not re-encoding the audio (`-c:a copy`), although you can specify another audio codec here if necessary.
  * We have specified the path and name of the output file

Generally, `ffplay` commands can be re-written as `ffmpeg` commands with similar tweaks to syntax, although commands containing more complex options may require more significant adjustments.

### Waveform (Audio)
Waveforms are one of the most common visualizations of sonic information. Most simply, a waveform expresses changes in an audio signal's [amplitude](https://en.wikipedia.org/wiki/Amplitude) over a given period of time. To create a single image of a single-channel (mono) waveform from an input file:

`ffmpeg -i bigbuckbunny.webm -filter_complex "aformat=channel_layouts=mono,showwavespic=s=600x240" -frames:v 1 bbb_waveimage.png`

* `ffmpeg` = starts the command
* `-i bigbuckbunny.webm` = path and name of input file
* `-filter_complex` = creates a complex filter-graph
* `"` = quotation mark to start the filter-graph. Information inside the quotation
marks will specify the parameters of the waveform's appearance and size.
* `aformat=channel_layouts=mono` = sets the waveform to represent the audio information as a single-channel (mono)
* `showwavespic=s=600x240` = activates the `showwavespic` filter; `s=600x240` designates the size
of the image to be 600 pixels wide and 240 pixels tall.
* `"` = ends the filter-graph
* `frames:v 1` = assigns the output to one single frame (one image)  
* `bbb_waveimage.png` = path and name of output file. The extension should be an appropriate image format such as `.jpeg` or `.png`

{% include figure.html filename="bbb_waveimage.png" caption="Single-image output of the above command" %}

#### To create a video waveform:
A static image of an audio signal can certainly be useful, but you may want to also output a video of the waveform as well. This can more dynamically and vividly visualize how an audio signal changes in real-time:

`ffmpeg -i bigbuckbunny.webm -filter_complex "[0:a]showwaves=s=1280x720:mode=line,format=yuv420p[v]" -map "[v]" -map 0:a -c:v libx264 -c:a copy bbb_wavevideo.mp4`

* `ffmpeg` = starts the command
* `-i bigbuckbunny.webm` = path and name of input file
* `-filter_complex` = creates a complex filter-graph
* `"` = quotation mark to start the filter-graph. Information inside the quotation
marks will specify the parameters of the waveform's appearance and size.
* `[0:a]showwaves=s=1280x720` = activates the `showwaves` filter and sets it to a certain size. This will be the size of the output video.
* `:mode=line,format=yuv420p[v]` = specifies what style of graph will be created in addition to the colorspace `yuv` and resolution `420p`
* `"` = ends the filter-graph
* `-map "[v]" -map 0:a` = maps the output of the filter-graph onto the output file
* `-c:v libx264 -c:a copy` = encode output video with an H.264 codec; encode output audio with the same codec as the input file
* `bbb_wavevideo.mp4` = path and name of output file.

{% include figure.html filename="waveform-video.gif" caption="GIF representation of the video output of the above command" %}

# Conclusion
In this tutorial, we have learned:
  * To install FFmpeg on different operating systems and how to access the framework in the web-browser
  * The basic syntax and structure of FFmpeg commands
  * To view basic technical metadata of an audiovisual file
  * To transform an audiovisual file through transcoding and re-wrapping
  * To parse and edit that audiovisual file by demuxing it and creating excerpts
  * To playback audiovisual files using `ffplay`
  * To generate different kinds of metadata using `ffprobe`
  * To generate different kinds of audiovisual data visualizations using complex filters and syntax

At a broader level, this tutorial aspires to provide an informed and enticing introduction to how audiovisual tools and methodologies can be incorporated in Digital Humanities projects and practices. With open and powerful tools like FFmpeg, there is vast potential for expanding the scope of the field to include more rich and complex types of media and analysis than ever before.

## Further Resources
FFmpeg has a large and well-supported community of users across the globe. As such, there are many open-source and free resources for discovering new commands and techniques for working with audio-visual media. Please contact the author with any additions to this list, especially educational resources in Spanish for learning FFmpeg.

* The Official [FFmpeg Documentation](https://www.ffmpeg.org/ffmpeg.html)
* [FFmpeg Wiki](https://trac.ffmpeg.org/wiki/WikiStart)
* [ffmprovisr](https://amiaopensource.github.io/ffmprovisr/) from the [Association of Moving Image Archivists](https://amianet.org/)
* Ashley Blewer's [Audiovisual Preservation Training](https://training.ashleyblewer.com/)
* Andrew Weaver's [Demystifying FFmpeg](https://github.com/privatezero/NDSR/blob/master/Demystifying_FFmpeg_Slides.pdf)
* Ben Turkus' [FFmpeg Presentation](https://docs.google.com/presentation/d/1NuusF948E6-gNTN04Lj0YHcVV9-30PTvkh_7mqyPPv4/present?ueb=true&slide=id.g2974defaca_0_231)
* Reto Kromer's [FFmpeg Cookbook for Archivists](https://avpres.net/FFmpeg/)

## Open-Source AV Analysis Tools using FFmpeg

* [MediaInfo](https://mediaarea.net/en/MediaInfo)
* [QC Tools](https://bavc.org/preserve-media/preservation-tools)

# References

* Camlot, J. (2015) “Historicist Audio Forensics: The Archive of Voices as Repository of Material and Conceptual Artefacts.”19: Interdisciplinary Studies in the Long Nineteenth Century. 2015(21)

* Champion, E. (2017) “Digital Humanities is text heavy, visualization light, and simulation poor,” Digital Scholarship in the Humanities 32(S1), i25-i32

* Flueckiger, B. (2017). "A Digital Humanities Approach to Film Colors". The Moving Image, 17(2), 71-94.

* Hockey, S. (2004) “The History of Humanities Computing,” A Companion to Digital Humanities, ed. Susan Schreibman, Ray Siemens, John Unsworth. Oxford: Blackwell

# About the Author

Dave Rodriguez is an audiovisual archivist, curator, and filmmaker. He is currently a Resident Librarian at Florida State University.

### This tutorial was made possible with the support of the British Academy and written during the Programming Historian Workshop at La Universidad de Los Andes in Bogotá, Colombia, 31 July - 3 August, 2018.
