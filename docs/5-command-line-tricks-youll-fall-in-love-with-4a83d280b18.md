# 你会爱上的 5 个命令行技巧

> 原文：<https://blog.devgenius.io/5-command-line-tricks-youll-fall-in-love-with-4a83d280b18?source=collection_archive---------13----------------------->

通过命令行界面(CLI)与计算机交互是一项强大的技术，因为它极大地提高了开发过程中的生产率。事实上，掌握命令行是每个程序员或数据科学家的必备技能。

从我早期编程开始，我就一直使用 CLI 来安装库或执行 python 命令。但是随着我学习更新的命令，特别是在编程领域之外，我比以往任何时候都更喜欢 CLI。下面列出了我最喜欢的五个命令，我相信你们也会喜欢的:

# 1.视频下载

下载视频+音频单个文件或整个播放列表。youtube-dl 是一个命令行程序，允许你从 youtube 和 Vimeo 等其他网站下载视频。它需要 Python 解释器，版本 2.6、2.7 或 3.2+，并且它不是特定于平台的，可以在您的 Unix 机器、Windows 或 macOS 上工作。

安装:

可以下载 [youtube-dl](https://github.com/rg3/youtube-dl) 复制到/usr/local/bin/。它在 [github](https://github.com/ytdl-org/youtube-dl) 上发布到公共领域，这意味着你可以修改它，重新发布它，或者按照你喜欢的方式使用它。

```
sudo curl -L [https://yt-dl.org/downloads/latest/youtube-dl](https://yt-dl.org/downloads/latest/youtube-dl) -o /usr/local/bin/youtube-dlsudo chmod a+rx /usr/local/bin/youtube-dl
```

或者，您也可以使用画中画:

```
sudo -H pip install --upgrade youtube-dl
```

语法:

```
youtube-dl [OPTIONS] URL [URL...] Options
   -F, --list-formats         List all available formats.
   -f, --format FORMAT        Video format code, see example below.
   -i, --ignore-errors        Continue on download errors, for example to skip unavailable
                              videos in a playlist.
   --abort-on-error           Cancel downloading of further videos (in the playlist or the
                              command line) if an error occurs.
   --no-playlist              Download only the video, if the URL refers to a video and a playlist.
   --yes-playlist             Download the playlist, if the URL refers to a video and a playlist.
   -a, --batch-file FILE      File containing URLs to download ('-' for stdin.)
   --restrict-filenames       Restrict filenames to only ASCII characters, and avoid "&" and
                              spaces in filenames.
   -w, --no-overwrites        Do not overwrite files.
   --write-description        Write video description to a .description file.
   --write-info-json          Write video metadata to a .info.json file.
   --write-annotations        Write video annotations to a .annotations.xml file.
   -q, --quiet                Activate quiet mode.
   --no-mtime                 Set the file last modified date/time to the download date/time.
   --no-warnings              Ignore warnings.
   --console-title            Display progress in console titlebar.
   --sleep-interval SECONDS   Number of seconds to sleep before each download.
   -h, --help                 Print this help text and exit.
   -U, --update               Update this program to latest version. Make sure that you have
                              sufficient permissions (run with sudo if needed.)
   --version                  Print program version and exit.
```

用法:

首先用-F 列出可用的格式

```
$ youtube-dl -F [https://www.youtube.com/watch?v=VG1VVFfOnYQ](https://www.youtube.com/watch?v=VG1VVFfOnYQ)140          m4a        audio only DASH audio  127k , m4a_dash container, aac  @128k (44100Hz), 3.77MiB
141          m4a        audio only DASH audio  255k , m4a_dash container, aac  @256k (44100Hz), 7.57MiB ***
160          mp4        256x144    DASH video  113k , 12fps, video only, 3.24MiB
133          mp4        426x240    DASH video  269k , 24fps, video only, 7.27MiB
134          mp4        640x360    DASH video  272k , 24fps, video only, 6.55MiB
244          webm       854x480    DASH video  504k , 24fps, video only, 7.95MiB
135          mp4        854x480    DASH video  540k , 24fps, video only, 13.33MiB
136          mp4        1280x720   DASH video 1155k , 24fps, video only, 26.62MiB
248          webm       1920x1080  DASH video 1797k , 24fps, video only, 30.81MiB
137          mp4        1920x1080  DASH video 2750k , 24fps, video only, 57.97MiB   ***   
43           webm       640x360    
18           mp4        640x360    
22           mp4        1280x720   (best)
```

使用第一列中的代码，您可以选择特定大小的视频和音频。

例如，让我们下载 1920x1080 分辨率的视频(137)加上 255K 的音频(141)，因此命令变成:

```
youtube-dl -f 137+141 [https://www.youtube.com/watch?v=VG1VVFfOnYQ](https://www.youtube.com/watch?v=VG1VVFfOnYQ)
```

如果您只想获得尽可能高的质量，这是默认设置，因此您只需:

```
youtube-dl [https://www.youtube.com/watch?v=VG1VVFfOnYQ](https://www.youtube.com/watch?v=VG1VVFfOnYQ)
```

默认情况下，下载文件的日期/时间将等于上传日期/时间，要禁用此选项并返回下载日期，请使用选项-no-mtime。

# 2.清醒

代表实用程序阻止系统休眠。

语法:

```
caffeinate [-disu] [-t timeout] [-w pid] [utility arguments...]Key
   -d      Create an assertion to prevent the display from sleeping. -i      Create an assertion to prevent the system from idle sleeping. -m      Create an assertion to prevent the disk from idle sleeping. -s      Create an assertion to prevent the system from sleeping. This
           assertion is valid only when system is running on AC power. -u      Create an assertion to declare that user is active.
           If the display is off, this option turns the display on and prevents the display from going
           into idle sleep. If a timeout is not specified with '-t' option, then this assertion is
           taken with a default of 5 second timeout. -t      Specifies the timeout value in seconds for which this assertion has to be valid.
           The assertion is dropped after the specified timeout.
           Timeout value is not used when an utility is invoked with this command. -w      Waits for the process with the specified pid to exit. Once the  the process exits, the assertion is also released.  This option is ignored when used with utility option.
```

用法:

阻止睡眠 1 小时(3600 秒)

```
caffeinate -u -t 3600
```

使 caffeinate fork 成为一个进程，exec 在其中“Make ”,并持有一个断言，只要该进程正在运行，就防止空闲睡眠:

```
caffeinate -i make
```

# 3.屏幕上显示程序运行的图片

捕捉整个或部分屏幕的图像。

语法:

```
screencapture [options] [file]Key
   -c   Force screen capture to go to the clipboard.
   -C   Capture the cursor as well as the screen.  Only allowed in 
        non-interactive modes. -d   Display errors to the user graphically. -i   Capture screen interactively, by selection or window.
          The control key = copy screen to the clipboard.
          The space key will toggle between mouse selection and
             window selection modes.
          The escape key will cancel the screen shot. -m   Only capture the main monitor, undefined if -i is set.
   -M   Open the taken picture in a new Mail message. -o   In window capture mode, do not capture the shadow of the window. -P   Open the taken picture in a Preview window. -s   Only allow mouse selection mode.
   -S   In window capture mode, capture the screen instead of the window. -t format   Image format to create, default is png (other options
               include pdf, jpg, tiff and others). -T seconds  Take the picture after a delay of seconds, default=5
               Handy for arranging windows/menus before taking the screenshot. -w   Only allow window selection mode.
   -W   Start interaction in window selection mode. -x   Do not play sounds. -l windowid  Capture a specific windowsid. -R x,y,w,h   Capture a screen rectangle, top,left,width,height. file  Where to save the screen capture, 1 file per screen. -help Display brief syntax summary.
```

用法:

使用下面的命令拍摄选定区域的屏幕截图。鼠标会变成十字光标，点击空格键进入相机模式，现在点击窗口，一个名为“example.png”的文件会出现在你的桌面上。

```
screencapture -i ~/Desktop/example.png
```

# 4.文本到语音

将文本转换成语音。

此工具使用语音合成管理器将输入文本转换为可听语音，并通过在“系统偏好设置”中选取的声音输出设备播放，或者将其存储到 AIFF 文件中。

语法:

```
say [-v voice] [-o out.aiff | -n name:port ] [-f file.in | string ...]Key string   The text to speak on the command line.
            This can consist of multiple arguments, which are
            considered to be separated by spaces. --input-file=file
   -f file  A file to be spoken.
            If file is - or neither this parameter nor a message
            is specified, read from standard input. --file-format=format
            The format of the file to write (AIFF, caff, m4af, WAVE).
            Generally, it's easier to specify a suitable file extension
            for the output file. To obtain a list of writable file formats,
            specify '?' as the format name. --data-format=format
            The format of the audio data to be stored. default=linear PCM. --progress    Display a progress meter during synthesis. --rate=rate
   -r rate       Speech rate to be used, in words per minute. --voice=voice
   -v voice      The voice to be used: English language = Alex, Daniel, Fiona, Fred, Samantha or Victoria
                 Default is the voice selected in System Preferences | Speech
                 Other voices are available for foreign languages.
   --voice=?     List all available voices. --output-file=fileout.aiff
   -o fileout.aiff
                 An AIFF file to be written, some voices support other file formats. --channels=channels   The number of channels. Most synthesizers produce mono audio only.
   --bit-rate=rate       The bit rate for formats, default=AAC.specify '?' as the rate.
   --quality=quality     The audio converter quality level between 0 (lowest) and 127 (highest). --network-send=name
   -n name --network-send=name:port
   -n name:port --network-send=:port
   -n :port --network-send=:
   -n :          Specify a service name (default "AUNetSend") and/or IP port to be
                 used for redirecting the speech output through AUNetSend.
                 specify '?' as the device name to obtain a list of audio output devices.
```

用法:

```
say -f myfile.txtcat myfile.txt | say
```

# 5.系统设置

配置某些通常在“系统偏好设置”应用程序中配置的每台机器的设置。

语法:

```
sudo systemsetup
```

systemsetup 命令至少需要“管理员”权限才能运行。

用法:

```
sudo systemsetup -getcomputernamesudo systemsetup -setdate 12:31:17Key:[-getdate] [-setdate mm:dd:yy] [-gettime] [-settime hh:mm:ss]
                 [-gettimezone] [-listtimezones] [-settimezone timezone]
                 [-getusingnetworktime] [-setusingnetworktime on | off]
                 [-getnetworktimeserver] [-setnetworktimeserver timeserver]
                 [-getsleep] [-setsleep minutes] [-getcomputersleep]
                 [-setcomputersleep minutes] [-getdisplaysleep]
                 [-setdisplaysleep minutes] [-getharddisksleep]
                 [-setharddisksleep minutes] [-getwakeonmodem]
                 [-setwakeonmodem on | off] [-getwakeonnetworkaccess]
                 [-setwakeonnetworkaccess on | off] [-getrestartpowerfailure]
                 [-setrestartpowerfailure on | off] [-getrestartfreeze]
                 [-setrestartfreeze on | off]
                 [-getallowpowerbuttontosleepcomputer]
                 [-setallowpowerbuttontosleepcomputer on | off]
                 [-getremotelogin] [-setremotelogin on | off]
                 [-getremoteappleevents] [-setremoteappleevents on | off]
                 [-getcomputername] [-setcomputername computername]
                 [-getstartupdisk] [-liststartupdisks] [-setstartupdisk path]
                 [-getwaitforstartupafterpowerfailure]
                 [-setwaitforstartupafterpowerfailure value]
                 [-getdisablekeyboardwhenenclosurelockisengaged]
                 [-setdisablekeyboardwhenenclosurelockisengaged yes | no]
                 [-getkernelbootarchitecturesetting]
                 [-setkernelbootarchitecture i386 | x86_64 | default][-version] [-help] [-printCommands]
```