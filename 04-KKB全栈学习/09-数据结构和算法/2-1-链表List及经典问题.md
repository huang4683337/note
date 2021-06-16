![img](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-01-%E4%BB%80%E4%B9%88%E6%98%AF%E9%93%BE%E8%A1%A8-1.png)

![1614860332824](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-01-%E4%BB%80%E4%B9%88%E6%98%AF%E9%93%BE%E8%A1%A8-2.png)

## 需要了解的知识

+ 双向链表

+ 地址？指针？引用？



## 知识记录

链表中 head 不是一个节点  ，head 之后的才是节点





## 链表实现方法

+ 第一种：对象形式

  ```js
  {
    data:'存放数据',
   	node:'指向下一个节点'
  }
  
  // 最后一个节点的指向为空  node:null
  ```

+ 第二种：两个数组

  ```js
  // 数组1:数据域
  // 数组2:指针域
  data[10]
  next[10]
  
  // 添加节点
  add(index, p, val){
    next[p] = next[index];
    next[index] = p;
    data[p] = val
  }
  
  data[3] = 0;
  add(3,5,1)
  add(5,5,2)
  add(2,7,3)
  add(7,9,,100)
  
  ```

  ![1614861865886](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-02-%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E5%AE%9E%E7%8E%B0%E9%93%BE%E8%A1%A8.png)

  ```js
  const data = [];    // 数据域
  const next = [];    // 指针域
  /* ind - 索引、p - 指针、val - 节点的值 */
  const add = (ind, p, val) => {
      next[p] = next[ind];
      next[ind] = p;
      data[p] = val;
  }
  const main = () => {
      // 头节点为3，值为0
      const head = 3;
      data[3] = 0;
      add(3, 5, 1);
      add(5, 2, 2);
      add(2, 7, 3);
      add(7, 9, 100);
      let p = head;
      while (p !== undefined) {
          console.log('->', data[p]);
          p = next[p];
      }
  } 
  main();
  
  console.log('data',data);
  console.log('next',next);
  ```

  

## 彩蛋

