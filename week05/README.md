## 1011. 在 D 天内送达包裹的能力
### 解题思路
**主体思路**
- 具有理论上的两端极值
- 具有满足条件变更中位值

故可以利用二分法

**细节**
1. 最低运值是最大重量
2. 最高运值是一天运完
3. 返回值取高值

### 代码

```cpp
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        // 最低算力 是 最大的重量
        int _min = 0;
        // 最大算力 是 1天全部运完
        int _max = 0;
        
        for (auto w: weights) {
            _min = max(_min, w);
            _max += w;
        }

        int l = _min, h = _max;
        while(l < h) {
            int mid = (l + h) / 2;
            // 如果满足则运力过剩
            if (satisfy(weights, days, mid)) {
                h = mid;
            } else {
                l = mid + 1;
            }
        }
        return h;
    }
private:
    /**
     * 如果按照最低搬运力进行搬运能够满足
     * 实际使用时间 <= days
     * 则满足二分条件
     */
    bool satisfy(vector<int>& weights, int days, int min) {
        int n = weights.size();
        // 搬运的第x天
        int cur = 1;
        // 当天搬运总量
        int sum = 0;
        for (int i = 0; i < n; sum = 0) {
            // 当前已经无法搬运
            while(i < n && sum + weights[i] <= min) {
                sum += weights[i];
                i++;
            }
            cur++;
        }
        return cur - 1 <= days;
    }
};
```

## 875. 爱吃香蕉的珂珂

### 解题思路
**主体思路**
- 存在两个极值
- 存在满足条件

故使用二分法

**细节**
- 二分法端点不用关注精确值，故取最小值为1
- a/b 向上取整: (a+b-1)/b

### 代码

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        // 最小速度为1
        int _min = 1;
        // 比目标速度还要快的速度可以吃完香蕉的速度为：1小时内能吃完一堆，也即最大堆的数量(piles.length <= H)
        int _max = 0;
        for(auto p: piles) {
            _max = max(_max, p);
        }
        // 二分法模板
        int l = _min, r = _max;
        while(l < r) {
            int mid = (l + r) / 2;
            if (satify(piles, h, mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return r;
    }
private:
    bool satify(vector<int>& piles, int h, int K) {
        // 当前时间
        int cur = 0;
        for (auto p: piles) {
            // 当前缀需要x小时吃完
            cur += (p + K - 1) / K;
        }

        return cur <= h;
    }
};
```
