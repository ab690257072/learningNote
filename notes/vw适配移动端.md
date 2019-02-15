##   [使用Flexible实现手淘H5页面的终端适配](https://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)
### 原则
*   选择一种尺寸作为设计和开发基准
*   定义一套适配规则，自动适配剩下的两种尺寸(其实不仅这两种，你懂的)
*   特殊适配效果给出设计效果

![](http://www.w3cplus.com/sites/default/files/blogs/2015/1511/rem-6.jpg)
> 手淘设计师常选择 **iPhone6** 作为基准设计尺寸，交付给前端的设计尺寸是按 **750px * 1334px** 为准(高度会随着内容多少而改变)。前端开发人员通过一套适配规则自动适配到其他的尺寸。
***
### 基本概念
####    视窗 viewport
在桌面端，`viewport` 就是浏览器的宽高；移动端较复杂，为了更好的css布局，移动端分为两个 `viewport`：虚拟的 `viewportvisualviewport` 和 布局的 `viewportlayoutviewport`。
>   参考：
[stackoverflow的二者区别阐述](https://stackoverflow.com/questions/6333927/difference-between-visual-viewport-and-layout-viewport)
[viewport](https://www.w3cplus.com/css/viewports.html)
####    物理像素
显示设备真实的最小物理部件。
####    设备独立像素
设备独立像素也成为**密度无关像素**，代表一个可以由程序使用的虚拟像素点（如css像素），然后转换为物理像素。
####    css像素
一个抽象单位，主要用来精确度量 Web页面的内容。也称 `DIPS`，即与设备无关的像素。
####    屏幕密度
每英寸的像素数（PPI）。
####    设备像素比
dpr，**设备像素比 = 物理像素 / 设备独立像素**
访问：js 中 `window.devicePixelRatio`，css 中 `-webkit-device-pixel-ratio`、`-webkit-min-device-pixel-ratio`、`-webkit-max-device-pixel-ratio`（webkit 内核浏览器和 webview）
####    eg:
``` css
div {
    width: 2px; /* 在普通屏幕上对应2个物理像素，Retina屏因为dpr为2，物理像素数翻倍，所以对应4个物理像素 */
    height: 2px;
}
```
>参考：[走向视网膜（Retina）的Web时代](https://www.w3cplus.com/css/towards-retina-web.html)
####    适配图片的简单示范
![](http://www.w3cplus.com/sites/default/files/blogs/201212/retina-web-10.jpg)
####    meta标签
告诉浏览器如何规范渲染页面
``` html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
```
####    css单位 rem
根元素字号。
####    flexible 实质
`flexible` 实际上就是能过 `JS` 来动态改写 `meta` 标签。

它上做了一些事情：
*   动态改写 meta 标签
*   给 html 元素添加 `data-dpr` 属性，并且动态改写 `data-dpr` 的值
*   给 html 元素添加 `font-size` 属性，并且动态改写 `font-size` 的值
***
### 实战
####    把视觉稿中的 px 转换成 rem
设计师给了 750px 的设计稿，按 flexible 的原理是，将设计稿分成 100份，一份为 a，1rem 为 10a。所以 750px 设计稿的单位是：
``` javascript
1a = 7.5px
1rem = 75px
```
所以在设计稿中的任意尺寸，如 `176px * 176px` 就可以换算为 `2.346667rem * 2.346667rem`。
####    自动转rem的几种方案
1.  自动转换插件（cssrem），或手动转换插件（px to rem）
2.  预处理器（sass、less、postCSS）
>参考：
[sass中 px 转 rem](https://www.w3cplus.com/blog/tags/143.html)
####    字号不使用 rem
在部分屏幕上希望字号保持一致，或者一些大屏手机上能看到更多的文字，且不希望出现13、15px 这种奇葩字号（因为大多数字体自带些点阵尺寸，一般为16px 和 24px），所以 rem 方案不适合段落文本。所以文本还是用 px，用 `[data-dpr]` 来区分不同 `dpr` 下文本字号大小。其他特殊情况仍然可以用 rem 特殊处理。
***
##  [再聊移动端页面的适配](https://www.w3cplus.com/css/vw-for-layout.html)
之前的 flexible 是模拟了 `vw`，现在考虑直接用 `vw` 方案。
`vw` 基于 `viewport`，而 `viewport` 就是浏览器可视区域。
![](https://www.w3cplus.com/sites/default/files/blogs/2017/1707/vw-layout-4.png)
`viewport` 相关单位有**4**个：
*   `vw`：`window.innerWidth` 的 **1%**；
*   `vh`：`window.innerHeight` 的 **1%**；
*   `vmin`：`vw` 和 `vh` 中的较小值；
*   `vmax`：`vw` 和 `vh` 中的较大值

![](https://www.w3cplus.com/sites/default/files/blogs/2017/1707/vw-layout-5.png)
设计稿是 `750px` 的话，`100vw = 750px`，`1vw = 7.5px`，可以使用 `postCSS` 的插件 `postcss-px-to-viewport` 将 `px` 转换成 `vw` 值。

以下情况均可使用 `vw` 适配我们的页面：
*   容器适配
*   文本适配
*   **大于** `1px` 的边框、圆角、阴影
*   内外边距

####    关于长宽比
有些宽度不一致的但是需要高度一致的设计稿，需要计算长宽比，具体不再赘述，参考以下文章：
>参考：
[CSS实现长宽比的几种方案](https://www.w3cplus.com/css/aspect-ratio.html)
[容器长宽比](https://www.w3cplus.com/css/aspect-ratio-boxes.html)
[Web中如何实现纵横比](https://www.w3cplus.com/css/experiments-in-fixed-aspect-ratios.html)
[实现精准的流体排版原理](https://www.w3cplus.com/css/css-polyfluidsizing-using-calc-vw-breakpoints-and-linear-equations.html)

目前采用PostCSS插件只是一个过渡阶段，在将来我们可以直接在CSS中使用 `aspect-ratio` 属性来实现长宽比。
####    解决 1px 方案
在《[再谈Retina下1px的解决方案](https://www.w3cplus.com/css/fix-1px-for-retina.html)》中有多种解决方案，再补充以下一种，使用 `PostCSS` 插件——[postcss-write-svg](https://github.com/jonathantneal/postcss-write-svg)。
####    PostCSS 的参考文章
>参考：
[w3plus 里的 PostCSS 相关文章](https://www.w3cplus.com/blog/tags/516.html)
《深入PostCSS Web设计》
####    降级处理
部分机型不支持 `vw`，如果需要，可以做特殊处理：
*   `CSS Houdini`：通过 `CSS Houdini` 针对 `vw` 做处理，调用 `CSS Typed OM Level1` 提供的 `CSSUnitValue API`；
*   `CSS Polyfill`：通过相应的 `Polyfill` 做相应的处理，目前针对于 `vw` 单位的 `Polyfill` 主要有：`vminpoly`、`Viewport Units Buggyfill`、`vunits.js` 和 `Modernizr`。个人推荐采用 `Viewport Units Buggyfill`
####    viewport 的不足之处
*   采用 `vw` 和 `px` 混合时，计算不准确易造成溢出，以后浏览器对 `calc` 支持，可以解决这种问题；
*   `px` 转 `vw` 无法整除，有误差
####    测试自己的机器对方案支持情况
![](https://ws2.sinaimg.cn/large/006tKfTcly1g05tk96mflj30na0e0aap.jpg)
***
##   [如何在Vue项目中使用vw实现移动端适配](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)
前两者并不完美，在此提供一个更为完美的方案
### 准备工作
>参考：
[Webpack文档](https://doc.webpack-china.org/)
[Awesome Webpack](https://github.com/webpack-contrib/awesome-webpack)
[Webpack 教程资源收集](https://segmentfault.com/a/1190000005995267)
[Vue+Webpack开发可复用的单页面富应用教程](https://zhuanlan.zhihu.com/p/21702056)
### 安装依赖
``` shell
npm i postcss-import postcss-url postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext postcss-viewport-units cssnano -S
```
``` shell
npm i cssnano-preset-advanced -D
```
### 配置 .postcssrc.js
``` javascript
module.exports = {
    "plugins": {
        "postcss-import": {}, // 处理@import
        "postcss-url": {}, // 处理图片、字体之类的路径
        "postcss-aspect-ratio-mini": {}, // 处理宽高比
        "postcss-write-svg": {
            utf8: false // 处理 1px 边框
        },
        "postcss-cssnext": {}, // css未来的特性支持，https://cssnext.io/
        "postcss-px-to-viewport": { // px 自动转 vw
            viewportWidth: 750, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
            viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
            unitPrecision: 3, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
            viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
            selectorBlackList: ['.ignore', '.hairlines', /^body$/], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名，hairlines一般用于 0.5px 的边框
            minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
            mediaQuery: false, // 允许在媒体查询中转换`px`
            exclude: /^node_modules$/
        },
        "cssnano": { // 压缩清理 css 代码，在 Webpack 中和 `css-loader` 捆在一起不用手动加载它
            preset: "advanced",
            autoprefixer: false, // 默认的 autoprefixer 去掉，只用 cssnext里带的，cssnano 里的也不用
            "postcss-zindex": false // 启动这个插件的话 z-index 会重置为 1，所以禁用，其他集成的插件如果也想禁用，就类似这种用 false
        }
    }
}
```
`postcss-aspect-ratio-mini` 处理宽高比，用法如下：
``` html
    <!-- 基本结构 -->
    <div aspectratio w-188-246 class="color">
        <div aspectratio-content>123</div>
    </div>
```
``` css
/* 默认样式，用类也行，属性选择器看着简洁 */
[aspectratio] {
  position: relative;
}
[aspectratio]::before {
  content: '';
  display: block;
  width: 1px;
  margin-left: -1px;
  height: 0;
}
[aspectratio-content] {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
}
/* 容器样式，注意不能 aspect-ratio 写一起，会被清除 */
[w-188-246] {
  width: 188px;
  background-color: red;
}
[w-188-246] {
  aspect-ratio: '188:246';
}
```
编译后：
``` css
[w-188-246] {
    width: 25.067vw;
    background-color: red;
}
[w-188-246]:before {
    padding-top: 130.85106382978725%;
}
```
`postcss-write-svg` 插件主要用来处理 `1px` 的边框问题，主要使用的是 `border-image` 和 `background` 来做 `1px` 的相关处理，比如：
``` css
    @svg 1px-border {
        height: 2px;
        @rect: {
            fill: var(--color, black);
            width: 100%;
            height: 50%;
        }
    }
    .example {
        border: 1px solid transparent;
        border-image: svg(1px-border param(--color #00b1ff)) 2 2 stretch;
    }
```
编译后：
``` css
    .example {
        border: 1px solid transparent;
        border-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' height='2px'%3E%3Crect fill='%2300b1ff' width='100%25' height='50%25'/%3E%3C/svg%3E") 2 2 stretch;
    }
```
还有用 `background-image`的，比如：
``` css
    @svg square {
        @rect {
            fill: var(--color, black);
            width: 100%;
            height: 100%;
        }
    }
    #example {
        background: white svg(square param(--color #00b1ff));
    }
```
编译后：
``` css
#example {
    background: white url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Crect fill='%2300b1ff' width='100%25' height='100%25'/%3E%3C/svg%3E");
}
```
其他方法上文《再谈Retina下1px的解决方案》有写，不再赘述。
>由于有一些低端机对border-image支持度不够友好，个人建议你使用background-image的这个方案。
### CSS Modules
[官方文档](https://vue-loader.vuejs.org/guide/css-modules.html#usage)
