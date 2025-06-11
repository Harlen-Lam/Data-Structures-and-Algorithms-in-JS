# 高级排序

## 目录
### 一.希尔排序
- [希尔排序的介绍](#希尔排序的介绍)
- [希尔排序的实现](#希尔排序的实现)
- [希尔排序的效率](#希尔排序的效率)
### 二.快速排序
- [快速排序的介绍](#快速排序的介绍)
- [快速排序的枢纽](#快速排序的枢纽)
- [快速排序的实现](#快速排序的实现)
- [快速排序的效率](#快速排序的效率)
### 三.封装排序算法的完整代码
- [排序算法的完整代码](#排序算法的完整代码)

## 一.希尔排序
`希尔排序是插入排序的一种高效的改进版, 并且效率比插入排序要更快.`
### 希尔排序的介绍
- 回顾插入排序:
  - 由于希尔排序是基于插入排序, 所以有必须回顾一下前面的插入排序.
  - 我们设想一下, 在插入排序执行到一半的时候, 标记符左边这部分数据项都是排好序的, 而标识符右边的数据项是没有排序的.
  - 这个时候, 取出指向的那个数据项, 把它存储在一个临时变量中, 接着, 从刚刚移除的位置左边第一个单元开始, 每次把有序的数据项向右移动一个单元, 直到存储在临时变量中的数据项可以成功插入.
- 插入排序的问题:
  - 假设一个很小的数据项在很靠近右端的位置上, 这里本来应该是较大的数据项的位置.
  - 把这个小数据项移动到左边的正确位置, 所有的中间数据项都必须向右移动一位.
  - 如果每个步骤对数据项都进行N次复制, 平均下来是移动N/2, N个元素就是 N*N/2 = N²/2.
  - 所以我们通常认为插入排序的效率是O(N²)
  - 如果有某种方式, 不需要一个个移动所有中间的数据项, 就能把较小的数据项移动到左边, 那么这个算法的执行效率就会有很大的改进.
- 希尔排序的做法:
  - 比如下面的数字, 81, 94, 11, 96, 12, 35, 17, 95, 28, 58, 41, 75, 15.
  - 我们先以间隔为5, 分别进行排序. (81, 35, 41), (94, 17, 75), (11, 95, 15), (96, 28), (12, 58)
  - 35, 17, 11, 28, 12, 41, 75, 15, 96, 58, 81, 94, 95
  - 排序后的新序列, 一定可以让数字离自己的正确位置更近一步.
  - 我们再让间隔为3, 进行排序. (35, 28, 75, 58, 95), (17, 12, 15, 81), (11, 41, 96, 94)
  - 28, 12, 11, 35, 15, 41, 58, 17, 94, 75, 81, 96, 95
  - 排序后的新序列, 一定可以让数字离自己的正确位置又近了一步.
  - 最后, 我们让间隔为1, 也就是正确的插入排序. 这个时候数字都离自己的位置更近, 那么需要复制的次数一定会减少很多.  
    ![image](https://github.com/user-attachments/assets/912d9c82-98f8-4785-bd71-e41eab829f2e)
- 选择合适的增量:
  - 在希尔排序的原稿中, 他建议的初始间距是N / 2, 简单的把每趟排序分成两半.
  - 也就是说, 对于N = 100的数组, 增量间隔序列为: 50, 25, 12, 6, 3, 1.
  - 这个方法的好处是不需要在开始排序前为找合适的增量而进行任何的计算.
  - 我们先按照这个增量来实现我们的代码.
### 希尔排序的实现
- 希尔排序的代码实现
  ```
  ArrayList.prototype.shellSort = function () {
      // 1.获取数组的长度
      var length = this.array.length
  
      // 2.根据长度计算增量
      var gap = Math.floor(length / 2)
  
      // 3.增量不断变小, 大于0就继续排序
      while (gap > 0) {
          // 4.实现插入排序
          // 依旧默认初始位置为是已经排序好的,
          // 但此时的初始位置根据gap的值分为多个序列起点
          // 例如gap为50, 则存在50个序列, 前50个元素即为相应序列的起点
          for (var i = gap; i < length; i++) {
              // 4.1.保存临时变量
              var j = i
              var temp = this.array[i]
  
              // 4.2.插入排序的内层循环
              // 内部循环是在每一个gap的序列中从后往前插入
              while (j > gap - 1 && this.array[j - gap] > temp) {
                  this.array[j] = this.array[j - gap]
                  j -= gap
              }
  
              // 4.3.将选出的j位置设置为temp
              this.array[j] = temp
          }
        
          // 5.重新计算新的间隔, 并再次进入循环直至gap不大于0
          gap = Math.floor(gap / 2)
      }
  }
  ```
- 测试代码
  ```
  // 测试希尔排序
  list.shellSort()
  alert(list)
  ```
### 希尔排序的效率
- 希尔排序的效率
  - 希尔排序的效率跟增量是有关系的.
  - 但是, 它的效率证明非常困难, 甚至某些增量的效率到目前依然没有被证明出来.
  - 但是经过统计, 希尔排序使用原始增量, 最坏的情况下时间复杂度为O(N²), 通常情况下都要好于O(N²)
- Hibbard 增量序列
  - 增量的算法为2^k - 1. 也就是为1 3 5 7...等等.
  - 这种增量的最坏复杂度为O(N^3/2), 猜想的平均复杂度为O(N^5/4), 目前尚未被证明.
- Sedgewick增量序列
  - {1, 5, 19, 41, 109, … }, 该序列中的项或者是94^i - 9*2^i + 1或者是4^i - 32^i + 1
  - 这种增量的最坏复杂度为O(N^4/3), 平均复杂度为O(N^7/6), 但是均未被证明.
- 总之, 我们使用希尔排序大多数情况下效率都高于简单排序, 甚至在合适的增量和N的情况下, 还好于快速排序.
## 二.快速排序
`快速排序几乎可以说是目前所有排序算法中, 最快的一种排序算法.`
### 快速排序的介绍
- 快速排序的重要性:
  - 如果有一天你面试的时候, 让你写一个排序算法, 你可以洋洋洒洒的写出多个排序算法, 但是如果其中没有快速排序, 那么证明你对排序算法也只是浅尝辄止, 并没有深入的研究过.
  - 因为快速排序可以说是排序算法中最常见的, 无论是C++的STL中, 还是Java的SDK中其实都能找到它的影子.
  - 快速排序也被列为20世纪十大算法之一.
- 快速排序是什么?
  - 希尔排序相当于插入排序的升级版, 快速排序其实是我们学习过的最慢的**冒泡排序**的升级版.
  - 我们知道冒泡排序需要经过很多次交换, 才能在一次循环中, 将最大值放在正确的位置.
  - 而快速排序可以在一次循环中(其实是递归调用)找出某个元素的正确位置, 并且该元素之后不需要任何移动.
- 快速排序的思想:
  - 快速排序最重要的思想是**分而治之**.
  - 比如我们下面有这样一组数字需要排序:
    - 第一步: 从其中选出了65. (其实可以是选出任意的数字, 我们以65举个栗子)
    - 第二步: 我们通过算法: 将所有小于65的数字放在65的左边, 将所有大于65的数字放在65的右边.
    - 第三步: 递归的处理左边的数据.(比如你选择31来处理左侧), 递归的处理右边的数据.(比如选择75来处理右侧, 当然选择81可能更合适)
    - 以此类推, 递归处理
    - 最终: 排序完成
  - 和冒泡排序不同的是什么呢?
    - 我们选择的65可以一次性将它放在最正确的位置, 之后不需要任何移动.
    - 需要从开始位置两个两个比较, 如果第一个就是最大值, 它需要一直向后移动, 直到走到最后.
    - 也就是即使已经找到了最大值, 也需要不断继续移动最大值. 而插入排序对数字的定位是一次性的(即插入排序只需要交换一次即可).
    ![image](https://github.com/user-attachments/assets/6707f6da-ddd6-42a2-947b-9e6bc21723c9)
### 快速排序的枢纽
- 在快速排序中有一个很重要的步骤就是**选取枢纽**(pivot也称为主元).
- 如何选择最合适的枢纽呢?
  - 一种方案是直接选择第一个元素作为枢纽.
    - 但第一个元素作为枢纽在某些情况下, 效率并不是特别高.
    ![image](https://github.com/user-attachments/assets/723ae72b-d22e-4951-9d5b-fe750079f255)
  - 另一种方案是使用随机数:
    - 随机取 pivot？但是随机函数本身就是一个耗性能的操作.
  - 另一种比较优秀的解决方案: 取头、中、尾的中位数
    - 例如 8、12、3的中位数就是8
- 枢纽选择的代码实现:
  ```
  // 选择枢纽
  // 采用第三方法, 找出头中尾的中位数
  ArrayList.prototype.median = function (left, right) {
      // 1.求出中间的位置
      var center = Math.floor((left + right) / 2)
  
      // 2.判断并且进行交换
      // 假设为 12, 8, 3
      if (this.array[left] > this.array[center]) {
          this.swap(left, center)  // 8, 12, 3
      }
      if (this.array[left] > this.array[right]) {
          this.swap(left, right)  // 3, 12, 8
      }
      if (this.array[center] > this.array[right]) {
          this.swap(center, right)  // 3, 8, 12
      }
  
      // 3.巧妙的操作: 将center移动到right - 1的位置.
      // 这样操作的目的是在之后交换的时候, pivot的值不需要移动来移动去.
      // 可以在最后选定位置后, 直接再交换到正确的位置即可(也是最终的位置).
      this.swap(center, right - 1)
  
      // 4.返回pivot
      return this.array[right - 1]
  }
  ```
- 测试代码
  ```
  // 测试中位数选取
  // 原来的数组: 3,6,4,2,11,10,5
  var pivot = list.median(1, 6) // left:6 right:5 center:2
  alert(pivot) // pivot:5
  alert(list) // 3,2,4,10,11,5,6
  ```
### 快速排序的实现
- 快速排序的代码实现
  ```
  // 快速排序实现
  ArrayList.prototype.quickSort = function () {
      this.quickSortRec(0, this.array.length - 1)
  }
  
  ArrayList.prototype.quickSortRec = function (left, right) {
      // 0.递归结束条件
      if (left >= right) return
  
      // 1.获取枢纽
      var pivot = this.median(left, right)
  
      // 2.开始进行交换
      // 2.1.记录左边开始位置和右边开始位置
      var i = left
      var j = right - 1
      // 2.2.循环查找位置
      // 如果i不小于j, 说明左右两边已经排序完毕, i = j + 1, 且array[i] > array[j]
      // 即此时的i、j位于小于和大于的交界处, j小于pivot, i大于pivot
      // 且此处可以避免left = 0, right = 1出现的pivot下标为0, --j = -1 的报错(197行)
      while (i < j) {
          // 找到左边第一个大于pivot的元素
          while (this.array[++i] < pivot) { }
          // 找到右边第一个小于pivot的元素
          while (this.array[--j] > pivot) { }
          if (i < j) {
              // 2.3.交换两个数值
              this.swap(i, j)
          }
      }
  
      // 3.将枢纽放在正确的位置
      // i正好是大于序列(右边序列)的第一个位置, 也就是pivot应该放的位置
      this.swap(i, right - 1)
  
      // 4.递归调用左边
      this.quickSortRec(left, i - 1)
      this.quickSortRec(i + 1, right)
  }
  ```
- 测试代码
  ```
  // 测试快速排序
  list.quickSort()
  alert(list)
  ```
### 快速排序的效率
- 快速排序的最坏情况效率
  - 什么情况下会有最坏的效率呢? 就是每次选择的枢纽都是最左边或者最后边的.
  - 那么效率等同于冒泡排序.
  - 而我们的例子可能有最坏的情况吗? 是不可能的. 因为我们是选择三个值的中位值.
- 快速排序的平均效率:
  - 快速排序的平均效率是O(N * logN).
  - 虽然其他某些算法的效率也可以达到O(N * logN), 但是快速排序是最好的.
## 三.封装排序算法的完整代码
### 排序算法的完整代码
```
// 封装ArrayList
function ArrayList() {
    this.array = []

    ArrayList.prototype.insert = function (item) {
        this.array.push(item)
    }

    ArrayList.prototype.toString = function () {
        return this.array.join()
    }

    ArrayList.prototype.bubbleSort = function () {
        // 1.获取数组的长度
        var length = this.array.length

        // 2.反向循环, 因此次数越来越少
        for (var i = length - 1; i >= 0; i--) {
            // 3.根据i的次数, 比较循环到i位置
            for (var j = 0; j < i; j++) {
                // 4.如果j位置比j+1位置的数据大, 那么就交换
                if (this.array[j] > this.array[j+1]) {
                    // 交换
                    this.swap(j, j+1)
                }
            }
        }
    }

    ArrayList.prototype.selectionSort = function () {
        // 1.获取数组的长度
        var length = this.array.length

        // 2.外层循环: 从0位置开始取出数据, 直到length-2位置
        for (var i = 0; i < length - 1; i++) {
            // 3.内层循环: 从i+1位置开始, 和后面的内容比较
            var min = i
            for (var j = min + 1; j < length; j++) {
                // 4.如果i位置的数据大于j位置的数据, 记录最小的位置
                if (this.array[min] > this.array[j]) {
                    min = j
                }
            }
            this.swap(min, i)
        }
    }

    ArrayList.prototype.insertionSort = function () {
        // 1.获取数组的长度
        var length = this.array.length

        // 2.外层循环: 外层循环是从1位置开始, 依次遍历到最后
        for (var i = 1; i < length; i++) {
            // 3.记录选出的元素, 放在变量temp中
            var j = i
            var temp = this.array[i]

            // 4.内层循环: 内层循环不确定循环的次数, 最好使用while循环
            while (j > 0 && this.array[j-1] > temp) {
                this.array[j] = this.array[j-1]
                j--
            }

            // 5.将选出的j位置, 放入temp元素
            this.array[j] = temp
        }
    }

    ArrayList.prototype.shellSort = function () {
        // 1.获取数组的长度
        var length = this.array.length

        // 2.根据长度计算增量
        var gap = Math.floor(length / 2)

        // 3.增量不断变量小, 大于0就继续排序
        while (gap > 0) {
            // 4.实现插入排序
            for (var i = gap; i < length; i++) {
                // 4.1.保存临时变量
                var j = i
                var temp = this.array[i]

                // 4.2.插入排序的内存循环
                while (j > gap - 1 && this.array[j - gap] > temp) {
                    this.array[j] = this.array[j - gap]
                    j -= gap
                }

                // 4.3.将选出的j位置设置为temp
                this.array[j] = temp
            }

            // 5.重新计算新的间隔
            gap = Math.floor(gap / 2)
        }
    }

    ArrayList.prototype.swap = function (m, n) {
        var temp = this.array[m]
        this.array[m] = this.array[n]
        this.array[n] = temp
    }

    // 选择枢纽
    ArrayList.prototype.median = function (left, right) {
        // 1.求出中间的位置
        var center = Math.floor((left + right) / 2)

        // 2.判断并且进行交换
        if (this.array[left] > this.array[center]) {
            this.swap(left, center)
        }
        if (this.array[left] > this.array[right]) {
            this.swap(left, right)
        }
        if (this.array[center] > this.array[right]) {
            this.swap(center, right)
        }

        // 3.巧妙的操作: 将center移动到right - 1的位置.
        this.swap(center, right - 1)

        // 4.返回pivot
        return this.array[right - 1]
    }

    // 快速排序实现
    ArrayList.prototype.quickSort = function () {
        this.quickSortRec(0, this.array.length - 1)
    }

    ArrayList.prototype.quickSortRec = function (left, right) {
        // 0.递归结束条件
        if (left >= right) return

        // 1.获取枢纽
        var pivot = this.median(left, right)

        // 2.开始进行交换
        var i = left
        var j = right - 1
        while (i < j) {
            while (this.array[++i] < pivot) { }
            while (this.array[--j] > pivot) { }
            if (i < j) {
                this.swap(i, j)
            }
        }

        // 3.将枢纽放在正确的位置
        this.swap(i, right - 1)

        // 4.递归调用左边
        this.quickSortRec(left, i - 1)
        this.quickSortRec(i + 1, right)
    }
}
```
