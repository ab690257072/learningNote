#   Less
### 基础用法
***
####    变量
1.  值变量
``` less
@red: #e56a69;
div {
    color: @red;
}
```
2.  选择器变量
``` less
@mySelector: #wrap;
@Wrap: wrap;
@{mySelector} {
    color: blue;
}
#@{Wrap} {
    color: #ccc;
}
```
3.  属性变量
``` less
@borderStyle: border-style;
@Solid: solid;
div {
    @{borderStyle}: @Solid;
}
```
4.  url变量
``` less
@images: '../img';
div {
    background: url('@{images}/dog.png');
}
```
5.  声明变量
``` less
/* 注意有个冒号 */
@background: {
    background: red;
}
div {
    @background();
}
@Rules: {
    width: 200px;
    height: 200px;
    border: 1px solid red;
}
#wrap {
    @Rules();
}
```
6.  变量运算
``` less
@width: 300px;
@color: #222;
div {
    width: @width-20;
    color: @color + #111;
}
```
7.  变量作用域
``` less
@var: @a;
@a: 100%;
#wrap {
  width: @var;
  @a: 9%;
}
/* 生成的 CSS */
#wrap {
  width: 9%;
}
```
8.  用变量去定义变量
``` less
@a: 'I am a';
@b: 'a';
div:after {
    content: @@b;
}
/* 生成的 CSS */
div:after {
    content: 'I am a';
}
```
####    嵌套
1.  `&` 代表上级
2.  媒体查询
    *   写在选择器里，以前是需要分开写
    *   唯一的缺点就是 每一个元素都会编译出自己 @media 声明，并不会合并。
``` less
/* Less */
#main{
    //something...
    @media screen{
        @media (max-width:768px){
          width:100px;
        }
    }
    @media tv {
      width:2000px;
    }
}
/* 生成的 CSS */
@media screen and (maxwidth:768px){
  #main{
      width:100px; 
  }
}
@media tv{
  #main{
    width:2000px;
  }
}
```
####    混合
1.  无参数方法
    *   方法和 变量声明集合 类似
``` less
/* Less */
.card { // 等价于 .card()
    background: #f6f6f6;
    -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
#wrap{
  .card;//等价于.card();
}
/* 生成的 CSS */
#wrap{
  background: #f6f6f6;
  -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
  box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
```
其中 .card 与 .card() 是等价的。 个人建议，为了避免 代码混淆，应写成 :
``` less
.card(){
  //something...
}
#wrap{
  .card();
}
```
要点：
    *   `.` 和 `#` 皆可作为 方法前缀；
    *   写不写 `()` 看个人习惯

2.  默认参数方法
    *   如果没传参，就会使用默认参数；
    *   `@arguments` 代表所有参数；
    *   传的参数必须带单位（关键字除外）
``` less
/* Less */
.border(@a:10px,@b:50px,@c:30px,@color:#000){
    border:solid 1px @color;
    box-shadow: @arguments;//指代的是 全部参数
}
#main{
    .border(0px,5px,30px,red);//必须带着单位
}
#wrap{
    .border(0px);
}
#content{
  .border;//等价于 .border()
}
/* 生成的 CSS */
#main{
    border:solid 1px red;
    box-shadow:0px,5px,30px,red;
}
#wrap{
    border:solid 1px #000;
    box-shadow: 0px 50px 30px #000;
}
#content{
    border:solid 1px #000;
    box-shadow: 10px 50px 30px #000;
}
```
3.  方法的匹配模式
``` less
/* Less */
.triangle(top,@width:20px,@color:#000){
    border-color:transparent  transparent @color transparent ;
}
.triangle(right,@width:20px,@color:#000){
    border-color:transparent @color transparent  transparent ;
}
.triangle(bottom,@width:20px,@color:#000){
    border-color:@color transparent  transparent  transparent ;
}
.triangle(left,@width:20px,@color:#000){
    border-color:transparent  transparent  transparent @color;
}
.triangle(@_,@width:20px,@color:#000){
    border-style: solid;
    border-width: @width;
}
#main{
    .triangle(left, 50px, #999)
}
/* 生成的 CSS */
#main{
  border-color:transparent  transparent  transparent #999;
  border-style: solid;
  border-width: 50px;
}
```
