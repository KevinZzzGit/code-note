# html5中的多媒体标签

## 视频标签Video

```html
<body>
    <video controls>
    	<source src="movie.mp4" type="video/mp4">
    	<source src="movie.ogg" type="video/ogg">
        您的浏览器不支持video元素
    </video>
</body>
```

### 常用标签属性

| **属性**             | **属性值** | **注释**                                                     |
| -------------------- | ---------- | ------------------------------------------------------------ |
| src                  | url        | 播放的视频的url地址                                          |
| preload              | preload    | 预加载（在页面被加载时进行加载或者说缓冲视频），如果使用了autoplay的话那么该属性失效。 |
| loop                 | loop       | 循环播放                                                     |
| controls             | controls   | 是否显示默认控制条                                           |
| autoplay             | autoplay   | 自动播放                                                     |
| muted                | boolean    | 设定静音                                                     |
| poster               | url        | 封面图                                                       |
| webkit-playsinline   | boolean    | ios10中让视频小窗播放                                        |
| playsinline          | boolean    | ios微信浏览器小窗播放                                        |
| x5-video-orientation | portraint  | 播放方向，landscape横屏，portraint竖屏                       |

### 支持格式

| **音频格式** | **Chrome** | **Firefox** | **IE9** | **Opera** | **Safari** |
| ------------ | ---------- | ----------- | ------- | --------- | ---------- |
| MP4          | √          | √           | √       | ×         | ×          |
| WebM         | √          | ×           | √       | ×         | √          |
| Ogg          | ×          | √           | ×       | √         | ×          |

### Video属性

