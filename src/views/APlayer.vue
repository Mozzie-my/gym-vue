<script setup>
import APlayer from "APlayer"; // 引入音乐插件
import "APlayer/dist/APlayer.min.css";
import {onMounted} from "vue"; // 引入音乐插件的样式
const getUrl = (name) => {
  return new URL(`../assets/${name}`, import.meta.url).href
}

//  更多歌曲下载，请去 https://www.kugou.com/
const audio = [ // 歌曲列表
  {
    name: "晴天", // 歌曲名字
    artist: "叶惠美", // 歌曲演唱者
    url: // 歌曲地址（这里用外链地址）
        getUrl("03 - 晴天.wav"),
    cover: "https://imgessl.kugou.com/stdmusic/20150720/20150720192701255833.jpg", // 歌曲头像
    lrc: "", // 歌词
    theme: "rgb(127, 218, 180)", // 播放这首歌曲时的主题色
  },
  {
    name: "稻香",
    artist: "魔杰座",
    url: getUrl("11 - 稻香.wav"),
    cover: "https://imgessl.kugou.com/stdmusic/20200909/20200909135350181905.jpg",
    lrc: "",
    theme: "rgb(61, 162, 230)",
  },
]
const info = {
  fixed: false, // 不开启吸底模式
  listFolded: true, // 折叠歌曲列表
  autoplay: true, // 开启自动播放
  preload: "auto", // 自动预加载歌曲
  loop: "all", // 播放循环模式、all全部循环 one单曲循环 none只播放一次
  order: "list", //  播放模式，list列表播放, random随机播放
}

const initAudio = () => {
  // 创建一个音乐播放器实例，并挂载到DOM上，同时进行相关配置
  const ap = new APlayer({
    container: document.getElementById("aplayer"),
    audio: audio, // 音乐信息
    info: info, // 其他配置信息
  })
}
onMounted(() => {
  // 初始化播放器
  initAudio()
})
</script>

<template>
  <div>
    <!-- 准备一个容器用来存放音乐播放器 -->
    <div id="aplayer" style="width: 50%; margin: 0 auto"></div>
  </div>
</template>