[leetcode插件使用](https://xue.kaikeba.com/static/KKB000000.mp4)



## 链表的应用场景

### 场景一：操作系统内的动态内存分布



## 场景二：LRU 缓存淘汰算法

存储：硬盘等

缓存：内存（cpu经常使用的数据）

cpu从存储中读取数据较慢，所以它会将经常使用的数据翻入缓存中，每当使用时先从缓存中找，没有再去存储中找。



## 经典面试题 - 链表的访问

### 1 - [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```js
// 1 - 定义双指针，慢指针为 pre, 快指针为 cur
// 2 - 两个指针从一同一个节点出发
// 3 - 假设快指针，每次走两步; 慢指针，每次走一步
// 如果两者相遇，代表存在环，
// 没有相遇（快指针先指向null） 证明不存在环
```

```js
var hasCycle = function (head) {
    // 头节点不能为空
    if (!head) return false;
    
    // 慢指针 pre 指向 head, 快指针 cur 指向 head 
    let pre = head, cur = head;
    
    // 当 cur 为真, 并且 cur.next 为真时, 
    // 证明后面节点的指向不为 null,
    // 证明没有到尾节点
    while (cur && cur.next) {
        // 慢指针 pre 后移一位
        pre = pre.next;

        // 快指针 cur 后移两位
        cur = cur.next.next;

        // 两者相等就是环
        if (pre === cur) return true;
    }

    // 走出循环 pre 还不等于 cur 证明不是环
    return false;
};
```







### 2 - [142. 环形链表 II - 找到环的起点位置](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

![1615382851564](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-142.%20%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20II%20-%20%E6%89%BE%E5%88%B0%E7%8E%AF%E7%9A%84%E8%B5%B7%E7%82%B9%E4%BD%8D%E7%BD%AE.png)



```js
// 1 - 当两个节点相遇时证明有环 （leetCode141题实现）
// 2 - 一个从头开始, 一个从相遇的节点开始, 各自后移一个节点,当两者再次相遇时，就是环的起始位置
```

```js
// 为什么双指针可以这么做？
// 路程问题，a追b
// a、b一个快一个慢，路程是一个直线和圈的结合，类似于字母q，想要追上只能在圈子追上，可以通过画图理解
```

```js
var detectCycle = function (head) {
    if (!head) return null;
    let pre = head, cur = head;
    while (cur && cur.next) {
        // 慢指针
        pre = pre.next;
        
        // 快指针
        cur = cur.next.next;

        // 存在环
        // 慢指针 pre 放在头节点, pre 和 cur 同时向后移动一位
        if (pre === cur) {
            pre = head;
            while (pre != cur) {
                pre = pre.next;
                cur = cur.next;
            }
            return pre;
        }
    }
    return null;
};
```





### 3 - [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)

leetCode202题 - 使用了链表思维

快乐数：数字经历某规则不断变换后，最终结果为 1

```js
// 1 - 不会存在环形链表（不会无限循环）,通过链表快慢指针处理
// 2 - 不会无限增长
```



快乐数为什么最多只有731个节点，为什么不会无限增长？

```js
999 就是 81+81+81 = 243 不可能比 999 大
计算机最多能存在 2的35次方减去1的值
```

```js
var isHappy = function (n) {
    let pre = n, cur = getNext(n);

    // 此处使用快(pre)慢(cur)指针判断是否有环, 如果 pre === cur 证明有环
    // 通过 cur 是否等于 1, 来判断是否是快乐数
    while (pre != cur && cur != 1) {
        // 快指针后移一位
        pre = getNext(pre);

        // 慢指针后移两位
        cur = getNext(getNext(cur));
    }

    return cur === 1;
};

// 获取下一个数
var getNext = function (n) {
    let num = 0;
    while (n) {
        num += (n % 10) * (n % 10);
        n = Math.floor(n / 10);
    }
    return num;
}
```







### 4 - [206. 反转链表-从头到尾反转](https://leetcode-cn.com/problems/reverse-linked-list/)

![](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-206.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8-%E4%BB%8E%E5%A4%B4%E5%88%B0%E5%B0%BE%E5%8F%8D%E8%BD%AC.png)

```js
// 1 - 定义一个指针 pre  指向空, 代表反转链表 
// 2 - 定义一个指针 cur  指向头节点，代表原始链表
// 3 - 定义一个指针 next 指向 cur.next 用于暂存原始链表，保证链表完整性


// 每次从原始链表头部取出一个，当作反转链表的头


1 - next = cur.next; // next 指向原始链表的第二个节点，用来暂存原始链表，相当于原始链表头向后移动一位
2 - cur.next = pre;	// 将原始链表的头当作新链表的头,生成新链表
3 - pre = cur; // 反转链表
4 - cur = next; // 原始链表
5 - 如果 cur 等于 null 代表反转结束
```

```js
var reverseList = function (head) {
    let pre = null, cur = head;
    while (cur) {
        // let next = cur.next;
        // cur.next = pre;
        // pre = cur;
        // cur = next;
        [cur.next, pre, cur] = [pre, cur, cur.next];
    }
    return pre;
};
```



### 5 - [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

![1615649664844](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-92.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%20II-1.png)

![1615650139267](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-92.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%20II-2.png)



```js
// 1 - 定义一个指针 p 指向虚拟头节点 hair;
// 2 - 计算出待反转的节点个数 rever = n-m+1；  p 指向待反转节点的前一个
// 3 - 反转从 p.next 开始的节点，一共 rever 个
// 4 - 反转完毕后, cur 指向反转链表的后一个节点
// 将需要反转的头节点的 next 指向 cur
// 将反转后的链表指向 p.next
// 返回虚拟头节点的 next 就是反转后的链表
```

```js
var reverseBetween = function (head, left, right) {
    if (!head) return null;
    let ret = new ListNode(-1, head), p = ret, rever = right - left + 1;
    while (--left) {
        p = p.next;
    }
    p.next = reverseList(p.next, rever);
    return ret.next;
};

var reverseList = function (head, n) {
    let pre = null, cur = head;
    while (n--) {
        [cur.next, pre, cur] = [pre, cur, cur.next];
    }
    head.next = cur;
    return pre;
};
```



### 6 - [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

![1615386797433](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-25.%20K%20%E4%B8%AA%E4%B8%80%E7%BB%84%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.png)

```js
// 1 - 以 k 节点为一组进行翻转，翻转时判断当前待翻转链表长度是否有够一组
// 2 - 每次翻转完毕将 pre 指向待翻转节点的头一个节点
// 如果 pre 指向 null 代表没有需要翻转的节点了
```

```js
var reverseKGroup = function (head, k) {
    if (!head) return null;
    let ret = new ListNode(-1, head), pre = ret;
    do {
        // 先进行一次翻转
        pre.next = reverse(pre.next, k);
        // 每次翻转完毕后，pre 指向下一组待翻转的前一个节点，并且下一个节点不为 null
        for (let i = 0; i < k && pre; i++) {
            pre = pre.next;
        }
        // 如果 pre 为空，证明到了尾节点了，跳出循环
        if (!pre) break;
    } while (1);
    return ret.next;
};

var reverse = function (head, k) {
    let pre = null, cur = head, cont = k;

    // 是否够一组翻转
    while (--cont && cur) cur = cur.next;
    // 如果 cur 等于 null  证明不够翻转,  直接返回
    if (!cur) return head;

    // 重置 cur，开始翻转
    cur = head;
    while (k--) {
        [cur.next, pre, cur] = [pre, cur, cur.next];
    }
    head.next = cur;
    return pre;
}
```



### 7 - [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

![1615650950783](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-61.%20%E6%97%8B%E8%BD%AC%E9%93%BE%E8%A1%A8-1.png)

![1615650972445](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-61%E9%93%BE%E8%A1%A8%E6%97%8B%E8%BD%AC-2.png)

![1615650982019](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-61.%20%E6%97%8B%E8%BD%AC%E9%93%BE%E8%A1%A8-3.png)

```js
// 1 - 找到链表尾节点，将链表串成一个环，为了方便旋转
// 2 - 向右移动两个位置就是将环转两个节点，但是我们又不能真的旋转环
// 通过自己旋转发现，旋转之后，尾节点位置为 （链表长度 - 移动的次数 - 1）

// 3 - 从旋转后的头节点位置将他们断开

// 注意：当需要移动的次数大于链表长度时 i< size - k%size -1
```

```js
var rotateRight = function (head, k) {
    if (!head) return null;
    let pre = head, size = 1;
    // 确定最后一个节点和链表长度
    while (pre.next) pre = pre.next, size++;
    // 成环
    pre.next = head;

    // 旋转
    for (let i = 0; i < size - k % size - 1; i++) {
        head = head.next;
    }

    // 断开环，形成新链表
    pre = head.next;
    head.next = null;

    return pre;
};
```



### 8 - [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

![1615305476800](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-24.%20%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.png)

```js
// 1 - 定义一个虚拟头节点，temp 指向虚拟头节点
// 2 - 定义指针 pre 指向需要交换的第一个节点 temp.next
// 定义指针 cur 指向需要交换的第二个节点 temp.next.next
// 3 - 节点反转 
// pre.next = cur.next; 
// cur.next = pre;
// temp.next = cur;
// 4 - temp 后移动两位, 继续交换, temp = pre;
// 5 - 只要 temp.next 和 temp.next.nett 存在就一直交换
```

```js
var swapPairs = function (head) {
    if (!head) return null;
    let ret = new ListNode(-1, head), temp = ret;
    while (temp.next && temp.next.next) {
        let pre = temp.next, cur = temp.next.next;
        pre.next = cur.next;
        cur.next = pre;
        temp.next = cur;
        temp = pre;
    }
    return ret.next;
};
```





### 9 - [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

![1615211235266](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-19%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.png)

```js
// 因为链表没有索引，所以没法直接找到对应节点，我们需要想办法找到需要删除节点的前一个节点，直接让这个节点的 next 为 null
// 定义两个指针，两者之间的差距就是需要删除的节点数，同时后移，后面节点为空时，前面的节点就是要删除的前一个节点 

// 1 - 定义两个节点 pre 指向虚拟头节点 、cur 指向头节点
// 2 - cur 向后移动 n 个节点
// 3 - 两个节点同时向后移动，直到 cur 指向 null
// 此时 pre 指向倒数 n+1 个节点，cur 指向尾节点的指向 null
// 4 - 进行删除操作
```

```js
var removeNthFromEnd = function (head, n) {
    let ret = new ListNode(-1, head), pre = ret, cur = head;
    while (n--) cur = cur.next;

    // 链表长度小于要删除的长度
    if (!cur) return head.next;

    while (cur) pre = pre.next, cur = cur.next;
    pre.next = pre.next.next;
    return ret.next;
}
```





### 10 - [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

![1615212210351](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-83.%20%E5%88%A0%E9%99%A4%E6%8E%92%E5%BA%8F%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0.png)

```js
// 1 - 定义指针 pre 指向 head，定义指针 cur 指向 head.next
// 2 - 如果 pre.val == cur.val , cur 向后移一位
// 3 - 如果 pre.val != cur.val , 删除 pre 和 cur 之间相同的
// pre 指向 cur， cur 后移一位
// 4 - cur 存在一直循环
```

````js
var deleteDuplicates = function (head) {
    if (!head) return null;
    let pre = head, cur = head.next;
    while (cur) {
        if (pre.val === cur.val) {
            cur = cur.next;
        } else {
            pre.next = cur;
            pre = cur;
            cur = cur.next;
        }

    }
    // 跳出循环时 cur 指向 null
    // pre.next 指向上一次操作后的节点，就会出现 2 - 2 - 2 - null 这种情况
    // pre.next = null 实现了2 - null 的操作
    pre.next = null;
    return head;
};
````

**推荐此方法**

```js
// 1 - 定义指针 pre 指向头节点
// 2 - 如果当前节点的值pre.val 等于下个节点的值 pre.next.val
// 删除下一个节点 pre.next = pre.next.next
// 3 - 如果但给俺节点的值
```

```js
var deleteDuplicates = function (head) {
    if (!head) return null;
    let pre = head;
    while (pre && pre.next) {
        if (pre.val === pre.next.val) {
            pre.next = pre.next.next;
        } else {
            pre = pre.next;
        }
    }
    return head;
};
```





### 11 - [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

![1615648406921](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-82.%20%E5%88%A0%E9%99%A4%E6%8E%92%E5%BA%8F%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0%20II.png)

```js
// 1 - 定义虚拟头节点 ret, 定义指针 pre 指向虚拟头，定义 cur 指向 head
// 2 - pre.next.val 不等于 cur.next.val , pre、cur 后移一位
// 3 - 如果 pre.next.val == cur.next.val，cur 不断向后找，直到 pre.next.val != cur.next.val。删除掉重复节点
```

```js
var deleteDuplicates = function (head) {
    if (!head) return null;
    // 定义虚拟头节点
    let ret = new ListNode(-1, head), pre = ret, cur = head;
    while (cur && cur.next) {
        if (pre.next.val != cur.next.val) {
            pre = pre.next;
            cur = cur.next;
        } else {
            // 遍历后续节点，如果有重复节点，cur 一直后移
            while (cur.next && pre.next.val == cur.next.val) {
                cur = cur.next;
            }
            // 删除重复节点
            pre.next = cur.next;
            // cur 后移
            cur = cur.next;
        }
    }
    return ret.next;
};
```



## 什么情况下才使用虚拟头节点？

链表头地址有可能发生改变的情况下



## 作业

10万以内快乐数的总和 -- 692159746







## 12 - [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

![1615651740281](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E5%89%B2%E9%93%BE%E8%A1%A8-1.png)

![1615651758393](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E5%89%B2%E9%93%BE%E8%A1%A8-2.png)

![1615651824340](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E5%89%B2%E9%93%BE%E8%A1%A8-3.png)

![1615651846817](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E9%9A%94%E9%93%BE%E8%A1%A8-4.png)

![1615651889097](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E9%9A%94%E9%93%BE%E8%A1%A8-5.png)

![1615652035888](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%93%BE%E8%A1%A8-86%E5%88%86%E9%9A%94%E9%93%BE%E8%A1%A8-6.png)

```js
1 - 设置两个虚拟头指针，small、big；

2 - 定义 p 指针遍历整个链表， 小于的放在 small 下面 (small->)， 大样图等于的放在 big 下面 ( big -> x)

3 - small -> big 连起来
```

```js
var partition = function (head, x) {
    if (!head) return null;
    // 虚拟节点，拼接时使用
    let bigRet = new ListNode(), smallRet = new ListNode();
    // 大小链表的指针，一开始指向各自虚拟节点
    let bigP = bigRet, smallP = smallRet;

    // for (let cur = head, next; cur; cur = next) {
    //     next = cur.next;
    //     cur.next = null;

    //     if (cur.val < x) {
    //         smallP.next = cur;
    //         smallP = cur;
    //     } else {
    //         bigP.next = cur;
    //         bigP = cur;
    //     }
    // }

    // 遍历链表
    let cur = head, next;
    while (cur) {
        // 保存下一个节点
        next = cur.next;
        // 断开链表
        cur.next = null;

        // 分别拼接到大、小链表后面
        if (cur.val < x) {
            smallP.next = cur;
            smallP = cur;
        } else {
            bigP.next = cur;
            bigP = cur;
        }
        cur = next;
    }

    // 拼接
    smallP.next = bigRet.next;
    return smallRet.next;
};
```





## 13 - [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

```js
1 - 链表两两复制 成为一个新链表
 	 1 -> 2 -> 3 
   1 -> 1‘ -> 2 -> 2‘ -> 3 -> 3‘

2 - 修正 random 指针域
    假设原来 1 指向 3节点，那么复制后 1‘ 也指向3节点
    我们需要将 1‘ 指向 3‘

3 - 拆成两个链表
  1 -> 2 -> 3
  1‘ -> 2‘ ->  3‘
```

```js
var copyRandomList = function (head) {
    if (!head) return null;
    /**
     * 复制链表
     */
    let cur = head, copyNode;
    while (cur) {
        // 复制当前节点
        copyNode = new ListNode();
        copyNode.next = cur.next;
        copyNode.random = cur.random;
        copyNode.val = cur.val;

        // 复制的节点拼接在当前节点的后面
        cur.next = copyNode;
        // 后移两位
        cur = cur.next.next;
    }

    /**
     * 修正 random 指针
     */
    // 重置 cur 指向第一个复制的节点
    cur = head.next;
    while (cur) {
        cur.random && (cur.random = cur.random.next);
        // 后移两位
        (cur = cur.next) && (cur = cur.next);
        // cur.next && (cur = cur.next.next);
    }

    /**
     * 拆分
     */
    cur = copyNode = head.next;
    while (cur.next) {
        head.next = head.next.next;
        cur.next = cur.next.next;
        head = head.next;
        cur = cur.next;
    }
    head.next = null;
    return copyNode;
};
```



