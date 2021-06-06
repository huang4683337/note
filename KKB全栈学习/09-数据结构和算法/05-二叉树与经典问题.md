## 第一部分

链表是一种特殊的树形结构



### 二叉树

+ 每个节点度最多为2
+ 度为0的节点比度为2的节点多1个

度（出度）：就是有几个孩子



**画出树来**

前：1 2 4 9 5 6 10 3 7 8

中：4 9 2 10 6 5 1 3 8 7



### 计算式、记录式

计算式：节省空间，通过计算的到后续的值

记录式：节省时间，每个值都存储起来了



### 完全二叉树 --- 这节课左右价值的点

什么是完全二叉树？

最后一层的最右侧缺少节点



优势：

+ 不需要记录子树的地址，可以节省大量的存储空间。

  实现计算式

+ 可以用连续的空间存储（数组）完全二叉树



![1617282219755](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91.png)



+ 条件1的好处：不需要记录子树的地址，可以节省大量的存储空间，因为可以计算出来子节点

+ 实际在程序中实现的完全二叉树是一个线性序列（数组），但是在脑海中他是一个树，是一个二维结构





### 满二叉树

+ 没有度为1的节点
+ 只有度为0和2的节点

![1617282112758](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E5%AE%8C%E7%BE%8E%E4%BA%8C%E5%8F%89%E6%A0%91.png)

### 完美二叉树

除了最后一层，每个节点都是2个子节点

![1617282093946](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E6%BB%A1%E4%BA%8C%E5%8F%89%E6%A0%91.png)





### 为什么树形结构很重要？

数就是用于各种场景的查找工作

+ 节点代表什么?

  节点代表集和

+ 边代表什么？

  边代表关系





## 第二部分：学习二叉树的作用

![1617283654867](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E5%AD%A6%E4%B9%A0%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%BD%9C%E7%94%A8.png)

B-树：B树

B+树：B加树



![1617284831688](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E9%80%92%E5%BD%92%E6%8A%80%E5%B7%A7.png)

+ 数学归纳法   ？？？？？

  已知：k（0） 正确

  假设 k（i）正确，那么k(i+1) 正确 ==> k(n) 正确

+ 赋予递归函数明确的意义



![1617284973605](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E5%B7%A6%E5%AD%A9%E5%AD%90%E5%8F%B3%E5%85%84%E5%BC%9F%E8%A1%A8%E7%A4%BA%E6%B3%95.png)

> 使用二叉树表示三叉树
>
> 使用二叉树表示森林：根节点的右孩子，就是根节点的另一个兄弟树



> 为什么会节省空间？
>
> 比如：一个三叉树的每个节点需要存储三个指针域。6个节点18个指针域，只有5个是有效指针域，浪费了13个指针域
>
> 转换成二叉树后：一共12个指针域，有效指针域为5个，浪费了7个指针域

![1617285367332](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%8A%82%E7%9C%81%E7%A9%BA%E9%97%B4-1.png)

