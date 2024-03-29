## 所发生的



## 面试算法题

### 1 - [面试题 03.04. 化栈为队](https://leetcode-cn.com/problems/implement-queue-using-stacks-lcci/)

```js
// 1 - 用两个栈实现队列的操作
// 2 - 将每个值放入输入栈 pushStack = [1,2,3], 相当于入队操作
// 3 - 出队时, 将 pushStack  = [1,2,3] 中的数据倒序放入输出栈 popStack = [3,2,1], pushStack 满足了栈的先进后出的特性
// 4 - 然后从输出栈 popStack 中出栈, 也就是队列的出队操作, popStack = [3,2,1] ==> popStack = [3,2] , 同时 popStack 也符合栈的先进后出的特性

// 5 - pushStack  = [1,2,3] ==> popStack = [3,2,1] ==> 1 整体实现了队列的先进先出
```

```js
var MyQueue = function () {
    // 输入栈
    this.pushStack = [];
    // 输出栈
    this.popStack = [];
};

/**
 * Push element x to the back of queue. 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function (x) {
    // 入栈 （入队）
    this.pushStack.push(x);
};

/**
 * Removes the element from in front of queue and returns that element.
 * @return {number}
 */
MyQueue.prototype.pop = function () {
    // 如果输出栈为空，从输入栈导入
    // [1,2,3] --> [3,2,1]
    // 此过程实现了栈的先入后出
    if (!this.popStack.length) {
        while (this.pushStack.length) {
            this.popStack.push(this.pushStack.pop());
        }
    }
    // 出栈（出队）
    // 对于栈来说 popStack 实现了先入后出
    // 对于队列来说 pushStack = [1,2,3] --> popStack = [3,2,1] --> 实现了队列的先入先出
    return this.popStack.pop();
};

/**
 * Get the front element.
 * @return {number}
 */
MyQueue.prototype.peek = function () {
    if (!this.popStack.length) {
        while (this.pushStack.length) {
            this.popStack.push(this.pushStack.pop())
        }
    };
    let num = this.popStack.pop();
    this.popStack.push(num);
    return num;
}

/**
 * Returns whether the queue is empty.
 * @return {boolean}
 */
MyQueue.prototype.empty = function () {
    // 输入、输出栈都为空，证明这个队列是空的
    return !this.pushStack.length && !this.popStack.length;
};
```



### 2 - [682. 棒球比赛](https://leetcode-cn.com/problems/baseball-game/)

```js
var calPoints = function (ops) {
    let result = [];
    for (let num of ops) {
        switch (num) {
            case 'C':
                result.pop();
                break;
            case 'D':
                result.push(result[result.length - 1] * 2);
                break;
            case '+':
                result.push(result[result.length - 1] + result[result.length - 2]);
                break;
            default:
                result.push(Number(num));
                break;
        }
    }
    return result.reduce((a, b) => a + b);
};
```



### 3 - [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)

```js
var backspaceCompare = function (S, T) {
    return processed(S) === processed(T);
};

// 去除空格
const processed = function (str) {
    const stack = [];
    for (let char of str) {
        if (char === '#') {
            stack.pop();
        } else {
            stack.push(char);
        }
    }
    return stack.join('');
}
```



### 4 - [946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)

```js
1 - 有一个新栈 
2 - 按照 pushed 中的顺序入栈
3 - 按照 popped 中的顺序出栈
4 - 返回 true, 否则返回 false
```

```js
var validateStackSequences = function (pushed, popped) {
    let stack = [], pre = 0;
    for (let i of pushed) {
        stack.push(i);
        // 每次入栈后，判断新栈 stack 中的最后一位是不是需要弹栈的
        // 如果是就弹出，指向 popped 的针后移一位
        while (stack.length && stack[stack.length - 1] == popped[pre]) {
            stack.pop();
            pre++;
        }
    }

    return !stack.length;
};
```



