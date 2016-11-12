# CSS 编码技巧

## 尽量减少代码重复 (DRY)
## Minimize code duplication

- minimizing the amount of edits necessary to make a change.

关于按钮的例子:

```CSS
  /* 全部都是绝对值，产生了相互依赖 */
  font-size: 20px;
  line-height: 30px;
```

```CSS
  /* 某些值相互依赖的时候，代码上也要反应依赖关系 */
  font-size: 20px;
  line-height: 1.5;
```

```CSS
  /* 别用绝对值，使用百分比或 em  */
  font-size: 125%;
  line-height: 1.5;
```

```CSS
  /*
    所有长度相关都使用 em 或者 %
    所有效果都依赖字号进行缩放
    但是边框绝对值不变
  */
  padding: .3em .8em;
  border: 1px solid #4d8;
  border-radius: .2em;
  text-shadow: 0 -.05em .25em #335166;
  box-shadow: 0 .05em .25em gray;
  font-size: 125%;
  line-height: 1.5;
```

```CSS
  /*
    颜色：
    要根据按钮的亮面和暗面相对于主色调 #58a 变亮或变暗
  */
  background: #58a linear-gradient(#77a0bb, #58a);
  text-shadow: 0 -.05em .25em #335166;
  box-shadow: 0 .05em .25em gray;
```

```CSS
  /*
    把半透明的黑色或白色叠加在主色调，
    即可以产生主色调亮色和暗色变体
  */
  /* 推荐使用 HSLA 而不是 RGBA，字符长度更短 */
  background: #58a linear-gradient(hsla(0, 0%, 100%, .2),
                                    transparent);
  text-shadow: 0 -.05em .25em rgba(0, 0, 0, .5);
  box-shadow: 0 .05em .25em rgba(0, 0, 0, .5);
```

```CSS
  /* 覆盖 background-color */
  button.cancel {
    background-color: #c00;
  }
  button.ok {
    background-color: #6b0;
  }
```

- 代码量少和代码易维护不可兼得

- `currentColor`
From SVG, 并没有被绑定到一个固定值，而是被解析为 `color`

```CSS
  /* 使 <hr> 自动与文本颜色一致 */
  hr {
    height: .5em;
    background: currentColor;
  }
```
如果没有给边框指定颜色，它会自动从文本颜色那里获得颜色
currentColor 本身就是很多 CSS 颜色属性的初始值


- 继承 inherit
`inherit` 可以用在任何 CSS 属性中，且总是绑定到父元素的计算值
对于伪元素，则取生成该伪元素的宿主元素

```CSS
  /* 设置表单元素字体与页面其他部分相同 */
  input, select, button { font: inherit; }

  /* 设置超链接颜色与页面其他文本相同 */
  a { color: inherit; }
```

```CSS
  /* 设置伪元素背景、边框样式继承父元素 */
  .callout { position: relative; }
  .callout::before {
    content: "";
    position: absolute;
    top: -.4em; left: 1em;
    padding: .35em;
    background: inherit;
    border: inherit;
    border-right: 0;
    border-bottom: 0;
    transform: rotate(45deg);
  }
```

## 相信你的眼睛，而不是数字
- 字母形状在两端比较整齐，但顶部和底部则往往参差不齐。
- 需要减少顶部和底部内边距，不一定非得绝对居中


## 响应式
- 每个媒介查询都会增加成本
- 媒介查询不能以一种连续的方式来修复问题，它工作原理是基于某几个特定的阶梯（断点）
- 样式和设计最好是用弹性布局
- 使用百分比长度来取代固定长度
- `background-size: cover` ，在移动端需要慎用
- 为替换元素 img / object / video / iframe ... 设置一个 `max-width: 100%;`
- 使用 `max-width` ，可以自适应分辨率
- 行列式布局（grid），使用 flexbox 或者 `display: inline-block;` 加上常规的文本折行行为
- 多列文本时，指定 `column-width` 而不是 `column-count` ，使它在较小屏幕下自动显示单列布局


## 合理使用简写
- 这是一种防御式编码方式，确保声明不会被覆盖
- 「列表扩散」：如果只为某个属性提供一个值，它就会扩散并应用到列表中的每一项

```CSS
  background: url(1.png) top right,
              url(2.png) bottom right,
              url(3.png) bottom left,
  background-size: 2em 2em;
  background-repeat: no-repeat;
```

**Tips:** `background-size` `background-position` 同时存在时，最好使用 `/` 分隔声明，或者直接拆开。

