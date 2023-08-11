# CSS 选择器权重

声明：本文参考来源于[MDN CSS 优先级](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)（80％）和B站尚硅谷的[课程](https://m.bilibili.com/video/BV1p84y1P7Z5)（20％）

我想你一定是来进行速查的，所以我会尽量精简文章内容，保证不浪费时间，不说废话

文章包括新的`:is`和`:where（实验性）`的[说明](#速查：六点)

## 优先级：概念

优先级就是分配给 CSS 声明的一个权重，由 匹配的选择器中的 每一种选择器类型的 数值 决定。

多个 CSS 优先级相等的时候，最后的那个选择器将会被应用到元素上。

> 注意：[文档树中元素的接近度对优先级没有影响。](#无视：DOM树)

## 速查：六点

1. 格式：(1 | 0,a,b,c)
 - 1 | 0: 是否有!important
 - a: id的个数
 - b: .class :matix [key]的个数
 - c: el, ::el 的个数
2. 冲突比较：从左到右，后来居上
3. 简记：
 - !imp > 行内 > id > .class > el > 通配符 > 继承
4. 如果有!imp，那么即使是jsDOM也无法修改
5. 提示：VSCode能自动计算权重
6. 注意：
 - `:not`和`:is`本身对优先级没有影响，但是，在 :not和is内部声明的选择器会影响优先级
 - `:where()` 和其中的选择器的优先级是 0

## 比较：后来者居上，a>b>c

1. 从左到右：a->b->c，从左到右比较
2. 后来居上：前面写的被后来覆盖（前提：后者优先级>=前者）
3. 无影响者：通配选择符（`*`）关系选择符（`+`, `>`, `~`, `" "`, `||`）和 否定伪类`:not()`对优先级没有影响

>从左到右

```css
/* A选择器：(0,2,3) */
:is(h1) div:nth-child(2n):not(p:selected)

/* B选择器：(0,2,1) */
h1 div.headtext[needcolor]

/*
1. 0=0
2. 2=2
3. 3>1
4. A选择器胜出
*/
```

>后来居上

```html
<link href="foo.css">
<link href="bar.css">

<TodoList /> <!-- green -->
```

```css
/* foo.css */
TodoList {
  color: red;
}
/* bar.css */
TodoList {
  color: green;
}
```

## !important：核弹级声明

当在一个样式声明中使用它，此声明将覆盖**任何**其他声明

## 无视：DOM树

什么意思？直接上[代码](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)！

```css
body h1 {
  color: green;
}

/* 后来居上 */
html h1 {
  color: purple;
}

```

```html
<html>
  <body>
    <h1>Here is a title!</h1>
  </body>
</html>
```
h1将被渲染成紫色。

## 其他：关于where选择器

>实验性: 这是一项实验性技术，在将其用于生产之前，请仔细检查[MDN浏览器兼容性表格](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:where#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9%E6%80%A7)。
>给你康好了QAQ：
>- Chrome 88+
>- Firefox 78+
>- Safari 14+
>- IE ？？？ 早没有了


## 工具：计算优先级

如果你的编辑器没有查看优先级的功能

那么这是一个网站，供你计算优先级：[Specificity Calculator](https://specificity.keegan.st/)