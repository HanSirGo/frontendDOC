# 前端如何利用 ffmpeg 和 sharp 玩转音视频和图片

# 使用 ffmpeg 将视频转 GIF 格式

在开始之前我们大概来了解一下什么是 ffmpeg。

ffmpeg 是一个非常流行的开源软件套件，用于处理音频和视频数据。它提供了多种工具集，如 `ffmpeg`、`ffplay` 和 `ffprobe`，这些工具可以用于：

1. 转码：可以将视频和音频从一种格式转换为另一种格式。
2. 播放：使用 `ffplay` 可以播放多种格式的音频和视频文件。
3. 提取信息：使用 `ffprobe` 可以获取关于音频和视频文件的详细信息。
4. 流处理：可以捕获和编码实时音频/视频流。
5. 过滤：可以应用各种过滤器来处理音频和视频数据，例如调整亮度、裁剪、调整速度等。

ffmpeg 支持多种音频、视频、字幕和其他相关媒体数据的格式，它的功能非常丰富且灵活，可以满足许多媒体处理的需求。由于其强大的功能和开源的特性，ffmpeg 在多个平台和应用程序中得到了广泛的应用。

大概了解了之后，我们来看看如何在 node 中实现视频转 webp 的，首先安装相关的依赖包：

```
pnpm add @ffmpeg-installer/ffmpeg fluent-ffmpeg
```

然后编写一下代码：

```
const ffmpegPath = require("@ffmpeg-installer/ffmpeg").path;
const ffmpeg = require("fluent-ffmpeg");

const video2Gif = async (videoPath, gifPath, width, fps) => {
  ffmpeg.setFfmpegPath(ffmpegPath); // 设置二进制客户端路径
  ffmpeg(videoPath) // 读入路径
    .outputOptions("-pix_fmt", "rgb24") // 设置像素格式为rgb24
    .output(gifPath) // 输出路径
    .withFPS(fps) // 设置输出GIF的帧率
    .size(`${width}x?`) // 设置输出GIF的宽度，高度等比缩放
    .noAudio() // 禁用音频输出
    .setStartTime("00:00:10") // 开始时间
    .setDuration(15) // 持续时间
    .videoFilters({
      filter: "crop",
      options: {
        out_w: `iw/1.5`, // 裁剪到原始宽度
        out_h: "ih", // 高度保持不变
        x: "(iw-out_w)/2", // 从中心开始裁剪
        y: 0, // y坐标保持在顶部
      },
    })
    .on("end", () => {
      console.log("转换完成！");
    })
    .on("error", (err) => console.error(err))
    .run();
};

video2Gif("./i.mp4", "./index.gif", 600, 80);
```

这段代码利用 ffmpeg 将一个视频文件的特定 15 秒段落（从 10 秒开始）转换为 GIF 格式。在转换过程中，视频的宽度被裁剪到其原始的 2/3，高度保持不变，同时移除了音频部分。最后，生成的 GIF 保存到指定的路径。

这个时候我们使用 node 运行该文件，内容变输出了。查看文件大小的时候，我们发现文件太大了，虽然我们可以在上面调整参数，但是我偏不哈哈哈哈哈，我就要倔

![1710058729465](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710058729465.png)

# 使用 sharp 压缩 gif

要想对图片之类的进行压缩，我们可以选择 sharp 来进行操作，这里我就不过多说了，直接上代码：

```
const sharp = require("sharp");
const fs = require("fs");

async function compressGif(filePath, outputPath) {
  const data = await sharp(filePath, {
    animated: true,
    limitInputPixels: false,
  })
    .gif({
      colours: 60,
    })
    .toBuffer();

  // 写入Buffer到文件
  fs.writeFileSync(outputPath, data);
}

compressGif("./index.gif", "./php.gif");
```

上面这段代码使用 sharp 库对 GIF 文件进行压缩，将其颜色范围减少到 60 fps，从而减小文件大小。压缩后的数据被保存为一个 Buffer，然后写入到新的 GIF 文件路径。

这个时候，gif 被我们压缩到了 20mb 了，也终于能在掘金上面上传了。



# 将任何格式的图片转换为 webp 格式并添加水印

WebP 是 Google 开发的图片格式，它提供比 JPEG 和 PNG 更高效的压缩，从而达到更小的文件大小而不牺牲图像质量。同时，它支持透明度和动画，且得到了大多数现代浏览器的支持，使其成为网站和应用中优化图像性能的理想选择。

在下面，我们就来使用 sharp 来实现这种转换，如下代码所示：

