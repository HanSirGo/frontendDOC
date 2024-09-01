# CSS按钮悬停效果

## 演示效果

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/3tk5sUAlYXo2vDHbic1iczLlmpJz7TTVtFNY12ibPHuzgibn9CZ6JjJbIZgLj2u5SJgCxEEY9gXW9992Z9ylys2CdQ/640?wx_fmt=gif&from=appmsg&wxfrom=13)

## HTML

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS按钮悬停效果</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
<div class="container">
  <a href="#" style="--clr:#1e9bff">
    <span></span><span></span><span></span><span></span>
    Button
  </a>
  <a href="#" style="--clr:#ff1867">
    <span></span><span></span><span></span><span></span>
    Button
  </a>
  <a href="#" style="--clr:#6eff3e">
    <span></span><span></span><span></span><span></span>
    Button
  </a>
</div>
</body>
</html>
```

## CSS

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #27282c;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 120px;
}
.container a {
  position: relative;
  padding: 16px 30px;
  font-size: 1.5em;
  color: var(--clr);
  text-shadow: 0 0 15px var(--clr);
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  transition: 0.5s;
  border: 2px solid rgba(0,0,0,0.5);
}
.container a:hover {
  color: #fff;
  border: 2px solid rgba(0,0,0,0);
  text-shadow: 0 0 0 var(--clr);
}
.container a::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: var(--clr);
  z-index: -1;
  transform: scale(0);
  transition: 0.5s;
}
.container a:hover::before {
  transform: scale(1);
  transition-delay: 0.5s;
  box-shadow: 0 0 10px var(--clr),
  0 0 30px var(--clr),
  0 0 60px var(--clr);
}
.container a span {
  position: absolute;
  background: var(--clr);
  pointer-events: none;
  box-shadow: 0 0 10px var(--clr),
  0 0 20px var(--clr),
  0 0 30px var(--clr),
  0 0 50px var(--clr),
  0 0 100px var(--clr);
}
.container a span:nth-child(1),
.container a span:nth-child(3) {
  width: 40px;
  height: 4px;
}
.container a span:nth-child(2),
.container a span:nth-child(4) {
  width: 4px;
  height: 40px;
}
.container a span:nth-child(1) {
  top: calc(50% - 2px);
  left: -50px;
  transform-origin: left;
  transition: 0.5s ease-in-out;
  transition-delay: 0.25s;
}
.container a:hover span:nth-child(1) {
  left: 50%;
  transform: translateX(-50%);
  transition-delay: 0s;
  z-index: -1;
}
.container a span:nth-child(3) {
  top: calc(50% - 2px);
  right: -50px;
  transform-origin: right;
  transition: 0.5s ease-in-out;
  transition-delay: 0.25s;
}
.container a:hover span:nth-child(3) {
  right: 50%;
  transform: translateX(50%);
  transition-delay: 0s;
  z-index: -1;
}
.container a span:nth-child(2) {
  left: calc(50% - 2px);
  top: -50px;
  transform-origin: top;
  transition: 0.5s ease-in-out;
  transition-delay: 0.25s;
}
.container a:hover span:nth-child(2) {
  top: 50px;
  transform: scaleY(0);
  opacity: 0;
  transition-delay: 0s;
}
.container a span:nth-child(4) {
  left: calc(50% - 2px);
  bottom: -50px;
  transform-origin: bottom;
  transition: 0.5s ease-in-out;
  transition-delay: 0.25s;
}
.container a:hover span:nth-child(4) {
  bottom: 50px;
  transform: scaleY(0);
  opacity: 0;
  transition-delay: 0s;
}
```

