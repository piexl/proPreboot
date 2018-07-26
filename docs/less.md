# ProPreboot-Less介绍

## 了解less

+ [less 中文官网](http://lesscss.cn/)
+ [less 教程](http://www.bootcss.com/p/lesscss)

>Less 是一门 CSS 预处理语言，它扩充了 CSS 语言，增加了诸如变量、混合（mixin）、函数等功能，让 CSS 更易维护、方便制作主题、扩充。
> Less 可以运行在 Node、浏览器和 Rhino 平台上。网上有很多第三方工具帮助你编译 Less 源码。

# 变量（Variables）

CSS中没有变量这一概念，但是在LESS或其他CSS预处理器中是存在的。ProPreboot包含了几组有意义并且有用的变量，这些可以被用到任何项目中。

## 移动端相对单位@rpx

这变量的定义是为了做移动端的适配，与库`Flexible`配合使用，了解原理[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)

```less
//设计稿750px分成100份，1a=7.5px, 1rem=75px, 设计稿中的1px = 1/75rem;
@rpx:1/75rem;
```

## 媒体查询节点的定义

媒体查询设置了小屏`xs`、`sm`、`md`、`lg`,

```less
/*===== 媒体查询节点 ======*/
@screen-xs:         576px; //超小屏幕
@screen-sm:         768px; //小屏幕
@screen-md:         992px; //中等屏幕
@screen-lg:         1200px; //超大屏幕
@screen-xs-min:     @screen-xs;
@screen-sm-min:     @screen-sm;
@screen-md-min:     @screen-md;
@screen-lg-min:     @screen-lg;
@screen-xs-max:     (@screen-sm-min - 1);
@screen-sm-max:     (@screen-md-min - 1);
@screen-md-max:     (@screen-lg-min - 1);
```

## 颜色的定义

可以根据项目定制自己项目的颜色方案和品牌颜色

```less
/*===== 颜色的定义 =====*/
@red            : #e22428;
@orange         : #F2711C;
@yellow         : #FBBD08;
@olive          : #B5CC18;
@green          : #21BA45;
@teal           : #00B5AD;
@blue           : #2185D0;
@violet         : #6435C9;
@purple         : #A333C8;
@pink           : #E03997;
@brown          : #A5673F;
@grey           : #767676;
@black          : #1B1C1D;
@white          : #FFFFFF;

//灰色
@gray-darker:     lighten(@black, 13.5%); // #222
@gray-dark:       lighten(@black, 20%);   // #333
@gray:            lighten(@black, 33.5%); // #555
@gray-light:      lighten(@black, 60%);   // #999
@gray-lighter:    lighten(@black, 93.5%); // #eee

/*===== 品牌颜色设置 =====*/
@brand-primary  : @blue;
@brand-success  : @green;
@brand-info     : @teal;
@brand-warning  : @yellow;
@brand-danger   : @red;
```

项目中使用：

```less
.link-primary{
    color:@brand-primary;
}
```

## 网站外观的关键颜色设置

网站的外观

```less
@body-bg:    #fff;
@text-color: lighten(@black, 10%);
```

## 链接

很简单的修改和设置页面链接的样式

```less
/*===== 链接颜色设置 =====*/
@link-color:       @brand-primary;
@link-hover-color: darken(@link-color, 10%);
@link-hover-decoration: underline;

//用法
a {
  color: @link-color;
  text-decoration: none;
  &:hover {
    color: @link-hover-color;
    text-decoration: @link-hover-decoration;
  }
}
```

*注意： `@link-color-hover` 变量是通过一个函数赋值的，这是LESS中另一个非常好的工具，它能自动产生正确的颜色。还可以使用的函数由：`darken`、`lighten`、`saturate` 和 `desaturate`。* 

了解更多： [less Color函数](http://www.bootcss.com/p/lesscss/#-color-functions)

## 排版

定义了页面的字体大小、字体、行高等

```less
// 文字字体
@font-family-sans-serif:  'Microsoft YaHei',"Helvetica Neue", Helvetica, Arial, sans-serif;
@font-family-serif:       Georgia, "Times New Roman", Times, serif;
@font-family-monospace:   Monaco, Menlo, Consolas, "Courier New", monospace;
@font-family-base:        @font-family-sans-serif;

/*===== 文字大小 =====*/
@font-size-base:          14px;
@font-size-xxxl:          ceil((@font-size-base * 3)); //42px
@font-size-xxl:           ceil((@font-size-base * 2.28)); //32px
@font-size-xl:            ceil((@font-size-base * 1.71)); //24px
@font-size-lg:            ceil((@font-size-base * 1.25)); //18px
@font-size-md:            ceil((@font-size-base * 1.14)); //16px
@font-size-sm:            ceil((@font-size-base * 0.85)); //12px

// 文字行高
@line-height-base:        1.5;
```

# 栅格系统

## 栅格变量

下面是两个栅格默认变量，定制这些变量可以自动影响到栅格

```less
// 栅格变量
@grid-columns:          24; // 默认栅格列数
@grid-column-gutters:   30px; // 默认间隙沟槽
```

## 栅格混合函数

这三个函数配合使用来制作我们想要的栅格系统

+ `.make-row(@grid-column-gutters)` 列的包装器，以通过负边距对齐内容并清除浮动,`@grid-column-gutters`为间隙沟槽
+ `.make-column(@columns,@grid-columns,@grid-column-gutters)` 生成列，`@columns`为列数，`@grid-columns`为栅格列数，`@grid-column-gutters`为间隙沟槽
+ `.make-column-offset(@columns:1,@grid-columns:@grid-columns)`,生成偏移列

```less
// 制作网格
.make-row(@grid-column-gutters: @grid-column-gutters) {
    //设置负边距让行对其
    margin-left: -@grid-column-gutters/2;
    margin-right: -@grid-column-gutters/2;
    // 清除浮动
    .clearfix();
}
// 制作列
.make-column(@columns:1,@grid-columns:@grid-columns,@grid-column-gutters: @grid-column-gutters) {
    float: left;
    //根据列数和总列计算宽
    width: percentage(@columns / @grid-columns);
    // 防止列空时崩溃
    min-height: 1px;
    // 设置padding作为槽
    padding-left: @grid-column-gutters/2;
    padding-right: @grid-column-gutters/2;
    // 设置为border-box（填充不能增加宽度）
    .box-sizing(border-box);
}
// 制作偏移列
.make-column-offset(@columns:1,@grid-columns:@grid-columns) {
    margin-left: percentage(@columns / @grid-columns);
}
```

## 使用案例

使用这些mixin可以很容易的生成所有列，**下面就是HTML和CSS共同组成的一个实例**

```html
    <div class="wrapper">
        <div class="content-main">
            ...
        </div>
        <div class="content-sidebar">
            ...
        </div>
    </div>
```

```less
@grid-columns:10;
@grid-column-gutters:20px;
.wrapper {
    .make-row(@grid-column-gutters:@grid-column-gutters);
}
.content-main {
    .make-column(@columns:6,@grid-columns:@grid-columns,@grid-column-gutters: @grid-column-gutters);
}
.content-sidebar {
    .make-column(@columns:3,@grid-columns:@grid-columns,@grid-column-gutters: @grid-column-gutters);
    .make-column-offset(@columns:1,@grid-columns:@grid-columns);
}
```

如果传参数`@grid-columns`和`@grid-column-gutters`,将使用默认的`24`栅格和默认间隙槽`30px`生成栅格

为了做响应式我们还可以配合我们定义的媒体查询断点做栅格的响应式处理，如下面代码

```less
@media(max-width:@screen-md-max){
    @grid-columns:12;
    @grid-column-gutters:0;
    .wrapper {
        .make-row(@grid-column-gutters:@grid-column-gutters);
    }
    .content-main {
        .make-column(@columns:6,@grid-columns:@grid-columns,@grid-column-gutters: @grid-column-gutters);
    }
    .content-sidebar {
        .make-column(@columns:4,@grid-columns:@grid-columns,@grid-column-gutters: @grid-column-gutters);
        .make-column-offset(@columns:2,@grid-columns:@grid-columns);
    }
}
```

这样我们就是设置了在中等屏幕`@screen-md-max`下生成了分别为`6`和`4`栅格的`12`栅格，并设置了`4`栅格偏移`2`列


# 混合函数

特定特定厂商的mixin

## 盒模型（box-sizing）

通过一个mixin可以重置组件的盒模型(box model)，`@boxmodel`参数为：`content-box | border-box | inherit`

```less
.box-sizing(@boxmodel:border-box) {
  -webkit-box-sizing: @boxmodel;
    -moz-box-sizing: @boxmodel;
      box-sizing: @boxmodel;
}
```

## 清除浮动

用来清除浮动

```less
//清除浮动
.clearfix() {
  &:before,
  &:after {
    content: " ";
    display: table;
  }
  &:after {
    clear: both;
  }
}
```

## 水平居中

```less
//水平居中
.center-block() {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
```

## 设置尺寸

```less
// 设置尺寸
.size(@width, @height) {
  width: @width;
  height: @height;
}
// 设置方形
.square(@size) {
  .size(@size, @size);
}
```

## 显示与隐藏

```less
// 显示
.responsive-visibility() {
  display: block !important;
}
// 不显示
.responsive-invisibility() {
  display: none !important;
}
```

## 响应图片

```less
// 响应图片
.img-responsive(@display: block) {
  display: @display;
  max-width: 100%;
  height: auto;
}
```

## 可滚动的

```less
//可滚动的
.scrollable() {
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}
```

## 透明度(opacity)

```less
//透明度
.opacity(@opacity) {
  opacity: @opacity;
  @opacity-ie: @opacity * 100;
  filter: ~"alpha(opacity=@{opacity-ie})"; // IE8
}
```

## 文字颜色

```less
//文字颜色
.text-color(@color) {
  color: @color;
  a[href]&:hover,
  a[href]&:focus {
    color: darken(@color, 10%);
  }
}
```

## 文字大小（只要适用于移动端）

```less
//文字大小
.font-dpr(@fontSize){
  font-size:@fontSize;
  [data-dpr="2"] & {
    font-size:@fontSize * 2;
  }
  [data-dpr="3"] & {
    font-size:@fontSize * 3;
  }
}
```

## 单行文字超出截断

`@display` 为`block`或`inline-block`

```less
//单行文字超出隐藏
.text-truncate(@display: block) {
  display: @display;
  word-wrap: normal; /* for IE */
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
```

## 多行文字超出截断

+ `@line-num` 显示的行数
+ `@line-height` 行高

```less
//多行文字超出隐藏
.text-truncate-lines(@line-num:3, @line-height:1.5em){
  overflow : hidden;
  height:@line-height*@line-num;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp:@line-num;
  -webkit-box-orient: vertical;
}
```

## placeholder文字颜色

```less
//placeholder文字颜色
.placeholder(@color: @input-color-placeholder) {
  &:-moz-placeholder            { color: @color; } // Firefox 4-18
  &::-moz-placeholder           { color: @color; } // Firefox 19+
  &:-ms-input-placeholder       { color: @color; } // Internet Explorer 10+
  &::-webkit-input-placeholder  { color: @color; } // Safari and Chrome
}
```

## 可调节 textareas 

`@direction`:resize方向 `horizontal`(水平方向), `vertical`(垂直方向), `both`

```less
// 可调节 textareas 
.resizable(@direction: both) {
  resize: @direction; // Options: horizontal, vertical, both
  // Safari fix
  overflow: auto;
}
```

## 圆角

如今所有现代浏览器都能支持不带前缀的border-radius 属性,所以我们只做了单边的圆角

```less
//单边圆角
.border-top-radius(@radius) {
  border-top-right-radius: @radius;
  border-top-left-radius: @radius;
}
.border-right-radius(@radius) {
  border-bottom-right-radius: @radius;
  border-top-right-radius: @radius;
}
.border-bottom-radius(@radius) {
  border-bottom-right-radius: @radius;
  border-bottom-left-radius: @radius;
}
.border-left-radius(@radius) {
  border-bottom-left-radius: @radius;
  border-top-left-radius: @radius;
}
```

## 阴影

```less
.box-shadow(@shadow) {
  -webkit-box-shadow: @shadow; // iOS <4.3 & Android <4.1 & bb7.0
    box-shadow: @shadow;
}
```

## 箭头

+ `@color`: 箭头颜色 如`#DDD`;
+ `@width`: 箭头大小 如`6px`;
+ `@border-width`: 箭头线宽大小2px;
+ `@deg`: 箭头旋转角度45deg

```less
.arrow(@color:#DDD; @width:6px; @border-width: 2px; @deg: 45deg) {
  display: inline-block;
  width: @width;
  height: @width;
  border: @color solid;
  border-width: @border-width @border-width 0 0;
  transform: rotate(@deg);
}
```

## 渐变

渐变背景

+ `@start-color`       起始颜色
+ `@mid-color`         结束颜色
+ `@end-color`         结束颜色
+ `@start-percent`     起始角度
+ `@end-percent`       结束角度

水平方向直线渐变

```less
//水平方向直线渐变
.gradient-horizontal(@start-color: #555; @end-color: #333; @start-percent: 0%; @end-percent: 100%) {
  background-image: -webkit-gradient(linear, @start-percent top, @end-percent top, from(@start-color), to(@end-color)); // Safari 4+, Chrome 2+
  background-image: -webkit-linear-gradient(left, color-stop(@start-color @start-percent), color-stop(@end-color @end-percent)); // Safari 5.1+, Chrome 10+
  background-image: -moz-linear-gradient(left, @start-color @start-percent, @end-color @end-percent); // FF 3.6+
  background-image: linear-gradient(to right, @start-color @start-percent, @end-color @end-percent); // Standard, IE10
  background-repeat: repeat-x;
  filter: e(%("progid:DXImageTransform.Microsoft.gradient(startColorstr='%d', endColorstr='%d', GradientType=1)",argb(@start-color), argb(@end-color))); // IE9 and down
}
```

垂直方向直线渐变

```less
//垂直方向直线渐变
.gradient-vertical(@start-color: #555; @end-color: #333; @start-percent: 0%; @end-percent: 100%) {
  background-image: -webkit-gradient(linear, left @start-percent, left @end-percent, from(@start-color), to(@end-color)); // Safari 4+, Chrome 2+
  background-image: -webkit-linear-gradient(top, @start-color @start-percent, @end-color @end-percent); // Safari 5.1+, Chrome 10+
  background-image: -moz-linear-gradient(top, @start-color @start-percent, @end-color @end-percent); // FF 3.6+
  background-image: linear-gradient(to bottom, @start-color @start-percent, @end-color @end-percent); // Standard, IE10
  background-repeat: repeat-x;
  filter: e(%("progid:DXImageTransform.Microsoft.gradient(startColorstr='%d', endColorstr='%d', GradientType=0)",argb(@start-color), argb(@end-color))); // IE9 and down
}
```

定向直线渐变

```less
//定向直线渐变
.gradient-directional(@start-color: #555; @end-color: #333; @deg: 45deg) {
  background-repeat: repeat-x;
  background-image: -webkit-linear-gradient(@deg, @start-color, @end-color); // Safari 5.1+, Chrome 10+
  background-image: -moz-linear-gradient(@deg, @start-color, @end-color); // FF 3.6+
  background-image: linear-gradient(@deg, @start-color, @end-color); // Standard, IE10
}
```

水平方向3色直线渐变

```less
//水平方向3色直线渐变
.gradient-horizontal-3c(@start-color: #00b3ee; @mid-color: #7a43b6; @color-stop: 50%; @end-color: #c3325f) {
  background-image: -webkit-gradient(left, linear, 0 0, 0 100%, from(@start-color), color-stop(@color-stop, @mid-color), to(@end-color));
  background-image: -webkit-linear-gradient(left, @start-color, @mid-color @color-stop, @end-color);
  background-image: -moz-linear-gradient(left, @start-color, @mid-color @color-stop, @end-color);
  background-image: linear-gradient(to right, @start-color, @mid-color @color-stop, @end-color);
  background-repeat: no-repeat;
  filter: e(%("progid:DXImageTransform.Microsoft.gradient(startColorstr='%d', endColorstr='%d', GradientType=1)",argb(@start-color), argb(@end-color))); // IE9 and down, gets no color-stop at all for proper fallback
}
```

垂直方向3色直线渐变

```less
//垂直方向3色直线渐变
.gradient-vertical-3c(@start-color: #00b3ee; @mid-color: #7a43b6; @color-stop: 50%; @end-color: #c3325f) {
  background-image: -webkit-gradient(linear, 0 0, 0 100%, from(@start-color), color-stop(@color-stop, @mid-color), to(@end-color));
  background-image: -webkit-linear-gradient(@start-color, @mid-color @color-stop, @end-color);
  background-image: -moz-linear-gradient(top, @start-color, @mid-color @color-stop, @end-color);
  background-image: linear-gradient(@start-color, @mid-color @color-stop, @end-color);
  background-repeat: no-repeat;
  filter: e(%("progid:DXImageTransform.Microsoft.gradient(startColorstr='%d', endColorstr='%d', GradientType=0)",argb(@start-color), argb(@end-color))); // IE9 and down, gets no color-stop at all for proper fallback
}
```

放射状渐变

```less
//放射状渐变
.gradient-radial(@inner-color: #555; @outer-color: #333) {
  background-image: -webkit-gradient(radial, center center, 0, center center, 460, from(@inner-color), to(@outer-color));
  background-image: -webkit-radial-gradient(circle, @inner-color, @outer-color);
  background-image: -moz-radial-gradient(circle, @inner-color, @outer-color);
  background-image: radial-gradient(circle, @inner-color, @outer-color);
  background-repeat: no-repeat;
}
```

条纹渐变

```less
//条纹渐变
.gradient-striped(@color: rgba(255,255,255,.15); @angle: 45deg) {
  background-image: -webkit-gradient(linear, 0 100%, 100% 0, color-stop(.25, @color), color-stop(.25, transparent), color-stop(.5, transparent), color-stop(.5, @color), color-stop(.75, @color), color-stop(.75, transparent), to(transparent));
  background-image: -webkit-linear-gradient(@angle, @color 25%, transparent 25%, transparent 50%, @color 50%, @color 75%, transparent 75%, transparent);
  background-image: -moz-linear-gradient(@angle, @color 25%, transparent 25%, transparent 50%, @color 50%, @color 75%, transparent 75%, transparent);
  background-image: linear-gradient(@angle, @color 25%, transparent 25%, transparent 50%, @color 50%, @color 75%, transparent 75%, transparent);
}
```

## CSS3 transform

matrix(n,n,n,n,n,n) 定义 2D 转换，使用六个值的矩阵。
matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n) 定义 3D 转换，使用 16 个值的 4x4 矩阵。

+ translate(x,y)  定义 2D 转换
+ translate3d(x,y,z)  定义 3D 转换。 
+ translateX(x) 定义转换，只是用 X 轴的值
+ translateY(y) 定义转换，只是用 Y 轴的值
+ translateZ(z) 定义 3D 转换，只是用 Z 轴的值。 
+ scale(x,y)  定义 2D 缩放转换
+ scale3d(x,y,z)  定义 3D 缩放转换。 
+ scaleX(x) 通过设置 X 轴的值来定义缩放转换。
+ scaleY(y) 通过设置 Y 轴的值来定义缩放转换。
+ scaleZ(z) 通过设置 Z 轴的值来定义 3D 缩放转换。  
+ rotate(angle) 定义 2D 旋转，在参数中规定角度。
+ rotate3d(x,y,z,angle) 定义 3D 旋转。 
+ rotateX(angle)  定义沿着 X 轴的 3D 旋转。
+ rotateY(angle)  定义沿着 Y 轴的 3D 旋转。
+ rotateZ(angle)  定义沿着 Z 轴的 3D 旋转。
+ skew(x-angle,y-angle) 定义沿着 X 和 Y 轴的 2D 倾斜转换。
+ skewX(angle)  定义沿着 X 轴的 2D 倾斜转换。
+ skewY(angle)  定义沿着 Y 轴的 2D 倾斜转换。  
+ perspective(n)  为 3D 转换元素定义透视视图。

```less
//偏移
.translate(@x,@y){
  -webkit-transform:translate(@x,@y);
          transform:translate(@x,@y);
}
//3d偏移
.translate3d(@x,@y,@z){
  -webkit-transform:translate3d(@x,@y,@z);
          transform:translate3d(@x,@y,@z);
}
//缩放
.scale(@x,@y){
  -webkit-transform:scale(@x,@y);
          transform:scale(@x,@y);
}
//3d缩放
.scale3d(@x,@y,@z){
  -webkit-transform:scale3d(@x,@y,@z);
          transform:scale3d(@x,@y,@z);
}
//角度旋转
.rotate(@angle){
  -webkit-transform:rotate(@angle);
          transform:rotate(@angle);
}
//3dX轴旋转
.rotateX(@angle){
  -webkit-transform:rotateX(@angle);
          transform:rotateX(@angle);
}
//3dY轴旋转
.rotateY(@angle){
  -webkit-transform:rotateY(@angle);
          transform:rotateY(@angle);
}
//3dZ轴旋转
.rotateZ(@angle){
  -webkit-transform:rotateZ(@angle);
          transform:rotateZ(@angle);
}
//倾斜
.skew(@x, @y) {
  -webkit-transform: skew(@x, @y);
          transform: skew(@x, @y);
  -webkit-backface-visibility: hidden;
}
//X轴倾斜
.skewX(@angle){
  -webkit-transform:skewX(@angle); 
          transform:skewX(@angle);
}
//Y轴倾斜
.skewY(@angle){
  -webkit-transform:skewY(@angle);
          transform:skewY(@angle);
}
//变换中心
.transform-origin(@x,@y){
  -webkit-transform-origin:@x @y;
          transform-origin:@x @y;
}
```

##背面可见性

隐藏被旋转的 div 元素的背面
`@visibility` 为`visible`可见、`hidden`不可见

```less
//背面可见性
.backface-visibility(@visibility){
  -webkit-backface-visibility: @visibility;
          backface-visibility: @visibility;
}
```

## 动画混合

用于制作动画

+ `@name` 动画的名称
+ `@duration` 规定动画完成一个周期所花费的秒或毫秒
+ `@speed` 规定动画完成一个周期所花费的秒或毫秒
  + linear (动画从头到尾的速度是相同的)
  + ease (默认。动画以低速开始，然后加快，在结束前变慢。)
  + ease-in (动画以低速开始。)
  + ease-out (动画以低速结束。)
  + ease-out (动画以低速结束。)
  + ease-in-out (动画以低速开始和结束。)
  + cubic-bezier(n,n,n,n) (在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。)
+ `@delay` 定义动画开始前等待的时间，以秒或毫秒计
+ `@number` 定义动画播放次数的数值 n 或 infinite（规定动画应该无限次播放）。
+ `@direction` 规定动画是否在下一周期逆向地播放。normal（默认值。动画应该正常播放。） alternate（动画应该轮流反向播放。）；
+ `@State` 规定动画是否在下一周期逆向地播放。 paused（规定动画已暂停。） running（规定动画正在播放。）；
+ `@mode` 规定动画是否在下一周期逆向地播放。
  + none(不改变默认行为。)
  + forwards(当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）)
  + backwards(在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）)
  + both(向前和向后填充模式都被应用)

```less
//动画混合函数
.animation(@name,@duration,@speed,@number,@direction, @mode){
  -webkit-animation: @name @duration @speed  @number @direction @mode;
    animation: @name @duration @speed  @number @direction @mode;
}
//动画名
.animation-duration(@name){
  -webkit-animation-name:@name;
          animation-name:@name;
}
//动画执行时间
.animation-duration(@time){
  -webkit-animation-duration:@time;
          animation-duration:@time;
}
//动画延时时间
.animation-delay(@time){
  -webkit-animation-delay:@time;
          animation-delay:@time;
}
```

过度动画混合函数使用示例：

```less
// 动画示例
.animation{
  from {
    width:100px;
    height:100px;
    background-color:blue;
  }
  to {
    width:300px;
    height:300px;
    background-color:red;
  }
}
@keyframes animation{ .animation}
@-moz-keyframes animation{ .animation}
@-webkit-keyframes animation { .animation}
@-o-keyframes animation { .animation}

.animation_demo{
  width:100px;
  height:100px;
  background-color:blue;
  .animation(animation,2s,ease-in-out,1,normal,forwards);
}
```

## 过度动画混合

用于制作过度动画
过度动画函数：

+ `@property` 过渡效果的 CSS 属性的名称
  + none  没有属性会获得过渡效果
  + all  所有属性都将获得过渡效果。
  + property  定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔。如：width,height
+ `@duration` 完成过渡效果需要多少秒或毫秒, time 如：'2s' '10000ms'
+ `@timing-function`速度效果的速度曲线
  + linear (动画从头到尾的速度是相同的)
  + ease (默认。动画以低速开始，然后加快，在结束前变慢。)
  + ease-in (动画以低速开始。)
  + ease-out (动画以低速结束。)
  + ease-out (动画以低速结束。)
  + ease-in-out (动画以低速开始和结束。)
  + cubic-bezier(n,n,n,n) (在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。)
+ `@delay`  过渡效果何时开始, time 如：'2s' '10000ms'

```less

//过度动画
.transition(@property,@duration,@timing-function,@delay){
  transition:@property @duration @timing-function @delay;
  -ms-transition:@property @duration @timing-function @delay;
  -moz-transition:@property @duration @timing-function @delay;
  -webkit-transition:@property @duration @timing-function @delay;
  -o-transition:@property @duration @timing-function @delay;
}
```

过度动画混合函数使用示例：

```less
.transition_demo{
  width:100px;
  height:100px;
  background-color:blue;
  .transition(all,2s,ease,0s);
  &:hover{
    width:300px;
    height:300px;
    background-color:red;
  }
}
```