```
const sharp = require("sharp");
const fs = require("fs");
const path = require("path");

const sourceDirectory = "./image";
const outputDirectory = "./webp_images";

if (!fs.existsSync(outputDirectory)) {
  fs.mkdirSync(outputDirectory);
}

fs.readdir(sourceDirectory, (err, files) => {
  if (err) {
    console.error("Error reading the directory:", err);
    return;
  }

  files.forEach((file) => {
    const extname = path.extname(file).toLowerCase();
    const filename = path.basename(file, extname);

    const allowedFormats = [
      ".jpg",
      ".jpeg",
      ".png",
      ".tif",
      ".tiff",
      ".bmp",
      ".gif",
    ];

    if (allowedFormats.includes(extname)) {
      const inputFile = path.join(sourceDirectory, file);
      const outputFile = path.join(outputDirectory, `${filename}.webp`);

      const watermarkSvg = `
                <svg width="200" height="80" xmlns="http://www.w3.org/2000/svg">
                    <text x="10" y="40" font-family="Arial" font-size="30" fill="pink" stroke="black" stroke-width="2">Moment</text>
                </svg>
            `;

      sharp(inputFile)
        .webp()
        .composite([
          {
            input: Buffer.from(watermarkSvg),
            gravity: "southeast",
          },
        ])
        .toFile(outputFile, (err, info) => {
          if (err) {
            console.error(`Failed to convert and watermark ${file}:`, err);
          } else {
            console.log(`Converted and watermarked ${file} to WebP format.`);
          }
        });
    }
  });
});
```

在上面这段代码从一个指定的文件夹中读取图片，将其转换为 WebP 格式并添加一个 SVG 水印在右下角，然后保存转换后的图片到另一个文件夹中。

最终输出结果如下图所示：

![1710058806465](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710058806465.png)

水印也被加上去了。



# FFmpeg

FFmpeg 是一个强大的开源多媒体框架，能够处理音频、视频和其他多媒体文件的录制、转换和流式传输。它支持几乎所有的音频和视频格式，并提供了强大的命令行工具和库。

### 基本使用

以下是 FFmpeg 的一些基本使用示例：

1、 **查看媒体文件信息**：

```
   ffmpeg -i input.mp4
```

2、 **转换视频格式**：将 MP4 文件转换为 AVI 格式：

```
   ffmpeg -i input.mp4 output.avi
```

3、 **提取音频**：从视频中提取音频并保存为 MP3 格式：

```
   ffmpeg -i input.mp4 -q:a 0 -map a output.mp3
```

4、 **压缩视频**：将视频压缩为较小的文件：

```
   ffmpeg -i input.mp4 -vcodec libx264 -crf 23 output.mp4
```

5、 **剪切视频**：从视频中提取特定时间段：

```
   ffmpeg -i input.mp4 -ss 00:00:30 -to 00:01:00 -c copy output.mp4
```

6、 **合并视频**：使用文件列表合并多个视频文件：

```
 # 创建一个文件 list.txt，内容如下：   # file 'file1.mp4'   # file 'file2.mp4'   ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4
```

7、 **添加水印**：在视频上添加图片水印：

```
   ffmpeg -i input.mp4 -i watermark.png -filter_complex "overlay=10:10" output.mp4
```

8、 **转换为 GIF**：将视频转换为 GIF 动画：

```
   ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v gif output.gif
```

### 常用参数说明

•`-i`：指定输入文件。•`-c`：指定编解码器。•`-vf`：应用视频过滤器。•`-q:a`：音频质量（0 为最佳质量，最高为 51）。•`-ss` 和 `-to`：指定剪切的起始和结束时间。•`-f`：指定格式。

### 安装 FFmpeg

在不同操作系统上安装 FFmpeg 的方法：

•**Windows**：可以从 FFmpeg 官网[1] 下载预编译的二进制文件，解压后添加到系统 PATH。•**macOS**：使用 Homebrew 安装：

```
brew install ffmpeg
```

•**Linux**：使用包管理器安装（如 apt、yum 等）：

```
sudo apt update
sudo apt install ffmpeg
```

### 结论

FFmpeg 是一个功能强大的工具，适用于各种多媒体处理任务。通过掌握基本命令和参数，用户可以高效地处理音视频文件。

如果你不喜欢命令方式处理文件，那么推荐大家使用 EZGIF 网站，也同样可以处理 GIF 文件。

### EZGIF

EZGIF[2] 是一个在线工具网站，专注于 GIF 动画的创建和编辑。它提供了一系列功能，允许用户轻松地处理 GIF 文件。以下是 EZGIF 的主要特点和功能：

1.**GIF 创建**：用户可以通过上传图片或视频文件来创建 GIF 动画。支持多种格式的图片，如 JPG、PNG、WEBP 等。2.**GIF 编辑**：提供多种编辑功能，包括裁剪、调整大小、旋转、添加文本、调整速度等。用户可以对现有的 GIF 进行修改和优化。3.**GIF 转换**：可以将视频文件（如 MP4、WEBM 等）转换为 GIF，或将 GIF 转换为其他格式的图片。4.**GIF 压缩**：提供压缩工具，帮助用户减小 GIF 文件的大小，以便于分享和存储。5.**GIF 分割和合并**：用户可以将 GIF 动画分割成单独的帧，或将多个 GIF 合并为一个。6.**无水印**：EZGIF 提供的工具通常不带水印，用户可以自由使用。7.**简单易用**：界面直观，操作简单，适合各种水平的用户。

总的来说，EZGIF 是一个功能强大且易于使用的在线工具，适合需要处理 GIF 动画的用户。

### References

`[1]` FFmpeg 官网: *https://ffmpeg.org/download.html*
`[2]` EZGIF: *https://ezgif.com/*