## 151. 翻转字符串里的单词
### 解题思路
**主体思路**

- 倒序遍历
- 每次循环搜索一个单词，空格跳过，非空格累计单词长度

### 代码

```cpp
class Solution {
public:
    string reverseWords(string s) {
        string ans;
        // 索引
        int index = s.size() - 1;
        while(index >= 0) {
            int len = 0;
            // 如果当前遍历的是字符，则忽略该字符
            while (index >= 0 && s[index] == ' ') index--;
            while (index >= 0 && s[index] != ' ') {
                len++;
                index--;
            }
            if (len) {
                // 注意，1. index 被多减少了一次；2. 最后会多一个空格
                ans += getStr(s, index + 1, len) + " ";
            }
        }
        return getStr(ans, 0, ans.size() - 1);
    }
private:
    // 获取子串
    string getStr(string s, int index, int len) {
        string subs;
        int last = s.size() - 1;
        while(len > 0 && index <= last) {
            subs += s[index++];
            len--;
        }
        return subs;
    }
};
```

```typescript
function reverseWords(s: string): string {
    return s.split(' ').map(str => str.trim()).filter(str => str != '').reverse().join(' ')
};
```
