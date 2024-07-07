## vue中使用audio标签

```vue
<template>
  <!-- 音频一般放在public中 -->
  <audio id="audio" src="/audio/music.mp3" autoplay muted preload="auto" webkit-playsinline="true" playsinline="true">
    不支持
  </audio>
  <!-- controls -->
  <el-popover :visible="showpopover" placement="left" :width="270">
    <div>
      <span class="mr-[2px]">是否开启背景音乐？</span>
      <el-button type="primary" color="#fcb955" size="small" @click="handleshowpopover">开启</el-button>
      <el-button type="info" size="small" @click="showpopover = false">取消</el-button>
    </div>
    <!-- <div><span class="text-[blue] text-[14px]">开启</span>
      <span class="text-[red] text-[14px]">取消</span></p> -->
    <template #reference>
      <el-icon color="#fff" size="24">
        <Bell v-show="musicplay" @click="handleStopMusic" />
        <MuteNotification v-show="!musicplay" @click="handlePlayMusic" />
      </el-icon>
    </template>
  </el-popover>
</template>
<script setup lang="ts">
import { ref, onMounted, onUnmounted, defineProps, watch } from 'vue';
import 'element-plus/es/components/icon/style/css';
import 'element-plus/es/components/popover/style/css';
import 'element-plus/es/components/button/style/css';
import { ElIcon, ElPopover, ElButton } from 'element-plus';
import { Bell, MuteNotification } from '@element-plus/icons-vue';
const props = defineProps(['isCloseMusci']);
// 音乐
const musicplay = ref(true);
const showpopover = ref(false);
const handleStopMusic = () => {
  let audio = document.getElementById('audio') as any;
  audio?.pause();
  musicplay.value = false;
};
const handlePlayMusic = () => {
  let audio = document.getElementById('audio') as any;
  audio?.play();
  musicplay.value = true;
};
const handleshowpopover = () => {
  showpopover.value = false;
  handlePlayMusic();
};
onMounted(() => {
  // handlePlayMusic();
  const audio = document.getElementById('audio') as any;
  audio.volume = 0.4;
  audio.muted = false;
  document.body.addEventListener('mousemove', () => {
    if (musicplay.value) {
      handlePlayMusic();
    }
  });
});
watch(
  () => props.isCloseMusci,
  (val: boolean) => {
    console.log(val);
    if (val) {
      handleStopMusic();
    }
  },
);
onMounted(() => {
  showpopover.value = true;
});
onUnmounted(() => {});
</script>
```

