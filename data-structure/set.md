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
### 三.完整代码
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
## 三.完整代码
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

    // 获取集合中所有的值
    Set.prototype.values = function () {
        return Object.keys(this.items)

        /*
        考虑兼容性问题, 使用下面的代码
        var keys = []
        for (var value in this.items) {
            keys.push(value)
        }
        return keys
        */
    }
}
```
