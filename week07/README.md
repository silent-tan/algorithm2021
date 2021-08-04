## 200. 岛屿数量

### 解题思路

主体思路：

岛屿数量 = 并查集连通图数量 - 水位

细节：

一维表示二维标识：x * row + col

### 代码

```cpp
class DisjoinSet {
public:
    int size = 0;
    DisjoinSet(int n) {
        size = n;
        fa = vector<int>(n);
        for(int i = 0; i < n; i++) {
            fa[i] = i;
        }
    }
    int find(int x) {
        if (x == fa[x]) return x;
        fa[x] = find(fa[x]);
        return fa[x];
    }
    void unionSet(int x, int y) {
        int xFa = find(x);
        int yFa = find(y);
        if (xFa != yFa) fa[xFa] = yFa;
    }
    vector<int> getFa() {
        return fa;
    }
private:
    vector<int> fa;
};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        // 按顺序建立并查集标识
        // 行
        int row = grid.size();
        if (row == 0) {
            return 0;
        }
        // 列
        int col = grid[0].size();
        int water = 0;
        // 建立并查集
        DisjoinSet ds = DisjoinSet(row * col);
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == '0') {
                    water++;
                } else {
                    // 向下合并
                    if (i + 1 < row && grid[i+1][j] == '1') {
                        int x = i * col + j;
                        int y = (i + 1) * col + j;
                        ds.unionSet(x, y);
                    }
                    // 向右合并
                    if (j + 1 < col && grid[i][j + 1] == '1') {
                        int x = i * col + j;
                        int y = i * col + (j + 1);
                        ds.unionSet(x, y);
                    }
                }
            }
        }

        int ans = 0;
        for (int i = 0; i < ds.size; i++) {
            if (ds.find(i) == i) ans++;
        }
        return ans - water;
    }
};

```

## 684. 冗余连接

### 解题思路

主体思路：

- 如果遍历中根节点不相同，则在未联通前不存在环
- 如果遍历中根节点相同，则说明在未进行遍历前，该边已经联通

### 代码

```cpp
class DisjoinSet {
public:
    int size = 0;
    DisjoinSet(int n) {
        size = n;
        fa = vector<int>(n);
        for(int i = 0; i < n; i++) {
            fa[i] = i;
        }
    }
    int find(int x) {
        if (x == fa[x]) return x;
        fa[x] = find(fa[x]);
        return fa[x];
    }
    void unionSet(int x, int y) {
        int xFa = find(x);
        int yFa = find(y);
        if (xFa != yFa) fa[xFa] = yFa;
    }
    vector<int> getFa() {
        return fa;
    }
private:
    vector<int> fa;
};

class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        // 点最多比数组长度多 1
        DisjoinSet ds = DisjoinSet(edges.size() + 1);

        for (auto& edge: edges) {
            int n1 = edge[0];
            int n2 = edge[1];

            if (ds.find(n1) != ds.find(n2)) {
                ds.unionSet(n1, n2);
            } else {
                return edge;
            }
        }

        return {};
    }
};
```
