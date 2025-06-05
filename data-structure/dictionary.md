# 字典
`数组-集合-字典是几乎编程语言都会默认提供的数据类型.`

## 目录
### 一.认识字典
- [字典的介绍](#字典的介绍)
- [创建字典类](#创建字典类)
### 二.操作字典
- [常见的操作](#常见的操作)
- [操作的实现](#操作的实现)
- [字典的使用](#字典的使用)

## 一.认识字典
### 字典的介绍
- 字典有什么特点呢?
  - 字典的主要特点是**一一对应**的关系.
  - 使用数组的方式: [18, "Coderwhy", 1.88]. 可以通过下标值取出信息.
  - 使用字典的方式: {"age" : 18, "name" : "Coderwhy", "height": 1.88}. 可以通过key取出value
- 字典的映射关系:
  - 有些编程语言中称这种映射关系为**字典**, 因为它确实和生活中的字典比较相似. (比如Swift中Dictionary, Python中的dict)
  - 有些编程语言中称这种映射关系为**Map**, 注意Map在这里不要翻译成地图, 而是翻译成**映射**. (比如Java中就有HashMap&TreeMap等)
- 字典和数组:
  - 字典和数组对比的话, 字典可以非常方便的通过**key**来搜索对应的**value**, key可以包含特殊含义, 也更容易被人们记住.
- 字典和对象:
  - 很多编程语言(比如Java)中对字典和对象区分比较明显, 对象通常是一种在编译期就确定下来的结构, **不可以动态的添加或者删除属性**. 而字典通常会使用类似于哈希表的数据结构去实现一种可以**动态的添加数据**的结构.
  - 但是在JavaScript中, 似乎对象本身就是一种字典. 所有在早期的JavaScript中, 没有字典这种数据类型, 因为你完全可以使用对象去代替.
  - 但是这里我们还是按照其他语言经常使用字典的方式去封装一个字典类型, 方便我们按照其他语言的方式去使用字典. (虽然本质上它内部还是用了一个对象, 后面学习完哈希表我会简单谈一下对象和哈希表的关系)
### 创建字典类
- 创建一个字典类
  ```
  // 创建字典的构造函数
  function Dictionay() {
      // 字典属性
      this.items = {}
      
      // 字典操作方法
  }
  ```
## 二.操作字典
### 常见的操作
- 字典常见的操作
  - `set(key,value)`：向字典中添加新元素。
  - `remove(key)`：通过使用键值来从字典中移除键值对应的数据值。
  - `has(key)`：如果某个键值存在于这个字典中，则返回true，反之则返回false。
  - `get(key)`：通过键值查找特定的数值并返回。
  - `clear()`：将这个字典中的所有元素全部删除。
  - `size()`：返回字典所包含元素的数量。与数组的length属性类似。
  - `keys()`：将字典所包含的所有键名以数组形式返回。
  - `values()`：将字典所包含的所有数值以数组形式返回。
### 操作的实现
- 方法实现
  ```
  // 创建字典的构造函数
  function Dictionay() {
      // 字典属性
      this.items = {}
  
      // 字典操作方法
      // 在字典中添加键值对
      Dictionay.prototype.set = function (key, value) {
          this.items[key] = value
      }
  
      // 判断字典中是否有某个key
      Dictionay.prototype.has = function (key) {
          return this.items.hasOwnProperty(key)
      }
  
      // 从字典中移除元素
      Dictionay.prototype.remove = function (key) {
          // 1.判断字典中是否有这个key
          if (!this.has(key)) return false
  
          // 2.从字典中删除key
          delete this.items[key]
          return true
      }
  
      // 根据key去获取value
      Dictionay.prototype.get = function (key) {
          return this.has(key) ? this.items[key] : undefined
      }
  
      // 获取所有的keys
      Dictionay.prototype.keys = function () {
          return Object.keys(this.items)
      }
  
      // 获取所有的value
      // Object.values用于获取一个对象的所有自有属性的值，并将这些值返回为一个数组。
      Dictionay.prototype.values = function () {
          return Object.values(this.items)
      }
  
      // size方法
      Dictionay.prototype.size = function () {
          return this.keys().length
      }
  
      // clear方法
      Dictionay.prototype.clear = function () {
          this.items = {}
      }
  }
  ```
### 字典的使用
- 使用和测试字典类
  ```
  // 创建字典对象
  var dict = new Dictionay()
  
  // 在字典中添加元素
  dict.set("age", 18)
  dict.set("name", "Harlen")
  dict.set("height", 1.88)
  dict.set("address", "HK")
  
  // 获取字典的信息
  alert(dict.keys()) // age,name,height,address
  alert(dict.values()) // 18,Harlen,1.88,HK
  alert(dict.size()) // 4
  alert(dict.get("name")) // Harlen
  
  // 字典的删除方法
  dict.remove("height")
  alert(dict.keys())// age,name,address
  
  // 清空字典
  dict.clear()
  ```
