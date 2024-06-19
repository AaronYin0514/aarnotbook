---
tags: [javascript]
---

# Map

## 定义

```javascript
const m = new Map();

const m = new Map([['name', '张三'], ['age', 10]])
console.log(m) // Map(2) { 'name' => '张三', 'age' => 10 }
```

## 添加元素

### `set`

```javascript
//声明 Map
const m = new Map();

// 添加元素
m.set('name','JS');
m.set('change', function(){
	console.log("我们可以改变你!!");
});
let key = {
	school : 'ATGUIGU'
};
m.set(key, ['北京','上海','深圳']);

console.log(m);
```

## 获取元素

### `get`

```javascript
const m = new Map();
m.set('name','JS');
m.set('change', function(){
	console.log("我们可以改变你!!");
});
let key = {
	school : 'ATGUIGU'
};
m.set(key, ['北京','上海','深圳']);

console.log(m.get('change'));
console.log(m.get(key));
```

## 删除元素`delete`

```javascript
const m = new Map();
m.set('name','JS');

m.delete('name');
console.log(m);
```

## 清空`clear`

```javascript
const m = new Map();
m.set('name','JS');

m.clear();
console.log(m);
```

## 获取元素数量`size`

```javascript
const m = new Map();
m.set('name','JS');

console.log(m.size); // 1
```

## 遍历Map

### `for of`

```javascript
const m = new Map();
m.set('name','JS');

for (const iterator of m) {
	console.log(typeof iterator)
	console.log(iterator[0]);
	console.log(iterator[1]);
}
```

### `forEach`

```js
const m = new Map([['name', '张三'], ['age', 10]])
m.forEach((value, key, map) => {
	console.log(`m[${key}] = ${value}`)
})
```

结果：

```
m[name] = 张三
m[age] = 10
```

## `has`判断是否包含key 

```js
const m = new Map([['name', '张三'], ['age', 10]])
console.log(m.has('name')) // true
```

## `keys`获取所有的key

```js
const m = new Map([['name', '张三'], ['age', 10]])
console.log(m.keys()) // [Map Iterator] { 'name', 'age' }
```

## `values`获取所有value

```js
const m = new Map([['name', '张三'], ['age', 10]])
console.log(m.values()) // [Map Iterator] { '张三', 10 }
```

