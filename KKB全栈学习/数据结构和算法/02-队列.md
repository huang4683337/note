## 队列应用

### cup 的超线程技术

真双核cpu：有两个处理器，相当于有两个指令队列

伪四核处理器：两个处理起，每个处理器有两个指令队列（一个cpu带两个指令队列是最合适的）

多路cpu：一个cpu叫做一路，多个cpu就是多路cpu



### 线程池的任务队列

一个多线程的程序中的开一个进程，这个进程中的线程是会频繁的创建和销毁线程，这样是比较消耗性能的。

**线程池**

+ 为了解决这个问题，我们会一次性的创建多个线程，将需要执行的任务放入任务队列中。如果有线程是空的，就从任务队列中取任务去执行。

+ 任务队列是作为一个缓冲区来使用的



## 函数式编程、泛型编程 是什么？



## 算法的递归、回溯、动态规划？



## [vim配置](https://github.com/ma6174/vim)



## 面试算法题

###  1 - [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

```js
// 防止数组索引为复数
假设索引为负数相当于从数组末尾读取，-1 就相当于数组的最后一个元素
arrIndex = (index + arr.length)% arr.length
arrIndex = (1+9)/9 = 1
arrIndex = (-1+9)/9 = 8
```

```js
var MyCircularQueue = function (k) {

    // 模拟队列
    // 为什么是 K+1，是因为循环对列刚好能放下所有元素时，
    // 队列空和满都市 头指针 = 尾指针，无法判断空还是满足
    // 所以要放一个多余的空间， 尾 = （ 尾 - 头 + 队列长度 ） % （队列长度），取模时为了解决尾在头前面的情况
    this.queue = Array(k + 1);
    // 头指针
    this.front = 0;
    // 尾指针
    this.rear = 0;
    // 最大容量
    this.max = k;
};

/** 
 * @param {number} value
 * @return {boolean}
 */
MyCircularQueue.prototype.enQueue = function (value) {
    // 如果满就不添加
    if (this.isFull()) return false;
    // 入队
    this.queue[this.rear] = value;
    // 尾指针后移一位
    this.rear = (this.rear + 1) % (this.max + 1);
    return true;
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.deQueue = function () {
    // 如果为空就不删除
    if (this.isEmpty()) return false;
    // 头指针后移一位进行删除
    this.front = (this.front + 1) % (this.max + 1);
    return true;
};

/**
 * @return {number}
 */
MyCircularQueue.prototype.Front = function () {
    if (this.isEmpty()) return -1;
    return this.queue[this.front];
};

/**
 * @return {number}
 */
MyCircularQueue.prototype.Rear = function () {
    // 如果为空
    if (this.isEmpty()) return -1;
    // 不为空，返回队尾元素，尾指针的前一位
    return this.queue[(this.rear + this.max) % (this.max + 1)];
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.isEmpty = function () {
    return ((this.rear - this.front + this.max + 1) % (this.max + 1)) == 0;
};

/**
 * @return {boolean}
 */
MyCircularQueue.prototype.isFull = function () {
    return ((this.rear - this.front + this.max + 1) % (this.max + 1)) == this.max;
};
```



### 2 - [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

```js
头部可以出、入队
尾部可以出、入队
```

双向循环队列（又叫 deQue、dobuleQue）

```js
var MyCircularDeque = function (k) {
    this.queue = Array(k + 1);
    this.front = 0;
    this.rear = 0;
    this.max = k;
};

/**
 * Adds an item at the front of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertFront = function (value) {
    if (this.isFull()) return false;
    this.front = (this.front - 1 + this.max + 1) % (this.max + 1);
    this.queue[this.front] = value;
    return true;
};

/**
 * Adds an item at the rear of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertLast = function (value) {
    if (this.isFull()) return false;
    this.queue[this.rear] = value;
    this.rear = (this.rear + 1) % (this.max + 1);
    return true;
};

/**
 * Deletes an item from the front of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteFront = function () {
    if (this.isEmpty()) return false;
    this.front = (this.front + 1) % (this.max + 1);
    return true;
};

/**
 * Deletes an item from the rear of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteLast = function () {
    if (this.isEmpty()) return false;
    this.rear = (this.rear - 1 + this.max + 1) % (this.max + 1);
    return true;
};

/**
 * Get the front item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getFront = function () {
    if (this.isEmpty()) return -1;
    return this.queue[(this.front + this.max + 1) % (this.max + 1)];
};

/**
 * Get the last item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getRear = function () {
    if (this.isEmpty()) return -1;
    return this.queue[(this.rear - 1 + this.max + 1) % (this.max + 1)];
};

/**
 * Checks whether the circular deque is empty or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isEmpty = function () {
    return ((this.rear - this.front + this.max + 1) % (this.max + 1)) == 0;
};

/**
 * Checks whether the circular deque is full or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isFull = function () {
    return ((this.rear - this.front + this.max + 1) % (this.max + 1)) == this.max;
};
```



