

# 基于 vue 的音频播放功能

## 前言

最近公司要做一个H5的分享页面，需要实现的功能是音频、视频分享。在多次试坑之后选择了[vue-aplayer](https://github.com/SevenOutman/vue-aplayer/blob/develop/docs/README.zh-CN.md)，这个插件将会抛出它内部的 `<audio>` 元素上触发的所有媒体事件，意味着我们可以将这个插件当作 `<audio>` 来处理。

## 使用

### 安装

```shell
$ npm i vue-aplayer   	# 如果你使用的是npm
$ yarn add vue-aplayer 	# 如果你使用的是yarn

"vue-aplayer": "^1.6.1"	# 版本
```

```javascript
CND引入
<script src="//cdn.jsdelivr.net/npm/vue-aplayer"></script>
<script>
  Vue.component('aplayer', VueAPlayer)
</script>
```



### 引入

```javascript
import Aplayer from 'vue-aplayer';

components:{
  Aplayer
}
```



### 配置

#### html

```javascript
<aplayer autoplay :music="songList"></aplayer>
```

#### JavaScript

```javascript
export default {
	data() {
			return { 
      	songList:{},
      } 
  },
  
  // 创建前初始化音频信息
  created() {
			this.songList = {
				artist: '演唱者',
				src: '音频文件的 URL',
				pic: '封面图片 URL',
			}
	},
  
  //在 mounted 中可以监听 audio 标签自带的方法
  mounted(){
		var btns = document.getElementsByTagName('audio')[0];
			
    btns.addEventListener("pause", function () {
        
				// console.log('暂停');
			
  	});
  }
}
```



## audio 

### 属性

| 属性        | 注释                                                         |
| ----------- | ------------------------------------------------------------ |
| src         | 播放的音乐的url地址（火狐只支持ogg的音乐，而IE9只支持MP3格式的音乐。chrome貌似全支持） |
| preload     | 预加载（在页面被加载时进行加载或者说缓冲音频），如果使用了autoplay的话那么该属性失效。 |
| loop        | 循环播放                                                     |
| controls    | 是否显示默认控制条（控制按钮）                               |
| autoplay    | 自动播放                                                     |
| duration    | 获取媒体文件的总时长，以s为单位，如果无法获取，返回NaN       |
| paused      | 如果媒体文件被暂停，那么paused属性返回true，反之则返回false  |
| ended       | 如果媒体文件播放完毕返回true                                 |
| muted       | 用来获取或设置静音状态。值为boolean                          |
| volume      | 控制音量的属性值为0-1;0为音量最小，1为音量最大               |
| startTime   | 返回起始播放时间                                             |
| error       | 返回错误代码，为uull的时候为正常。否则可以通过Music.error.code来获取具体的错误码：<br/>1.用户终止 <br/>2.网络错误 <br/>3.解码错误<br/> 4.URL无效 |
| currentTime | 用来获取或控制当前播放的时间，单位为s。                      |
| currentSrc  | 以字符串形式返回正在播放或已加载的文件                       |

### 事件

| 事件名称       | 事件功能                                         |
| -------------- | ------------------------------------------------ |
| loadstart      | 客户端开始请求数据                               |
| progress       | 客户端正在请求数据（或者说正在缓冲）             |
| play           | play()和autoplay播放时                           |
| pause          | pause()方法触发时                                |
| ended          | 当前播放结束                                     |
| timeupdate     | 当前播放时间发生改变的时候。播放中常用的时间处理 |
| canplaythrough | 歌曲已经载入完全完成                             |
| canplay        | 缓冲至目前可播放状态。                           |



### 函数

| 函数名称         | 函数功能                                             |
| ---------------- | ---------------------------------------------------- |
| load()           | 加载音频、视频软件                                   |
| play()           | 加载并播放音频、视频文件或重新播放暂停的的音频、视频 |
| pause()          | 暂停出于播放状态的音频、视频文件                     |
| canPlayType(obj) | 测试是否支持给定的Mini类型的文件                     |

