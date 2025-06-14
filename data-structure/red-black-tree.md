# 红黑树
`用作数据结构中, 树结构的补充说明.`

## 目录
### 一.二叉搜索树的缺陷
- [二叉搜索树的缺陷](#二叉搜索树的缺陷)
- [树的平衡性](#树的平衡性)
### 二.认识红黑树
- [红黑树的规则](#红黑树的规则)
- [红黑树的相对平衡](#红黑树的相对平衡)
### 三.红黑树的变换
- [变色](#变色)
- [旋转](#旋转)
### 四.插入操作的情况分析
- [插入操作](#插入操作)
### 五.红黑树的案例
- [红黑树的案例](#红黑树的案例)

## 一.二叉搜索树的缺陷
### 二叉搜索树的缺陷
- 二叉搜索树的优势
  - 可以快速地找到给定关键字的数据项, 并且可以快速地插入和删除数据项.
- 但是二叉搜索树有一个问题:
  - 如果插入的数据是**有序的数据**, 如下面的情况
  - 有一棵初始化为9,8,12的二叉树
  - 插入7,6,5,4,3  
    <img width="362" alt="image" src="https://github.com/user-attachments/assets/27cfb4e1-4946-4b5a-9690-978fb764c7f5" />
- 非平衡树
  - 比较好的二叉搜索树数据应该是**左右分布均匀**的.
  - 但是插入连续数据后, 数据分布不均匀, 这种树称为非平衡树.
  - 对于一棵平衡二叉树来说, 插入/查找等操作的效率是O(logN).
  - 对于一棵非平衡二叉树, 相当于编写了一个链表, 查找效率变成了O(N).
### 树的平衡性
- 为了能以较快的时间O(logN)来操作一棵树, 我们需要保证树总是平衡的.
  - 至少大部分是平衡的, 那么时间复杂度也是接近O(logN).
  - 即树中每个节点左边的子孙节点个数, 应该尽可能等于右边的子孙节点个数.
- 常见的平衡树:
  - AVL树:
    - AVL树是最早的一种平衡树, 它有办法保持树的平衡(每个节点多存储了一个额外的数据).
    - 因为AVL树是平衡的, 所以时间复杂度也是O(logN).
    - 但是, 每次插入/删除操作相对于红黑树效率都不高, 所以整体效率不如红黑树.
  - 红黑树:
    - 红黑树也通过一些特性来保持树的平衡.
    - 因为是平衡树, 所以时间复杂度也是O(logN).
    - 另外插入/删除等操作, 红黑树的性能要优于AVL树, 所以现在平衡树的应用基本都是红黑树.
## 二.认识红黑树
`红黑树是数据结构中一个难点中的难点.`
### 红黑树的规则
- 红黑树, 除了符合二叉搜索树的基本规则外, 还有以下特性:
  - 1.节点是红色或黑色.
  - 2.根节点是黑色.
  - 3.每个叶子结点都是黑色的空节点(NIL节点).
  - 4.每个红色节点的两个字节点都是黑色.(从每个叶子到根的所有路径上不能有两个连续的红色节点)
  - 5.从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点.  
    <img width="533" alt="image" src="https://github.com/user-attachments/assets/2fe31558-7439-4e25-bc95-575284bbf691" />
### 红黑树的相对平衡
- 上面的约束, 确保了红黑树的关键特性:
  - 从根到叶子的**最长**可能路径, 不会超过**最短**可能路径的两倍长.
  - 结果就是这个树基本是平衡的.
  - 虽然没有做到绝对的平衡, 但是可以保证在最坏的情况下, 依然是高效的.
- 为什么可以做到 最长路径不超过最短路径的两倍 呢？
  - 性质4决定了路径不能有两个相连的红色节点.
  - 最短的可能路径都是黑色节点.
  - 最长的可能路径是红色和黑色交替.
  - 性质5决定了所有路径都有相同数目的黑色节点.
  - 这就表明了没有路径能超过任何其他路径的两倍长.(两个黑色节点中间最多有一个红色节点)
## 三.红黑树的变换
### 变色
- 插入一个新节点时, 有可能树不再平衡, 可以通过三种方式的变换, 让树保持平衡.
  - 变色 - 左旋转 - 右旋转
- 变色
  - 为了重新符合红黑树的规则, 尝试把红色节点变为黑色, 或者把黑色节点变为红色.
- 首先, 需要知道插入的新节点通常都是红色节点.
  - 因为在插入节点为红色的时候, 有可能这次插入是不违反红黑树任何规则的.
  - 而插入黑色节点, 必然会导致有一条路径上多了黑色节点, 这是很难调整的.
  - 红色节点可能导致出现**红红相连**的情况, 但是这种情况可以通过**颜色调换和旋转**来调整.  
    <img width="374" alt="image" src="https://github.com/user-attachments/assets/f37c9362-70cd-4bdf-aba8-97ed898a2990" />
### 旋转
- 左旋转
  - **逆时针**旋转红黑树的两个节点, 使得父节点被自己的右孩子取代, 而自己成为自己的左孩子.  
    <img width="519" alt="image" src="https://github.com/user-attachments/assets/23fcd2b6-5db5-45af-9cdc-3fa2dc71c7ed" />
- 右旋转
  - **顺时针**旋转红黑树的两个节点, 使得父节点被自己的左孩子取代, 而自己成为自己的右孩子.  
  ![image](https://github.com/user-attachments/assets/1d542792-ec5a-4e1c-9771-66e24d5e912f)
- 上面两个旋转都是子节点Y取代了父节点X, 如果它们有子树是否会影响旋转?
## 四.插入操作的情况分析
### 插入操作
- 插入的情况
  - 设要插入的节点为N, 其父节点为P.
  - 其祖父节点为G, 其父亲的兄弟节点为U(即P和U是同一个节点的子节点).  
    <img width="227" alt="image" src="https://github.com/user-attachments/assets/82537f35-7394-4432-976a-6075d6b8c4b7" />  
- 情况一
  - 新节点N位于树的根上, 没有父节点.
  - 这种情况下, 我们直接将红色变换成黑色即可, 满足性质2.
- 情况二
  - 新节点的父节点P是黑色.
  - 性质4没有失效(新节点也是红色的), 性质5也没有问题.
  - 尽管新节点N有两个黑色的叶子结点nil, 但是新节点N是红色的, 所以通过它的路径中的黑色节点的个数依然相同, 满足性质5.
- 情况三
  - P为红色, U也是红色.
  - 父红叔红 祖黑 -> 父黑叔黑 祖红  
    <img width="598" alt="image" src="https://github.com/user-attachments/assets/4b038bab-cb97-452f-ac2c-e7c1feca5d0a" />  
  - 操作方案:
    - 将P和U变换为黑色, 并且将G变换为红色.
    - 现在新节点N有了一个黑色父节点P, 所以每条路径上黑色节点的数目没有改变.
    - 而从更高的路径上, 必然都会经过G节点, 所以那些路径的黑色节点数目也是不变的. 符合性质5.
  - 可能出现的问题:
    - N的祖父节点G的父节点也可能是红色, 这就违反了性质3, 可以递归地调整颜色.
    - 如果递归调整颜色到了根节点, 使得根节点为红色, 此时可以将根节点变换为黑色, 即前两层树的节点都为黑色, 或者考虑旋转的可能.
- 情况四
  - N的叔叔U是黑色节点, 且N是左孩子.
  - 父红 叔黑祖黑 N是左孩子 -> 父黑 -> 祖红 -> 右旋转
    <img width="635" alt="image" src="https://github.com/user-attachments/assets/43dc11b9-9216-40a1-8d7a-ac867b6c2e23" />  
  - 操作方案:
    - 对祖父节点G进行一次右旋转.
    - 在旋转的树中, 以前的父节点P现在是以前的祖父节点G的父节点.
    - 交换以前的父节点P和祖父节点G的颜色.(P为黑色, G为红色 ———— G原来一定是黑色, 因为P是红色)
    - B节点向右平移, 成为G节点的左子节点.
- 情况五
  - N的叔叔U是黑色节点, 且N是右孩子.
  - 父红 叔黑祖黑, N是右孩子.
  - -> 以P为根, 左旋转
    - 将P作为新插入的红色节点考虑即可
  - 自己变成黑色
  - 祖变成红色
  - 以祖为根, 进行右旋转  
    <img width="590" alt="image" src="https://github.com/user-attachments/assets/c7a17b02-cc50-4442-8dcf-6bb8c3b8bfcf" />  
  - 操作方案:
    - 对P节点进行一次左旋转, 形成情况四的结果.
    - 对祖父节点G进行一次右旋转, 并且改变颜色即可.
## 五.红黑树的案例
### 红黑树的案例  
![image](https://github.com/user-attachments/assets/dd65da37-8be7-4f0e-92a3-904249aac52b)  
![image](https://github.com/user-attachments/assets/8eabc2ac-d08a-4c25-a499-1bd450c6784b)  
![image](https://github.com/user-attachments/assets/87a679f8-3ae5-422f-b916-c4980ac65179)  
![image](https://github.com/user-attachments/assets/5f875ae5-560e-4f2c-a090-73a87579d90c)  
![image](https://github.com/user-attachments/assets/7619ea8b-f57f-4911-9cea-fd3cb77fc9b5)  
![image](https://github.com/user-attachments/assets/ac6034c7-c3e8-49ba-8468-9b4124890dde)  
![image](https://github.com/user-attachments/assets/ddf4db54-f34d-4b1b-a0c8-67c531ff7d42)  
![image](https://github.com/user-attachments/assets/24706c6f-c7be-41c9-8a8c-f746e976092f)  
![image](https://github.com/user-attachments/assets/d1ff48a3-6122-4d1a-a91c-e52227787bfb)







