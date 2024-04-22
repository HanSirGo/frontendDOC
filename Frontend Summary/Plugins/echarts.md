## vue项目中echarts按需引入

在vue中使用Echarts可视化图表，[Handbook - Apache ECharts](https://echarts.apache.org/zh/index.html)

#### 安装
```html
npm install echarts --save
```
#### 使用
##### 新建echarts.js

```html
//按需引入echarts，在echarts.js里
import * as echarts from 'echarts'
 
// 引入柱状图图表和折线图表、饼图表、散点图，图表后缀都为 Chart
import { BarChart ，  LineChart, PieChart, ScatterChart} from 'echarts/charts';
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent
} from 'echarts/components';
// 标签自动布局，全局过渡动画等特性
import { LabelLayout, UniversalTransition } from 'echarts/features';
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from 'echarts/renderers';
 
// 注册必须的组件
echarts.use([
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  BarChart,
  LineChart,
  PieChart,
  ScatterChart,
  LabelLayout,
  UniversalTransition,
  CanvasRenderer
]);
 
 
// 导出
export default echarts
```
##### echarts.js 引入（vue2）
一定要给容器设置高度！不然图表出不来！ 
 - **main.js中引入**
```html
// main.js
import echarts from './js/echarts'
Vue.prototype.$echarts = echarts //原型链挂载到全局
```
 - **单页面文件引入**

```html
<template>
<div id="chart" style="height: 400px" ref="chart"></div>
</template>
 
<script>
  export default {
    data() {
      chart: null // echarts图表实例
    },
 
    methods: {
    initChart () {
 
      this.chart = this.$echarts.init(this.$refs.chart)
   
      this.chart.setOption({
        title: {
         text: 'Echarts大标题',
         subtext: 'Echarts副标题'
     },
       xAxis: {
         type: 'category',
         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
     },
       yAxis: {
         type: 'value'
     },
       series: [
          {
            name: '销量',
            type: 'bar', // 柱状图
            data: [5, 20, 36, 10, 10, 20]
          },
          {
            name: '利润',
            smooth: true,
            type: 'line', // 折线图
            data: [2, 23, 5, 54, 9, 33]
          }
        ]
 
      })
    },
},
 
   mounted () {
    this.initChart();//页面挂载echarts
  },
</script>
```
##### echarts.js 引入（vue3）
一定要给容器设置高度！不然图表出不来！ 

**组件引入**
```javascript
<template>
	<div ref="progress" style="height:400px"></div>
</template>
<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import echarts from './echarts'
const progress = ref<any>(null)
const myecharts = ref<any>(null)
onMounted(() => {
	myecharts.value = echarts.init(progress.value)
	const option = {
        title: {
         text: 'Echarts大标题',
         subtext: 'Echarts副标题'
     },
       xAxis: {
         type: 'category',
         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
     },
       yAxis: {
         type: 'value'
     },
       series: [
          {
            name: '销量',
            type: 'bar', // 柱状图
            data: [5, 20, 36, 10, 10, 20]
          },
          {
            name: '利润',
            smooth: true,
            type: 'line', // 折线图
            data: [2, 23, 5, 54, 9, 33]
          }
        ]
 
      }
	myecharts.value.setOption(option)
	// 自适应宽高大小
	window.addEventListener("resize", function () {
	    myChart.resize();
	})
})
onBeforeUnmount(() => {
	// 销毁 echarts
	myecharts.value.dispose()
})
</script>
```

#### Echarts图表自适应宽高大小
```html
myChart.setOption(option);
window.addEventListener("resize", function () {
    myChart.resize();
});
```
#### vite打包优化
在vite.config.ts的build通过设置 manualChunks方案，将echarts单独打包并通过按需引入减少主包体积
```javascript
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          echarts: ['echarts']
        }
      },
    },
  },
```
#### 取消放大效果
> 饼图为例

在使用echarts的饼图类图表时，我们用上动画属性就自动的会有鼠标移入对应图形放大的一个效果。如图：
![1713699787591](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699787591.png)
只想保留图表的加载动画和更新动画等等，并不想要这个放大的效果有两个办法：

1、在series数据的每一项添加属性silent并设置为ture (silent:ture)；意思是图形是否不响应和触发鼠标事件
![1713699777996](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699777996.png)
 这个方法虽然实现了单个扇区不放大但是存在一个很大的问题，就是我们的提示文字也不会触发，就是第一张图里的黑色提示框。

> 注意他说的不响应和触发鼠标事件,也就是说要想保留tooltip就不能使用silent
>
> 缩放设置在同样在series数据的每一项使用emphasis属性.属性配置如下：
> ![1713699766934](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699766934.png)

```javascript
series: [
	{
		type: 'bar',
		// silent:true,取消了silent
		emphasis:{
			disable:false,  //是否关闭扇区高亮效果
			scale:false,    //扇区是否缩放
			scaleSize:0,    //放大的尺寸，这里为了保证不放大扇区设置的，可要可不要
		}
	}
]
```
#### 动画配置

```javascript
option = {
    // animation:false,//控制动画，默认为true开启动画
    animationDuration:5000,//控制动画时长,他也可以接收一个回调函数
    //例子： animationDuration:function(arr){//arr:参数代表图表的每一项，显示平均线再是最大和最小值，然后再是数据
    //    return 2000 * arr
   // },
   animationEasing:'linear',//缓动动画有很多个参数：linear（均衡），bounceOut(有回弹效果)等，详情可以查看官方文档
   animationThreshold:7,//动画的阈值，单种形式的元素数量大于这个阈值时会关闭动画
}
```
#### 半环形进度图效果
应用步骤1: 通过npm/yarn 下载echarts或者html页面引入echartjs的cdn地址 可参照官方引入
应用步骤2：根据需要显示的进度数动态计算环形的起止刻度，设置后可显示
![1713699749691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699749691.png)
![1713699739205](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699739205.png)
封装为ProcessChart组件直接使用:
![在这里插入图片描述](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699730518.png)
最终效果![1713699720088](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713699720088.png)