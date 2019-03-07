### 属性介绍
####    1.  animation-delay
从动画应用在元素，到动画开始的时间间隔。
*   s、ms，可接收多个值，正值为延迟数；
*   0 和负值为应用在元素时立刻执行动画，负值的起始位置是动画序列中对应这个负值的位置
####    2.  animation-direction
动画是否反向播放。
*   `normal` 正向循环，结束回到起始位；
*   `alternate` 动画交替反向运行，时间功能函数也反向，如：`ease-in` 变为 `ease-out`；
*   `reverse` 反向循环，起始值在尾处，结束回到尾处；
*   `alternate-reverse` 反向交替，第一次是反向的，然后交替
####    3.  animation-duration
一个动画周期的时长。
*   单位是 s、ms；
*   没单位的话无效，负值无效，早期有些前缀的算为0s，可以忽略
####    4.  animation-fill-mode
动画执行之前和之后，如何给动画对象应用样式。
*   `none` 不改变样式；
*   `forwards` 执行之后保存为动画最后一帧的样式，最后一帧取决为 `animation-direction` 和 `animation-iteration-count`（方向和循环次数）；
*   `backwards` 执行之前保持第一帧样式；
*   `both` 执行之前保留第一帧，执行之后保留最后一帧（[图表示例](https://segmentfault.com/q/1010000003867335)）
####    5.  animation-iteration-count
动画循环次数。
*   `infinite` 无限循环；
*   数值，不可为负数，可以为小数，即动画播放到关键帧的百分比
####    6.  animation-name
动画名称。
*   可以指定为 `none` 使动画失效
####    7.  animation-play-state
定义一个动画是运行还是暂停（可通过类名添加 class 控制，或者直接改属性）。
*   `running` 运行
*   `paused` 暂停
####    8.  animation-timing-function
动画周期的节奏，作用于关键帧。
*   待补充细化
####    9.  animation
*   `animation-name`
*   `animation-duration`
*   `animation-timing-function`
*   `animation-delay`
*   `animation-direction`
*   `animation-iteration-count`
*   `animation-fill-mode`
*   `animation-play-state`
####    10. transition
过渡
####    11. transform-origin
2D 与 3D 变换
***
### 练习
*   codepen
*   掘金
*   慕课网
### 总结 Demo

### Done
*   慕课网-CSS动画实用技巧
*   