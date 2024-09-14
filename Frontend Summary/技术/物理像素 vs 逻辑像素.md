# 物理像素 vs 逻辑像素：移动端为什么离不开 @2x 和 @3x 图片？

作为一名前端开发者，我们每天都在处理各种设备的屏幕适配问题，尤其是在移动端开发中，不同设备的分辨率和屏幕密度差异巨大。为了确保我们所开发的应用在各类设备上都能保持清晰、优美的视觉效果，理解物理像素、逻辑像素、像素密度的概念，并合理使用 @2x、@3x 图片资源是至关重要的。

#### 1. **物理像素和逻辑像素的区别**

首先，作为开发者，我们必须理解设备的 **物理像素** 和 **逻辑像素** 之间的区别。

- **物理像素** 是设备屏幕上的最小显示单元，直接与设备的硬件相关。例如，iPhone 12 Pro 的物理分辨率为 1170x2532，这意味着屏幕实际包含了 1170 列和 2532 行的物理像素点。
- **逻辑像素** 则是我们开发过程中使用的虚拟单位，它通过设备的像素比（DPR，Device Pixel Ratio）映射到物理像素上。对于同样的 1170x2532 分辨率，系统可能会用较少的逻辑像素来表示，便于我们统一设计和开发。这就是为什么我们在编写 CSS 或处理布局时，不必去关心每个设备的物理分辨率，而只需处理逻辑像素。

通过使用逻辑像素，操作系统可以自动适配不同分辨率的屏幕，让开发者不必为每种设备单独设计 UI。然而，仅仅使用逻辑像素并不足以让所有设备都显示出清晰的图片，这就是像素密度和多分辨率图片的重要性。

#### 2. **像素密度和设备像素比（DPR）**

每个设备的屏幕都有不同的像素密度，通常以 PPI（每英寸像素数）来表示。屏幕像素密度越高，显示的内容越清晰。在移动端，像素密度从低分辨率屏幕的 160 PPI，到中等分辨率的 320 PPI，再到 Retina 显示屏的 460 PPI 甚至更高。

为了解决不同像素密度设备上图像显示的问题，操作系统引入了 **设备像素比（DPR）** 的概念。DPR 是逻辑像素到物理像素的映射比例。例如，DPR 为 2 的设备意味着每个逻辑像素会映射到 2x2 个物理像素，也就是四个物理像素，这就是 @2x 图片的概念；同理，DPR 为 3 的设备每个逻辑像素对应 9 个物理像素，就是 @3x。

#### 3. **为什么我们需要 @2x 和 @3x 图片？**

作为开发者，单单设计一套图片资源已经不够了。假如我们只提供标准分辨率（@1x）的图片，这些图片在高像素密度设备上会显得模糊，因为图像的物理像素不够多，导致图像拉伸时失去了细节。

**@2x 和 @3x 图片的引入** 解决了这个问题。它们专门为高像素密度设备设计，提供了更高的分辨率和更精细的细节。系统会根据设备的像素密度自动选择最合适的图片资源，因此，我们需要为每个图片资源准备三种分辨率的文件：

- **@1x**：标准分辨率图片，适用于低像素密度设备。
- **@2x**：适用于中等像素密度设备（如 Retina 屏幕设备）。
- **@3x**：适用于高像素密度设备（如 iPhone X、iPhone 12 Pro 等）。

这种图片资源的多版本支持，确保了无论用户使用的是什么设备，图像都能保持清晰、精致的视觉效果。

#### 4. **如何在项目中使用 @2x 和 @3x 图片？**

在日常开发中，我们需要为每个图片资源提供 @1x、@2x 和 @3x 三种分辨率的版本，并且按照一定的命名规则存放。通常情况下，图片命名规则如下：

- `image.png`（标准分辨率图片，适用于低密度屏幕）。
- `image@2x.png`（适用于 Retina 屏幕）。
- `image@3x.png`（适用于超高像素密度设备）。

