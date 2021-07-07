## 106. 从中序与后序遍历序列构造二叉树

### 解题思路
**主体思路**

1. 后续遍历 postorder = [9, 15,  7, 20, 3]   => 跟节点为 3
2. 中序遍历 inorder     = [9,  3, 15, 20, 7]   => 左树为 [9],右子树 [15, 20, 7]
3. 由中序遍历知道左树大小为1，结合后续遍历，可以得到后序遍历的左子树为 [9], 右子树为 [15,7,20]
4. 通过上述，可以知道左子树后续遍历为 [9], 中序遍历为 [9]；右子树后续遍历为[15,7,20]，中序遍历为 [15,20,7]

**细节**

- 需要准备把握子树的起点和终点

### 代码

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return build(postorder, 0, postorder.size() - 1, inorder, 0, inorder.size() - 1);
    }

    /**
     * 根据中序和后续遍历还原二叉树
     *                     l1             r1
     * 后续遍历 postorder = [9, 15,  7, 20, 3]   => 跟节点为 3
     * 中序遍历 inorder   = [9,  3, 15, 20, 7]   => 左树为 [9],右子树 [15, 20, 7]
     *                     l2  mid        r2
     * 
     */
    TreeNode* build(vector<int>& postorder, int l1, int r1, vector<int>& inorder, int l2, int r2) {
        if (l1 > r1) {
            return nullptr;
        }
        // 后序遍历的最后一个元素是 root 节点
        TreeNode* root = new TreeNode(postorder[r1]);

        // 通过遍历查询 root 节点在中序遍历 inorder 中的位置，确认左右子树大小
        int mid = l2;
        while (inorder[mid] != root->val) {
            mid++;
        };

        // 左子树大小
        int leftSize = mid - l2;

        // 还原左子树
        root->left = build(postorder, l1, l1 + leftSize - 1, inorder, l2, mid - 1);
        // 还原右子树
        root->right = build(postorder,l1 + leftSize, r1 - 1, inorder, mid + 1, r2);
        return root;
    }
};
```
