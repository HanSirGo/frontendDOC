### 下载Swiper
[Swiper文档](https://www.swiper.com.cn/)
![1713698022909](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713698022909.png)

> Swiper Vue.js组件
> Swiper Vue组件可能会在未来的版本中被删除。建议改为迁移到Swiper Element。
> 如果您要从Swiper 9升级到Swiper 10，请查看Swiper 10迁移指南
> 如果您正在寻找v9文档，请访问v9.wiperjs.com

```html
npm i swiper@9.4.1
```
结合 swiper 9.4.1 的文档 
### 使用Swiper

```html
<template>
  <swiper
    class="h-[100%]"
    :modules="modules"
    :autoplay="{
    	delay: 3000,
    	disableOnInteraction: false, // 用户操作后，是否禁止autoplay，默认true 
    }"
    dir="rtl"
    :loop="true"
    :grab-cursor="true"
    :initial-slide="1"
    :centered-slides="true"
    :slides-per-view="3"
    :space-between="5"
    navigation
    mousewheel
    :pagination="{
      clickable: true, //bullets  圆点（默认） fraction  分式  ‘progressbar’   ‘custom’ 自定义
      type: 'fraction',
    }"
    :effect="'coverflow'"
    :coverflow-effect="{
      rotate: 0, // 图片翻转
      stretch: 100, //集中/分散
      depth: 300, //越小展示的越少
      modifier: 1,
      slideShadows: false,
    }"
    :zoom="{
      maxRatio: 1.3,
    }"
    @swiper="onSwiper"
    @slideChange="onSlideChange"
  >
    <!-- :scrollbar="{ draggable: true }" 滚动条 -->
    <swiper-slide v-for="item in data" :key="item.src" v-slot="{ isActive }">
      <div
        class="w-[100%] h-[100%] swiper-zoom-container"
        :class="isActive ? 'filter blur-[0px]' : 'filter blur-[4px]'"
        @click="handleZoomToggle"
      >
        <img :src="item.src" class="p-[10px] bg-[#fff]" alt="" />
      </div>
    </swiper-slide>
    <swiper-slide v-slot="{ isActive }">
      <div
        class="w-[100%] h-[100%] swiper-zoom-container"
        :class="isActive ? 'filter blur-[0px]' : 'filter blur-[4px]'"
      >
        <div class="relative">
          <img src="/src/assets/images/home/ss.png" class="p-[10px] bg-[#fff]" alt="" />
          <el-button
            class="absolute bottom-0 right-0"
            style="bottom: 0; right: 0"
            type="primary"
            size="large"
            color="#fcb955"
            @click="handleToPictureLibrary"
            >查看更多</el-button
          >
        </div>
      </div>
    </swiper-slide>
  </swiper>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import 'element-plus/es/components/button/style/css';
import { ElButton } from 'element-plus';
// import { useRouter } from 'vue-router';
// import Swiper core and required modules
import { Navigation, Pagination, Scrollbar, A11y, Mousewheel, EffectCoverflow, Autoplay, Zoom } from 'swiper';

// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue';

// Import Swiper styles
import 'swiper/css';
import 'swiper/css/navigation';
import 'swiper/css/pagination';
import 'swiper/css/scrollbar';
import 'swiper/css/a11y';
import 'swiper/css/mousewheel';
import 'swiper/css/effect-coverflow';
import 'swiper/css/autoplay';
import 'swiper/css/zoom';

const modules = ref([Navigation, Pagination, Scrollbar, A11y, Mousewheel, EffectCoverflow, Autoplay, Zoom]);
const mySwiper = ref<any>(null);
const onSwiper = (swiper: any) => {
  console.log(swiper);
  mySwiper.value = swiper;
};
const onSlideChange = () => {
  // console.log('slide change');
};
// const router = useRouter();
const handleToPictureLibrary = () => {
  // router.push({ path: '/Picture', name: 'Picture' });
  window.open('/Picture');
};
const handleZoomToggle = () => {
  // console.log('点击，缩放');
  mySwiper.value.zoom.toggle();
};
const data = ref([
  { src: '/images/picture/2014-1-Principle-Mustafa Sagun.jpg.JPG' },
  { src: '/images/picture/2014-2-Citadel-Derek Kaufman.JPG' },
  { src: '/images/picture/2014-3-Morgan Stanley-Vincent Reinhart.JPG' },
  { src: '/images/picture/2015-1-Cambridge Associates-Celia Dallas.JPG' },
  { src: '/images/picture/2015-2-Fidelity-Andrew Wells.JPG' },
  { src: '/images/picture/2015-3-Dimensional-Robert Merton.JPG' },
  // { src: '/images/picture/ss.png',type:'more' },
]);
</script>
<style>
/* 设置左右导航按钮的大小和颜色 */
.swiper {
  --swiper-navigation-size: 70px;
  --swiper-navigation-color: #fcb955;
}
/* 设置左右导航按钮的选中时，有黑框的问题 */
.swiper-button-prev,
.swiper-button-next:focus {
	outline: none;
}
</style>
```
#### 设置左右导航按钮的大小和颜色
```css
.swiper {
  --swiper-navigation-size: 70px;
  --swiper-navigation-color: #fcb955;
}
```
#### 设置左右导航按钮的选中时，有黑框的问题
```css
.swiper-button-prev,
.swiper-button-next:focus {
	outline: none;
}
```
#### 调整轮播方向
```html
 <swiper dir="rtl" > </swiper>
```