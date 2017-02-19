---
layout: default
---

# 行间距，对齐以及线框（Line box）的结构

```
<p>The <strong>Moon</strong>…[etc]</p> 
```

![]({{site.baseurl}}/images/line-box.png)

Finally, the line height defines the total height of the line box. This is more commonly known as 
line spacing , or in typographic terms, leading (pronounced as “ledding”) due to the blocks of 
lead typesetters used to separate lines of characters on a printing press. Unlike in mechanical 
type, the leading in CSS is always applied to both the top and bottom of line boxes. 

最后，行高定义了线框（line box)的高度。这就是众所周知的行间距（line spacing）。在印刷术语里叫做行距，因为它是
用于分隔印刷排版中行与行之间的字符。在CSS中，行距为线框顶部和底部之间的距离。

The font-size is subtracted from the total line height, and the resulting measurement is divided in two 
equal parts (known as half-leading ). If the font-size is 21px and the line-heigh t is 30px , each strip of halfleading 
will be 4.5px tall. 

 `Note` If a line box contains inline boxes of varying line height, the line height for the line box as a whole
will be at least as tall as the tallest inline box. 

`备注`：如果一个线框中包含多个不同行高的行内框，则这个线框的行高至少会和最高那个行内框的高度相同。