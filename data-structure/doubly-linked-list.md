# 双向链表

## 目录
### 一.认识双向链表
- [双向链表介绍](#双向链表介绍)
- [双向链表的创建](#双向链表的创建)
### 二.操作双向链表
- [尾部追加数据](#尾部追加数据)
- [正向反向遍历](#正向反向遍历)
- [任意位置插入](#任意位置插入)
- [位置移除数据](#位置移除数据)
- [获取元素位置](#获取元素位置)
- [根据元素删除](#根据元素删除)
- [其他方法实现](#其他方法实现)
### 三.完整代码
- [双向链表完整代码](#双向链表完整代码)

## 一.认识双向链表
### 双向链表介绍
- 单向链表:
  - 只能从头遍历到尾或者从尾遍历到头(一般从头到尾)
  - 也就是链表相连的过程是单向的. 实现的原理是上一个节点中有一个指向下一个节点的引用.
  - 单向链表有一个比较明显的缺点:
    - 我们可以轻松的到达下一个节点, 但是回到前一个节点是很难的. 但是, 在实际开发中, 经常会遇到需要回到上一个节点的情况
    - 举个例子: 假设一个文本编辑用链表来存储文本. 每一行用一个String对象存储在链表的一个节点中. 当编辑器用户向下移动光标时, 链表直接操作到下一个节点即可. 但是当用于将光标向上移动呢? 这个时候为了回到上一个节点, 我们可能需要从first开始, 依次走到想要的节点上.
- 双向链表
  - 既可以从头遍历到尾, 又可以从尾遍历到头
  - 也就是链表相连的过程是双向的. 那么它的实现原理, 你能猜到吗?
  - 一个节点既有向前连接的引用, 也有一个向后连接的引用.
  - 双向链表可以有效的解决单向链表中提到的问题.
  - 双向链表有什么缺点呢?
    - 每次在插入或删除某个节点时, 需要处理四个节点的引用, 而不是两个. 也就是实现起来要困难一些
    - 并且与单向链表相比, 必然占用内存空间更大一些.
    - 但是这些缺点和我们使用起来的方便程度相比, 是微不足道的.
- 双向连接的图解:
  ![image](https://github.com/user-attachments/assets/8bec3b15-d20e-4b5c-bb9c-ca3094226f7b)

### 双向链表的创建
- 创建一个双向链表的类
  ```
  // 创建双向链表的构造函数
  function DoublyLinkedList() {
      // 创建节点构造函数
      function Node(element) {
          this.element = element  // 元素的值
          this.next = null  // 指向下一个节点
          this.prev = null // 新添加的属性，用于指向前一个节点
      }
  
      // 定义属性
      this.length = 0
      this.head = null  // 链表的头部
      this.tail = null // 新添加的属性，表示链表的尾部，即最后一个节点
  
      // 定义相关操作方法
  }
  ```
## 二.操作双向链表
### 尾部追加数据
- append方法
  ```
  // 在尾部追加数据
  DoublyLinkedList.prototype.append = function (element) {
      // 1.根据元素创建节点
      var newNode = new Node(element)
  
      // 2.判断列表是否为空列表
      if (this.head == null) {
          this.head = newNode
          this.tail = newNode
      } else {  // 若链表不为空，则直接通过 tail 属性来向尾部插入节点
          this.tail.next = newNode
          newNode.prev = this.tail
          this.tail = newNode
      }
      
      // 3.length+1
      this.length++
  }
  ```
### 正向反向遍历
- 链表的遍历
  - 之前我们在单向链表中实现了一个toString方法, 它是一种正向的遍历.
  - 现在, 为了用户使用方便, 我们实现三个方法
    - forwardString: 正向遍历转成字符串的方法
    - reverseString: 反向遍历转成字符串的方法
    - toString: 正向遍历转成字符串的方法
- 方法的相关实现:
  ```
  // 正向遍历的方法
  DoublyLinkedList.prototype.forwardString = function () {
      var current = this.head
      var forwardStr = ""
      
      while (current) {
          forwardStr += "," + current.element
          current = current.next
      }
      
      return forwardStr.slice(1)
  }
  
  // 反向遍历的方法
  DoublyLinkedList.prototype.reverseString = function () {
      var current = this.tail
      var reverseStr = ""
      
      while (current) {
          reverseStr += "," + current.element
          current = current.prev
      }
      
      return reverseStr.slice(1)
  }
  
  // 实现toString方法
  DoublyLinkedList.prototype.toString = function () {
      return this.forwardString()
  }
  ```
- 测试append方法
  ```
  // 1.创建双向链表对象
  var list = new DoublyLinkedList()
  
  // 2.追加元素
  list.append("abc")
  list.append("cba")
  list.append("nba")
  list.append("mba")
  
  // 3.获取所有的遍历结果
  alert(list.forwardString()) // abc,cba,nba,mba
  alert(list.reverseString()) // mba,nba,cba,abc
  alert(list) // abc,cba,nba,mba
  ```
### 任意位置插入
- insert方法
  ```
  // 在任意位置插入数据
  DoublyLinkedList.prototype.insert = function (position, element) {
      // 1.判断越界的问题
      if (position < 0 || position > this.length) return false
  
      // 2.创建新的节点
      var newNode = new Node(element)
  
      // 3.判断插入的位置
      if (position === 0) { // 在第一个位置插入数据
          // 判断链表是否为空
          if (this.head == null) {
              this.head = newNode
              this.tail = newNode
          } else {
              this.head.prev = newNode
              newNode.next = this.head
              this.head = newNode
          }
      } else if (position === this.length) { // 插入到最后的情况
          // 思考: 这种情况是否需要判断链表为空的情况呢? 答案是不需要, 为什么?
          // 因为链表为空的情况，插入尾部等于第一个位置，会走上面的判断
          this.tail.next = newNode
          newNode.prev = this.tail
          this.tail = newNode
      } else { // 在中间位置插入数据
          // 定义属性
          var index = 0
          var current = this.head
          var previous = null
          
          // 查找正确的位置
          while (index++ < position) {
              previous = current
              current = current.next
          }
          
          // 交换节点的指向顺序
          newNode.next = current
          newNode.prev = previous
          current.prev = newNode
          previous.next = newNode
      }
      
      // 4.length+1
      this.length++
      
      return true
  }
  ```
- 测试insert方法
  ```
  // 4.insert方法测试
  list.insert(0, "100")
  list.insert(2, "200")
  list.insert(6, "300")
  alert(list) // 100,abc,200,cba,nba,mba,300
  ```
- 课下思考: 代码性能能否改进一点呢?
  - 如果我们position大于length/2, 是否从尾部开始迭代性能更高一些呢?
  - 对于初学者来说, 可以作为思考. 但是先搞定上面的内容吧.
- 思考？
  - 如果position大于一般长度，可以考虑从尾部倒序遍历来寻找位置，只需用length - position ？
### 位置移除数据
- removeAt方法：通过下标值（位置）来删除元素
  ```
  // 根据位置删除对应的元素
  DoublyLinkedList.prototype.removeAt = function (position) {
      // 1.判断越界的问题
      if (position < 0 || position >= this.length) return null
  
      // 2.判断移除的位置
      var current = this.head
      if (position === 0) {  // 删除第一个节点
          if (this.length == 1) {  // 链表长度为1，即清空链表
              this.head = null
              this.tail = null
          } else {  // 链表长度不为1，将head指向第二个节点
              this.head = this.head.next
              this.head.prev = null
          }
      } else if (position === this.length -1) {  // 删除最后一个节点
          current = this.tail
          this.tail = this.tail.prev
          this.tail.next = null
      } else {  // 删除中间节点
          var index = 0
          var previous = null
  
          while (index++ < position) {
              previous = current
              current = current.next
          }
  
          previous.next = current.next
          current.next.prev = previous
      }
  
      // 3.length-1
      this.length--
  
      return current.element
  }
  ```
- 测试removeAt方法
  ```
  // 5.removeAt方法测试
  alert(list.removeAt(0)) // 100
  alert(list.removeAt(1)) // 200
  alert(list.removeAt(4)) // 300
  alert(list) // abc,cba,nba,mba
  ```
### 获取元素位置
- indexOf方法
  ```
  // 根据元素获取在链表中的位置
  DoublyLinkedList.prototype.indexOf = function (element) {
      // 1.定义变量保存信息
      var current = this.head
      var index = 0
  
      // 2.查找正确的信息
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
  // 6.indexOf方法测试
  alert(list.indexOf("abc")) // 0
  alert(list.indexOf("cba")) // 1
  alert(list.indexOf("nba")) // 2
  alert(list.indexOf("mba")) // 3
  ```
### 根据元素删除
- remove方法
  ```
  // 根据元素删除
  DoublyLinkedList.prototype.remove = function (element) {
      var index = this.indexOf(element)
      return this.removeAt(index)
  }
  ```
- 测试remove方法
  ```
  // 7.remove方法测试
  alert(list.remove("abc")) // abc
  alert(list) // cba,nba,mba
  ```
### 其他方法实现
- 其余四个方法: isEmpty, size, getHead, getTail
  ```
  // 判断是否为空
  DoublyLinkedList.prototype.isEmpty = function () {
      return this.length === 0
  }
  
  // 获取链表长度
  DoublyLinkedList.prototype.size = function () {
      return this.length
  }
  
  // 获取第一个元素
  DoublyLinkedList.prototype.getHead = function () {
      return this.head.element
  }
  
  // 获取最后一个元素
  DoublyLinkedList.prototype.getTail = function () {
      return this.tail.element
  }
  ```
- 测试其余方法
  ```
  // 8.测试最后四个方法
  alert(list.getHead())
  alert(list.getTail())
  alert(list.isEmpty())
  alert(list.size())
  ```
## 三.完整代码
### 双向链表完整代码
```
// 创建双向链表的构造函数
function DoublyLinkedList() {
    // 创建节点构造函数
    function Node(element) {
        this.element = element
        this.next = null
        this.prev = null // 新添加的
    }

    // 定义属性
    this.length = 0
    this.head = null
    this.tail = null // 新添加的

    // 定义相关操作方法
    // 在尾部追加数据
    DoublyLinkedList.prototype.append = function (element) {
        // 1.根据元素创建节点
        var newNode = new Node(element)

        // 2.判断列表是否为空列表
        if (this.head == null) {
            this.head = newNode
            this.tail = newNode
        } else {
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        }

        // 3.length+1
        this.length++
    }

    // 在任意位置插入数据
    DoublyLinkedList.prototype.insert = function (position, element) {
        // 1.判断越界的问题
        if (position < 0 || position > this.length) return false

        // 2.创建新的节点
        var newNode = new Node(element)

        // 3.判断插入的位置
        if (position === 0) { // 在第一个位置插入数据
            // 判断链表是否为空
            if (this.head == null) {
                this.head = newNode
                this.tail = newNode
            } else {
                this.head.prev = newNode
                newNode.next = this.head
                this.head = newNode
            }
        } else if (position === this.length) { // 插入到最后的情况
            // 思考: 这种情况是否需要判断链表为空的情况呢? 答案是不需要, 为什么?
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        } else { // 在中间位置插入数据
            // 定义属性
            var index = 0
            var current = this.head
            var previous = null

            // 查找正确的位置
            while (index++ < position) {
                previous = current
                current = current.next
            }

            // 交换节点的指向顺序
            newNode.next = current
            newNode.prev = previous
            current.prev = newNode
            previous.next = newNode
        }

        // 4.length+1
        this.length++

        return true
    }

    // 根据位置删除对应的元素
    DoublyLinkedList.prototype.removeAt = function (position) {
        // 1.判断越界的问题
        if (position < 0 || position >= this.length) return null

        // 2.判断移除的位置
        var current = this.head
        if (position === 0) {
            if (this.length == 1) {
                this.head = null
                this.tail = null
            } else {
                this.head = this.head.next
                this.head.prev = null
            }
        } else if (position === this.length -1) {
            current = this.tail
            this.tail = this.tail.prev
            this.tail.next = null
        } else {
            var index = 0
            var previous = null

            while (index++ < position) {
                previous = current
                current = current.next
            }

            previous.next = current.next
            current.next.prev = previous
        }

        // 3.length-1
        this.length--

        return current.element
    }

    // 根据元素获取在链表中的位置
    DoublyLinkedList.prototype.indexOf = function (element) {
        // 1.定义变量保存信息
        var current = this.head
        var index = 0

        // 2.查找正确的信息
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

    // 根据元素删除
    DoublyLinkedList.prototype.remove = function (element) {
        var index = this.indexOf(element)
        return this.removeAt(index)
    }

    // 判断是否为空
    DoublyLinkedList.prototype.isEmpty = function () {
        return this.length === 0
    }

    // 获取链表长度
    DoublyLinkedList.prototype.size = function () {
        return this.length
    }

    // 获取第一个元素
    DoublyLinkedList.prototype.getHead = function () {
        return this.head.element
    }

    // 获取最后一个元素
    DoublyLinkedList.prototype.getTail = function () {
        return this.tail.element
    }

    // 遍历方法的实现
    // 正向遍历的方法
    DoublyLinkedList.prototype.forwardString = function () {
        var current = this.head
        var forwardStr = ""

        while (current) {
            forwardStr += "," + current.element
            current = current.next
        }

        return forwardStr.slice(1)
    }

    // 反向遍历的方法
    DoublyLinkedList.prototype.reverseString = function () {
        var current = this.tail
        var reverseStr = ""

        while (current) {
            reverseStr += "," + current.element
            current = current.prev
        }

        return reverseStr.slice(1)
    }

    // 实现toString方法
    DoublyLinkedList.prototype.toString = function () {
        return this.forwardString()
    }
}
```
