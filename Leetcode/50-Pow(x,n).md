 ### 50. Pow(x, n)
来源：力扣（LeetCode）

链接: [https://leetcode.cn/problems/powx-n/](https://leetcode.cn/problems/powx-n/)

实现 pow(x, n) ，即计算 x 的整数 n 次幂函数（即，$x^n$ ）。

 

**示例 1：**
```
输入：x = 2.00000, n = 10
输出：1024.00000
```

**示例 2：**
```
输入：x = 2.10000, n = 3
输出：9.26100
```

**示例 3：**
```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2^-2 = 1/2^2 = 1/4 = 0.25
```

**提示：**
* -100.0 < x < 100.0
* $-2^{31}$ <= n <= $2^31$-1
* $-10^4$ <= $x^n$ <= $10^4$

### 解法
* **暴力法**: for循环，n次相乘，时间复杂度O(N)
*  **递归**：分治法，分奇偶 
	- 分治法递归
	- 指数二进制分治法循环
	![在这里插入图片描述](https://img-blog.csdnimg.cn/ac9effffbdd24bcba180ee58dfbfccbc.png)

	![在这里插入图片描述](https://img-blog.csdnimg.cn/0d215da1133148a1a28b020b0220d487.png)
	注意x与n的边界，x为0，n正负问题，这里$0^0$约定于0


### 代码实现
#### 递归
**python实现**
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if x == 0:
            return 0
        
        if n == 0:
            return 1
        
        if n < 0:
            n = -n
            x = 1 / x
        
        def helper(x: float, n: int) -> float:
            if n == 0:
                return 1
            
            sub_res = helper(x, n//2)
            return x * sub_res * sub_res if n & 1 else sub_res * sub_res
        return helper(x, n)
```


**c++实现**
```cpp
class Solution {
public:
    double pow(double x, long long n) {
        if (n == 0) return 1;
        double sub_res = pow(x, n/2);
        return n&1 ? x * sub_res * sub_res : sub_res * sub_res;
    }

    double myPow(double x, int n) {
        if (x == 0) return 0;
        if (n == 0) return 1;

        if (n < 0) {
            long long n = -1 * n;
            x = 1 / x;
        }

        return pow(x, n);
    }
};
```


**复杂度分析**
* 时间复杂度： $O(logN)$   
* 空间复杂度： $O(logN)$ 

#### 二进制分治法迭代
**python实现**
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if x == 0:
            return 0
        
        if n == 0:
            return 1
        
        if n < 0:
            n = -n
            x = 1 / x
        
        res = 1.0

        while n:
            if n & 1:
                res *= x
            x *= x
            n >>= 1
        return res
```

**c++实现**
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if (x == 0) return 0;
        if (n == 0) return 1;
        long int N = n;
        if (n < 1) {
            N = -N;
            x = 1/x;
        }
        
        double res = 1.0;
        while (N) {
            if (N & 1)
                res *= x;
            x *= x;
            N >>= 1;
        }
        return res;
    }
};
```

**复杂度分析**
* 时间复杂度： $O(logN)$   
* 空间复杂度： $O(1)$ 

### 参考
*  [https://leetcode.cn/problems/powx-n/solution/50-powx-n-kuai-su-mi-qing-xi-tu-jie-by-jyd/](https://leetcode.cn/problems/powx-n/solution/50-powx-n-kuai-su-mi-qing-xi-tu-jie-by-jyd/)