### 3 - [1670. 设计前中后队列](https://leetcode-cn.com/problems/design-front-middle-back-queue/)

```js
看成两个双端队列来实现
```

```js
使用链表实现双端队列
```

```js
var FrontMiddleBackQueue = function () {
    this.leftArr = [];
    this.rightArr = []; // 优先向右边存
};

/** 
 * @param {number} val
 * @return {void}
 */
FrontMiddleBackQueue.prototype.pushFront = function (val) {
    this.leftArr.unshift(val);

    // 相当于中间值处理优先于右边数组
    if (this.leftArr.length > this.rightArr.length) {
        // 左数组取出一个放入右数组
        this.rightArr.unshift(this.leftArr.pop());
    }
};

/** 
 * @param {number} val
 * @return {void}
 */
FrontMiddleBackQueue.prototype.pushMiddle = function (val) {
    // 左右相等存入右边
    if (this.leftArr.length == this.rightArr.length) {
        this.rightArr.unshift(val);
    } else {
        // 否则存入左边
        this.leftArr.push(val);
    }
};

/** 
 * @param {number} val
 * @return {void}
 */
FrontMiddleBackQueue.prototype.pushBack = function (val) {
    this.rightArr.push(val);

    // 如果右边比左边多两个或者两个以上，从右边向左边存入
    if (this.rightArr.length == this.leftArr.length + 2) {
        this.leftArr.push(this.rightArr.shift());
    }
};

/**
 * @return {number}
 */
FrontMiddleBackQueue.prototype.popFront = function () {
    // 因为值优先存在右边,所以右边没有值，就证明左边肯定没有 
    if (!this.rightArr.length) return -1;
    // 左边没有，右边有一个
    if (!this.leftArr.length) return this.rightArr.shift();

    let ret = this.leftArr.shift();
    // 如果右边比左边多两个或者两个以上，从右边向左边存入
    if (this.rightArr.length > this.leftArr.length + 1) {
        this.leftArr.push(this.rightArr.shift());
    }
    return ret;
};

/**
 * @return {number}
 */
FrontMiddleBackQueue.prototype.popMiddle = function () {
    if (!this.rightArr.length) return -1;
    if (!this.leftArr.length) return this.rightArr.shift();

    // 左右两边长度相等
    if (this.leftArr.length == this.rightArr.length) {
        return this.leftArr.pop();
    }

    // 因为右边优先存入，如果左右长度不相等，那么右边多
    return this.rightArr.shift();
};

/**
 * @return {number}
 */
FrontMiddleBackQueue.prototype.popBack = function () {
    if (!this.rightArr.length) return -1;
    if (!this.leftArr.length) return this.rightArr.shift();

    let ret = this.rightArr.pop();
    if (this.rightArr.length < this.leftArr.length) {
        this.rightArr.unshift(this.leftArr.pop());
    }
    return ret;
};
```

