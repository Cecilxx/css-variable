[slide]
# 如何在CSS中使用变量？
----
* 熟练使用的CSS预处理器 {:&.rollIn}
* 引人瞩目的CSS自定义变量

[slide]
## 面向级联变量的CSS自定义属性 
[CSS Custom Properties for Cascading Variables](https://www.w3.org/TR/2013/WD-css-variables-1-20130620/)
# 
# 
---
定义了一种用户自定义变量机制，允许任何CSS属性引用变量的取值 {:&.rollIn}

[slide]
## 第一个CSS变量
-----
<a href='http://www.w3cplus.com/css3/extending-the-color-cascade-with-the-css-currentcolor-variable.html'>currentColor</a>

```css
div { 
  color: red; 
  border: 1px solid currentColor;
  box-shadow: inset 2px 2px 3px currentColor;
  background-color: currentColor; /* not a good idea! */
  background-image: linear-gradient(currentColor, transparent);
}
```
{:&.rollIn}

[slide]
## CSS变量基本语法
* 定义：<span class='text-success'>--variable-name: variable-value</span> {:&.rollIn}
```css
:root{
    --fontcolor: #333;
    --main-bg: rgb(255, 255, 255);
    --logo-border-color: rebeccapurple;
    --header-height: 68px;
    --content-padding: 10px 20px;
    --base-line-height: 1.428571429;
    --transition-duration: .35s;
    --external-link: "external link";
    --margin-top: calc(2vh + 20px);
}
```
* 引用：<span class='text-success'>var(</span><span class='text-danger'>--variable-name [, declaration-value]</span><span class='text-success'>)</span>
```css
p {
    margin: var(--p-margin) 0 0 10px;
}
```
[slide]
#作用域

[slide]
## 使用<span class='text-danger'>:root</span> 作用域来定义全局变量
```css
:root{
    --global-var: 'global';
}
```

[slide]
## 特定元素/组件下的作用域
```
<div class="block">
  My block is
  <div class="block__highlight">awesome</div>
</div>
```

```css
.block {
  --block-font-size: 1rem;
  font-size: var(--block-font-size);
}
.block__highlight {
  --block-highlight-font-size: 1.5rem;
  font-size: var(--block-highlight-font-size);
}
```

[slide]
## 媒体查询下的作用域
```css
@media screen and (min-width: 1025px) {
  :root {
    --screen-category: 'desktop';
  }
}
```

[slide]
## 伪类下的作用域
```css
body {
  --bg: #f00;
  background-color: var(--bg);
  transition: background-color 1s;
}

body:hover {
  --bg: #ff0;
}
```
[slide]
## CSS变量级联(继承)
```
<div>
  <p>Content</p>
</div>
```
```css
div {
  --main-color: red;
}
p {
  color: var(--main-color); /* color: red */
}
```
[slide]
## 变量组合
```css
.block {
    --block-text: 'This is my block';
    --block-highlight-text: var(--block-text)' with highlight';
}

.block:before {
    content: var(--block-text);
}

.block__highlight:before {
    content: var(--block-highlight-text); /*This is my block with highlight*/
}
```

[slide]
## 计算方法 <span class='text-success'>calc()</span>

```css
:root {
  --fontSize: 20px;
}
p {
  font-size: calc(var(--fontSize,0) * 0.8) /* font-size: 16px*/ 
}
```

[slide]
## CSS变量与JS
```js
/* READ */
const rootStyles = getComputedStyle(document.documentElement);
const varValue = rootStyles.getPropertyValue('--screen-category').trim();

/* WRITE */
document.documentElement.style.setProperty('--screen-category', value);
```
[slide]
## 浏览器支持程度
<img src="/images/cssvar.png" alt="">
{:&.rollIn}
* CSS自定义变量已经在正式版Chrome，Firefox和桌面版Safari 9.1中获得支持 
{:&.rollIn}

[slide]
## 检测浏览器是否支持的方法
* [CSS条件查询规范：支持查询](http://www.w3cplus.com/css3/supports-will-change-your-life.html) {:&.rollIn}
```css
@supports ( (--a: 0)) {
    /* supported */
}
@supports ( not (--a: 0)) {
    /* not supported */
}
```
* [window.CSS.supports](https://developer.mozilla.org/en-US/docs/Web/API/CSS/supports) 
```js
if (window.CSS && window.CSS.supports && window.CSS.supports('--a', 0)) {
    alert('CSS properties are supported');
} else {
    alert('CSS properties are NOT supported');
}
```
[slide]
### [css-variables.js](https://gist.github.com/wesbos/8b9a22adc1f60336a699)
###  
```js
function () {
 var color = 'rgb(255, 198, 0)';
 var el = document.createElement('span');

 el.style.setProperty('--color', color);
 el.style.setProperty('background', 'var(--color)');
 document.body.appendChild(el);

 var styles = getComputedStyle(el);
 var doesSupport = styles.backgroundColor === color;
 document.body.removeChild(el);
 return doesSupport;
}
```

[slide]
# CSS变量与CSS预处理器变量的区别

[slide]
## 预处理器变量的限制
-----
```scss 
$gutter: 1em;

@media (min-width: 30em) {
  $gutter: 2em;
}

.Container {
  padding: $gutter;
}
``` 
{:&.rollIn}
```css
.Container {
  padding: 1em;
}
```

[slide]
## 预处理器变量不能级联(继承)
----
```
<div>
  <p>content</p>
</div>
```
```scss
$font-size: 1em;

div {
  $font-size: 1.5em;
}

p {
  font-size: $font-size;
}
```
{:&.rollIn}
```css
p {
  font-size: 1em;
}
```

[slide]
## CSS变量有何不同 ？
---
* CSS变量的动态性，能在页面运行时更改，而传统预处理器变量编译后是静态的 {:&.rollIn}
* CSS变量能够继承，能够组合使用，具有作用域，作用域是DOM
* 可以配合媒体查询使用
* 可以配合 Javascript 使用，方便的从 JS 中读/写

[slide]
## 使用注意事项
---
* 在一些浏览器中，针对CSS变量的复杂calc()运算可能不能工作 {:&.rollIn}
* 不能使用CSS自定义属性作为CSS属性名称：var(--side): 10px
* 进行calc()运算时，最好能提供默认值: calc(var(--base-line-height, 0) * 1rem)
* 不能作为媒体查询条件值使用：@media screen and (min-width: var(--desktop-breakpoint) {
* 图片地址，如url(var(--image-url)) ，不会生效

[slide]
# THANKS!

