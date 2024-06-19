# 表格元素

本章主要探讨 HTML5 中表格元素的用法。表格的主要用途是以网格的形式显示二维数据。

## 一.表格元素总汇

表格的基本构成最少需要三个元素:`<table>`、`<tr>`、`<td>`，其他的一些作为可选辅 助存在。

| 元素名称 | 说明 |
| ---- | ---- |
| table | 表示表格 |
| thead | 表示标题行 |
| tbody | 表示表格主体 |
| tfoot | 表示表脚 |
| tr | 表示一行单元格 |
| th | 表示标题行单元格 |
| td | 表示单元格 |
| col | 表示一列 |
| colgroup | 表示一组列 |
| caption | 表示表格标题 |

## 二.构建表格解析 

### 1.`<table><tr><td>`构建基础表格 

```html
<table border="1">
  <tr> 
    <td>张三</td>
    <td>男</td>
    <td>未婚</td> 
  </tr>
  <tr> 
    <td>李四</td>
    <td>女</td>
    <td>已婚</td> 
  </tr>
</table>
```

解释:`<table>`元素表示一个表格的声明，`<tr>`元素表示表格的一行，`<td>`元素表示 一个单元格。默认情况下表格是没有边框的，所以，在`<table>`元素增加一个border属性， 设置为 1 即可显示边框。

### 2.`<th>`为表格添加标题单元格

```html
<table border="1" style="width:300px;">
  <tr> 
    <th>姓名</th>
    <th>性别</th>
    <th>婚姻</th> 
  </tr>
  <tr> 
    <td>张三</td>
    <td>男</td>
    <td>未婚</td> 
  </tr>
  <tr> 
    <td>李四</td>
    <td>女</td>
    <td>已婚</td> 
  </tr>
</table>
```

解释:`<th>`元素主要是添加标题行的单元格，实际作用就是将内部文字居中且加粗。 这里使用了一个通用属性 style，主要用于 CSS 样式设置，以后会涉及到。`<th><td>`均属 于单元格，包含两个合并属性:colspan、rowspan 等。

### 3.`<thead>`添加表头 

```html
<thead>
  <tr> 
    <th>姓名</th>
    <th>性别</th>
    <th>婚姻</th> 
  </tr>
</thead>
```

解释:`<thead>`元素就是限制和规范了表格的表头部分。尤其是以后动态生成表头，它 的位置是不固定的，使用此元素可以限定在开头位置。

4.`<tfoot>`添加表脚 

```html
<tfoot>
  <tr>
    <td colspan="3">统计:共两名</td>
  </tr>
</tfoot>
```

解释:`<tfoot>`元素为表格生成表脚，限制在表格的底部。

### 5.`<tbody>`添加表主体 

```html
<tbody>
  <tr> 
    <td>张三</td>
    <td>男</td>
    <td>未婚</td> 
  </tr>
  <tr> 
    <td>李四</td>
    <td>女</td>
    <td>已婚</td> 
  </tr>
</tbody>
```

解释:`<tbody>`元素主要是包含住非表头表脚的主体部分，有助于表格格式的清晰，更 加有助于后续 CSS 和 JavaScript 的控制。

### 6.`<caption>`添加表格标题 

```html
<caption>这是一个人物表</caption> 
```

解释:`<caption>`元素给表格添加一个标题。

### 7.`<colgroup>`设置列

```html
<colgroup span="2" style="background:red;">
```

解释:`<colgroup>`元素是为了处理某个列，span 属性定义处理哪些列。1 表示第一列，
2 表示前两列。如果要单独设置第二列，那么需要声明两个，先处理第一个，将列点移入第 二位，再处理第二个即可。

### 8.`<col>`更灵活的设置列 

```html
<colgroup>
<col>
<col style="background:red;" span="1"> </colgroup>
```

解释:`<col>`元素表示单独一列，一个表示一列，控制更加灵活。如果设置了 span 则， 控制多列。

