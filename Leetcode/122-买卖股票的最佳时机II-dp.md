 ### 122. 买卖股票的最佳时机 II
来源：力扣（LeetCode）

链接: [https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。(**无限次交易次数**)

返回 你能获得的 **最大利润** 。

 

**示例 1：**
```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例 2：**
```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例 3：**
```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

**提示：**
* 1 <= prices.length <= $3 * 10^4$
* 0 <= prices[i] <= $10^4$


### 解法
* 动态规划：涉及状态，前后又有关联的情况，考虑动态规划方法
	动态规划：四个步骤：
	- **问题定义**
	- **状态转移方程**
	- **初始条件和边界情况**
	- **确定计算顺序（自顶向下，还是自下向上）**

**问题定义：**
每天都有两种状态，当天买了还是卖了，因此考虑使用两个变量进行标识，`dp_i_0`表示当天手上没有股票，没有股票意味着两种情况，之前没有买，今天也没有买，或者之前买了，今天卖了。`dp_i_1`表示当天手上有股票，有股票也有两种情况，昨天买了，今天不买，或者昨天没有买，今天买了。


**状态转移方程：**
* dp_i_0 = max(dp_i_0, dp_i_1+prices[i])
* dp_i_1 = max(dp_i_1, dp_i_0-prices[i])  
这里由于没有限制交易次数，所以之前可能会有盈余，使用dp_i_0-prices[i]



**初始化条件和边界条件**
* `dp_i_0 = 0`
* `dp_i_1 = float('-inf')`

**确定计算顺序**:
这个是从下向上的方向计算即可

### 代码实现
#### 动态规划
**python实现**
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 1:
            return 0

        dp_i_0 = 0
        dp_i_1 = float('-inf')
        for i in range(n):
            dp_i_0 = max(dp_i_0, dp_i_1+prices[i])
            dp_i_1 = max(dp_i_1, dp_i_0-prices[i])
        return dp_i_0
```


**c++实现**
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 1) return 0;
        int dp_i_0 = 0, dp_i_1 = INT_MIN;
        for(int price: prices) {
            dp_i_0 = max(dp_i_0, dp_i_1+price);
            dp_i_1 = max(dp_i_1, dp_i_0-price);
        }
        return dp_i_0;
    }
};
```


**复杂度分析**
* 时间复杂度： $O(N)$
* 空间复杂度： $O(1)$  

### 参考
* [https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-leetcode-s/](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-leetcode-s/)