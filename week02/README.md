# 第二周作业

## 811. 子域名访问计数

### 解题思路
主体思路：

1. 遍历所有域名对，通过空格分割获取域名访问次数 和 域名
2. 判断域名是否存在父域名，如果存在递归父域名

### 代码

```typescript
type Domain = string
type DomainCount = number
type CountMap = Map<Domain, DomainCount>


function visit(domain: string, count: number, map: CountMap): void {
    if (map.get(domain)) {
        map.set(domain, map.get(domain) + count)
    } else {
        map.set(domain, count);
    }
    const matches = domain.match(/^.*?\.(.*)$/)
    if (matches && matches[1]) {
        visit(matches[1], count, map);
    }
}

function subdomainVisits(cpdomains: string[]): string[] {
    const map: CountMap = new Map();
    const ans: string[] = [];
    cpdomains.forEach(cpdomain => {
        const [countStr, domain] = cpdomain.split(' ');
        visit(domain, parseInt(countStr, 10), map);
    })
    for(const [domain, count] of map.entries()) {
        ans.push(`${count} ${domain}`)
    }
    return ans;
};
```

## 697. 数组的度

### 解题思路
主体思路：
1. 先求出数组的度
2. 求拥有最大度的元素
3. 在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组 => 拥有最大度元素的第一个索引 至 拥有最大度元素的最后一个索引 组成的子段

细节：
拥有最大度的元素不是唯一的，所以需要比较拥有所有最大度元素对应的最短连续子数组

### 代码

```typescript
function findShortArray(nums: number[], num: number): number[] {
    const start = nums.indexOf(num);
    const end = nums.lastIndexOf(num);
    return nums.slice(start, end + 1);
}

function findShortestSubArray(nums: number[]): number {
    // 求数组的度
    const map: Map<number, number> = new Map();
    let max = 0;
    nums.forEach(num => {
        if (!map.get(num)) {
            map.set(num, 0);
        }
        const count = map.get(num) + 1;
        if (count > max) {
            max = count
        }
        map.set(num, count);
    });

    // 存储拥有最大度的元素
    const maxMap: Map<number, number> = new Map([...map].filter(([k, v]) => v === max));

    let ansArray = nums;

    maxMap.forEach((count, num) => {
        if (count === max) {
            const shortArray = findShortArray(nums, num);
            if (shortArray.length <= ansArray.length) {
                ansArray = shortArray;
            }
        }
        
    });

    return ansArray.length;
};
```
