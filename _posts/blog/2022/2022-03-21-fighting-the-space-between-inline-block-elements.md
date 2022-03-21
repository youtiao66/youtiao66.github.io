---
title: "【译】干掉行内块级元素之间的空隙"
categories:
  - Blog
tags:
  - CSS
---

经过再寻常不过的方式格式化的多个行内块级元素之间会出现空隙

<!--more-->

换句话说：

```html
<nav>
  <a href="#">One</a>
  <a href="#">Two</a>
  <a href="#">Three</a>
</nav>
```

```CSS
nav a {
  display: inline-block;
  padding: 5px;
  background: red;
}
```

将得到以下结果：

![space-between-inline-block-elements][space-between-inline-block-elements]

我们总是希望多个元素紧挨着彼此。对于导航来说，这就意味着难以处理的一丁点不可以点击的空隙。

这不是 *BUG* 哟（我认为不是）。这就是把行内块级元素放到一行将会自然发生的事情。你希望有意输入的单词之间的空格才产生空隙对吗？这些块级元素之间的空隙就像单词之间的空格一样。并不是说浏览器规范不能更新为行内块级元素之间的空格应该什么都有没有，我敢肯定这就像太阳打西边出来一样不可能会发生的。

这里有一些方式可以干掉这些空隙然后让行内块级元素紧挨着彼此。



## 移除空隙
产生空隙的原因恰恰就是因为行内块级元素之间有空白符（一个换行符和几个以空格代表的制表符，我想说清楚）。压缩 HTML 可能可以解决这些问题，或者采用以下技巧：

```html
<ul>
  <li>
   one</li><li>
   two</li><li>
   three</li>
</ul>
```

或者

```html
<ul>
  <li>one</li
  ><li>two</li
  ><li>three</li>
</ul>
```

或者加上注释

```html
<ul>
  <li>one</li><!--
  --><li>two</li><!--
  --><li>three</li>
</ul>
```

以上方法都有些独特，但它确实起作用。



## 负的外边距
可以使用负的 4px 外边距方式偷偷地干掉元素之间的空隙（具体大小可能需要根据父元素的字体大小做适当调整）。显然在老版本 IE(6 & 7) 中这仍然是有问题的，但如果你根本不关心这些浏览器的话你就可以保持 HTML 代码格式化的整洁。

```CSS
nav a {
  display: inline-block;
  margin-right: -4px;
}
```


## 跳过关闭标签
HTML5 不在意关闭标签。尽管你不得不承认，这感觉很奇怪。

```html
<ul>
  <li>one
  <li>two
  <li>three
</ul>
```

## 字体大小设置为 0
空隙的字体大小为 0 的话，宽度也为 0

```css
nav {
  font-size: 0;
}
nav a {
  font-size: 16px;
}
```



## 浮动
可能你根本不需要行内块级元素，你可以使用浮动的方式解决这个问题。这样也允许你设置他们的宽度、高度、内边框和其他块级元素属性。但同时你也不能通过给行内块级元素的父级设置 `text-align: center;` 的方式让它们居中。



## 弹性盒子
如果浏览器支持对你来说是可接受的，并且你需要的是行内块级元素居中，你可以使用弹性盒子(flexbox). 它们不是完全可以互换的布局模型或其他任何东西，但你可能会从中得到你需要的东西



