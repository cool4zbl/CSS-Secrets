## CSS 编码技巧

### 尽量减少代码重复 (DRY)

- minimizing the amount of edits necessary to make a change.

关于按钮的例子:

```css
  /* 全部都是绝对值，产生了相互依赖 */
  font-size: 20px;
  line-height: 30px;
```

```css
  /* 某些值相互依赖的时候，代码上也要反应依赖关系 */
  font-size: 20px;
  line-height: 1.5;
```

```css
  /* 别用绝对值，使用百分比或 em  */
  font-size: 125%;
  line-height: 1.5;
```

```css
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

```css
  /*
    颜色：
    要根据按钮的亮面和暗面相对于主色调 #58a 变亮或变暗
  */
  text-shadow: 0 -.05em .25em #335166;
  box-shadow: 0 .05em .25em gray;

```



