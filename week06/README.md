## 70. 爬楼梯
### 解题思路
**主体思路**
如果走到第 n 阶有两种可能：
- 最后走1阶，那么上一个就是 n - 1 阶
- 最后走2阶，那么上一个就是 n - 2 阶

所以第 n 阶有 dp[n] = dp[n-1] + dp[n-2] 种走法

又知道 dp[1] = 1, dp[2] = 2 => dp[0] = 1

**细节**
求第 n 阶，为了数组索引下标考虑，设置数组长度为 n + 1

### 代码

```cpp
class Solution {
public:
    int climbStairs(int n) {
        // n - 1: + 1 阶到 n
        // n - 2: + 2 阶到 n
        // dp[n] = dp[n-1] + dp[n-2]
        // dp[1] = 1
        // dp[2] = 2
        int dp[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};
```
