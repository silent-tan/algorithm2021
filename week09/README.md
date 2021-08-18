# 1091. 二进制矩阵中的最短路径
### 解题思路

**主体思路**
BFS 解法

**细节**
在原数组上维护当前走过的步长

### 代码

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        if (grid[0][0] == 1) {
            return -1;
        }
        int n = grid.size(); // n * n

        // 大小为1
        if (n == 1) {
            return 1;
        }
        // 无法到达终点
        if (grid[n-1][n-1] == 1) {
            return -1;
        }

        // 方向
        //                 →  ↘   ↓    ↙   ←   ↖  ↑  ↗
        const int dx[8] = {1, 1,  0,  -1, -1, -1, 0, 1};
        const int dy[8] = {0, -1, -1, -1,  0,  1, 1, 1};

        // bfs 队列
        queue<pair<int, int>> q;
        // 从 (0,0) 开始
        q.emplace(0, 0);
        // 标记已经走过的步数
        grid[0][0] = 1; 
        
        // bfs
        // grid[n-1][n-1] == 0 表示还没有走到最后一步
        while(!q.empty() && grid[n-1][n-1] == 0) {
            auto cur = q.front();
            int x = cur.first;
            int y = cur.second;
            q.pop();
            // 当前路径长
            int len = grid[x][y];

            // 8个方向遍历
            for (int i = 0; i < 8; i++) {
                int next_x = x + dx[i];
                int next_y = y + dy[i];

                // 越界处理
                if (next_x < 0 || next_x >= n || next_y < 0 || next_y >= n) {
                    continue;
                }

                // 如果可以走，则进入队列，标记已走步数
                if (grid[next_x][next_y] == 0) {
                    q.emplace(next_x, next_y);
                    grid[next_x][next_y] = len + 1;
                }
            }
        }

        return grid[n - 1][n - 1] == 0 ? -1 : grid[n-1][n-1];
    }
};
```
