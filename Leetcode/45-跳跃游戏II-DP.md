### 45. 跳跃游戏 II
来源：力扣（LeetCode）

链接: [https://leetcode.cn/problems/jump-game-ii/](https://leetcode.cn/problems/jump-game-ii/)



给你一个非负整数数组 nums ，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

假设你总是可以到达数组的最后一个位置。

 

**示例 1:**
```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**
```
输入: nums = [2,3,0,1,4]
输出: 2
```

**提示:**

* 1 <= nums.length <= $10^4$
* 0 <= nums[i] <= 1000


### 解法
这题有多种解法，之前介绍过贪心的解法，可以找找，这里只介绍动态规划的解法。
* 动态规划：前后有关联的情况，依赖之前的状况，可以考虑使用动态规划方法，
	**动态规划：四个步骤：**
	- **问题定义**
	- **状态转移方程**
	- **初始条件和边界情况**
	- **确定计算顺序（自顶向下，还是自下向上）**

**问题定义：**
dp[i] 表示位置i的时候需要的最小次数


**状态转移方程：**
* dp = [float('inf')] * n
* dp[i] = min(dp[i], dp[j]+1, j<i if j + nums[j] >= i

从i之前的位置找一个 如果j+nums[j] 能够到达i，则只需要从该位置+1就能到达i位置



**初始化条件和边界条件**
* dp[0] = nums[0]

**确定计算顺序**:
这个是从下向上的方向计算即可

### 代码实现
#### 动态规划
**python实现**
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        if n <= 1:
            return 0
        
        dp = [float('inf')] * n  # 每个下标需要走的次数
        dp[0] = 0
        j = 0
        for i in range(1, n):
            while j + nums[j] < i:
                j += 1
            dp[i] = min(dp[i], dp[j]+1)
        return dp[-1]
```


**c++实现**
```cpp
class Solution {
public:
    int jump(vector<int>& nums) { 
        int n = nums.size();
        if (n<=1) return 0;
        vector<int> dp(n, INT_MAX);
        dp[0] = 0;
        int j = 0;
        for (int i=1; i<n; i++) {
            while (j + nums[j] < i) j++;  // while 比for节省时间
            dp[i] = min(dp[i], dp[j]+1);
        }
        return dp[n-1];
    }
};
```


**复杂度分析**
* 时间复杂度： $O(N)$
* 空间复杂度： $O(N)$  

### 参考
* [https://leetcode.cn/problems/jump-game-ii/solution/by-itcharge-xn4b/](https://leetcode.cn/problems/jump-game-ii/solution/by-itcharge-xn4b/)