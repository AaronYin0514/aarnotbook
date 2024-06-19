# 全局属性和其他

本章主要探讨 HTML5 中的 HTML 实体、以及 HTML 核心构成的元数据，最后了解一
下 HTML 中的全局属性。 

## 一.实体

HTML 实体就是将有特殊意义的字符通过实体代码显示出来。

| 显示结果 | 实体名称 | 实体编号 | 描述 |
| --- | --- | --- | --- |
| | `&nbsp;` | `&#160;` | 空格 |
| `<` | `&lt;` | `&#60;` | 小于号 |
| `>` | `&gt;` | `&#62;` | 大于号 |
| `&` | `&amp;` | `&#38;` | 和号 |
| `"` | `&quot;` | `&#34;` | 引号 |
| `'` | `&apos;` | `&#39;` | 撇号 |
| `¢` | `&cent;` | `&#162;` | 分 |
| `£` | `&pound;` | `&#163;` | 镑 |
| `¥` | `&yen;` | `&#165;` | 日圆 |
| `€` | `&euro;` | `&#8364;` | 欧元 |
| `§` | `&sect;` | `&#167;` | 小节 |
| `©` | `&copy;` | `&#169;` | 版权 |
| `®` | `&reg;` | `&#174;` | 注册商标 |
| `TM` | `&trade;` | `&#8482;` | 商标 |
| `×` | `&times;` | `&#215;` | 乘号 |
| `÷` | `&divide;` | `&#247;` | 除号 |

## 二.元数据

`<meta>`元素可以定义文档中的各种元数据，而且一个 HTML 页面可以包含多个`<meta>`元素。

### 1.指定名/值元数据对

```html
<meta name="author" content="bnbbs">
<meta name="description" content="这是一个 HTML5 页面">
<meta name="keywords" content="html5,css3,响应式">
<meta name="generator" content="sublime text 3">
```

| 元数据名称 | 说明 |
| --- | --- |
| author | 当前页面的作者 |
| description | 当前页面的描述 |
| keywords | 当前页面的关键字 |
| generator | 当前页面的编码工具 |

### 2.声明字符编码

```html
<meta charset="utf-8">
```

### 3.模拟 HTTP 标头字段

```html
<meta http-equiv="refresh" content="5;http://li.cc">
//另一种声明字符编码
<meta http-equiv="content-type" content="text/html charset=utf-8">
```

| 属性值 | 说明 |
| --- | --- |
| refresh | 重新载入当前页面，或指定一个 URL。单位秒。 |
| content-type | 另一种声明字符编码的方式 |

## 三.全局属性

在此之前，我们涉及到的元素都讲解了它的局部数据，当然也知道一些全局属性，比如 id。全局属性是所有元素共有的行为，HTML5 还提供了一些其他的全局属性。

### 1.id 属性

```html
<p id="abc">段落</p>
```

解释:id 属性给元素分配一个唯一标识符。这种标识符通常用来给 CSS 和 JavaScript 调用选择元素。一个页面只能出现一次同一个 id 名称。

### 2.class 属性

```html
<p class="abc">段落</p>
<p class="abc">段落</p>
<p class="abc">段落</p>
```

解释:class 属性用来给元素归类。通过是文档中某一个或多个元素同时设置 CSS 样式。
   
### 3.accesskey 属性

```html
<input type="text" name="user" accesskey="n">
```

解释:快捷键操作，windows 下 alt+指定键，前提是浏览器 alt 并不冲突。

### 4.contenteditable 属性

```html
<p contenteditable="true">我可以修改吗</p>
```

解释:让文本处于可编辑状态，设置 true 则可以编辑，false 则不可编辑。或者直接设置属性。

### 5.dir 属性

```html
<p dir="rtl">文字到右边了</p>
```

解释:让文本从左到右(ltr)，还是从右到左(rtl)。

### 6.hidden 属性

```html
<p hidden>文字到右边了</p> 解释:隐藏元素。
```

### 7.lang 属性

```html
<p lang="en">HTML5</p> 解释:可以局部设置语言。
```

### 8.title 属性

```html
<p title="HTML5 教程">HTML5</p> 解释:对元素的内容进行额外的提示。
```

### 9.tabindex 属性

```html
<input type="text" name="user" tabindex="2">
<input type="text" name="user" tabindex="1">
```

解释:表单中按下 tab 键，实现获取焦点的顺序。如果是-1，则不会被选中。

### 10.style 属性

```html
<p style="color:red;">CSS 样式</p>
``` 

解释:设置元素行内 CSS 样式。

