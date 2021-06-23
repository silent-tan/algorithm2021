## 66. 加1

### 解题思路
![image.png](https://pic.leetcode-cn.com/1624435463-sjGIaV-image.png)

主体解法思路为：末位 + 1 结果进1位判断

优化：进位数最大值不会超过1，如果进位数为0意味着其他位不会变动，可以直接结束循环

### 代码

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int size = digits.size();
        // 最低位 + 1
        int tap = 1;
        for(int i = size - 1; i >= 0; i--) {
            if (tap == 0) { 
                break;
            }
            digits[i] += tap;
            tap = digits[i] / 10;
            digits[i] %= 10;

            

            // 如果最高位依然超出9，数组前插入1
            if (i == 0 && tap != 0) {
                digits.insert(digits.begin(), tap);
            }
        }

        return digits;

    }
};
```

## 560. 和为K的子数组
### 解题思路

1. 假设数组为 nums, 对于连续子数组 [nums[j], nums[j+1], ..., nums[i]] (0 <= j <= i )的元素和为 k
2. 假设有数组 sums, sums[j] 表示 nums 数组下标 0 到下标j的元素和；sums[i] 表示 nums 数组下标0 到 i 的元素和
3. 那么对于 k, k = sums[i] - sums[j-1]，也就是对于求以i为下标的子段和为k的子数组个数，只要满足
    对于下标 i(0~n-1), 有多少个 j(0~i)，使得 sums[i] - sums[j-1] == k
    对于下标 i, 有多少个 s[j] 等于 s[i] - k
    

### 代码

```cpp
class Solution {
public:
    // 分析：
    // k 范围为 [-1e7, 1e7]，那存储 k 的类型需要为有符号类型，并且范围需要大于 [-1e7, 1e7]，所以使用 signed int, -2147483648 到 2147483647
    int subarraySum(vector<int>& nums, int k) {
        
        map<int, int> countmap;
        countmap[0] = 1;
        int count = 0, prev = 0;
        
        for (int num: nums) {
            prev += num;
            if (countmap.find(prev - k) != countmap.end()) {
                count += countmap[prev - k];
            }
            countmap[prev]++;
        }

        return count;
    }
};
```