## 参考
> 示例 [on CodePen][https://codepen.io/chriscoyier/pen/kaEvMk]

> 本文译自 [Fighting the Space Between Inline Block Elements][https://css-tricks.com/fighting-the-space-between-inline-block-elements/]



[space-between-inline-block-elements]: data:image/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAKoAAABDCAYAAAAMPZ7IAAAAAXNSR0IArs4c6QAAB/dJREFUeF7tm29o1GUcwD97s2l4EewiZhC7QS5h80WzZonhDXHDYsuUBuGE4kZUTghTaIZDYZNUeqES1S4IJ6EriQ0096KdFOsPbQQ7oRRye9MZeINoC9u9WTz3+/3cdd5f73nkHvjeK3d3v+/v+3y+n9/zfJ9nrmJpaWkJeQmBMidQIaKWeYUkvSQBEVVEsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJEVUcsIKAiGpFmSRJfaJWVJQfzfQ/XijHHFOp2ZZvropr/sMREbWcHi8RNWs1RFQR1QwBmVGL4GrbDGVbvrL0FyFjMbCkR9UEtoAwMqMWAMn7im0zlG35FjNJFFG2TF+VHrVEgFovF1HLZDPVEITHVsLtOYj8pLXGGYPZVnjd+QaaofYfiFw1zzr9DlYu/Tv64eNeqAYSCaisdIY13AedR8xBzFv4BohFoSZPCmMHoO24uTwLblUKzHd8CNZuh5pV8H0fbDTIOBsV60Ttj0DvZliYgi1t8FMcCMDQCOxqhJujsLrDjASFiDofhfEBGAhDdQgu9kLie3iiA9a+COFBuHYAgmUiaqH5Hn8ELu6DK/cpd6tn1IZDED0M3IS61TCTNppIHDZXw+he6DilX9a8ojbDzAcQ2OjcO7AfbhyDuTHwtznvdZ2H1ybLRNQi8n0NZyxje6HNANt81bJqRh2JQXsNTA3A+oN3Dy14Asb3AVGoaIH+o7BzO1TPwkQltDcCC3BgCxx3e9pt/XC+F1apNuImnOiBgxcyY8sratplmUR1DIbpaahX/56Hd47CuyfA57YxZ16BbiD2mdNG7F4HQzPQ3APnBqBWtTqVEB2FVzogW8uoLd+Uh25qGBY3wbMqsQXofgbCf0LP29D9KsQugG+H8/npnfD1k9n5lsI+n9h5Pje7678cg9YayNbjBXrgxklgDhr98HoE9mx2Up4dBeXfvnZYGANfGzT3w4+90NcCR1ZC/KLT9+6uc8TIt/zkO0fNKirQfAh+PAw3h2F1JzTsh+gxWLgCvqBz51PX4IVvIdAN3thmz0KgC0JDMLgLuA519XevLup6E6KquFNnYexB6PVYvg+TX0CTgqcWvFmoqYXvLsGmbZn5Xg+Vxt4KUbMt7Z54nqhXG2AxCglXTPwQuwU+9+eJeXgWCJ9zhv18yJnBsm0YdBZe3W96ERoTsMEHaoKfnIcmoMUHETf3t+ogPAPnY/ByNbRUQcSt0tA12LUGRruhI2z2wfIeujurmR9mbkFtFB5eB/EGUP0u7oMWCMDZ6ex82V8a+/IWNQ6t1YXPqErUeBSqPFGBy3EITkLVO85nTMFXv8CKFfDvX8AKmP4cTnk2pBDRLWpoBAbbYXgndD4AS2ecmw3vho+2wvhaqFgP6gGb/gMa552VwlvqQ+dh8OXsGxyd+Xqipq5myVbMB40+uIu1yz4j3x+g79PS2Je1qIPTEGqE62GoV01c2qtrCM7sSlk+M4k6D8GJZVGrRsGXfkrgB9RpQtpLZ+GToYOwOA7zV+Cb1fB0DKo2g28KZurh1zehc8jpaa/9BmtSZl91udcOZNuJ68zXEzX1Xuqhb8V9eNJZez9n4rsJ4t9CKezLWlR/F9xSs86CuzymZZtcSishvBO6VUOaS9Q3IHbDWer3NsIpd5pSG7LLTVDl9omptyi68G7PnLrrTwfsLd/q/ZYKeGka9ribvuRM5V7gbST7NsARdyO4YxC+DGXfievM986MmrLrT+ZUmSaq12MHcvDdCHMbSmNf1qKq5PaPwLF2ZxPR0gYRtekJwOVxaK2F2WEIdLrD8Pomb+n34F2Hino4FIHDarOVgOFP4O/HIdQKAy1wUMPS74mUukFKB+wduXmbJE+I6GlY17P8be9EIzEFj653Jvxk3+otvRkqV6youfL198Ctk+DlqdqRZI86B3Xq30GYH4dVU267Qm6+i4dKY1/2oqoEU38ztbAAq5JnS3D2PejyDtIDMPnz8k70ShgWn4PWNc4Qo2FY9y6MTEC7+556P9wN3Rk2JuqzYgo/OAkhtTNyX4k5CD2V4TRBbfD+gKNNy7P6zCJ82ATH086deobgpNrpz8GsD2oT7vFQlvMpXfn6dzh9e437G8Cx08DWZZa/T8BDG50TE/WaHYOn2iDuz8E312cFPHRWiOol2RyE6pXAbbiUYQYsdDANzfDwAxCNZGxN74QppvCF3lt9zx+AeMpxWPrP/4sVgG1PwsrbcOFS7ruYyreYsanv5uJ7r+yLzSHt+2bPUTX/diLnWDOdkRZS+PuZY+oAbMs3F/xCxlLWopaYXMmXFyJqyTfRGMC2fHMNXfMEYHZG1VjDewplW+Fty1dEvSct777ItsLblq+IKqImCeT7vwmaMBkJI0t/EVhtm6Fsy1dm1CJkLAZWuc9QImrWaspmStMzoSWMiHofRNVSKQkiBDIT0DejCmEhYJCAiGoQroTWR0BE1cdSIhkkIKIahCuh9REQUfWxlEgGCYioBuFKaH0ERFR9LCWSQQIiqkG4ElofARFVH0uJZJCAiGoQroTWR0BE1cdSIhkkIKIahCuh9REQUfWxlEgGCYioBuFKaH0ERFR9LCWSQQIiqkG4ElofARFVH0uJZJCAiGoQroTWR0BE1cdSIhkkIKIahCuh9RH4DzrgQnQyBAG/AAAAAElFTkSuQmCC