```js
// 链表实现
// 前中后队列 - 链表实现
var Node = function (val) {
    this.val = val;
    this.pre = null;
    this.next = null;
}
Node.prototype.insert_pre = function (p) {
    p.next = this;
    p.pre = this.pre;
    this.pre && (this.pre.next = p);
    this.pre = p;
    return;
}

Node.prototype.insert_next = function (p) {
    p.pre = this;
    p.next = this.next;
    this.next && (this.next.pre = p);
    this.next = p;
    return;
}
Node.prototype.erase = function () {
    this.next && (this.next.pre = this.pre);
    this.pre && (this.pre.next = this.next);
    return;
}
var deQueue = function () {
    this.cnt = 0;
    this.head = new Node(-1);
    this.tail = new Node(-1);
    this.head.next = this.tail;
    this.tail.pre = this.head;
    console.log(this.head)
}

deQueue.prototype.push_back = function (value) {
    this.tail.insert_pre(new Node(value))
    this.cnt += 1;
    return;
}
deQueue.prototype.push_front = function (value) {
    this.head.insert_next(new Node(value));
    this.cnt += 1;
}
deQueue.prototype.pop_back = function () {
    let ret = this.tail.pre.val;
    if (this.cnt) {
        this.tail.pre.erase();
        this.cnt -= 1;
    }
    return ret;
}
deQueue.prototype.pop_front = function () {
    let ret = this.head.next.val;
    if (this.cnt) {
        this.head.next.erase();
        this.cnt -= 1;
    }
    return ret;
}
deQueue.prototype.front = function () {
    return this.head.next.val;
}
deQueue.prototype.back = function () {
    return this.tail.pre.val;
}
deQueue.prototype.size = function () {
    return this.cnt;
}
var FrontMiddleBackQueue = function () {
    this.q1 = new deQueue();
    this.q2 = new deQueue();
};

FrontMiddleBackQueue.prototype.maintain = function () {
    if (this.q2.size() > this.q1.size()) {
        this.q1.push_back(this.q2.pop_front());
    } else if (this.q1.size() == this.q2.size() + 2) {
        this.q2.push_front(this.q1.pop_back());
    }
    return;
}

/**
* @param {number} val
* @return {void}
*/
FrontMiddleBackQueue.prototype.pushFront = function (val) {
    let ret = this.q1.push_front(val);
    this.maintain();
    return;
};

/**
* @param {number} val
* @return {void}
*/
FrontMiddleBackQueue.prototype.pushMiddle = function (val) {
    if (this.q1.size() == this.q2.size() + 1) {
        this.q2.push_front(this.q1.pop_back())
    }
    this.q1.push_back(val);
    this.maintain();
    return;
};

/**
* @param {number} val
* @return {void}
*/
FrontMiddleBackQueue.prototype.pushBack = function (val) {
    this.q2.push_back(val);
    this.maintain();
    return;
};

/**
* @return {number}
*/
FrontMiddleBackQueue.prototype.popFront = function () {
    let ret = this.q1.pop_front();
    this.maintain();
    return ret;
};

/**
* @return {number}
*/
FrontMiddleBackQueue.prototype.popMiddle = function () {
    let ret = this.q1.pop_back();
    this.maintain();
    return ret;
};

/**
* @return {number}
*/
FrontMiddleBackQueue.prototype.popBack = function () {
    let ret = this.q2.size() ? this.q2.pop_back() :
        this.q1.pop_back();
    this.maintain();
    return ret;
};

```



### 4 - [933. 最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)

```js
var RecentCounter = function () {
    this.pingArr = [];
};

/** 
 * @param {number} t
 * @return {number}
 */
RecentCounter.prototype.ping = function (t) {
    this.pingArr.push(t);

    while (this.pingArr[0] < t - 3000) {
        this.pingArr.shift();
    }
    return this.pingArr.length;
};
```



### 5 - leetCode - [面试题 17.09. 第 k 个数](https://leetcode-cn.com/problems/get-kth-magic-number-lcci/)

```js
// 数学归纳法
var getKthMagicNumber = function (k) {
    let p3 = 0;
    let p5 = 0;
    let p7 = 0;
    let dp = new Array(k);
    dp[0] = 1;
    for (let i = 1; i < k; i++) {
        dp[i] = Math.min(dp[p3] * 3, Math.min(dp[p5] * 5, dp[p7] * 7));
        if (dp[i] === dp[p3] * 3) p3++;
        if (dp[i] === dp[p5] * 5) p5++;
        if (dp[i] === dp[p7] * 7) p7++;
    }
    return dp[k - 1];
};
```

**第二种方法**

![1616069879272](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-1.png)

