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
