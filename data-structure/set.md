# 集合
```
几乎每种编程语言中, 都有集合结构.

集合比较常见的实现方式是哈希表(后续会学习), 我们这里来实现一个封装的集合类.
```

## 目录
### 一.集合介绍
- [集合的特点](#集合的特点)
### 二.封装集合
- [创建集合类](#创建集合类)
- [操作的方法](#操作的方法)
- [集合的使用](#集合的使用)
### 三.集合间的操作
- [集合间操作](#集合间操作)
- [集合间操作实现](集合间操作实现)
### 四.完整代码
- [集合的完整代码](#集合的完整代码)

## 一.集合介绍
### 集合的特点
- 集合通常是由一组**无序的**, **不能重复**的元素构成.
  - 和数学中的集合名词比较相似, 但是数学中的集合范围更大一些, 也允许集合中的元素重复.
  - 在计算机中, 集合通常表示的结构中元素是不允许重复的.
- 看成一种**特殊的数组**.
  - 特殊之处在于里面的元素没有顺序, 也不能重复.
  - 没有顺序意味着不能通过下标值进行访问, 不能重复意味着相同的对象在集合中只会存在一份.
## 二.封装集合
### 创建集合类
- 创建一个Set类
  ```
  // 封装集合的构造函数
  function Set() {
      // 使用一个对象来保存集合的元素
      this.items = {} 
      
      // 集合的操作方法
  }
  ```
### 操作的方法
- 集合常见的操作方法
  - `add(value)`：向集合添加一个新的项。
  - `remove(value)`：从集合移除一个值。
  - `has(value)`：如果值在集合中，返回true，否则返回false。
  - `clear()`：移除集合中的所有项。
  - `size()`：返回集合所包含元素的数量。与数组的length属性类似。
  - `values()`：返回一个包含集合中所有值的数组。
  - 还有一些集合其他相关的操作, 暂时用不太多, 这里暂不封装.
- has(value)方法
  ```
  // 判断集合中是否有某个元素
  Set.prototype.has = function (value) {
      // hasOwnProperty用于检查对象是否具有某个特定的自有属性（即直接定义在对象上的属性，而不是从原型链继承的属性）。
      return this.items.hasOwnProperty(value)
  }
  ```
- add方法
  ```
  // 向集合中添加元素
  Set.prototype.add = function (value) {
      // 1.判断集合中是否已经包含了该元素
      if (this.has(value)) return false
  
      // 2.将元素添加到集合中
      this.items[value] = value
      return true
  }
  ```
- remove方法
  ```
  // 从集合中删除某个元素
  Set.prototype.remove = function (value) {
      // 1.判断集合中是否包含该元素
      if (!this.has(value)) return false
  
      // 2.包含该元素, 那么将元素删除
      delete this.items[value]
      return true
  }
  ```
- clear方法
  ```
  // 清空集合中所有的元素
  Set.prototype.clear = function () {
      this.items = {}
  }
  ```
- size方法
  ```
  // 获取集合的大小
  Set.prototype.size = function () {
      // Object.keys用于获取一个对象的所有自有属性的键名，并将这些键名返回为一个数组。
      return Object.keys(this.items).length
      
      /*
      考虑兼容性问题, 使用下面的代码
      var count = 0
      for (var value in this.items) {
          if (this.items.hasOwnProperty(value)) {
              count++
          }
      }
      return count
      */
  }
  ```
- values方法
  ```
  // 获取集合中所有的值
  Set.prototype.values = function () {
      return Object.values(this.items)
  
      /*
      考虑兼容性问题, 使用下面的代码
      var keys = []
      for (var value in this.items) {
          keys.push(this.items[value])
      }
      return keys
      */
  }
  ```
### 集合的使用
- 简单使用和测试，封装的Set类
  ```
  // 测试和使用集合类
  var set = new Set()
  
  // 添加元素
  set.add(1)
  alert(set.values()) // 1
  set.add(1)
  alert(set.values()) // 1
  
  set.add(100)
  set.add(200)
  alert(set.values()) // 1,100,200
  
  // 判断是否包含元素
  alert(set.has(100)) // true
  
  // 删除元素
  set.remove(100)
  alert(set.values()) // 1, 200
  
  // 获取集合的大小
  alert(set.size()) // 2
  set.clear()
  alert(set.size()) // 0
  ```
  
## 三.集合间操作
### 集合间操作
- 集合间通常有如下操作:
  - **并集**：对于给定的两个集合, 返回一个包含两个集合中所有元素的新集合.  
    ![image](https://github.com/user-attachments/assets/295b66f3-7c4e-4de8-9576-b837fd5ea28e)
  - **交集**：对于给定的两个集合, 返回一个包含两个集合中共有元素的新集合.  
    ![image](https://github.com/user-attachments/assets/b450975e-c8f3-408b-a58f-af04d32f937f)
  - **差集**：对于给定的两个集合, 返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合.  
    ![image](https://github.com/user-attachments/assets/90dac8ee-72e1-4ef2-a14c-57df4ff5246c)
  - **子集**: 验证一个给定集合是否是另一集合的子集.(A是B的子集)  
    ![image](https://github.com/user-attachments/assets/66dccb59-e86a-4fcb-b189-ee1d99d8f915)
### 集合间操作实现
- 并集实现
  ```
  // 并集操作
  Set.prototype.union = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var unionSet = new Set()

    // 2.将A集合中的所有元素添加到新集合中
    var values = this.values()
    values.forEach(value => {
      unionSet.add(value)
    });

    // 3.将B集合中的所有元素添加到新集合中
    values = otherSet.values()
    values.forEach(value => {
      unionSet.add(value)
    });

    return unionSet
  }
  ```
- 交集实现
  ```
  // 交集操作
  Set.prototype.intersection = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var intersectionSet = new Set()

    // 2.判断A集合中的元素是否存在于B集合中
    var values = this.values()
    values.forEach(value => {
      if (otherSet.has(value)) {
        intersectionSet.add(value)
      }
    });

    return intersectionSet
  }
  ```
- 差集实现
  ```
  // 差集操作
  Set.prototype.difference = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var differenceSet = new Set()

    // 2.判断A集合中的元素是否存在于B集合中, 不存在添加到新集合中
    var values = this.values()
    values.forEach(value => {
      if (!otherSet.has(value)) {
        differenceSet.add(value)
      }
    });

    return differenceSet
  }
  ```
- 子集实现
  ```
  // 验证子集操作
  Set.prototype.subset = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B

    // 1.判断A集合中的所有元素是否存在于B集合中, 不存在则不是B的子集
    var values = this.values()
    return values.every(value => {
      return otherSet.has(value)
    });
  }
  ```
- 测试代码
  ```
  var setA = new Set()
  setA.add('aaa')
  setA.add('bbb')
  setA.add('abc')
  console.log('setA: ', setA.values())

  var setB = new Set()
  setB.add('abc')
  setB.add('cba')
  setB.add('mba')
  console.log('setB: ', setB.values())

  var newSet = setA.union(setB)
  console.log('union: ', newSet.values())
  console.log('intersection: ', setA.intersection(setB).values())
  console.log('difference: ', setA.difference(setB).values())

  var setC = new Set()
  setC.add('mba')
  console.log('setA & setB: ', setA.subset(setB))
  console.log('setC & setB: ', setC.subset(setB))
  ```

## 四.完整代码
### 集合的完整代码
```
// 封装集合的构造函数
function Set() {
  // 使用一个对象来保存集合的元素
  this.items = {}

  // 集合的操作方法
  // 判断集合中是否有某个元素
  Set.prototype.has = function (value) {
    return this.items.hasOwnProperty(value)
  }

  // 向集合中添加元素
  Set.prototype.add = function (value) {
    // 1.判断集合中是否已经包含了该元素
    if (this.has(value)) return false

    // 2.将元素添加到集合中
    this.items[value] = value
    return true
  }

  // 从集合中删除某个元素
  Set.prototype.remove = function (value) {
    // 1.判断集合中是否包含该元素
    if (!this.has(value)) return false

    // 2.包含该元素, 那么将元素删除
    delete this.items[value]
    return true
  }

  // 清空集合中所有的元素
  Set.prototype.clear = function () {
    this.items = {}
  }

  // 获取集合的大小
  Set.prototype.size = function () {
    return Object.values(this.items).length
  }

  // 获取集合中所有的值
  Set.prototype.values = function () {
    return Object.values(this.items)
  }

  // 并集操作
  Set.prototype.union = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var unionSet = new Set()

    // 2.将A集合中的所有元素添加到新集合中
    var values = this.values()
    values.forEach(value => {
      unionSet.add(value)
    });

    // 3.将B集合中的所有元素添加到新集合中
    values = otherSet.values()
    values.forEach(value => {
      unionSet.add(value)
    });

    return unionSet
  }

  // 交集操作
  Set.prototype.intersection = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var intersectionSet = new Set()

    // 2.判断A集合中的元素是否存在于B集合中, 存在添加到新集合中
    var values = this.values()
    values.forEach(value => {
      if (otherSet.has(value)) {
        intersectionSet.add(value)
      }
    });

    return intersectionSet
  }

  // 差集操作
  Set.prototype.difference = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B
    // 1.创建新的集合
    var differenceSet = new Set()

    // 2.判断A集合中的元素是否存在于B集合中, 不存在添加到新集合中
    var values = this.values()
    values.forEach(value => {
      if (!otherSet.has(value)) {
        differenceSet.add(value)
      }
    });

    return differenceSet
  }

  // 验证子集操作
  Set.prototype.subset = function (otherSet) {
    // this: 集合对象A
    // otherSet: 集合对象B

    // 1.判断A集合中的所有元素是否存在于B集合中, 不存在则不是B的子集
    var values = this.values()
    return values.every(value => {
      return otherSet.has(value)
    });
  }
}
```
