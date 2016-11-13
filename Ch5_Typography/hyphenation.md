# 字体排印 Typography

## 连字符断行 Hyphenation

文本折行的原理：
- Greedy Algorithm 贪婪算法
每次分析一行，吧尽可能多的单词（当连字符可用时则以音节为单位）填充进该行；
当遇到第一个装不下的单词或音节时，就移至下一行进行处理。

- Knuth-Plass Algorithm
会把整段文本纳入考虑范围，从而产生更美的效果，但计算性能更差。

大部分桌面文字处理程序采用 Knuth-Plass Algorithm。
出于浏览器性能考虑，采用的是贪婪算法。

`hyphens : none | manual | auto;`
default: manual 可以手动加入软连接字符 `&shy;`
浏览器会有限处理这个情况，然后再去计算**其他可以断词的地方**.
`hyphens: none;` 会禁用这种效果.

CSS 4 加入了更细粒度的
`hyphenate-limit-lines`
`hyphenate-limit-chars`
`hyphenate-limit-zone`
`hyphenate-limit-last`
`hyphenate-character`



