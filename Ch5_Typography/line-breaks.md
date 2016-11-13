## 插入换行 Inserting Line breaks

- 一般用于定义列表 `dt` `dd`
`<dt>` `<dd>` 均为块级元素

- 或许可以给每个 `<dd>` 后加入 `<br />` 手动换行，但是会污染结构层代码。


### Better Solution
1. Unicode 字符专门代表换行： `0x000A`
CSS 中可以写作 "\000A" 或者 "\A"，作为 `::after` 伪元素内容，并添加到每个 `<dd>` 尾部.

```CSS
dl:nth-of-type(2) dd::after {
  content: "\A";
}
```

这里其实不会有太大变化，因为这不过是在 HTML 中所有关闭标签`</dd>` 之前添加换行符.
而 HTML 中加入换行符会与相邻的其他空白符进行合并。
（当然这是件非常好的事情，可以自动 trim 源代码中的空白符和换行）
但如果希望保留的源码中的换行符及空白符，比如显示代码块，需要 `white-space: pre;`
`pre | pre-line | pre-wrap` all be fine, but `pre` has a better browsers support.
这里只对伪元素的换行符设置.

```CSS
  dl:nth-of-type(2) dd::after {
    content: "\A";
    white-space: pre;
  }

  dl:nth-of-type(2) dd,
  dl:nth-of-type(2) dt {
    display: inline;
  }
```

这里看起来已经挺好了，但是如果出现一些边界条件，
比如说给 `dl` 中的用户再添加一个邮箱，这个方法就不够健壮了。

因为这是在**每个`<dd>`后面** 都加入了一个换行符，每个值都分到了单独一行。
甚至不需要换行也是如此。


2. 所以转换下思路，不是把换行加在 `dd` 前面，而是 `dt` 后面
避免第一个 `<dt>` 也是生效，所以使用 `dd + dt`，在即使多个 `<dt>` 共用同一个值也可以正常工作。
还可以在多个 `<dd>` 时候告诉浏览器「只在后面还跟着一个 `<dd>` 的 `<dd>` 尾部插入逗号」,
但是这里就会出现之前的那个问题，
继续转换思路，换成「只在前面还跟着一个 `<dd>` 的 `<dd>` 头部插入逗号」
- **Note** 当结构代码在多个连续的 `<dd>` 之间包含了（未加注释的）空白符，那么逗号前面会有一个空格。
可以利用负外边距解决。空隙宽度可以按照不同字体和尺寸来调整，不一定刚好是`0.25em`

```CSS
dl:nth-of-type(3) dd,
dl:nth-of-type(3) dt {
  display: inline;
}
dl:nth-of-type(3) dd + dt::before {
  content: "\A";
  white-space: pre;
}
dl:nth-of-type(3) dd + dd::before {
  content: ", ";
  margin-left: -.25em;
  font-weight: normal;
}

```


