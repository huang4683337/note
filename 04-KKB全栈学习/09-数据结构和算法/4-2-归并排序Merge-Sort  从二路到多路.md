## 二路归并排序

将一个数组从中间值开始无限拆分（数组长大于1）成左右两个，然后进行归并

两个有序数组合并成一个有序数组 ==> 双指针对比合并

```shell
# 递归实现
指针 P_A 指向数组 A 的第一位
指针 P_B 指向数组 B 的第一位
建立一个额外的存储数组 C，用来存放A、B合并后的结果
比较指针 P_A 和 P_B 将最小的数字 push 到数组 C, 依此类推
	比如P_A小于P_B，将P_A放入素组c，P_A 指针后移一位，依此类推
```



## 多路归并排序

将两个以上的有序数组合并成一个有序数组



## 归并排序在大数据场景下的应用

问题：

​	电脑内存大小2GB

​	如何对一个 40GB 的文件进行排序

```shell
可以将电脑的内存当作当作额外的存储区，用来存储一个2GB大小的有序结果

将 40GB 拆分成 20 个 2GB 大小的文件，
将这个 2GB 文件进行归并排序
用电脑内存来存放2GB大小的文件的归并排序的结果
将这个结果放入到 40GB 对应的位置
```

![1620912380686](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1620912380686.png)



## 算法题

### 1、[剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

![1620914062194](C:\Users\Amd\AppData\Roaming\Typora\typora-user-images\1620914062194.png)



### 2、[23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)



### 3、[148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```js
使用归并排序实现
```