![1616069918620](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-2.png)

![1616069977843](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-3.png)

![1616070065043](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-4.png)

![1616070107411](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-5.png)

![1616070243018](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-%E9%9D%A2%E8%AF%95%E9%A2%98%2017.09.%20%E7%AC%AC%20k%20%E4%B8%AA%E6%95%B0-6.png)


```js
var getKthMagicNumber = function (k) {
    let p3 = 0, p5 = 0, p7 = 0;
    let dp = [1];

    for (let i = 1; i < k; i++) {
        // 求出三个数中的最小值
        let ans = dp[p3] * 3;
        ans = Math.min(ans, dp[p5] * 5);
        ans = Math.min(ans, dp[p7] * 7);

        // 三个数中，谁的值等于选中的最小值，他们对应的指针后移一位
        if (ans === dp[p3] * 3) p3++;
        if (ans === dp[p5] * 5) p5++;
        if (ans === dp[p7] * 7) p7++;
    }
    return dp[k - 1];
};
```



### 6 - [859. 亲密字符串](https://leetcode-cn.com/problems/buddy-strings/)

```js
var buddyStrings = function (a, b) {
    // 两个字符串长度是否相等
    if (a.length != b.length) return false;
    // 两个字符串是否一样,是否有重复字段
    if (a == b) return (a.length > new Set(a).size);

    let A = '';
    let B = '';
    for (let i = 0; i < a.length; i++) {
      	// 两者不一样时，通过拼接来实现交换
        if (a[i] != b[i]) A = a[i] + A, B = B + b[i];
    }

    return A.length == 2 && A == B;
};
```





### 7 - [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

```js
// 先从最大的零钱找
var lemonadeChange = function (bills) {
    if (bills[0] == 10) return false;
    if (bills[0] == 20) return false;
    let five = 0, ten = 0;
    for (const v of bills) {
        if (v == 5) five++;
        if (v == 10) ten++ , five--;
        if (v == 20) {
            if (five && ten) {
                five--;
                ten--;
            } else if (five >= 3) {
                five -= 3;
            } else {
                return false;
            }
        }
        if (five < 0 || ten < 0) return false;
    }

    return true;
};
```





### 8 - [969. 煎饼排序](https://leetcode-cn.com/problems/pancake-sorting/)

```js
var pancakeSort = function (arr) {
    // 记录 arr 元素索引位置
    let ind = [];
    for (let i = 0; i < arr.length; i++)  ind[arr[i]] = i;

    // 需要对 arr 翻转几位 ？？？？？
    let ret = [];
    for (let i = arr.length; i >= 1; i--) {

        // if (ind[i] == i - 1) continue;

        if (ind[i] + 1 != 1) {
            ret.push(ind[i] + 1);
            reverse(arr, ind[i] + 1, ind);
        }
        if (i != 1) {
            ret.push(i);
            reverse(arr, i, ind);
        }

    }
    return ret;
};

var reverse = function (arr, n, ind) {
    for (let i = 0, j = n - 1; i < j; i++ , j--) {
        [arr[i], arr[j]] = [arr[j], arr[i]]
        ind[arr[i]] = i;
        ind[arr[j]] = j;
    }
}
```





### 9 - [621. 任务调度器](https://leetcode-cn.com/problems/task-scheduler/)

![1616064877813](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%98%9F%E5%88%97-621.%20%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E5%99%A8.png)

```js
var leastInterval = function (tasks, n) {
  	// lodash 中序列化方法
    // const freq = _.countBy(tasks);
    const freq = getNum(tasks);

    // 得出某个任务出现的最大次数
    const maxExec = Math.max(...Object.values(freq));

    // 最大次数有几个
    let maxCont = 0;
    Object.values(freq).forEach(v => {
        if (v === maxExec) maxCont++;
    })

    return Math.max((maxExec - 1) * (n + 1) + maxCont, tasks.length);
};

// 获取每个字母出现个数
function getNum(arr) {
    const obj = {};
    arr.forEach(el => {
        obj[el] ? obj[el]++ : obj[el] = 1;
    });
    return obj;
}
```



## 作业

984132657 - 煎饼排序不压入第一位，结果排序 