我们在代码中只需要引用图片的基础名称，比如：

```
<img src="image.png" alt="example image">
```

系统会根据设备的像素密度自动选择最合适的图片，比如在 Retina 设备上，它会加载 `image@2x.png`，而在高密度屏幕上，它则会加载 `image@3x.png`。

**小程序中使用（uniapp）**在 **uniapp** 中，可以通过类似的方式处理图片，但是系统不会像原生 iOS/Android 那样自动根据设备像素密度选择 `@2x` 或 `@3x` 图片。需要手动管理这些不同分辨率的图片。不过，可以根据屏幕像素密度来动态选择图片，以确保高分辨率设备上图片的清晰度。

1. **使用条件判断选择图片**

你可以使用 `uni.getSystemInfoSync()` 来获取设备的像素密度（DPR），并根据 DPR 动态选择适合的图片：

```html
<template>  
    <image :src="imageSrc" alt="example image"></image>
</template>

<script>
    export default {  
        data() {    
            return {      
                imageSrc: '/static/image.png' // 默认图片路径    
            };  
        },  
        mounted() {    
            // 获取设备信息    
            const systemInfo = uni.getSystemInfoSync();    
            const dpr = systemInfo.pixelRatio;
    
            // 根据 DPR 设置图片    
            if (dpr >= 3) {      
                this.imageSrc = '/static/image@3x.png';    
            } else if (dpr >= 2) {      
                this.imageSrc = '/static/image@2x.png';    
            } else {      
                this.imageSrc = '/static/image.png'; // 标准分辨率图片    
            }  
        }
    };
</script>
```

在这个例子中，`mounted` 生命周期钩子会在组件加载时根据设备的 DPR（设备像素比）选择合适的图片资源。只需要提供不同分辨率的图片，并确保路径和命名规则正确即可。

1. **简化方法：CSS 媒体查询**

如果是背景图片，可以使用 CSS 媒体查询来根据设备的 DPR 自动选择图片：

```css
.example-image {  
    background-image: url('/static/image.png');
}

@media only screen and (-webkit-min-device-pixel-ratio: 2),        
    only screen and (min--moz-device-pixel-ratio: 2),        
    only screen and (-o-min-device-pixel-ratio: 2/1),        
    only screen and (min-device-pixel-ratio: 2),        
    only screen and (min-resolution: 192dpi) {  
        .example-image {    
            background-image: url('/static/image@2x.png');  
        }
}

@media only screen and (-webkit-min-device-pixel-ratio: 3),        
    only screen and (min--moz-device-pixel-ratio: 3),        
    only screen and (-o-min-device-pixel-ratio: 3/1),        
    only screen and (min-device-pixel-ratio: 3),        
    only screen and (min-resolution: 288dpi) {  
        .example-image {    
            background-image: url('/static/image@3x.png');  
        }
}
```

这可以自动根据设备的像素比来切换背景图片，不过这种方法更适合静态背景图片，不能直接用于 `<image>` 标签。

#### 5. **实际开发中的注意事项**

- **资源管理**：确保为每个图片资源提供所有必要的分辨率版本，避免在高像素密度设备上使用低分辨率图片造成模糊。
- **性能优化**：虽然提供高分辨率图片可以确保视觉效果，但是需要控制图片文件大小，避免加载过大的图片影响应用性能。使用适当的图片压缩工具来优化资源，同时保证图片的清晰度。
- **响应式设计**：在做响应式设计时，尽量使用矢量图（如 SVG）来代替位图，尤其是图标，这样可以更好地适配不同的分辨率，减少多版本图片的维护负担。

#### 6. **总结**

作为前端开发者，理解物理像素、逻辑像素和像素密度的概念，能帮助我们开发出在各种设备上都具备优秀显示效果的应用。@2x 和 @3x 图片的使用是为了解决不同像素密度设备上的模糊问题。