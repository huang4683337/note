堆又叫优先队列

## 堆的基础知识

![1617884432218](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%A0%86%E7%9A%84%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.png)



**完全二叉树和堆的关系很亲密**



### 完全二叉树在内存中的存储

+ 完全二叉树 是思维逻辑结构（二维）
+ 实际上在内存中存储结构为数组（一维）

![1617884544749](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E9%80%BB%E8%BE%91%E7%BB%93%E6%9E%84%E3%80%81%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84.png)





### 堆

![1617884848437](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%A4%A7%E9%A1%B6%E5%A0%86%E3%80%81%E5%B0%8F%E9%A1%B6%E5%A0%86.png)

+ 大顶堆：任意三元节组中，父节点比子节点大。（最大值在根节点）
+ 小顶堆：任意三元节组中，父节点比子节点小。（最小值在根节点）



**第二大值**：根的左子节点、或者根的右子节点（第二层）

**第三大值**：大小的比较只能是父子，不能是兄弟（第二层或者第三层）

第一大值在：第一层

第n大值：在 2 到 n 曾





![1617885414948](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%B0%BE%E9%83%A8%E6%8F%92%E5%85%A5%E8%B0%83%E6%95%B4.png)

+ 先向尾部追加一个元素
+ **向上调整（SIFT-UP）**这个元素和父节点去比，大于父节点，两两交换，直至小于父节点



![1617886002836](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%A4%B4%E9%83%A8%E5%BC%B9%E5%87%BA%E8%B0%83%E6%95%B4.png)



+ 弹出堆顶的最大值（数组中的组大值）
+ **因为堆是一个完全二叉树**（需要占用一个连续的存储空间），所以弹出一个之后，数组的有效值长度要减去1，将最后一个元素（有效值，但是不在有效存储空间上）放入数组最前面
+ 从根节点开始，任意三元组之间**向下调整（SIFT-DOWN）**将最大的放在父节点位置



![1617886471132](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E5%A0%86%E6%8E%92%E5%BA%8F.png)



**从小到大排序**

+ 建立一个大顶堆

+ 从首位开始弹出 

  + 对应堆的数组首位为空
  + 对应堆的数组长度减去1，原来数组最后一位不是有效位置
  + 位于原来数组的最后一位是有效值，所以需要将最后一位的有效值放入第一位
  + 对处理后的堆进行排序，保证任意三元组之间父节点值大于子节点值

+ 利用以上特性将弹出的值, 放入数组最后一位(也就是无效位置)

  将无效位置的有效值放入数组第一个位置, 以此类推

+  最大值和数组最后一个互换，次大值和数组倒数第二个互换。最终结果就是排序结果





![1617887228650](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-%E4%BC%98%E5%85%88%E9%98%9F%E5%88%97.png)

**优先队列通常情况下是用堆来实现的**

因为堆是从数组末尾进入元素, 从数组的头部出元素, 在数组层面和队列相似

为甚么叫优先队列: 每次出堆的都是优先级最高的(最大后者最小)



**堆是优先队列的实现方式**





### 堆的代码实现

+ 堆（完全二叉树）找到父节点：
  + 左子节点的索引 = 2父节点索引+1
  + 右子节点的索引 = 2父节点索引+2
  + （子节点索引 - 1） /  2  向下取整就是父节点索引
+ 堆（完全二叉树）确认是否还有子节点
  +  2父节点索引+1 < = 数组长度 - 1





## 堆适合处理什么问题

**堆适合维护**：集和最值





## 经典面试题：堆的基础应用

### 1、[剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

```js
1 - 有个备选集和，存储的值是最终答案
2 - 新的元素和备选集和的最大值去比较，小于最大值时，替换备选集合中的最大值

3 - 因为要和集和的最大值比较，所以需要一个大顶堆
```

```js

```



### 2、[1046. 最后一块石头的重量](https://leetcode-cn.com/problems/last-stone-weight/)

```js
// 集和中求最值
1 - 取出最大值、次大值
2 - 处理结果不相等，处理结果压入堆中
3 - 以此类推，直到剩下最后一个
// 因为最大值，所以大顶堆
```

