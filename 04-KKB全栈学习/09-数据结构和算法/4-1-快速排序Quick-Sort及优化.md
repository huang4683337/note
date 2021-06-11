

## 什么是快速排序

```
初始数据：arr = [5 7 8 6 4 3 1 2 7 5]
```

```
- 选一个基准值
- 小于基准值，放在前面
- 大于基准值，放在后面
```

```
- 随便选一个基准值，并将他移出去
- 我们数组第一个元素：5
arr = [null 7 8 6 4 3 1 2 7 5]

- 定义两个指针，头指针：arr[0] ， 尾指针：arr[length-1]

- 判断尾指针指向的值是否小于基准值，小于则放在前面，大于继续前移
- 尾指针：arr[length-1] 前移，arr[8] = 2 时，小于基准值
	- 放在前面：头指针 arr[0] = arr[8], arr[8] = null
	- 头指针后移一位, 头指针为 arr[1]
- 判断头指针指向的值是否大于基准值，大于则放在后面，小于继续后移
- 头指针 arr[1] = 7，大于基准值
	- 放在后面：arr[8] = arr[1], arr[1] = null
	- 尾指针前移一位：尾指针为 arr[7]
	
- 头尾指针交替各走一步
- 头指针索引 >= 尾指针索引时
- 将移除的基准值放入

- 进行递归得到结果
```





## 先放一个例子

```js
function quickSort(arr) {
    if (arr.length < 1) {
        return arr
    }
    var mid = Math.floor(arr.length / 2)
    var temp = arr.splice(mid, 1)
    var left = []
    var right = []
    for (var i = 0; i < arr.length; i++) {
        if (arr[i] < temp) {
            left.push(arr[i])
        } else if (arr[i] >= temp) {
            right.push(arr[i])
        }
    }
    return quickSort(left).concat(temp, quickSort(right))
}

var arr = [5, 3, 9, 4, 2, 0]
console.log(quickSort(arr))
```