### 5 - [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```js
1 - 左括号必须用相同类型的右括号闭合。
2 - 左括号必须以正确的顺序闭合。
'()'、'()[]{}' 为真
'(()'、'(])' 为假
```



```js
var isValid = function (s) {
    // 以为右边括号为 key，做一个映射
    let map = new Map([
        [')', '('],
        [']', '['],
        ['}', '{'],
    ]);

    let stack = [];

    for (let i of s) {

        // 1 - 栈不为空
        // 2 - 右括号匹配的左括号，在栈的最后一位
        // 3 - 栈为空，证明所有括号都能闭合
        if (stack.length && stack[stack.length - 1] == map.get(i)) {
            stack.pop();
        } else {
            stack.push(i);
        }
    }

    return !stack.length;
};
```



### 6 - [1249. 移除无效的括号](https://leetcode-cn.com/problems/minimum-remove-to-make-valid-parentheses/)

```js
var minRemoveToMakeValid = function (s) {
    // leftDel 需要删除的 (;
    // rightDel 需要删除的 );
    const leftDel = [], rightDel = [];

    // 遍历字符串 s
    for (let i = 0; i < s.length; i++) {
        // 遇到 ( 存入到 leftDel;
        if (s[i] === '(') {
            leftDel.push(i);
        } else if (s[i] === ')') {
            // 遇到 ) 并且 leftDel有 ( 存在, 证明匹配, 从 leftDel 删除;
            if (leftDel.length) {
                leftDel.pop();
            } else {
                // 遇到 ) 并且 leftDel 不存在 (, 证明不匹配, 加入到 rightDel;
                rightDel.push(i);
            }
        }
    }

    // 将 s 转成数组res, 将需要删除的括号拼成一个数组 del;
    const res = [...s], del = [...leftDel, ...rightDel];

    // 从 s 中删除需要删除的括号
    for (let i = 0; i < del.length; i++) {
        res[del[i]] = '';
    }

    // res 转成字符串输出
    return res.join('');
};
```



### 7 - [1021. 删除最外层的括号](https://leetcode-cn.com/problems/remove-outermost-parentheses/)

```js
var removeOuterParentheses = function (S) {

    // differ 为出现括号的个数
    // 只有同一个括号出现两次或者两次以上,才能证明不是最外层
    let res = '', differ = 0;
    for (let char of S) {
        // if (char === '(') {
        //     differ++;
        //     if (differ > 1) res += char;
        // } else if (char === ')') {
        //     differ--;
        //     if (differ > 0) res += char;
        // }
        if (char === '(' && differ++ > 0) res += char;
        if (char === ')' && differ-- > 1) res += char;
    }
    return res;
};
```



### 8 - [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)????????

```js
// 根据遍历根节点的顺序来决定是什么序遍历
// 前序遍历: 遍历子节点顺序为 根 - 左 - 右 
// 中序遍历: 遍历子节点顺序为 左 - 根 - 右
// 后序遍历: 遍历子节点顺序为 左 - 右 - 根
```

**递归实现**

```js
var postorderTraversal = function (root) {
    let res = [];
    return postorderTraversalNode(root, res);

};

