## 剑指offer刷题记录


### 03 数组中重复的数组
方法：哈希表，下标元素交换

### 04 二维数组中的查找
方法: 左右有序，上下有序，类似于二分查找的方式增行减列缩小查找范围

### 05 替换空格
方法: 遍历替换，如果原地替换的话先扩容，采用双指针形式从后往前遍历

### 06 从尾到头打印链表
方法: 单链表从左到右遍历，使用栈存储遍历结果，然后以出栈的形式返回结果,利用好栈的先进后出的性质

### 07 重建二叉树
方法：基于先序遍历和中序遍历重建二叉树，先找根节点；递归或者迭代的方式；


### 09 用两个栈实现队列
方法：模拟法，基于栈的后进先出的性质，先入栈再出栈保存到栈2后再出栈，模拟队列的先进先出的性质


### 10-I 斐波拉契数列
方法：递归和迭代法，因为只与相邻的两元素结果相关，使用迭代法更好，时间复杂度和空间复杂度都低

### 10-II 青蛙跳台阶问题
方法：递归和迭代法和动态规划，因为只与相邻的两元素结果相关，且状态转移方程给出，但使用迭代法更好，时间复杂度和空间复杂度都低

### 11-旋转数组的最小数字
方法：数组本身是有序数组，经过旋转后依然还是有有序这一性质，只不过需要分段有序，有序数组查找元素使用二分法不断去缩小查找空间，注意是和右边进行比较

### 12-矩阵中的路径
方法：回溯法+递归，通过上下左右遍历判断是否继续朝下搜索，如果不对，则向上回溯

### 13-机器人的运动范围
方法：广度优选遍历+栈+循环迭代，通过上下左右遍历判断是否继续朝下搜索，如果继续的话，将该点入栈

### 14-I-剪绳子
方法：动态规划，或者贪心算法尽可能分成长度为3的小段