![1617285382203](https://hkw-img.oss-cn-hongkong.aliyuncs.com/dataStructure/%E4%BA%8C%E5%8F%89%E6%A0%91-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%8A%82%E7%9C%81%E7%A9%BA%E9%97%B4-2.png)







## 经典面试题：二叉树的基本操作



### 1、[144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```js
var preorderTraversal = function (root) {
  const ans = [];
  traverse(root, ans);
  return ans;
};

var traverse = function (root, ans) {
  if (!root) return null;
  ans.push(root.val);
  traverse(root.left, ans);
  traverse(root.right, ans);
}
```



### 2、[589. N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

```js
var preorder = function (root) {
    const ans = [];

    preorderFn(root, ans);
    return ans;
};

const preorderFn = function (root, ans) {
    if (!root) return;

    ans.push(root.val);

    // for (let i = 0; i < root.children.length; i++) {
    //     preorderFn(root.children[i], ans);
    // }
    for (x of root.children) {
        preorderFn(x, ans);
    }
}

```



### 3、[226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

```js
// 交换当前节点的两个子树
```

```js
var invertTree = function (root) {
    if (!root) return null;
    // [root.left, root.right] = [root.right, root.left];
    // invertTree(root.left);
    // invertTree(root.right);
    [root.left, root.right] = [invertTree(root.right), invertTree(root.left)];
    return root;
};
```



### 4、[剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

```js
// 将二叉树的每层数据放入分别放入一个数组中
// 额外增加一个递归变量，用来确定当前数组放入第几个数组中，也就是记录当前数据对应的节点在第几层
```

```js
// 通过队列实现
// 1 - 先将根节点入队，根节点出队时，将根节点的子节点入队
// 2 - 以此类推
```

```js
// 锯齿形遍历和层序遍历都是这道题的变种
// dfs：深度优先搜索，先递归下去然后再回溯回来
```


```js
var levelOrder = function (root) {
    const ans = [];
    levelOrderFn(root, 0, ans);
    return ans;
};

// K: 遍历到第几层
const levelOrderFn = function (root, k, ans) {
    if (!root) return;
    if (!ans[k]) ans.push(new Array());;
    ans[k].push(root.val);
    // 注意是 k+1，不是k++
    levelOrderFn(root.left, k + 1, ans);
    levelOrderFn(root.right, k + 1, ans);
}
```



### 5、[107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

```js
// 数组翻转： [1,2,3]  ==> [3,2,1]
// 好的方法：定义两个指针，一个指向数组头部，一个指向数组尾部，两两互换，直到相遇
```

```js
var levelOrderBottom = function (root) {
    // 二维数组
    // [ [每层的节点值] ]
    const ans = [];
    levelOrderBottomFn(root, 0, ans);

    // 结果翻转
    // 定义两个指针，一个指向数组头部，一个指向数组尾部，两两互换，直到相遇
    for (let i = 0, j = ans.length - 1; i < j; i++ , j--) {
        [ans[i], ans[j]] = [ans[j], ans[i]];
    }

    return ans;
};

// 将每层的节点值放入对应数组
const levelOrderBottomFn = function (root, k, ans) {
    if (!root) return;
    // 不存在当前层的数组，添加一个
    if (!ans[k]) ans.push(new Array());
    // 节点值放入数组
    ans[k].push(root.val);
    // 递归子节点
  	// 注意是 k+1，不是k++
    levelOrderBottomFn(root.left, k + 1, ans);
    levelOrderBottomFn(root.right, k + 1, ans);
}
```





### 6、[103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```js
// 先正常求出
// 然后将偶数行翻转一下
```

```js
var zigzagLevelOrder = function (root) {
    const ans = [];
    zigzagLevelOrderFn(root, 0, ans);
    // 翻转偶数行
    for (let t = 0; t < ans.length; t += 2) {
        if (t % 2 === 1) {
            for (let i = 0, j = ans[t].length - 1; i < j; i++ , j--) {
                [ans[t][i], ans[t][j]] = [ans[t][j], ans[t][i]];
            }
        }
    }

    return ans;
};

const zigzagLevelOrderFn = function (root, k, ans) {
    if (!root) return;
    if (!ans[k]) ans.push(new Array());
    ans[k].push(root.val);
    zigzagLevelOrderFn(root.left, k + 1, ans);
    zigzagLevelOrderFn(root.right, k + 1, ans);
}
```







## 经典面试题：二叉树的进阶操作

### 1、[110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)???????????????

```js
// 一次递归遍历实现
// 递归判断树高，
```

```js
var isBalanced = function (root) {
    return getHigth(root) >= 0;
};

// 获取二叉树高度
const getHigth = function (root) {
    if (!root) return null;
    let l = getHigth(root.left);
    let r = getHigth(root.right);

    // 
    if (l < 0 || r < 0) return -1;

    // 左右子树的高度差大于1，不符合要求
    if (Math.abs(l - r) > 1) return -1;

    // 
    return Math.max(l, r) + 1;
}
```





### 2、[112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

```js
// 不是将所有的可能值加起来，然后判断可能值中是否有目标值
// 而是从根节点开始，用目标值减去当前节点的值，直到叶子节点时，如果目标值减完最终结果为0，就证明有一条路径和为目标值
```

```js
var hasPathSum = function (root, targetSum) {
    if (!root) return false;

    // 当是叶子节点时
    if (!root.left && !root.right) return targetSum === root.val

    // 不是叶子节点时，减去当前节点的值
    return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
};
```





### 3、[105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```js
// 1 - 根据前序 || 后续找到根节点的位置
// 2 - 递归建立左子树
// 3 - 递归建立右子树
```

```js
var buildTree = function (preorder, inorder) {

    let map = new Map();

    for (let i = 0; i < inorder.length; i++) {
        map.set(inorder[i], i);
    }

    const helper = function (pStart, pEnd, iStart, iEnd) {
        // 如果开始位置索引大于结束位置索引，证明结束
        if (pStart > pEnd) return null;

        // 根节点值 = 前序遍历结果的第一个值
        let rootVal = preorder[pStart];

        // 生成根节点
        let root = new TreeNode(rootVal);

        // 中间值 = 中序遍历根节点的索引
        let mid = map.get(rootVal);

        // 中序遍历结果中：当前节点左子树节点个数
        let leftNum = mid - iStart;

        root.left = helper(pStart + 1, pStart + leftNum, iStart, mid - 1);
        root.right = helper(pStart + leftNum + 1, pEnd, mid + 1, iEnd);

        return root;
    }

    return helper(0, preorder.length - 1, 0, inorder.length - 1);
};
```

```js
// 根据中序、后续生成二叉树
/**
 *
 * @param {*} postscript 后续
 * @param {*} inorder 中序
 * @returns
 */
var buildTree = function (postscript, inorder) {

    let map = new Map();

    for (let i = 0; i < inorder.length; i++) {
        map.set(inorder[i], i);
    }

    const helper = function (pStart, pEnd, iStart, iEnd) {
        // 如果开始位置索引大于结束位置索引，证明结束
        if (pStart > pEnd) return null;

        // 根节点值 = 后序遍历结果的最后一个值
        let rootVal = postscript[pEnd];

        // 生成根节点
        let root = new TreeNode(rootVal);

        // 中间值 = 中序遍历根节点的索引
        let mid = map.get(rootVal);

        // 中序遍历结果中：当前节点左子树节点个数
        let leftNum = mid - iStart;

        root.left = helper(pStart, pStart + leftNum - 1, iStart, mid - 1);
        root.right = helper(pStart + leftNum, pEnd - 1, mid + 1, iEnd);

        return root;
    }

    return helper(0, postscript.length - 1, 0, inorder.length - 1);
};

const tree = buildTree([9, 15, 7, 20, 3], [9, 3, 15, 20, 7]);
```



### 4、[222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```js
var countNodes = function (root) {
    if (!root) return 0;

    return countNodes(root.left) + countNodes(root.right) + 1;
};
```



### 5、[剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

```js
// 二叉搜索树：中序遍历结果是有序的数
// 中序：左中右 ==>遍历结果： [1,2.3,4]
// 逆向中序：右中左 ==> 遍历结果：[4,3,2,1]
// 由此可得，第K大节点就是逆中序结果的第k个
```

```js
var kthLargest = function (root, k) {
    if (!root) return null;

    // 目标节点值
    let tag = 0;

    // 逆中序：寻找目标节点值
    const dfs = function (root) {
        if (!root) return null;
        dfs(root.right);

        // 每遍历一个节点， 就对k-1，当k为0时就得到目标值
        if (!--k) return tag = root.val;
        dfs(root.left);
    }
    dfs(root);
    return tag;
};
```





### 6、[剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

```js
// 判断A树中是否有B树的根节点
// 如果有，判断后续节点是否相同
```

```js
var isSubStructure = function (A, B) {
    // A、B不为空 并且（从A的根开始包含B || A的左子树中包含B || A的右子树中包含B）
    return (!!A && !!B) && (recur(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B));
};

// 判断A是否包含B
const recur = function (A, B) {
    // B 为空
    if (!B) return true;

    // A、B 不为空，并且AB没有相同的
    if (!A || A.val != B.val) return false;

    // A的左边是否和B的左边相等，A的右边是否和B的右边相等
    return recur(A.left, B.left) && recur(A.right, B.right);
}
```



### 7、[968. 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)????????????



### 8、[662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

[最大数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

```js
// 最大宽度：最底层的最右边 - 最左边 + 1 

// 给 root 编号为 1
// root.left = 2root = 2 
// root.right = 2root + 1 = 3
```

```js
var widthOfBinaryTree = function (root) {
    if (!root) return 0;

    // 记录最大宽度
    let max = 1n;

    // 二维数组，存储当前层的序号和节点
    // 子元素为 [ 节点序号 ，当前节点 ] 
    let que = [[0n, root]];

    while (que.length) {
        // 计算宽度
        // 当前曾最右边节点 - 当前层数最左边节点 
        let width = que[que.length - 1][0] - que[0][0] + 1n;
        
        // 计算出的宽度大于保存的最大宽度，给最大宽度赋值
        if (width > max) max = width;

        // 记录下一层节点
        let temp = [];
        
        // 进入下一层，记录下一层节点
        for (const [i, q] of que) {
            q.left && temp.push([i * 2n, q.left]);
            q.right && temp.push([i * 2n + 1n, q.right]);
        }

        // 下一层节点赋值给 que
        que = temp;
    }

    return Number(max);
};
```