const postorderTraversalNode = function (node, res) {
    if (node) {
        // 遍历左节点
        postorderTraversalNode(node.left, res);
        // 遍历右节点
        postorderTraversalNode(node.right, res);
        // 中间节点
        res.push(node.val);
    }
    return res;
}
```

**迭代实现????????**

```js
var postorderTraversal = function (root) {
    let res = [];
    if (!root) return res;
    const stack = [root];
    while (stack.length) {
        root = stack.pop();
        res.unshift(root.val);

        if (root.left) stack.push(root.left);
        if (root.right) stack.push(root.right);
    }
    return res;
};
```





### 9 - [331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

![1616516562618](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-1.png)

![1616516883623](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-2.png)

![1616517067499](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-3.png)

![1616517249049](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-4.png)

![1616517399880](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-5.png)

![1616517437464](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E9%80%92%E5%BD%92%E5%92%8C%E6%A0%88-331.%20%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E5%BA%8F%E5%88%97%E5%8C%96-6.png)

```js
var isValidSerialization = function (preorder) {
    // stack 默认添加根节点
    const n = preorder.length, stack = [1];
    let i = 0;
    while (i < n) {
        if (!stack.length) return false;
        if (preorder[i] === ',') i++;
        else if (preorder[i] === '#') {
            stack[stack.length - 1]--;
            if (stack[stack.length - 1] === 0) {
                stack.pop();
            }
            i++;
        } else {
            while (i < n && preorder[i] !== ',') {
                i++;
            }
            stack[stack.length - 1]--;
            if (stack[stack.length - 1] === 0) {
                stack.pop();
            }
            stack.push(2);
        }
    }

    return !stack.length;
};
```

**我写的好理解**

```js
var isValidSerialization = function (preorder) {
    // stack 默认需要添加一个根节点
    // stack 存储的时当前节点下还没有分配的子节点数
    const stack = [1];

    // preorder 转数组
    const preorderArr = preorder.split(',');


    for (let i = 0; i < preorderArr.length; i++) {

        // 如果栈 stack 为空，但是 preorder 还没匹配完毕
        // 证明不是正确的二叉树的前序序列化
        if (!stack.length) return false;

        // 分配给节点一个子节点，所需子节点数减1
        stack[stack.length - 1]--;

        // 如果当前节点下的子节点够两个，证明当前子节点已经满了
        // 当前节点所需节点数为 0， 出栈
        if (stack[stack.length - 1] === 0) {
            stack.pop();
        }

        // 如果当前节点不等于#，证明当前节点不为空
        // 那么当前节点需要两个子节点
        // 需要的字节点个数入栈
        if (preorderArr[i] !== '#') {
            stack.push(2);
        }
    }

    // 如果匹配完毕后，栈 stack 为空
    // 证明是正确的二叉树的前序序列化
    return !stack.length;
};
```



10 - [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

```js
var calculate = function (s) {
    //  乘除优先，所以需要判断后面是否存在乘除符号

    // 去除空格
    s = s.trim();

    // 定义一个指针，指向运算符号
    let pre = '+';

    // 定义一个初始值
    let num = 0;

    // 定义一个栈，存储遇到的数字
    const stack = [];

    for (let i = 0; i < s.length; i++) {
        // i 是一个数字，并且 i 不为空字符串
        // isNaN(' ')  =  false
        if (!isNaN(s[i]) && s[i] !== ' ') {
            // 解决大于10的数字
            // 12 ==> 0*10 + 2
            num = num * 10 + Number(s[i]);
        }
        // i 是个符号
      	// + - 先入栈，* / 先计算
        if (isNaN(s[i]) || i == s.length - 1) {
            switch (pre) {
                case '+':
                    stack.push(num);
                    break;
                case '-':
                    stack.push(-num);
                    break;
                case '*':
                    stack.push(stack.pop() * num);
                    break;
                default:
                    stack.push(stack.pop() / num | 0);
                    break;
            }
            // 指向符号
            pre = s[i];
            // 初始化
            num = 0;
        }
    }

    // 数组求和
    return stack.reduce((a, b) => a + b);
};
```



### 11 - [636. 函数的独占时间](https://leetcode-cn.com/problems/exclusive-time-of-functions/)

```js
var exclusiveTime = function (n, logs) {
    // 每个任务花费时间
    let res = new Array(n).fill(0);

    // 执行第几个任务。任务的索引
    let go = 0;

    // 执行下一个任务 
    function next() {
        // 记录子任务花费时间
        let dure = 0;
        // [0,start,0] [任务id,开始,开始时间]
        const start = logs[go].split(':');

        // 当前任务还有子任务 
        while (go < logs.length - 1 && logs[++go].indexOf('s') !== -1) {
            dure = dure + next();
        }

        // [0,end,6]  [任务id,结束,结束时间]
        const end = logs[go].split(':');

        // 当前任务执行的时间 = 结束时间 - 开始时间 +1 - 子任务执行时间
        let sum = Number(end[2]) - Number(start[2]) + 1 - dure;

        res[Number(start[0])] = res[Number(start[0])] + sum;

        return sum + dure;
    }
    while (go < logs.length) {
        next();
        go++;
    }

    return res;
};
```



### 12 - [1124. 表现良好的最长时间段](https://leetcode-cn.com/problems/longest-well-performing-interval/)?????????????





## 答案

11186206671