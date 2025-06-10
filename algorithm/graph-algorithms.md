# 图算法
- [图结构](../data-structure/graph.md)

## 目录
### 一.图的遍历
- [遍历的方式](#遍历的方式)
- [广度优先搜索](#广度优先搜索)
- [深度优先搜索](#深度优先搜索)
### 二.其他算法
- 暂无

## 一.图的遍历
```
通过某种算法来遍历结构中每一个数据.
在需要的时候, 可以通过这种算法来访问某个顶点的数据以及它对应的边.
```
### 遍历的方式
- 图的遍历思想
  - 图的遍历算法的思想在于必须访问每个第一次访问的节点, 并且追踪有哪些顶点还没有被访问到.
- 有两种算法可以对图进行遍历
  - 广度优先搜索(Breadth-First Search, 简称BFS)
  - 深度优先搜索(Depth-First Search, 简称DFS)
  - 两种遍历算法, 都需要明确指定第一个被访问的顶点.
- 遍历的注意点:
  - 完全探索一个顶点要求我们查看该顶点的每一条边.
  - 对于每一个所连接的没有被访问过的顶点, 将其标注为被发现的, 并将其加进待访问顶点列表中.
  - 为了保证算法的效率: 每个顶点至多访问两次.
- 两种算法的思想:
  - BFS: 基于队列, 入队列的顶点先被探索.
  - DFS: 基于栈, 通过将顶点存入栈中, 顶点是沿着路径被探索的, 存在新的相邻顶点就去访问.
- 为了记录顶点是否被访问过, 我们使用三种颜色来反应它们的状态:(或者两种颜色也可以)
  - 白色: 表示该顶点还没有被访问.
  - 灰色: 表示该顶点被访问过, 但并未被探索过.
  - 黑色: 表示该顶点被访问过且被完全探索过.
- 初始化颜色代码:
  ```
  // 广度优先算法
  Graph.prototype.initializeColor = function () {
      var colors = []
      for (var i = 0; i < this.vertexes.length; i++) {
          colors[this.vertexes[i]] = "white"  // 初始化所有顶点颜色为 白色
      }
      return colors
  }
  ```
### 广度优先搜索
- 广度优先搜索算法的思路:
  - 广度优先算法会从指定的第一个顶点开始遍历图, 先访问其所有的相邻点, 就像一次访问图的一层.
  - 换句话说, 就是先宽后深的访问顶点
- 图解BFS
  ![image](https://github.com/user-attachments/assets/5724997c-b1d2-4d9d-894e-1aec23a92df6)
- 广度优先搜索的实现:
  - 创建一个队列Q.
  - 将v将入队列Q.
  - 如果Q非空, 执行下面的步骤:
    - 将v从Q中取出队列.
    - 将v标注为被发现的灰色.
    - 将v所有的未被访问过的邻接点(白色), 加入到队列中.
    - 将v标志为黑色.
- 广度优先搜索的代码:
  ```
  Graph.prototype.bfs = function (v, handler) {
      // 1.初始化颜色
      var color = this.initializeColor()
  
      // 2.创建队列
      var queue = new Queue()
  
      // 3.将传入的顶点放入队列中
      queue.enqueue(v)
  
      // 4.从队列中依次取出和放入数据
      while (!queue.isEmpty()) {
          // 4.1.从队列中取出数据
          var qv = queue.dequeue()
  
          // 4.2.获取qv相邻的所有顶点
          var qAdj = this.adjList.get(qv)
  
          // 4.3.将qv的颜色设置成灰色
          color[qv] = "gray"
  
          // 4.4.将qAdj的所有顶点依次压入队列中
          for (var i = 0; i < qAdj.length; i++) {
              var a = qAdj[i]
              if (color[a] === "white") {
                  color[a] = "gray"
                  queue.enqueue(a)
              }
          }
  
          // 4.5.因为qv已经探测完毕, 将qv设置成黑色
          color[qv] = "black"
  
          // 4.6.处理qv
          if (handler) {
              handler(qv)
          }
      }
  }
  ```
- 测试代码
  ```
  // 调用广度优先算法
  var result = ""
  graph.bfs(graph.vertexes[0], function (v) {
      result += v + " "
  })
  alert(result) // A B C D E F G H I 
  ```
### 深度优先搜索
- 深度优先搜索的思路:
  - 深度优先搜索算法将会从第一个指定的顶点开始遍历图, 沿着路径直到这条路径最后被访问了.(即访问到一个没有后续顶点的顶点, 树中的叶子结点)
  - 接着原路回退并探索下一条路径.
- 图解DFS:
  ![image](https://github.com/user-attachments/assets/9055236f-6396-4fb8-9969-e4e96bff7904)
- 深度优先搜索算法的实现:
  - 广度优先搜索算法我们使用的是队列, 这里可以使用栈完成, 也可以使用递归.
  - 方便代码书写, 我们还是使用递归(递归本质上就是函数栈的调用)
- 深度优先搜索算法的实现:
  ```
  // 深度优先搜索
  Graph.prototype.dfs = function (handler) {
      // 1.初始化颜色
      var color = this.initializeColor()
  
      // 2.遍历所有的顶点, 开始访问
      for (var i = 0; i < this.vertexes.length; i++) {
          // 对还没有被访问的顶点进行dfs
          if (color[this.vertexes[i]] === "white") {
              this.dfsVisit(this.vertexes[i], color, handler)
          }
      }
  }
  
  // dfs的递归调用方法
  // 传入遍历的第一个顶点、颜色数组、对顶点进行操作的函数
  Graph.prototype.dfsVisit = function (u, color, handler) {
      // 1.将u的颜色设置为灰色
      color[u] = "gray"
  
      // 2.处理u顶点
      if (handler) {
          handler(u)
      }
  
      // 3.u的所有邻接顶点的访问
      var uAdj = this.adjList.get(u)
      // 判断u的每一个相邻顶点是否访问过，若没访问过则执行dfs遍历
      for (var i = 0; i < uAdj.length; i++) {
          var w = uAdj[i]
          if (color[w] === "white") {
              this.dfsVisit(w, color, handler)
          }
      }
  
      // 4.将u设置为黑色
      color[u] = "black"
  }
  ```
- 代码图解  
  ![image](https://github.com/user-attachments/assets/d758322c-1655-429f-a1a5-dae41f623a13)
- DFS就是沿着一条路径走到尽头(分岔路的选择按照邻接表返回结果的顺序)，然后回退到最近的分岔路，走下一条路径，以此类推。
## 二.其他算法
- 暂无
