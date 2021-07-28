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

## 55. 跳跃游戏
### 解题思路
**主体思路**

如果可以到达第 n 个位置，必然可以到达 n - 1 个位置 可以推导 如果能够到达第 n 个位置，必然可以到达任意位置

**细节**

如果当前位置比最远跳跃距离大，证明该位置不可到达，即最后的位置不可到达

### 代码

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        // 从起点开始，对每个起跳点起跳，然后更新能够调到的最大位置
        int max_jump = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > max_jump) {
                return false;
            }
            max_jump = max(max_jump, i + nums[i]);
        }
        return true;
    }
};
```
