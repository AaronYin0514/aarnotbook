## require.main === module

今天要学习的是 **Node.js** 的 tips，是关于 `require.main === module` 这个条件判断语句的用处。

先了解两个前提知识：

1.  当 [Node](https://so.csdn.net/so/search?q=Node&spm=1001.2101.3001.7020).js 直接运行一个文件时，`require.main` 会被设为该文件模块的 `module`变量。
    
2.  在每个模块里面， `module` 表示指向当前模块的变量对象（可以理解成某种意义上的 `this` 变量）；注意 `module` 并不是全局对象，是局部变量。
    

这意味着可以通过 `require.main === module` 来判断一个文件是否被直接运行。

Node.js 官网 “Accessing the main [module](https://so.csdn.net/so/search?q=module&spm=1001.2101.3001.7020)” 中有言：

![Node.js 官网对 require.main 的解释](rquire_main.png)

简单翻译一下就是： 

**可以通过 `require.main === module` 来判断当前文件是否直接被 node.js 执行，比如对 `foo.js` 文件，如果你执行了 `node foo.js`，那么这个条件语句结果是 `true`，如果是被其他文件以 `require('./foo')` 引用则为 `false`**

