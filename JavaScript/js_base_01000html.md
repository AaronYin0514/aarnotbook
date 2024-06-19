---
tags: [javascript]
---

# JavaScript嵌入HTML

## 引入JS代码

在`<header>`中添加`<script type="text/javascript">js代码...</script>`标签*text/javascript*是**type**属性的默认值，可省略。

```javascript
<html>
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
<body>
  ...
</body>
</html>
```

## 引入JS文件

给`<script>`标签添加**src**属性，值是JS文件的路径。

```javascript
<html>
<head>
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
```



