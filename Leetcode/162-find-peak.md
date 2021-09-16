题目

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-peak-element

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞ 。

你必须实现时间复杂度为 O(log n) 的算法来解决此问题。

 

示例 1：

> 输入：nums = [1,2,3,1]
> 输出：2
> 解释：3 是峰值元素，你的函数应该返回其索引 2。



示例 2：

> Ss输入：nums = [1,2,1,3,5,6,4]
> 输出：1 或 5 
> 解释：你的函数可以返回索引 1，其峰值元素为 2；
>      或者返回索引 5， 其峰值元素为 6。



提示：

> 1 <= nums.length <= 1000
> -231 <= nums[i] <= 231 - 1
> 对于所有有效的 i 都有 nums[i] != nums[i + 1]






### 解法

方法1:

由于题目保证了nums[i] != nums[i+1]，那么数组ums 中最大值两侧的元素一定严格小于最大值本身。因此，最大值所在的位置就是一个可行的峰值位置。

我们对数组 nums 进行一次遍历，找到最大值对应的位置即可。



* python

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        n = len(nums)
        '''
        idx = 0
        for i in range(1, n):
            if nums[i] > nums[idx]:
                idx = i
        return idx
        '''
        max_value = max(nums)
        return nums.index(max_value)
```



方法2:

俗话说「人往高处走，水往低处流」。如果我们从一个位置开始，不断地向高处走，那么最终一定可以到达一个峰值位置。

因此，我们首先在 [0,n) 的范围内随机一个初始位置 i，随后根据nums[i−1],nums[i],nums[i+1] 三者的关系决定向哪个方向

如果nums[i−1]<nums[i]>nums[i+1]，那么位置 i 就是峰值位置，我们可以直接返回 i 作为答案；

如果 nums[i−1]<nums[i]<nums[i+1]，那么位置 i 处于上坡，我们需要往右走，即 i+1←i；

如果 nums[i−1]>nums[i]>nums[i+1]，那么位置 i 处于下坡，我们需要往左走，即i-1←i；

如果nums[i−1]>nums[i]<nums[i+1]，那么位置 i 位于山谷，两侧都是上坡，我们可以朝任意方向走。

如果我们规定对于最后一种情况往右走，那么当位置 ii不是峰值位置时：

如果 nums[i]<nums[i+1]，那么我们往右走；

如果 nums[i]>nums[i+1]，那么我们往左走。




* python

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        n = len(nums)
        idx = random.randint(0, n - 1)

        # 辅助函数，输入下标 i，返回 nums[i] 的值
        # 方便处理 nums[-1] 以及 nums[n] 的边界情况
        def get(i: int) -> int:
            if i == -1 or i == n:
                return float('-inf')
            return nums[i]
        
        while not (get(idx - 1) < get(idx) > get(idx + 1)):
            if get(idx) < get(idx + 1):
                idx += 1
            else:
                idx -= 1
        
        return idx
```



方法3:

可以发现，如果nums[i]<nums[i+1]，并且我们从位置 i 向右走到了位置 i+1，那么位置 i左侧的所有位置是不可能在后续的迭代中走到的。

这是因为我们每次向左或向右移动一个位置，要想「折返」到位置 i 以及其左侧的位置，我们首先需要在位置 i+1向左走到位置 i，但这是不可能的。

并且根据方法二，我们知道位置i+1 以及其右侧的位置中一定有一个峰值，因此我们可以设计出如下的一个算法：

对于当前可行的下标范围 l, r，我们随机一个下标 i；

如果下标 i 是峰值，我们返回 i 作为答案；

如果 nums[i]<nums[i+1]，那么我们抛弃[ l,i]的范围，在剩余[ i+1,r] 的范围内继续随机选取下标；

如果 nums[i]>nums[i+1]，那么我们抛弃[ i,r]的范围，在剩余[l,i−1] 的范围内继续随机选取下标。

在上述算法中，如果我们固定选取 i 为[l,r] 的中点，那么每次可行的下标范围会减少一半，成为一个类似二分查找的方法，时间复杂度为 O(logn)。

* python

  ```python
  class Solution:
      def findPeakElement(self, nums: List[int]) -> int:
          n = len(nums)
  
          # 辅助函数，输入下标 i，返回 nums[i] 的值
          # 方便处理 nums[-1] 以及 nums[n] 的边界情况
          def get(i: int) -> int:
              if i == -1 or i == n:
                  return float('-inf')
              return nums[i]
        
          left, right, ans = 0, n - 1, -1
          while left <= right:
              mid = left + (right-left) // 2
              if get(mid - 1) < get(mid) > get(mid + 1):
                  ans = mid
                  break
              if get(mid) < get(mid + 1):
                  left = mid + 1
              else:
                  right = mid - 1
          
          return ans
  ```

  



#### 复杂度分析

方法1:

* 时间复杂度：O(n)，其中 n 是数组nums 的长度。
* 空间复杂度：O(1)

方法2:

* 时间复杂度：O(n)，其中 n 是数组nums 的长度。在最坏情况下，数组nums 单调递增，并且随机到位置 0，这样就需要向右走到数组 nums 的最后一个位置。
* 空间复杂度：O(1)

方法3:

* 时间复杂度：O(logn)，其中 n 是数组nums 的长度。
* 空间复杂度：O(1)