![1618149423220](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E5%A0%86-1046.%20%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8F.png)

```js
// 使用了 leetcode 中自带的优先队列

var lastStoneWeight = function (stones) {

    // new 一个大顶堆
    const maxHeap = new MaxPriorityQueue();

    // 将所有的石头重量放入大顶堆中
    for (let i = 0; i < stones.length; i++) {
        maxHeap.enqueue('x', stones[i]);
    }

    
    while (maxHeap.size() > 1) {
        // 最重的石头
        const a = maxHeap.dequeue()['priority'];
        
        // 第二重石头
        const b = maxHeap.dequeue()['priority'];
        
        // 两个石头不相等，第二重的石头生成新的石头，放入大顶堆
        if (a > b) {
            maxHeap.enqueue('x', a - b);
        }
    }

    // 不为空，返回最后一块石头的重量
    return maxHeap.isEmpty() ? 0 : maxHeap.dequeue()['priority'];
};
```



### 3、[703. 数据流中的第 K 大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

```js
1 - 有个备选集和，存储的值是最终答案,前k大元素
2 - 新元素比备选集和中的最小值大，替换备选集和最小值
// 因为要和备选集和最小值比较，所以是个小顶堆
```



### 4、[215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

```js
// 保留几个大的，谁小谁出去
// 需要小顶堆
```



### 5、[373. 查找和最小的K对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)

```js
// 保留几个小的，谁大谁出去
// 需要大顶堆
```





## 经典面试题：进阶

### 1、[355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)

### 2、[692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

### 3、[面试题 17.20. 连续中值](https://leetcode-cn.com/problems/continuous-median-lcci/)

```js
1 - 一个大顶堆（前有序数组）
2 - 一个小顶堆（后有序数组）
3 - 数组为长度奇数时，保证大顶堆(前有序数组)中多一位
4 - 数组长度为偶数时，大小顶堆最值求和
```

```js
var MedianFinder = function () {
    this.item = []
};
MedianFinder.prototype.addNum = function (num) {
    this.item.push(num);
};
MedianFinder.prototype.findMedian = function () {
    // 排序
    this.item.sort((a, b) => a - b);
    // 中位数
    if (this.item.length % 2 === 0) {
        return (this.item[this.item.length / 2] + this.item[this.item.length / 2 - 1]) / 2;
    } else {
        return this.item[(this.item.length - 1) / 2];
    }
};
```



### 4、[295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

```js
// 同 连续中值一题
```



### 5、[1801. 积压订单中的订单总数](https://leetcode-cn.com/problems/number-of-orders-in-the-backlog/)

```js

```





## 智力发散题

### 1、[264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

```js
同 [面试题 17.09. 第 k 个数](https://leetcode-cn.com/problems/get-kth-magic-number-lcci/)
```



### 2、[313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)

```js
1 - 创建一个结果集 res = []
2 - 创建一个指针集，存质数列表中质数对应指针 p = new Array(质数集长度)
3 - 当前一轮中：结果集中指针对应的值*质数集中对应的质数
4 - 当前一轮中：求出最小数
5 - 当前一轮中：求出值等于最小值，对应的指针后移一位
```

```js
var nthSuperUglyNumber = function (n, primes) {
    // 结果集
    const res = [1];

    // 指针集,初始值为0，长度为质数列表长度
    const p = new Array(primes.length).fill(0);

    // map: 当前轮根据质数列表计算的结果
    // min：当前按轮最小值
    let map, min;
    for (let i = 1; i < n; i++) {
        map = primes.map((el, i) => {
            return res[p[i]] * el;
        });

        min = Math.min.apply(null, map);

        // 如果当前轮计算结果有和最小值相等的，指针+1
        map.forEach((el, i) => {
            if (el === min) p[i]++;
        });

        // 当前轮结果添加到结果集
        res.push(min);
    }

    return res[res.length - 1];
};
```





### 3、[1753. 移除石子的最大得分](https://leetcode-cn.com/problems/maximum-score-from-removing-stones/)

```js

```





## 彩蛋题 - 递归

### [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

```js

```



![1618238773414](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1618238773414.png)

