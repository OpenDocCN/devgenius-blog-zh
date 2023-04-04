# 我在围棋中创造了一个愚蠢的虚拟助手

> 原文：<https://blog.devgenius.io/i-created-a-dumb-virtual-assistant-in-go-9557ec28f71f?source=collection_archive---------4----------------------->

![](img/54fabe9b9ef93e99591fe5626d9089fa.png)

只是为了美观

最近，我在头脑风暴一个有趣的项目，我可以建立它来打发时间，也许还能从中学习到一两件事。我一直着迷的一个项目是虚拟助手，比如谷歌的 Android 和苹果的 Siri，所以我认为尝试构建一个简单的虚拟助手会很棒，嗯，只是一个概念验证。以下是我认为每个虚拟助理应该能够做到的基本事情

1.  将声音或语音作为输入
2.  将语音转换为文本
3.  从转换后的文本中确定要执行的操作，这通常使用自然语言处理(NLP)来完成
4.  执行确定的操作。
5.  向用户反馈

为了实现上述功能，我必须在我的电脑上安装一些第三方服务和库。

# **预先对等**

**PortAudio** : PortAudio 是一个免费的、跨平台的、[开源的](http://www.portaudio.com/license.html)，音频 I/O 库。它可以让你用 C 或 C++编写简单的音频程序，这些程序可以在许多平台上编译和运行，包括 Windows、Macintosh OS X 和 Unix (OSS/ALSA)。为您的操作系统安装 portAudio 开发头文件和库。

**Ubuntu**

> apt-get 安装门户 19-dev

**Mac**

> brew 安装门户音频

**Wit AI:** Wit.ai 是一个开源聊天机器人框架，具有高级自然语言处理(NLP)功能。归脸书所有。要使用 Wit.ai，你需要在 other 中注册一个 API 密钥

**Espeak:** 文本到语音软件语音合成器

**Ubuntu**

> sudo apt-get 安装 espeak

**Mac**

> brew 安装扬声器

在这个练习中，我们将只处理四个意图

1.  问候
2.  发动
3.  播放音乐

参见下面的教程，了解如何在 Wit.ai 上创建话语、意图和实体

创建一个新的围棋模块，你可以给它取任何名字，但我决定给我的助手杰罗姆取个名字

> 市场总监助理-杰罗姆
> 
> cd 助理-杰罗姆
> 
> go mod 初始化助手-jerome

将以下内容添加到 go.mod 文件中

```
module assistant-jerome

go 1.17

require (
   github.com/gordonklaus/portaudio v0.0.0-20200911161147-bb74aa485641
   github.com/mjibson/go-dsp v0.0.0-20180508042940-11479a337f12
)
```

github.com/gordonklaus/portaudio 包提供了一个到 [PortAudio](http://www.portaudio.com/) 音频 I/O 库的接口。

包 github.com/mjibson/go-dsp 是一个数字信号处理包的去，这是有益的实施语音活动检测(VAD)，这基本上是一种虚拟助理，以确定用户何时结束发言。

创建 3 包语音，文字和行动，你的项目结构应该像这样。

```
assistant-jerome
- actions
- text
- voice
- go.mod
- main.go
```

接下来在文本包中创建一个新的脚本 speech_to_text.go，这将处理与 Wit AI 的集成。这是剧本应该有的样子。

**N/B:** 以下脚本中的 API 密钥具有功能性，但仅限于此 POC，并且应该用于运行项目。

```
package text

import (
   "bytes"
   "encoding/json"
   "fmt"
   "io/ioutil"
   "log"
   "net/http"
)

const *ApiKey* = "UKHKHD37C43H4ZUU65WBZNMXVJKOO6RO"

type WitAiContact struct {
   Confidence float32 `json:"confidence"`
   Suggested  bool    `json:"suggested"`
   Type       string  `json:"type"`
   Value      string  `json:"value"`
}

type WitAiIntent struct {
   Confidence float32 `json:"confidence"`
   Value      string  `json:"value"`
}

type WitAiEntities struct {
   Contact []WitAiContact    `json:"contact"`
   Intent  []WitAiIntent     `json:"intent"`
   SongTitle  []WitAiContact `json:"song_title"`
}

type WitAiOutcome struct {
   Text     string        `json:"text"`
   Entities WitAiEntities `json:"entities"`
   Intent   string        `json:"intent"`
}

type WitAiResponse struct {
   Text     string         `json:"text"`
   Outcomes []WitAiOutcome `json:"outcomes"`
}

func ConvertAudioToWitAiResponse(speechByte *bytes.Buffer) *WitAiResponse {

   var response *WitAiResponse

   stringResponse := sendWitBuff(speechByte)

   fmt.Println(stringResponse)

   rawResponseByte := []byte(stringResponse)
   err := json.Unmarshal(rawResponseByte, &response)

   if err != nil {
      log.Println(err)
   }

   return response
}

func sendWitBuff(buffer *bytes.Buffer) string {
   url := "https://api.wit.ai/speech?v=20141022"
   client := &http.Client{}
   req, err := http.NewRequest("POST", url, buffer)

   if err != nil {
      log.Fatal(err)
   }
   req.Header.Set("Authorization", "Bearer "+*ApiKey*)
   req.Header.Set("Content-Type", "audio/raw;encoding=signed-integer;bits=16;rate=20k;endian=little")
   res, err := client.Do(req)
   if err != nil {
      log.Fatal(err)
   }
   body, err := ioutil.ReadAll(res.Body)
   if err != nil {
      log.Fatal(err)
   }
   return string(body)
}
```

在这里，我们使用 Wit.ai 提供的语音 API 发送音频缓冲区。API 确定并返回来自我们训练好的模型的意图和相应的实体。

接下来在语音包中创建 vad.go 并粘贴下面的内容

```
package voice

import (
   "math"

   "github.com/mjibson/go-dsp/fft"
)

// Voice Activity Detection (that's what they call it when you detect
// that someone has started (or stopped) talking).

type VAD struct {
   samples      []complex128
   fft          []complex128
   spectrum     []float64
   lastSpectrum []float64
}

func NewVAD(width int) *VAD {
   return &VAD{
      samples:      make([]complex128, width),
      spectrum:     make([]float64, width/2+1),
      lastSpectrum: make([]float64, width/2+1),
   }
}

// Flux Given the samples, return the spectral flux value as compared to the previous samples.
func (v *VAD) Flux(samples []int16) float64 {
   for i, s := range samples {
      v.samples[i] = complex(float64(s), 0)
   }

   v.fft = fft.FFT(v.samples)
   copy(v.spectrum, v.lastSpectrum)

   for i, _ := range v.spectrum {
      c := v.fft[i]
      v.spectrum[i] = math.Sqrt(real(c)*real(c) + imag(c)*imag(c))
   }

   var flux float64

   for i, s := range v.spectrum {
      flux += s - v.lastSpectrum[i]
   }

   return flux
}

func (v *VAD) FFT() []complex128 {
   return v.fft
}
```

我们将在后面看到它的用法。

在 actions 包中创建 talk.go 并粘贴以下内容

```
package actions

import (
   "log"
   "os/exec"
)

func SpeakText(textToSpeak string) {
   cmd := exec.Command("espeak", textToSpeak)
   if err := cmd.Run(); err != nil {
      log.Fatal(err)
   }
}
```

***SpeakText*** 函数执行 espeak 命令，该命令将读出所传递的文本。

在 actions 包中创建 action.go，内容如下

```
package actions

import (
   "fmt"
)

func Greet() {
   SpeakText("Hi, how can I help?")
}

func CommandUnknown() {
   SpeakText("Sorry, I don't understand. Please take that again")
}

func playMusic(songTitle string, artist string ) {
   if artist == ""{
      SpeakText(fmt.Sprintf("Playing %s",songTitle))
      return
   }
   SpeakText(fmt.Sprintf("Playing %s by %s",songTitle,artist))
}

func PlayMusic(songTitle string, artist string ) {

   if songTitle == ""{
      SpeakText("Playing requested song")
      return
   }

   playMusic(songTitle, artist)

}
```

**Greet** ， **PlayMusic** 和 **CommandUnknown** 是 action.go 脚本中要处理的场景，当调用这些函数时，不会发生任何复杂的事情，我们只是让 espeak 读出一些文本。

在语音包中创建一个新的 go 文件 hear.go，这是大部分逻辑发生的地方。

```
package voice

import (
   "assistant-jerome/actions"
   "assistant-jerome/text"
   "bytes"
   "encoding/binary"
   "fmt"
   "github.com/gordonklaus/portaudio"
   "log"
   "time"
)

var audioRunning bool
var handling bool

func InitAudio() {
   if !audioRunning {
      portaudio.Initialize()
      audioRunning = true
   }
}

func FreeAudio() {
   if audioRunning {
      portaudio.Terminate()
   }
}

const *DefaultQuietTime* = time.*Second* type State int

const (
   *Waiting* State = iota
   *Listening* )

type ListenOpts struct {
   State            func(State)
   QuietDuration    time.Duration
   AlreadyListening bool
}

func ListenIntoBuffer(opts ListenOpts) (*bytes.Buffer, error) {
   in := make([]int16, 8196)
   stream, err := portaudio.OpenDefaultStream(1, 0, 16000, len(in), in)
   if err != nil {
      return nil, err
   }

   defer stream.Close()

   err = stream.Start()
   if err != nil {
      return nil, err
   }

   var (
      buf            bytes.Buffer
      heardSomething = opts.AlreadyListening
      quiet          bool
      quietTime      = opts.QuietDuration
      quietStart     time.Time
      lastFlux       float64
   )

   vad := NewVAD(len(in))

   if quietTime == 0 {
      quietTime = *DefaultQuietTime* }

   if opts.State != nil {
      if heardSomething {
         opts.State(*Listening*)
      } else {
         opts.State(*Waiting*)
      }
   }

reader:
   for {
      err = stream.Read()
      if err != nil {
         return nil, err
      }

      err = binary.Write(&buf, binary.LittleEndian, in)
      if err != nil {
         return nil, err
      }

      flux := vad.Flux(in)

      if lastFlux == 0 {
         lastFlux = flux
         continue
      }

      if heardSomething {
         if flux*1.75 <= lastFlux {
            if !quiet {
               quietStart = time.Now()
            } else {
               diff := time.Since(quietStart)

               if diff > quietTime {
                  break reader
               }
            }

            quiet = true
         } else {
            quiet = false
            lastFlux = flux
         }
      } else {
         if flux >= lastFlux*1.75 {
            heardSomething = true
            if opts.State != nil {
               opts.State(*Listening*)
            }
         }

         lastFlux = flux
      }
   }

   err = stream.Stop()
   if err != nil {
      return nil, err
   }

   return &buf, nil
}

func Listen() {

   InitAudio()
   defer FreeAudio()

   opts := ListenOpts{
      QuietDuration:    1 * time.*Second*,
      AlreadyListening: true,
   }

   fmt.Printf("speak now\n")

   buf, err := ListenIntoBuffer(opts)
   if err != nil {
      log.Fatal(err)
   }

   fmt.Printf("recognizing...\n")
   handling = true

   convertedResponse := text.ConvertAudioToWitAiResponse(buf)

   determineAction(convertedResponse)

   Listen()
}

func determineAction(witAiResponse *text.WitAiResponse) {

   if len(witAiResponse.Outcomes) < 1 {
      return
   }

   if len(witAiResponse.Outcomes[0].Entities.Intent) < 1 {
      return
   }

   switch witAiResponse.Outcomes[0].Entities.Intent[0].Value {

   case "greetings":
      actions.Greet()
   case "play_music":
      //actions.PlayMusic(witAiResponse.Outcomes[0].Entities.SongTitle[0].Value,witAiResponse.Outcomes[0].Entities.Contact[0].Value)
      actions.PlayMusic("","")
   default:
      actions.CommandUnknown()
   }
}
```

**Listen** 函数不断递归调用它来保持应用程序运行。首先我们调用 **InitAudio** 函数，该函数将音频运行状态设置为 true 并初始化 portaudio，然后我们调用**listeningobuffer**函数启动 portAudio 音频流。我们读取音频流，同时将音频流写入缓冲区，一旦我们确定用户已经结束讲话，我们就关闭音频流并返回缓冲区。然后使用**文本将缓冲区传递给 Wit AI。ConvertAudioToWitAiResponse**函数，最后我们使用 **determineAction** 函数基于来自 Wit AI 的响应来确定动作。

最后，在项目目录中创建 main.go 文件，内容如下。

```
package main

import (
   "assistant-jerome/voice"
)

func main() {

   voice.Listen()

}
```

运行项目，尝试一些命令，如“早上好”，“播放音乐”等。通常你可以在 Github[https://github.com/okemechris/assistant-jerome](https://github.com/okemechris/assistant-jerome)上找到完整的源代码