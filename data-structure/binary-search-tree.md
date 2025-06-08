# 二叉搜索树

## 目录
### 一.二叉搜索树的概念
- [什么是二叉搜索树？](#什么是二叉搜索树)
- [二叉搜索树的操作](#二叉搜索树的操作)
### 二.二叉搜索树的实现
- [创建二叉搜索树](#创建二叉搜索树)
- [向树中插入数据](#向树中插入数据)
- [遍历二叉搜索树](#遍历二叉搜索树)
- [先序遍历](#先序遍历)
- [中序遍历](#中序遍历)
- [后序遍历](#后序遍历)
- [最大值&最小值](#最大值最小值)
- [搜索特定的值](#搜索特定的值)
### 三.二叉搜索树的删除
- [删除节点的思路](#删除节点的思路)
- [情况一:没有子节点](#情况一没有子节点)
- [情况二:一个字节点](#情况二一个字节点)
- [情况三:两个子节点](#情况三两个子节点)
- [删除节点完整代码](#删除节点完整代码)
- [删除节点的回顾](#删除节点的回顾)
### 四.完整代码
- [二叉搜索树完整代码](#二叉搜索树完整代码)

## 一.二叉搜索树的概念
### 什么是二叉搜索树？
- 二叉搜索树（BST，Binary Search Tree），也称二叉排序树或二叉查找树
- 二叉搜索树是一颗二叉树, 可以为空；如果不为空，满足以下性质：
  - 非空左子树的所有键值小于其根结点的键值。
  - 非空右子树的所有键值大于其根结点的键值。
  - 左、右子树本身也都是二叉搜索树。
- 二叉搜索树的特点:
  - 二叉搜索树的特点就是相对较小的值总是保存在左结点上, 相对较大的值总是保存在右结点上.
  - 因此，查找效率非常高, 这也是二叉搜索树中, 搜索的来源.
### 二叉搜索树的操作
- 二叉搜索树常见的操作
  - `insert(key)`：向树中插入一个新的键。
  - `search(key)`：在树中查找一个键，如果结点存在，则返回true；如果不存在，则返回false。
  - `inOrderTraverse`：通过中序遍历方式遍历所有结点。
  - `preOrderTraverse`：通过先序遍历方式遍历所有结点。
  - `postOrderTraverse`：通过后序遍历方式遍历所有结点。
  - `min`：返回树中最小的值/键。
  - `max`：返回树中最大的值/键。
  - `remove(key)`：从树中移除某个键。
## 二.二叉搜索树的实现
### 创建二叉搜索树
- 封装一个BinarySearchTree的类
  ```
  // 创建BinarySearchTree
  function BinarySerachTree() {
      // 创建结点构造函数
      function Node(key) {
          this.key = key    // 节点的数据
          this.left = null  // 指向左子树
          this.right = null // 指向右子树 
      }
      
      // 保存根的属性
      this.root = null  // 保存根节点信息
      
      // 二叉搜索树相关的操作方法
  }
  ```
### 向树中插入数据
- 外界调用的insert方法
  ```
  // 向树中插入数据
  BinarySerachTree.prototype.insert = function (key) {
      // 1.根据key创建对应的node
      var newNode = new Node(key)
      
      // 2.判断根结点是否有值
      if (this.root === null) {
          this.root = newNode
      } else {
          this.insertNode(this.root, newNode)  // 调用内部的插入方法
      }
  }
  ```
- 插入非根节点(类内部调用)
  ```
  BinarySerachTree.prototype.insertNode = function (node, newNode) {
      if (newNode.key < node.key) { // 1.准备向左子树插入数据
          if (node.left === null) { // 1.1.node的左子树上没有内容
              node.left = newNode
          } else { // 1.2.node的左子树上已经有了内容
              this.insertNode(node.left, newNode)
          }
      } else { // 2.准备向右子树插入数据
          if (node.right === null) { // 2.1.node的右子树上没有内容
              node.right = newNode
          } else { // 2.2.node的右子树上有内容
              this.insertNode(node.right, newNode)
          }
      }
  }
  ```
- 测试代码
  ```
  // 测试代码
  var bst = new BinarySerachTree()
  
  // 插入数据
  bst.insert(11)
  bst.insert(7)
  bst.insert(15)
  bst.insert(5)
  bst.insert(3)
  bst.insert(9)
  bst.insert(8)
  bst.insert(10)
  bst.insert(13)
  bst.insert(12)
  bst.insert(14)
  bst.insert(20)
  bst.insert(18)
  bst.insert(25)
  ```
- 形成的树  
  ![image](https://github.com/user-attachments/assets/0e7fec50-58b5-46e0-87cb-450815f65c90)
- 这个时候插入一个6
  ```
  bst.insert(6)
  ```
- 新的树  
  ![image](https://github.com/user-attachments/assets/dc35fdaf-79e1-4230-90a2-581585e4c7c6)
### 遍历二叉搜索树
- 前面, 我们向树中插入了很多的数据, 为了能很多的看到测试结果. 我们先来学习一下树的遍历.
  - 注意: 这里我们学习的树的遍历, 针对所有的二叉树都是适用的, 不仅仅是二叉搜索树.
- 树的遍历:
  - 遍历一棵树是指访问树的每个结点(也可以对每个结点进行某些操作, 我们这里就是简单的打印)
  - 但是树和线性结构不太一样, 线性结构我们通常按照从前到后的顺序遍历, 但是树呢?
  - 二叉树的遍历常见的有三种方式: 先序遍历/中序遍历/后续遍历. (还有程序遍历, 使用较少, 可以使用队列来完成)
### 先序遍历
- 遍历过程为：
  - ①访问根结点；
  - ②先序遍历其左子树；
  - ③先序遍历其右子树。
- 遍历过程:
  ![image](https://github.com/user-attachments/assets/3f28e809-9930-4164-9d6a-8e5332647760)
- 代码实现
  ```
  BinarySerachTree.prototype.preOrderTraversal = function (handler) {
      this.preOrderTranversalNode(this.root, handler)
  }
  
  BinarySerachTree.prototype.preOrderTranversalNode = function (node, handler) {
      if (node !== null) {
          // 1.打印当前经过的节点
          handler(node.key)
          // 2.遍历所有的左子树
          this.preOrderTranversalNode(node.left, handler)
          // 3.遍历所有的右子树
          this.preOrderTranversalNode(node.right, handler)
      }
  }
  ```
- 测试代码
  ```
  // 测试前序遍历结果
  var resultString = ""
  bst.preOrderTraversal(function (key) {
      resultString += key + " "
  })
  alert(resultString) // 11 7 5 3 6 9 8 10 15 13 12 14 20 18 25 
  ```
- 先序遍历代码图解
  ![image](https://github.com/user-attachments/assets/492c74a0-82c2-4f68-8ba7-587ac15a1b38)
### 中序遍历
- 遍历过程为:
  - ①中序遍历其左子树；
  - ②访问根结点；
  - ③中序遍历其右子树。
- 遍历过程:  
  ![image](https://github.com/user-attachments/assets/7fc2c1d3-f484-4d0c-961c-159164b156a4)
- 代码实现
  ```
  // 中序遍历
  BinarySerachTree.prototype.inOrderTraversal = function (handler) {
      this.inOrderTraversalNode(this.root, handler)
  }
  
  BinarySerachTree.prototype.inOrderTraversalNode = function (node, handler) {
      if (node !== null) {
          this.inOrderTraversalNode(node.left, handler)
          handler(node.key)
          this.inOrderTraversalNode(node.right, handler)
      }
  }
  ```
- 测试代码
  ```
  // 测试中序遍历结果
  resultString = ""
  bst.inOrderTraversal(function (key) {
      resultString += key + " "
  })
  alert(resultString) // 3 5 6 7 8 9 10 11 12 13 14 15 18 20 25 
  ```
- 中序遍历图解  
  ![image](https://github.com/user-attachments/assets/d6d0de44-48b7-4cdb-a995-e29056f5032e)
### 后序遍历
- 遍历过程为：
  - ①后序遍历其左子树；
  - ②后序遍历其右子树；
  - ③访问根结点。
- 遍历过程:  
  ![image](https://github.com/user-attachments/assets/c2b7e0d7-2257-4c45-907e-e969924d5af3)
- 代码实现
  ```
  // 后续遍历
  BinarySerachTree.prototype.postOrderTraversal = function (handler) {
      this.postOrderTraversalNode(this.root, handler)
  }
  
  BinarySerachTree.prototype.postOrderTraversalNode = function (node, handler) {
      if (node !== null) {
          this.postOrderTraversalNode(node.left, handler)
          this.postOrderTraversalNode(node.right, handler)
          handler(node.key)
      }
  }
  ```
- 测试代码
  ```
  // 测试后续遍历结果
  resultString = ""
  bst.postOrderTraversal(function (key) {
      resultString += key + " "
  })
  alert(resultString) // 3 6 5 8 10 9 7 12 14 13 18 25 20 15 11 
  ```
- 后序遍历图解
  ![image](https://github.com/user-attachments/assets/c5cfc557-a20a-47f0-8048-68cefc92bd46)
### 最大值&最小值
- 在二叉搜索树中搜索最值是一件非常简单的事情, 其实用眼睛看就可以看出来了.  
  ![image](https://github.com/user-attachments/assets/aa7f32fc-7b4a-4ca7-942f-3bb3d91e174c)
- 代码实现
  ```
  // 获取最大值和最小值
  BinarySerachTree.prototype.min = function () {
      var node = this.root
      while (node.left !== null) {
          node = node.left
      }
      return node.key
  }
  
  BinarySerachTree.prototype.max = function () {
      var node = this.root
      while (node.right !== null) {
          node = node.right
      }
      return node.key
  }
  ```
- 测试代码
  ```
  // 获取最值
  alert(bst.min()) // 3
  alert(bst.max()) // 25
  ```
### 搜索特定的值
- 二叉搜索树不仅仅获取最值效率非常高, 搜索特定的值效率也非常高.
  ```
  // 搜搜特定的值
  BinarySerachTree.prototype.search = function (key) {
      return this.searchNode(this.root, key)
  }
  
  BinarySerachTree.prototype.searchNode = function (node, key) {
      // 1.如果传入的node为null那么, 那么就退出递归
      if (node === null) {
          return false
      }
  
      // 2.判断node节点的值和传入的key大小
      if (node.key > key) { // 2.1.传入的key较小, 向左边继续查找
          return this.searchNode(node.left, key)
      } else if (node.key < key) { // 2.2.传入的key较大, 向右边继续查找
          return this.searchNode(node.right, key)
      } else { // 2.3.相同, 说明找到了key
          return true
      }
  }
  ```
- 代码解析:
  - 这里我们还是使用了递归的方式. 待会儿我们来写一个非递归的实现.
  - 递归必须有退出条件, 我们这里是两种情况下退出.
    - node === null, 也就是后面不再有节点的时候.
    - 找到对应的key, 也就是node.key === key的时候.
  - 在其他情况下, 根据node.的key和传入的key进行比较来决定向左还是向右查找.
    - 如果node.key > key, 那么说明传入的值更小, 需要向左查找.
    - 如果node.key < key, 那么说明传入的值更大, 需要向右查找.
- 测试代码
  ```
  // 查找特定的值
  alert(bst.search(10)) // true
  alert(bst.search(21)) // false
  ```
- 非递归代码实现:
  ```
  BinarySerachTree.prototype.search = function (key) {
      var node = this.root
      while (node !== null) {
          if (node.key > key) {
              node = node.left
          } else if (node.key < key) {
              node = node.right
          } else {
              return true
          }
      }
      return false
  }
  ```
- 递归or循环?
  - 其实递归和循环之间可以相互转换.
  - 大多数情况下, 递归调用可以简化代码, 但是也会增加空间的复杂度.
  - 循环空间复杂度较低, 但是代码会相对复杂.
  - 可以根据实际的情况自行选择, 不需要套死必须使用某种方式.
## 三.二叉搜索树的删除
### 删除节点的思路
- 删除节点要从查找要删除的节点开始, 找到节点后, 需要考虑三种情况:
  - 该节点是叶子结点(没有子节点, 比较简单)
  - 该节点有一个子节点(也相对简单)
  - 该节点有两个子节点.(情况比较复杂, 我们后面慢慢道来)
- 我们先从查找要删除的节点入手
  ```
  // 删除结点
  BinarySerachTree.prototype.remove = function (key) {
      // 1.定义临时保存的变量
      var current = this.root // 存储要删除的节点
      var parent = this.root  // 存储父节点，用于删除子节点后的更新操作
      var isLeftChild = true  // 判断删除节点是否位于父节点的左侧，来选择left还是right
  
      // 2.开始查找节点
      while (current.key !== key) {
          parent = current
          if (key < current.key) {
              isLeftChild = true
              current = current.left
          } else {
              isLeftChild = false
              current = current.right
          }
  
          // 如果发现current已经指向null, 那么说明没有找到要删除的数据
          if (current === null) return false
      }
  
      return true
  }
  ```
### 情况一:没有子节点
- 情况一: 没有子节点.
  - 这种情况相对比较简单, 我们需要检测current的left以及right是否都为null.
  - 都为null之后还要检测一个东西, 就是是否current就是根, 都为null, 并且为跟根, 那么相当于要清空二叉树(当然, 只是清空了根, 因为只有它).
  - 否则就把父节点的left或者right字段设置为null即可.
- 图解过程:
  - 如果只有一个单独的根, 直接删除即可  
    ![image](https://github.com/user-attachments/assets/b7865078-a45f-4a98-9b71-dc0dc856d8ed)
  - 如果是叶结点, 那么处理方式如下:  
    ![image](https://github.com/user-attachments/assets/72c87ff9-1bd4-41b3-ba65-2765d38bb131)
- 代码实现:
  ```
  // 3.删除的结点是叶结点
  if (current.left === null && current.right === null) {
      if (current == this.root) {
          this.root == null
      } else if (isLeftChild) {
          parent.left = null
      } else {
          parent.right = null
      }
  }
  ```
### 情况二:一个字节点
- 情况二: 有一个子节点
  - 这种情况也不是很难.
  - 要删除的current结点, 只有2个连接(如果有两个子结点, 就是三个连接了), 一个连接父节点, 一个连接唯一的子节点.
  - 需要从这三者之间: 爷爷 - 自己 - 儿子, 将自己(current)剪短, 让爷爷直接连接儿子即可.
  - 这个过程要求改变父节点的left或者right, 指向要删除节点的子节点.
  - 当然, 在这个过程中还要考虑是否current就是根.
- 图解过程:  
  - 如果是根的情况, 大家可以自己画一下, 比较简单, 这里不再给出.
  - 如果不是根, 并且只有一个子节点的情况.
    ![image](https://github.com/user-attachments/assets/bfcb71ff-9c12-4c1f-81b8-9846f8cca197)
- 代码实现:
  ```
  // 4.删除有一个子节点的节点
  else if (current.right === null) {
      if (current == this.root) {
          this.root = current.left
      } else if (isLeftChild) {
          parent.left = current.left
      } else {
          parent.right = current.left
      }
  } else if (current.left === null) {
      if (current == this.root) {
          this.root = current.right
      } else if (isLeftChild) {
          parent.left = current.right
      } else {
          parent.right = current.right
      }
  }
  ```
### 情况三:两个子节点
- 情况三: 有两个子节点.
  - 事情变得非常复杂, 也非常有趣了.
- 我们先来思考一下我提出的一些问题:
  ![image](https://github.com/user-attachments/assets/0d661da8-5cbc-4ca4-afc5-67457bbb8799)
- 先来, 我们来总结一下删除有两个子节点的规律:
  - 如果我们要删除的节点有两个子节点, 甚至子节点还有子节点, 这种情况下我们需要从下面的子节点中找到一个节点, 来替换当前的节点.
  - 但是找到的这个节点有什么特征呢? 应该是current节点下面所有节点中最接近current节点的.
    - 要么比current节点小一点点, 要么比current节点大一点点.
    - 总结: 你最接近current, 你就可以用来替换current的位置.
  - 这个节点怎么找呢?
    - 比current小一点点的节点, 一定是current左子树的最大值.
    - 比current大一点点的节点, 一定是current右子树的最小值.
  - 前驱&后继
    - 而在二叉搜索树中, 这两个特别的节点, 有两个特比的名字.
    - 比current小一点点的节点, 称为current节点的**前驱**.
    - 比current大一点点的节点, 称为current节点的**后继**.
  - 也就是为了能够删除有两个子节点的current, 要么找到它的前驱, 要么找到它的后继.
  - 所以, 接下来, 我们先找到这样的节点(前驱或者后继都可以, 我这里以找后继为例)
- 寻找后继的代码实现:
  ```
  // 找后继的方法
  BinarySerachTree.prototype.getSuccessor = function (delNode) {
      // 1.使用变量保存临时的节点
      var successorParent = delNode  // 后继结点的父节点
      var successor = delNode        // 后继结点
      var current = delNode.right    // 当前节点, 要从右子树开始找
  
      // 2.寻找节点
      while (current != null) {
          successorParent = successor
          successor = current
          current = current.left   // 找到右子树的最左边节点即为后继结点
      }
  
      // 3.如果是删除图中15的情况, 还需要如下代码
      // 如果后继结点不是删除节点的右子节点，
      // 那么就要将后继结点的右子树代替他的位置，
      // 即，让后继结点的父节点的左侧(原来后继结点的位置)连接后继结点的右子树
      // 然后，后继结点还需要将自己的右侧指向删除节点的右子树，因为替代了删除节点的位置
      // 至此，后续节点完成了对删除节点的右子树的调整，
      // 后续还需要完成左侧以及父节点的更新
      if (successor != delNode.right) {
          successorParent.left = successor.right
          successor.right = delNode.right
      }
      
      return successor
  }
  ```
- 找到后继后的处理代码:
  ```
  // 5.删除有两个节点的节点
  else {
      // 1.获取后继节点
      // 此时，已完成对右子树的更新
      var successor = this.getSuccessor(current)
      
      // 2.判断是否是根节点
      // 更新父节点信息
      if (current == this.root) {
          this.root = successor
      } else if (isLeftChild) {
          parent.left = successor
      } else {
          parent.right = successor
      }
      
      // 3.将删除节点的左子树赋值给successor
      // 更新左子树信息
      successor.left = current.left
  }
  ```
### 删除节点完整代码
```
// 删除结点
BinarySerachTree.prototype.remove = function (key) {
    // 1.定义临时保存的变量
    var current = this.root
    var parent = this.root
    var isLeftChild = true

    // 2.开始查找节点
    while (current.key !== key) {
        parent = current
        if (key < current.key) {
            isLeftChild = true
            current = current.left
        } else {
            isLeftChild = false
            current = current.right
        }

        // 如果发现current已经指向null, 那么说明没有找到要删除的数据
        if (current === null) return false
    }

    // 3.删除的结点是叶结点
    if (current.left === null && current.right === null) {
        if (current == this.root) {
            this.root == null
        } else if (isLeftChild) {
            parent.left = null
        } else {
            parent.right = null
        }
    }

    // 4.删除有一个子节点的节点
    else if (current.right === null) {
        if (current == this.root) {
            this.root = current.left
        } else if (isLeftChild) {
            parent.left = current.left
        } else {
            parent.right = current.left
        }
    } else if (current.left === null) {
        if (current == this.root) {
            this.root = current.right
        } else if (isLeftChild) {
            parent.left = current.right
        } else {
            parent.right = current.right
        }
    }

    // 5.删除有两个节点的节点
    else {
        // 1.获取后继节点
        var successor = this.getSuccessor(current)

        // 2.判断是否是根节点
        if (current == this.root) {
            this.root = successor
        } else if (isLeftChild) {
            parent.left = successor
        } else {
            parent.right = successor
        }

        // 3.将删除节点的左子树赋值给successor
        successor.left = current.left
    }

    return true
}

// 找后继的方法
BinarySerachTree.prototype.getSuccessor = function (delNode) {
    // 1.使用变量保存临时的节点
    var successorParent = delNode
    var successor = delNode
    var current = delNode.right // 要从右子树开始找

    // 2.寻找节点
    while (current != null) {
        successorParent = successor
        successor = current
        current = current.left
    }

    // 3.如果是删除图中15的情况, 还需要如下代码
    if (successor != delNode.right) {
        successorParent.left = successor.right
        successor.right = delNode.right
    }
    
    return successor
}
```
### 删除节点的回顾
- 看到这里, 你就会发现删除节点相当棘手.
- 实际上, 因为它非常复杂, 一些程序员都尝试着避开删除操作.
  - 他们的做法是在Node类中添加一个boolean的字段, 比如名称为isDeleted.
  - 要删除一个节点时, 就将此字段设置为true.
  - 其他操作, 比如find()在查找之前先判断这个节点是不是标记为删除.
  - 这样相对比较简单, 每次删除节点不会改变原有的树结构.
  - 但是在二叉树的存储中, 还保留着那些本该已经被删除掉的节点.
- 上面的做法看起来很聪明, 其实是一种逃避.
  - 这样会造成很大空间的浪费, 特别是针对数据量较大的情况.
  - 而且, 作为程序员要学会通过这些复杂的操作, 锻炼自己的逻辑, 而不是避重就轻.
## 四.完整代码
### 二叉搜索树完整代码
```
// 创建BinarySearchTree
function BinarySerachTree() {
    // 创建节点构造函数
    function Node(key) {
        this.key = key
        this.left = null
        this.right = null
    }

    // 保存根的属性
    this.root = null

    // 二叉搜索树相关的操作方法
    // 向树中插入数据
    BinarySerachTree.prototype.insert = function (key) {
        // 1.根据key创建对应的node
        var newNode = new Node(key)

        // 2.判断根节点是否有值
        if (this.root === null) {
            this.root = newNode
        } else {
            this.insertNode(this.root, newNode)
        }
    }

    BinarySerachTree.prototype.insertNode = function (node, newNode) {
        if (newNode.key < node.key) { // 1.准备向左子树插入数据
            if (node.left === null) { // 1.1.node的左子树上没有内容
                node.left = newNode
            } else { // 1.2.node的左子树上已经有了内容
                this.insertNode(node.left, newNode)
            }
        } else { // 2.准备向右子树插入数据
            if (node.right === null) { // 2.1.node的右子树上没有内容
                node.right = newNode
            } else { // 2.2.node的右子树上有内容
                this.insertNode(node.right, newNode)
            }
        }
    }

    // 获取最大值和最小值
    BinarySerachTree.prototype.min = function () {
        var node = this.root
        while (node.left !== null) {
            node = node.left
        }
        return node.key
    }

    BinarySerachTree.prototype.max = function () {
        var node = this.root
        while (node.right !== null) {
            node = node.right
        }
        return node.key
    }

    // 搜搜特定的值
    /*
    BinarySerachTree.prototype.search = function (key) {
        return this.searchNode(this.root, key)
    }

    BinarySerachTree.prototype.searchNode = function (node, key) {
        // 1.如果传入的node为null那么, 那么就退出递归
        if (node === null) {
            return false
        }

        // 2.判断node节点的值和传入的key大小
        if (node.key > key) { // 2.1.传入的key较小, 向左边继续查找
            return this.searchNode(node.left, key)
        } else if (node.key < key) { // 2.2.传入的key较大, 向右边继续查找
            return this.searchNode(node.right, key)
        } else { // 2.3.相同, 说明找到了key
            return true
        }
    }
    */
    BinarySerachTree.prototype.search = function (key) {
        var node = this.root
        while (node !== null) {
            if (node.key > key) {
                node = node.left
            } else if (node.key < key) {
                node = node.right
            } else {
                return true
            }
        }
        return false
    }

    // 删除节点
    BinarySerachTree.prototype.remove = function (key) {
        // 1.获取当前的node
        var node = this.root
        var parent = null

        // 2.循环遍历node
        while (node) {
            if (node.key > key) {
                parent = node
                node = node.left
            } else if (node.key < key) {
                parent = node
                node = node.right
            } else {
                if (node.left == null && node.right == null) {

                }
            }
        }
    }

    BinarySerachTree.prototype.removeNode = function (node, key) {
        // 1.如果传入的node为null, 直接退出递归.
        if (node === null) return null

        // 2.判断key和对应node.key的大小
        if (node.key > key) {
            node.left = this.removeNode(node.left, key)

        }
    }

    // 删除结点
    BinarySerachTree.prototype.remove = function (key) {
        // 1.定义临时保存的变量
        var current = this.root
        var parent = this.root
        var isLeftChild = true

        // 2.开始查找节点
        while (current.key !== key) {
            parent = current
            if (key < current.key) {
                isLeftChild = true
                current = current.left
            } else {
                isLeftChild = false
                current = current.right
            }

            // 如果发现current已经指向null, 那么说明没有找到要删除的数据
            if (current === null) return false
        }

        // 3.删除的结点是叶结点
        if (current.left === null && current.right === null) {
            if (current == this.root) {
                this.root == null
            } else if (isLeftChild) {
                parent.left = null
            } else {
                parent.right = null
            }
        }

        // 4.删除有一个子节点的节点
        else if (current.right === null) {
            if (current == this.root) {
                this.root = current.left
            } else if (isLeftChild) {
                parent.left = current.left
            } else {
                parent.right = current.left
            }
        } else if (current.left === null) {
            if (current == this.root) {
                this.root = current.right
            } else if (isLeftChild) {
                parent.left = current.right
            } else {
                parent.right = current.right
            }
        }

        // 5.删除有两个节点的节点
        else {
            // 1.获取后继节点
            var successor = this.getSuccessor(current)

            // 2.判断是否是根节点
            if (current == this.root) {
                this.root = successor
            } else if (isLeftChild) {
                parent.left = successor
            } else {
                parent.right = successor
            }

            // 3.将删除节点的左子树赋值给successor
            successor.left = current.left
        }

        return true
    }

    // 找后继的方法
    BinarySerachTree.prototype.getSuccessor = function (delNode) {
        // 1.使用变量保存临时的节点
        var successorParent = delNode
        var successor = delNode
        var current = delNode.right // 要从右子树开始找

        // 2.寻找节点
        while (current != null) {
            successorParent = successor
            successor = current
            current = current.left
        }

        // 3.如果是删除图中15的情况, 还需要如下代码
        if (successor != delNode.right) {
            successorParent.left = successor.right
            successor.right = delNode.right
        }
    }

    // 遍历方法
    // 先序遍历
    BinarySerachTree.prototype.preOrderTraversal = function (handler) {
        this.preOrderTranversalNode(this.root, handler)
    }

    BinarySerachTree.prototype.preOrderTranversalNode = function (node, handler) {
        if (node !== null) {
            handler(node.key)
            this.preOrderTranversalNode(node.left, handler)
            this.preOrderTranversalNode(node.right, handler)
        }
    }

    // 中序遍历
    BinarySerachTree.prototype.inOrderTraversal = function (handler) {
        this.inOrderTraversalNode(this.root, handler)
    }

    BinarySerachTree.prototype.inOrderTraversalNode = function (node, handler) {
        if (node !== null) {
            this.inOrderTraversalNode(node.left, handler)
            handler(node.key)
            this.inOrderTraversalNode(node.right, handler)
        }
    }

    // 后续遍历
    BinarySerachTree.prototype.postOrderTraversal = function (handler) {
        this.postOrderTraversalNode(this.root, handler)
    }

    BinarySerachTree.prototype.postOrderTraversalNode = function (node, handler) {
        if (node !== null) {
            this.postOrderTraversalNode(node.left, handler)
            this.postOrderTraversalNode(node.right, handler)
            handler(node.key)
        }
    }
}
```
