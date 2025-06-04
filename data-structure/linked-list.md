# 链表
`链表和数组一样, 可以用于存储一系列的元素, 但是链表和数组的实现机制完全不同.`
## 目录
### 一.认识链表
- [链表和数组](#链表和数组)
- [什么是链表？](#什么是链表)
### 二.链表封装
- [创建链表类](#创建链表类)
- [链表常见操作](#链表常见操作)
### 三.链表操作
- [尾部追加数据](#尾部追加数据)
- [toString方法](#tostring方法)
- [任意位置插入](#任意位置插入)
- [根据位置移除数据](#根据位置移除数据)
- [获取元素位置](#获取元素位置)
- [根据元素删除](#根据元素删除)
- [其他方法的实现](#其他方法的实现)
### 四.完整代码
- [完整代码](#四完整代码-1)  

## 一.认识链表
### 链表和数组
- 数组:
  - 要存储多个元素，数组（或列表）可能是最常用的数据结构。
  - 我们之前说过, 几乎每一种编程语言都有默认实现数组结构, 这种数据结构非常方便，提供了一个便利的`[ ]`语法来访问它的元素。
  - 但是数组也有很多缺点:
    - 数组的创建通常需要申请一段连续的内存空间(一整块的内存), 并且大小是固定的(大多数编程语言数组都是固定的), 所以当当前数组不能满足容量需求时, 需要扩容. (一般情况下是申请一个更大的数组, 比如2倍. 然后将原数组中的元素复制过去)
    - 而且在数组开头或中间位置插入数据的成本很高, 需要进行大量元素的位移.（尽管我们已经学过的JavaScript的`Array`类方法可以帮我们做这些事，但背后的原理依然是这样）。
- 链表
  - 要存储多个元素, 另外一个选择就是使用链表.
  - 但不同于数组, 链表中的元素在内存中不必是连续的空间.
  - 链表的每个元素由一个存储元素本身的节点和一个指向下一个元素的引用(有些语言称为指针或者链接)组成.
  - 相对于数组, 链表有一些优点:
    - 内存空间不是必须连续的. 可以充分利用计算机的内存. 实现灵活的内存动态管理.
    - 链表不必在创建时就确定大小, 并且大小可以无限的延伸下去.
    - 链表在插入和删除数据时, 时间复杂度可以达到O(1). 相对数组效率高很多.
  - 相对于数组, 链表有一些缺点:
    - 链表访问任何一个位置的元素时, 都需要从头开始访问.(无法跳过第一个元素访问任何一个元素).
    - 无法通过下标直接访问元素, 需要从头一个个访问, 直到找到对应的元素.
### 什么是链表？
- 链表有一个链表头(head)，指向第一个节点(node)，节点存放数据(item)，以及指向下一个节点(next)，以此类推，直至为空。
- 链表的数据结构
  ![image](https://github.com/user-attachments/assets/4fe4d9bf-341b-4c13-bfa3-5bb375f5cabe)

## 二.链表封装
`通过代码封装链表`
### 创建链表类
- 创建一个链表类
  ```
  // 封装链表的构造函数
  function LinkedList() {
      // 封装一个Node类, 用于保存每个节点信息
      function Node(element) {
          this.element = element
          this.next = null
      }
  
      // 链表中的属性
      this.length = 0  // 链表的长度
      this.head = null // 链表的第一个节点
      
      // 链表中的方法
  }
  ```
- 代码解析:
  - 封装LinkedList的类, 用于表示我们的链表结构. (和Java中的链表同名, 不同的是Java中的这个类是一个双向链表, 后面我们会讲解双向链表)
  - 在LinkedList类中有一个Node类, 用于封装每一个节点上的信息.(和优先级队列的封装一样)
  - 链表中我们保存两个属性, 一个是链表的长度, 一个是链表中第一个节点.
  - 当然, 还有很多链表的操作方法. 我们放在下一节中学习.
### 链表常见操作
- 链表的常见操作
  - `append(element)`：向列表尾部添加一个新的项
  - `insert(position, element)`：向列表的特定位置插入一个新的项。
  - `remove(element)`：从列表中移除相应的元素。
  - `indexOf(element)`：返回元素在列表中的索引。如果列表中没有该元素则返回`-1`。
  - `removeAt(position)`：从列表的特定位置移除一项。
  - `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于0则返回`false`。
  - `size()`：返回链表包含的元素个数。与数组的`length`属性类似。
  - `toString()`：由于列表项使用了`Node`类，就需要重写继承自JavaScript对象默认的`toString`方法，让其只输出元素的值。
- 方法解读:
  - 整体你会发现操作方法和数组非常类似, 因为链表本身就是一种可以代替数组的结构.
  - 但是某些方法实现起来有些麻烦, 所以我们一个个来慢慢实现它们.
## 三.链表操作
### 尾部追加数据
- 向链表尾部追加数据可能有两种情况:
  - 链表本身为空, 新添加的数据是唯一的节点.
  - 链表不为空, 需要向其他节点后面追加节点.
- append方法实现
  ```
  // 链表尾部追加元素方法
  LinkedList.prototype.append = function (element) {
      // 1.根据新元素创建节点
      var newNode = new Node(element)
  
      // 2.判断原来链表是否为空
      if (this.head === null) { // 链表尾空
          this.head = newNode
      } else { // 链表不为空
          // 2.1.定义变量, 保存当前找到的节点
          var current = this.head
          while (current.next) {
              current = current.next
          }
  
          // 2.2.找到最后一项, 将其next赋值为node
          current.next = newNode
      }
  
      // 3.链表长度增加1
      this.length++
  }
  ```
### toString方法
- toString方法实现
  ```
  // 链表的toString方法
  LinkedList.prototype.toString = function () {
      // 1.定义两个变量
      var current = this.head
      var listString = ""
  
      // 2.循环获取链表中所有的元素
      while (current) {
          listString += "," + current.element  // 拼接字符串
          current = current.next
      }
  
      // 3.返回最终结果
      return listString.slice(1)
  }
  ```
- 测试append方法
  ```
  // 测试链表
  // 1.创建链表
  var list = new LinkedList()
  
  // 2.追加元素
  list.append(15)
  list.append(10)
  list.append(20)
  
  // 3.打印链表的结果
  alert(list)  // 15,10,20
  ```
### 任意位置插入
- 在任意位置插入数据.
  // 根据下标插入元素
  LinkedList.prototype.insert = function (position, element) {
      // 1.检测越界问题: 越界插入失败
      if (position < 0 || position > this.length) return false
  
      // 2.找到正确的位置, 并且插入数据
      var newNode = new Node(element)
      var current = this.head
      var previous = null
      index = 0
  
      // 3.判断是否在第一个位置插入
      if (position == 0) {
          newNode.next = current
          this.head = newNode
      } else {
          while (index++ < position) {
              previous = current
              current = current.next
          }
          
          newNode.next = current
          previous.next = newNode
      }
      
      // 4.length+1
      this.length++
      
      return true
  }
- 测试insert方法
  ```
  // 4.测试insert方法
  list.insert(0, 100)
  list.insert(4, 200)
  list.insert(2, 300)
  alert(list) // 100,15,300,10,20,200
  ```
### 根据位置移除数据
- 移除数据有两种常见的方式:
  - 根据位置移除对应的数据
  - 根据数据, 先找到对应的位置, 再移除数据
- 我们这里先完成根据位置移除数据的方式
  ```
  // 根据位置移除节点
  LinkedList.prototype.removeAt = function (position) {
      // 1.检测越界问题: 越界移除失败, 返回null
      if (position < 0 || position >= this.length) return null
  
      // 2.定义变量, 保存信息
      var current = this.head
      var previous = null
      var index = 0
      
      // 3.判断是否是移除第一项
      if (position === 0) {
          this.head = current.next
      } else {
          while (index++ < position) {
              previous = current
              current = current.next
          }
          
          previous.next = current.next
      }
      
      // 4.length-1
      this.length--
      
      // 5.返回移除的数据
      return current.element
  }
  ```
- 测试removeAt方法
  ```
  // 5.测试removeAt方法
  list.removeAt(0)
  list.removeAt(1)
  list.removeAt(3)
  alert(list) // 15, 10, 20
  ```
### 获取元素位置
- indexOf方法实现
  ```
  // 根据元素获取链表中的位置
  LinkedList.prototype.indexOf = function (element) {
      // 1.定义变量, 保存信息
      var current = this.head
      index = 0
      
      // 2.找到元素所在的位置
      while (current) {
          if (current.element === element) {
              return index
          }
          index++
          current = current.next
      }
      
      // 3.来到这个位置, 说明没有找到, 则返回-1
      return -1
  }
  ```
- 测试indexOf方法
  ```
  // 6.测试indexOf方法
  alert(list.indexOf(15)) // 0
  alert(list.indexOf(10)) // 1
  alert(list.indexOf(20)) // 2
  alert(list.indexOf(100)) // -1
  ```
### 根据元素删除
- remove方法实现
  ```
  // 根据元素删除信息
  LinkedList.prototype.remove = function (element) {
      var index = this.indexOf(element)
      return this.removeAt(index)
  }
  ```
- 测试remove方法
  ```
  // 7.测试remove方法
  list.remove(15)
  alert(list) // 10,20
  ```
### 其他方法的实现
- isEmpty方法
  ```
  // 判断链表是否为空
  LinkedList.prototype.isEmpty = function () {
      return this.length == 0
  }
  ```
- size方法
  ```
  // 获取链表的长度
  LinkedList.prototype.size = function () {
      return this.length
  }
  ```
- getFirst方法
  ```
  // 获取第一个节点
  LinkedList.prototype.getFirst = function () {
      return this.head.element
  }
  ```
- 方法测试
  ```
  // 8.测试其他方法
  alert(list.isEmpty()) // false
  alert(list.size()) // 2
  alert(list.getFirst()) // 10
  ```
## 四.完整代码
- 链表的完整代码
```
// 封装链表的构造函数
function LinkedList() {
    // 封装一个Node类, 用于保存每个节点信息
    function Node(element) {
        this.element = element
        this.next = null
    }

    // 链表中的属性
    this.length = 0
    this.head = null

    // 链表尾部追加元素方法
    LinkedList.prototype.append = function (element) {
        // 1.根据新元素创建节点
        var newNode = new Node(element)

        // 2.判断原来链表是否为空
        if (this.head === null) { // 链表尾空
            this.head = newNode
        } else { // 链表不为空
            // 2.1.定义变量, 保存当前找到的节点
            var current = this.head
            while (current.next) {
                current = current.next
            }

            // 2.2.找到最后一项, 将其next赋值为node
            current.next = newNode
        }

        // 3.链表长度增加1
        this.length++
    }

    // 链表的toString方法
    LinkedList.prototype.toString = function () {
        // 1.定义两个变量
        var current = this.head
        var listString = ""

        // 2.循环获取链表中所有的元素
        while (current) {
            listString += "," + current.element
            current = current.next
        }

        // 3.返回最终结果
        return listString.slice(1)
    }

    // 根据下标删除元素
    LinkedList.prototype.insert = function (position, element) {
        // 1.检测越界问题: 越界插入失败
        if (position < 0 || position > this.length) return false

        // 2.定义变量, 保存信息
        var newNode = new Node(element)
        var current = this.head
        var previous = null
        index = 0

        // 3.判断是否列表是否在第一个位置插入
        if (position == 0) {
            newNode.next = current
            this.head = newNode
        } else {
            while (index++ < position) {
                previous = current
                current = current.next
            }

            newNode.next = current
            previous.next = newNode
        }

        // 4.length+1
        this.length++

        return true
    }

    // 根据位置移除节点
    LinkedList.prototype.removeAt = function (position) {
        // 1.检测越界问题: 越界移除失败, 返回null
        if (position < 0 || position >= this.length) return null

        // 2.定义变量, 保存信息
        var current = this.head
        var previous = null
        var index = 0

        // 3.判断是否是移除第一项
        if (position === 0) {
            this.head = current.next
        } else {
            while (index++ < position) {
                previous = current
                current = current.next
            }

            previous.next = current.next
        }

        // 4.length-1
        this.length--

        // 5.返回移除的数据
        return current.element
    }

    // 根据元素获取链表中的位置
    LinkedList.prototype.indexOf = function (element) {
        // 1.定义变量, 保存信息
        var current = this.head
        index = 0

        // 2.找到元素所在的位置
        while (current) {
            if (current.element === element) {
                return index
            }
            index++
            current = current.next
        }

        // 3.来到这个位置, 说明没有找到, 则返回-1
        return -1
    }

    // 根据元素删除信息
    LinkedList.prototype.remove = function (element) {
        var index = this.indexOf(element)
        return this.removeAt(index)
    }

    // 判断链表是否为空
    LinkedList.prototype.isEmpty = function () {
        return this.length == 0
    }

    // 获取链表的长度
    LinkedList.prototype.size = function () {
        return this.length
    }

    // 获取第一个节点
    LinkedList.prototype.getFirst = function () {
        return this.head.element
    }
}
```