| 属性                                                         | 注释                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| duration                                                     | 返回视频的长度（以秒计）。如果无法获取，返回NaN             |
| paused                                                       | 如果媒体文件被暂停，那么paused属性返回true，反之则返回false |
| ended                                                        | 如果媒体文件播放完毕返回true                                |
| muted                                                        | 用来获取或设置静音状态。值为boolean                         |
| volume                                                       | 控制音量的属性值为0-1;0为音量最小，1为音量最大              |
| startTime                                                    | 返回起始播放时间                                            |
| [seekable](https://www.runoob.com/jsref/prop-video-seekable.html) | 返回表示视频可寻址部分的 TimeRanges 对象。                  |
| currentTime                                                  | 用来获取或控制当前播放的时间，单位为s。                     |
| currentSrc                                                   | 以字符串形式返回正在播放或已加载的文件                      |
| [buffered](https://www.runoob.com/jsref/prop-video-buffered.html) | 返回表示视频已缓冲部分的 TimeRanges 对象。                  |
| [startDate](https://www.runoob.com/jsref/prop-video-startdate.html) | 返回表示当前时间偏移的 Date 对象。                          |
| [textTracks](https://www.runoob.com/jsref/prop-video-texttracks.html) | 返回表示可用文本轨道的 TextTrackList 对象。                 |
| [videoTracks](https://www.runoob.com/jsref/prop-video-videotracks.html) | 返回表示可用视频轨道的 VideoTrackList 对象。                |

### API

| Method                                                       | 描述                                   |
| :----------------------------------------------------------- | :------------------------------------- |
| [addTextTrack()](https://www.runoob.com/jsref/met-video-addtexttrack.html) | 向视频添加新的文本轨道。               |
| [canPlayType()](https://www.runoob.com/jsref/met-video-canplaytype.html) | 检查浏览器是否能够播放指定的视频类型。 |
| [load()](https://www.runoob.com/jsref/met-video-load.html)   | 重新加载视频元素。                     |
| [play()](https://www.runoob.com/jsref/met-video-play.html)   | 开始播放视频。                         |
| [pause()](https://www.runoob.com/jsref/met-video-pause.html) | 暂停当前播放的视频。                   |



## 音频标签Audio

```html
<body>
    <audio controls>
    	<source src="./audio.mp3" type="audio/mp3" />
        <source src="./audio.ogg" type="audio/ogg" />
        您的浏览器不支持audio元素
    </audio>
</body>
```



### 常用标签属性

| **属性** | **属性值** | **注释**                                                     |
| -------- | ---------- | ------------------------------------------------------------ |
| src      | url        | 播放的音乐的url地址（火狐只支持ogg的音乐，而IE9只支持MP3格式的音乐。chrome貌似全支持） |
| preload  | preload    | 预加载（在页面被加载时进行加载或者说缓冲音频），如果使用了autoplay的话那么该属性失效。 |
| loop     | loop       | 循环播放                                                     |
| controls | controls   | 是否显示默认控制条（控制按钮）                               |
| autoplay | autoplay   | 自动播放                                                     |

### 支持格式

| **音频格式** | **Chrome** | **Firefox** | **IE9** | **Opera** | **Safari** |
| ------------ | ---------- | ----------- | ------- | --------- | ---------- |
| OGG          | √          | √           | √       | ×         | ×          |
| MP3          | √          | ×           | √       | ×         | √          |
| WAV          | ×          | √           | ×       | √         | ×          |

### Audio属性

| 属性        | 注释                                                         |
| ----------- | ------------------------------------------------------------ |
| duration    | 获取媒体文件的总时长，以s为单位，如果无法获取，返回NaN       |
| paused      | 如果媒体文件被暂停，那么paused属性返回true，反之则返回false  |
| ended       | 如果媒体文件播放完毕返回true                                 |
| muted       | 用来获取或设置静音状态。值为boolean                          |
| volume      | 控制音量的属性值为0-1;0为音量最小，1为音量最大               |
| startTime   | 返回起始播放时间                                             |
| error       | 返回错误代码，为uull的时候为正常。否则可以通过Music.error.code来获取具体的错误代码： 1.用户终止 2.网络错误 3.解码错误 4.URL无效 |
| currentTime | 用来获取或控制当前播放的时间，单位为s。                      |
| currentSrc  | 以字符串形式返回正在播放或已加载的文件                       |

### API

| 函数             | 作用                                                 |
| ---------------- | ---------------------------------------------------- |
| load()           | 加载音频、视频软件                                   |
| play()           | 加载并播放音频、视频文件或重新播放暂停的的音频、视频 |
| pause()          | 暂停出于播放状态的音频、视频文件                     |
| canPlayType(obj) | 测试是否支持给定的Mini类型的文件                     |

| 事件           | 事件作用                             |
| -------------- | ------------------------------------ |
| loadstart      | 客户端开始请求数据                   |
| progress       | 客户端正在请求数据（或者说正在缓冲） |
| play           | play()和autoplay播放时               |
| pause          | pause()方法触发                      |
| ended          | 当前播放结束                         |
| timeupdate     | 当前播放时间发生改变的时候。         |
| canplaythrough | 歌曲已经载入完全完成                 |
| canplay        | 缓冲至目前可播放状态。               |



## 容器标签EMBED

```html
<embed src="test.mp4" type="video/mp4"  /> 
```

### 支持格式

Midi、Wav、AIFF、AU、MP3



## 嵌入对象 Object

```html
<object data="hello.flash">
     您的浏览器不支持Object元素
</object>
```

### 支持格式

支持 图像、音频、视频、Java applets、ActiveX、PDF 以及 Flash



## 媒体元素Source

用来规定多个文件对不同浏览器的支持

```html
<body>
    <audio controls>
    	<source src="./audio.mp3" type="audio/mp3" />
        <source src="./audio.ogg" type="audio/ogg" />
        您的浏览器不支持audio元素
    </audio>
</body>
```

## 外部轨道Track

规定外部文本轨道，也就是字幕，字幕格式有 WebVTT 格式（.vtt 格式文件）。

### kind属性

规定文本轨道的文本类型。

| key          | Value                                                        |
| ------------ | ------------------------------------------------------------ |
| captions     | 该轨道定义将在播放器中显示的简短说明。翻译。                 |
| chapters     | 该轨道定义章节，用于导航媒介资源。                           |
| descriptions | 该轨道定义描述，用于通过音频描述媒介的内容，假如内容不可播放或不可见。 |
| metadata     | 该轨道定义脚本使用的内容。                                   |
| subtitles    | 该轨道定义字幕，用于在视频中显示字幕。                       |

