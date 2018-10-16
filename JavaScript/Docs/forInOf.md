# for...in 和 for...of 区别

## for...in

for...in 语句以任意顺序遍历一个对象的可枚举属性。  
for...in 遍历对象本身的所有可枚举属性，以及对象从其构造函数原型中继承的属性。

```js
for (variable in object) {...}
```
- `variable` 在每次迭代时，将不同的属性名分配给变量。
- `object` 被迭代枚举其属性的对象。

只遍历自身的可枚举属性，可以使用 hasOwnProperty
```js
for (var prop in obj) {
  if (obj.hasOwnProperty(prop)) {
    console.log(`obj.${prop} = ${obj[prop]}`);
  } 
}
```

## for...of

for...of 语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。  
对于for...of的循环，可以由break, throw 或return终止(使用return报错？)。  

```js
for (variable of iterable) {
    //statements
}
```

- `variable` 在每次迭代中，将不同属性的值分配给变量。
- `iterable` 被迭代枚举其属性的对象。

```js
let iterable = [10, 20, 30];

for (const value of iterable) {
  console.log(value);
}
// 10
// 20
// 30
```

## for...of 与 for...in 区别

- for...in 语句以原始插入顺序迭代对象的可枚举属性。
- for...of 语句遍历可迭代对象定义要迭代的数据。

区别示例：
```js
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';

for (let i in iterable) {
  console.log(i); // 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // 0, 1, 2, "foo"
  }
}

for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```
