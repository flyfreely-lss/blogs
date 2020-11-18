# JavaScript 数组遍历及相关方法对比

## 前言
JavaScript提供了多种循环遍历的方法来方便用户处理数据，在产品开发中涉及到复杂数据处理的时候经常使用，有些使用细节经常忘记，在此做一个统一的整理记录，方便日后查询。本文主要从如何使用、常用用途和执行效率三个角度进行记录。

## 常用遍历方法
### `for` 循环
这是传统的遍历方法，没有什么可说的，代码如下：
```
const array = [1, 2, 3, 5, 6]
for (let i = 0, len = array.length; i < len; i++) {
  console.log(array[i]);
}
```

### `forEach`
`forEach` 方法按升序为数组中含有效值的每一项执行一次 callback 函数，那些已删除或者未初始化的项将被跳过（例如在稀疏数组上）。与 map() 或者 reduce() 不同的是，它总是返回 undefined 值，并且不可链式调用。其典型用例是在一个调用链的最后执行副作用（side effects，函数式编程上，指函数进行 返回结果值 以外的操作）。

除了抛出异常以外，没有办法中止或跳出 forEach() 循环。如果你需要中止或跳出循环，forEach() 方法不是应当使用的工具。

代码示例：
```
const arraySparse = [1,3,,7]; //稀疏数组
arraySparse.forEach(function(element){
  console.log(element);
});
// 1
// 3
// 7
```

### `for...of`（ES6）
`for...of` 语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。

### `for-in`

## 相关方法
### `map`（不改变原始数组）

### `reduce`

### `filter`（不改变原始数组）

### `every`（不改变原始数组）

用途：测试一个数组内的所有元素是否都能通过某个指定函数的测试。

`every`方法为数组中的每个元素执行一次 callback 函数，直到它找到一个会使 callback 返回 falsy 的元素。如果发现了一个这样的元素，every 方法将会立即返回 false。否则，callback 为每一个元素返回 true，every 就会返回 true。callback 函数只会为那些已经被赋值的索引调用，不会为那些被删除或从未被赋值的索引调用。
若为空数组，则返回 true。

代码如下：
```
function isBigEnough(element, index, array) {
  return element >= 10;
}
[12, 5, 8, 130, 44].every(isBigEnough);   // false
```

### `some`（不改变原始数组）

用途：测试数组中是不是至少有1个元素通过了被提供的函数测试。

`some` 为数组中的每一个元素执行一次 callback 函数，直到找到一个使得 callback 返回一个“真值”（即可转换为布尔值 true 的值）。如果找到了这样一个值，some() 将会立即返回 true。否则，some() 返回 false。callback 只会在那些”有值“的索引上被调用，不会在那些被删除或从来未被赋值的索引上调用。
若为空数组，任何情况下都返回 false。

代码如下：
```
function isBiggerThan10(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
```

### `find`

### `findIndex`

## 执行效率

## 相关阅读