### 1.Trees

#### Description

A tree is a hierarchical data structure consisting of nodes, with a single root node and zero or more child nodes per node. Common types include binary trees, binary search trees (BST), and N-ary trees.

#### Common Terminology

* **Root**: Topmost node
* **Leaf**: Node with no children
* **Height**: Number of edges on the longest path from root to leaf
* **Depth**: Number of edges from root to the node
* **Parent/Child**: Relationship between nodes

#### Binary Tree Node Structure (C++)

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

#### Traversals

1. **Inorder (LNR)**

```cpp
void inorder(TreeNode* root) {
    if(!root) return;
    inorder(root->left);
    cout << root->val << " ";
    inorder(root->right);
}
```

2. **Preorder (NLR)**

```cpp
void preorder(TreeNode* root) {
    if(!root) return;
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
}
```

3. **Postorder (LRN)**

```cpp
void postorder(TreeNode* root) {
    if(!root) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->val << " ";
}
```

4. **Level Order (BFS)**

```cpp
#include <queue>
void levelOrder(TreeNode* root) {
    if(!root) return;
    queue<TreeNode*> q;
    q.push(root);
    while(!q.empty()) {
        TreeNode* node = q.front(); q.pop();
        cout << node->val << " ";
        if(node->left) q.push(node->left);
        if(node->right) q.push(node->right);
    }
}
```

#### Binary Search Tree (BST) Operations

* **Insertion**

```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if(!root) return new TreeNode(val);
    if(val < root->val) root->left = insertBST(root->left, val);
    else root->right = insertBST(root->right, val);
    return root;
}
```

* **Search**

```cpp
bool searchBST(TreeNode* root, int val) {
    if(!root) return false;
    if(root->val == val) return true;
    if(val < root->val) return searchBST(root->left, val);
    return searchBST(root->right, val);
}
```

* **Deletion** (Node with 0,1,2 children) can be implemented using standard BST delete algorithm.

#### Hints & Tips

* Use recursion for simplicity in traversals.
* Use queue for level-order traversal.
* BST property: left < root < right.

### 2. Maximum Depth of Binary Tree

#### Description

Given a binary tree, find its maximum depth (number of nodes along the longest path from the root to a leaf).

#### Hint

Use recursion to find the depth of left and right subtrees, then take the maximum and add 1 for the current node.

#### Brute Force

* Traverse all paths from root to leaves, keeping track of the maximum depth.
* **TC:** O(n) | **SC:** O(n) due to recursion stack

#### Optimized Approach (DFS)

* Use simple recursion to compute depth.
* **TC:** O(n) | **SC:** O(h) where h is height of tree

```cpp
#include <iostream>
using namespace std;

int maxDepth(TreeNode* root) {
    if(!root) return 0;
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);
    return max(leftDepth, rightDepth) + 1;
}

int main() {
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    cout << maxDepth(root) << endl; // Output: 3
    return 0;
}
```

#### Optimized Approach (BFS)

* Use a queue to perform level-order traversal and count levels.
* **TC:** O(n) | **SC:** O(n)

```cpp
#include <queue>
int maxDepthBFS(TreeNode* root) {
    if(!root) return 0;
    queue<TreeNode*> q;
    q.push(root);
    int depth = 0;
    while(!q.empty()) {
        int sz = q.size();
        for(int i=0;i<sz;i++) {
            TreeNode* node = q.front(); q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        depth++;
    }
    return depth;
}
```

### 3. Same Tree

#### Description

Given two binary trees, check if they are structurally identical and the nodes have the same values.

#### Hint

Use recursion to compare both trees. Trees are the same if the current nodes have the same value and the left and right subtrees are also the same.

#### Brute Force

* Traverse both trees in any order (preorder/inorder) and compare corresponding nodes.
* **TC:** O(n) | **SC:** O(n) due to recursion stack

#### Optimized Approach (DFS)

* Recursively check if both nodes are null, or both exist with equal values and recursively check left and right children.
* **TC:** O(n) | **SC:** O(h) where h is the height of the tree

```cpp
#include <iostream>
using namespace std;

bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p && !q) return true;
    if(!p || !q) return false;
    if(p->val != q->val) return false;
    return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}

int main() {
    TreeNode* tree1 = new TreeNode(1);
    tree1->left = new TreeNode(2);
    tree1->right = new TreeNode(3);

    TreeNode* tree2 = new TreeNode(1);
    tree2->left = new TreeNode(2);
    tree2->right = new TreeNode(3);

    cout << (isSameTree(tree1, tree2) ? "True" : "False") << endl; // True
    return 0;
}
```

#### Optimized Approach (BFS)

* Use two queues for level-order traversal of both trees simultaneously.
* Compare nodes at each level.
* **TC:** O(n) | **SC:** O(n)

```cpp
#include <queue>
bool isSameTreeBFS(TreeNode* p, TreeNode* q) {
    queue<TreeNode*> qp, qq;
    qp.push(p); qq.push(q);
    while(!qp.empty() && !qq.empty()) {
        TreeNode* n1 = qp.front(); qp.pop();
        TreeNode* n2 = qq.front(); qq.pop();
        if(!n1 && !n2) continue;
        if(!n1 || !n2) return false;
        if(n1->val != n2->val) return false;
        qp.push(n1->left); qq.push(n2->left);
        qp.push(n1->right); qq.push(n2->right);
    }
    return qp.empty() && qq.empty();
}
```
### 4. Symmetric Tree

#### Description

Given a binary tree, check whether it is a mirror of itself (symmetric around its center).

#### Hint

Compare the left and right subtrees recursively. The tree is symmetric if the left subtree is a mirror of the right subtree.

#### Brute Force

* Traverse the tree and compare the left and right subtrees at each level.
* **TC:** O(n) | **SC:** O(n) due to recursion stack

#### Optimized Approach (DFS)

* Recursively check if the left child of the left subtree equals the right child of the right subtree and vice versa.
* **TC:** O(n) | **SC:** O(h)

```cpp
#include <iostream>
using namespace std;

bool isMirror(TreeNode* t1, TreeNode* t2) {
    if(!t1 && !t2) return true;
    if(!t1 || !t2) return false;
    return (t1->val == t2->val) && isMirror(t1->left, t2->right) && isMirror(t1->right, t2->left);
}

bool isSymmetric(TreeNode* root) {
    if(!root) return true;
    return isMirror(root->left, root->right);
}

int main() {
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(2);
    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);
    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(3);

    cout << (isSymmetric(root) ? "True" : "False") << endl; // True
    return 0;
}
```

#### Optimized Approach (BFS)

* Use a queue to perform iterative comparison of mirrored nodes.
* **TC:** O(n) | **SC:** O(n)

```cpp
#include <queue>
bool isSymmetricBFS(TreeNode* root) {
    if(!root) return true;
    queue<TreeNode*> q;
    q.push(root->left);
    q.push(root->right);
    while(!q.empty()) {
        TreeNode* t1 = q.front(); q.pop();
        TreeNode* t2 = q.front(); q.pop();
        if(!t1 && !t2) continue;
        if(!t1 || !t2) return false;
        if(t1->val != t2->val) return false;
        q.push(t1->left); q.push(t2->right);
        q.push(t1->right); q.push(t2->left);
    }
    return true;
}
```

### 5. Invert / Flip Binary Tree

#### Description

Given a binary tree, invert it (flip it around its center). Swap left and right children of all nodes.

#### Hint

Use recursion or BFS to swap left and right children at each node.

#### Brute Force

* Traverse each node and manually swap left and right children.
* **TC:** O(n) | **SC:** O(n) due to recursion stack

#### Optimized Approach (DFS)

* Recursively swap left and right subtrees.
* **TC:** O(n) | **SC:** O(h) where h is height of tree

```cpp
#include <iostream>
using namespace std;

TreeNode* invertTree(TreeNode* root) {
    if(!root) return nullptr;
    swap(root->left, root->right);
    invertTree(root->left);
    invertTree(root->right);
    return root;
}

int main() {
    TreeNode* root = new TreeNode(4);
    root->left = new TreeNode(2);
    root->right = new TreeNode(7);
    root->left->left = new TreeNode(1);
    root->left->right = new TreeNode(3);
    root->right->left = new TreeNode(6);
    root->right->right = new TreeNode(9);

    TreeNode* inverted = invertTree(root);
    // You can use any traversal to print inverted tree
    return 0;
}
```

#### Optimized Approach (BFS)

* Use a queue to perform level-order traversal and swap children iteratively.
* **TC:** O(n) | **SC:** O(n)

```cpp
#include <queue>
TreeNode* invertTreeBFS(TreeNode* root) {
    if(!root) return nullptr;
    queue<TreeNode*> q;
    q.push(root);
    while(!q.empty()) {
        TreeNode* node = q.front(); q.pop();
        swap(node->left, node->right);
        if(node->left) q.push(node->left);
        if(node->right) q.push(node->right);
    }
    return root;
}
```
### 6. Balanced Binary Tree

#### Description

Given a binary tree, determine if it is height-balanced. A binary tree is balanced if the heights of the two child subtrees of every node differ by no more than 1.

#### Hint

Use recursion to compute height and check balance at the same time.

#### Approach (DFS)

* Compute height of left and right subtrees.
* If difference > 1, tree is not balanced.
* **TC:** O(n) | **SC:** O(h)

```cpp
#include <iostream>
using namespace std;

pair<bool, int> checkBalance(TreeNode* root) {
    if(!root) return {true, 0};
    auto left = checkBalance(root->left);
    auto right = checkBalance(root->right);
    bool balanced = left.first && right.first && abs(left.second - right.second) <= 1;
    int height = max(left.second, right.second) + 1;
    return {balanced, height};
}

bool isBalanced(TreeNode* root) {
    return checkBalance(root).first;
}
```

---

### 7. Binary Tree Level Order Traversal

#### Description

Return the level order traversal of a binary tree (all nodes at each depth level).

#### Hint

Use BFS with a queue to traverse level by level.

```cpp
#include <queue>
#include <vector>
using namespace std;

vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    while(!q.empty()) {
        int sz = q.size();
        vector<int> level;
        for(int i=0;i<sz;i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
```

---

### 8. Binary Tree Zigzag Level Order Traversal

#### Description

Return the zigzag level order traversal of a binary tree (alternate left-to-right and right-to-left for each level).

#### Hint

Use BFS and reverse levels on alternate iterations.

```cpp
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    bool leftToRight = true;
    while(!q.empty()) {
        int sz = q.size();
        vector<int> level(sz);
        for(int i=0;i<sz;i++) {
            TreeNode* node = q.front(); q.pop();
            int idx = leftToRight ? i : sz - 1 - i;
            level[idx] = node->val;
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(level);
        leftToRight = !leftToRight;
    }
    return res;
}
```
## 9. Binary Tree Right Side View

### Problem Visualization

```
        1
       / \
      2   3
       \   \
        5   4
```

Right side view → `[1, 3, 4]`

You can see nodes **1 → 3 → 4** when viewing from the right.

### Input / Output

* **Input:** Root of binary tree
* **Output:** Vector of integers (right side view)

### Example

**Input:** `[1,2,3,null,5,null,4]`
**Output:** `[1,3,4]`

### Optimal Solution (BFS)

```cpp
vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front(); q.pop();
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
            if (i == size - 1) res.push_back(node->val);
        }
    }
    return res;
}
```

* **Time:** O(n)
* **Space:** O(n)

---

## 10. Subtree of Another Tree

### Visualization

**Root Tree:**

```
        3
       / \
      4   5
     / \
    1   2
```

**Sub Tree:**

```
    4
   / \
  1   2
```

The subtree starting at node 4 in `root` is identical to `subRoot`.

### Example

**Input:** `root = [3,4,5,1,2]`, `subRoot = [4,1,2]`
**Output:** `true`

### Optimal Solution

```cpp
bool isSame(TreeNode* s, TreeNode* t) {
    if (!s && !t) return true;
    if (!s || !t || s->val != t->val) return false;
    return isSame(s->left, t->left) && isSame(s->right, t->right);
}

bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    if (!root) return false;
    if (isSame(root, subRoot)) return true;
    return isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
}
```

* **Time:** O(n * m)
* **Space:** O(h)

---

## 11. Count Good Nodes in Binary Tree

### Visualization

```
        3  (good)
       / \
     1    4  (good)
    /    / \
   3    1   5 (good)
```

Good nodes: 3 (root), 3 (left leaf), 4, 5 → total = 4

### Example

**Input:** `[3,1,4,3,null,1,5]`
**Output:** `4`

### Optimal Solution (DFS)

```cpp
int dfs(TreeNode* node, int maxVal) {
    if (!node) return 0;
    int good = (node->val >= maxVal);
    maxVal = max(maxVal, node->val);
    good += dfs(node->left, maxVal);
    good += dfs(node->right, maxVal);
    return good;
}

int goodNodes(TreeNode* root) {
    if (!root) return 0;
    return dfs(root, root->val);
}
```

* **Time:** O(n)
* **Space:** O(h)

---

## 12. Diameter of Binary Tree

### Problem Visualization

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

The diameter is the number of nodes on the longest path between two leaves. In this tree the longest path could be 4 -> 2 -> 1 -> 3 -> 6 (length = 5 nodes), so diameter = 5.

### Input / Output

* **Input:** Root of binary tree
* **Output:** Integer (number of nodes on the longest path)

### Example

**Input:** `[1,2,3,4,5,null,6]`
**Output:** `5`

### Hints

1. Diameter can pass through the root or lie entirely in one subtree.
2. For each node, longest path through it = left_height + right_height + 1 (nodes count).
3. Use post-order DFS to compute heights and update global maximum.

### Optimal Solution (DFS)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int maxDiameter = 0; // store node-count diameter

int height(TreeNode* node) {
    if (!node) return 0;
    int lh = height(node->left);
    int rh = height(node->right);
    // nodes on path through this node
    int through = lh + rh + 1;
    maxDiameter = max(maxDiameter, through);
    return max(lh, rh) + 1;
}

int diameterOfBinaryTree(TreeNode* root) {
    maxDiameter = 0;
    height(root);
    return maxDiameter;
}
```

* **Time:** O(n)
* **Space:** O(h) (recursion stack)

---

## 13. Validate Binary Search Tree

### Problem Visualization

Valid BST example:

```
      5
     / \
    3   7
   / \   \
  2   4   9
```

Invalid BST example (4 in wrong position):

```
      5
     / \
    3   7
       / \
      4   9   // 4 is in the right subtree of 5 but less than 5 -> invalid
```

### Input / Output

* **Input:** Root of binary tree
* **Output:** Boolean — true if valid BST, else false

### Example

**Input:** `[5,3,7,2,4,null,9]`
**Output:** `true`

### Hints

1. Use recursion with value bounds (min, max) propagated down the tree.
2. Or use in-order traversal to ensure strictly increasing sequence.
3. Beware of integer limits — use long long for bounds.

### Optimal Solution (DFS with bounds)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

bool validBSTHelper(TreeNode* node, long long low, long long high) {
    if (!node) return true;
    if (node->val <= low || node->val >= high) return false;
    return validBSTHelper(node->left, low, node->val) &&
           validBSTHelper(node->right, node->val, high);
}

bool isValidBST(TreeNode* root) {
    return validBSTHelper(root, LLONG_MIN, LLONG_MAX);
}
```

* **Time:** O(n)
* **Space:** O(h)

---

## 14. Lowest Common Ancestor of BST

### Problem Visualization

```
        6
       / \
      2   8
     / \ / \
    0  4 7  9
      / \
     3   5
```

For nodes 2 and 8, LCA = 6. For nodes 2 and 4, LCA = 2.

### Input / Output

* **Input:** Root of BST, two nodes p and q
* **Output:** Node pointer to their lowest common ancestor

### Example

**Input:** `root = [6,2,8,0,4,7,9,null,null,3,5]`, `p = 2`, `q = 8`
**Output:** `6`

### Hints

1. Use BST property: if both p and q are less than node, go left; if both greater, go right.
2. First node where p and q diverge (or equal) is the LCA.

### Optimal Solution (Iterative)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    TreeNode* node = root;
    while (node) {
        if (p->val < node->val && q->val < node->val) {
            node = node->left;
        } else if (p->val > node->val && q->val > node->val) {
            node = node->right;
        } else {
            // split point or equals
            return node;
        }
    }
    return nullptr;
}
```

* **Time:** O(h)
* **Space:** O(1) iterative (or O(h) recursion)

---
## 15. Kth Smallest Element in BST

### Problem Visualization

```
        5
       / \
      3   6
     / \
    2   4
   /
  1
```

The in-order traversal gives sorted order: [1,2,3,4,5,6]. So the 3rd smallest element is 3.

### Input / Output

* **Input:** Root of BST, integer k
* **Output:** Integer (k-th smallest value)

### Example

**Input:** `[5,3,6,2,4,1], k = 3`
**Output:** `3`

### Hints

1. BST's in-order traversal yields sorted order.
2. Use DFS with counter to stop at k.
3. Iterative stack solution avoids recursion overhead.

### Optimal Solution (In-order Traversal)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> st;
    TreeNode* curr = root;
    while (curr || !st.empty()) {
        while (curr) {
            st.push(curr);
            curr = curr->left;
        }
        curr = st.top(); st.pop();
        if (--k == 0) return curr->val;
        curr = curr->right;
    }
    return -1;
}
```

* **Time:** O(h + k)
* **Space:** O(h)

---

## 16. Find Leaves of Binary Tree

### Problem Visualization

```
        1
       / \
      2   3
     / \
    4   5
```

Leaves removal order:

* Round 1: [4,5,3]
* Round 2: [2]
* Round 3: [1]

### Input / Output

* **Input:** Root of binary tree
* **Output:** List of lists (each level = nodes removed that round)

### Example

**Input:** `[1,2,3,4,5]`
**Output:** `[[4,5,3],[2],[1]]`

### Hints

1. Each node's height determines when it becomes a leaf.
2. Use DFS to compute height and group nodes by it.

### Optimal Solution (DFS height grouping)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int dfs(TreeNode* node, vector<vector<int>>& res) {
    if (!node) return -1;
    int lh = dfs(node->left, res);
    int rh = dfs(node->right, res);
    int h = max(lh, rh) + 1;
    if (res.size() <= h) res.push_back({});
    res[h].push_back(node->val);
    return h;
}

vector<vector<int>> findLeaves(TreeNode* root) {
    vector<vector<int>> res;
    dfs(root, res);
    return res;
}
```

* **Time:** O(n)
* **Space:** O(h)

---

## 17. Construct Binary Tree from Preorder & Inorder Traversal

### Problem Visualization

Preorder: [3,9,20,15,7]  → Root before children
Inorder: [9,3,15,20,7]   → Left, Root, Right

```
        3
       / \
      9   20
          / \
         15  7
```

### Input / Output

* **Input:** preorder[] and inorder[] arrays
* **Output:** Root of constructed binary tree

### Example

**Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
**Output:** Root node (tree as shown above)

### Hints

1. First element in preorder is root.
2. Locate root in inorder to divide left and right subtrees.
3. Use hashmap for quick index lookup.

### Optimal Solution (Recursive)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* buildTreeHelper(vector<int>& preorder, int preL, int preR,
                          vector<int>& inorder, int inL, int inR,
                          unordered_map<int,int>& inMap) {
    if (preL > preR || inL > inR) return NULL;

    int rootVal = preorder[preL];
    TreeNode* root = new TreeNode(rootVal);

    int inRoot = inMap[rootVal];
    int numsLeft = inRoot - inL;

    root->left = buildTreeHelper(preorder, preL + 1, preL + numsLeft,
                                 inorder, inL, inRoot - 1, inMap);

    root->right = buildTreeHelper(preorder, preL + numsLeft + 1, preR,
                                  inorder, inRoot + 1, inR, inMap);

    return root;
}

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    unordered_map<int,int> inMap;
    for (int i = 0; i < inorder.size(); ++i)
        inMap[inorder[i]] = i;
    return buildTreeHelper(preorder, 0, preorder.size()-1,
                           inorder, 0, inorder.size()-1, inMap);
}
```

* **Time:** O(n)
* **Space:** O(n)

---

## 18. Serialize and Deserialize Binary Tree

### Problem Visualization

```
        1
       / \
      2   3
         / \
        4   5
```

Serialized form (Level Order): "1,2,3,null,null,4,5"

### Input / Output

* **Input:** Root of binary tree
* **Output:** Serialized string representation

and vice versa for deserialization.

### Example

**Input:** `[1,2,3,null,null,4,5]`
**Output:** Serialized string: "1,2,3,null,null,4,5"

### Hints

1. Use BFS to traverse the tree level by level.
2. Represent null children as "null".
3. For deserialization, use a queue to rebuild connections.

### Optimal Solution (BFS)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

string serialize(TreeNode* root) {
    if (!root) return "";
    string s = "";
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        TreeNode* curr = q.front();
        q.pop();
        if (curr) {
            s += to_string(curr->val) + ",";
            q.push(curr->left);
            q.push(curr->right);
        } else {
            s += "null,";
        }
    }
    return s;
}

TreeNode* deserialize(string data) {
    if (data.empty()) return NULL;
    stringstream ss(data);
    string item;
    getline(ss, item, ',');
    TreeNode* root = new TreeNode(stoi(item));
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        TreeNode* node = q.front();
        q.pop();

        if (!getline(ss, item, ',')) break;
        if (item != "null") {
            node->left = new TreeNode(stoi(item));
            q.push(node->left);
        }
        if (!getline(ss, item, ',')) break;
        if (item != "null") {
            node->right = new TreeNode(stoi(item));
            q.push(node->right);
        }
    }
    return root;
}
```

* **Time:** O(n)
* **Space:** O(n)

---

## 19. Binary Tree Maximum Path Sum

### Problem Visualization

```
        -10
        /  \
       9   20
           / \
          15  7
```

Maximum Path: 15 → 20 → 7 = 42

### Input / Output

* **Input:** Root of binary tree (values may be negative)
* **Output:** Integer (maximum path sum)

### Example

**Input:** `[-10,9,20,null,null,15,7]`
**Output:** `42`

### Hints

1. A path can start and end anywhere, not necessarily the root.
2. For each node, compute max gain from left and right.
3. Update global maximum at each node.

### Optimal Solution (DFS)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int maxPathSumUtil(TreeNode* root, int &maxSum) {
    if (!root) return 0;
    int left = max(0, maxPathSumUtil(root->left, maxSum));
    int right = max(0, maxPathSumUtil(root->right, maxSum));
    maxSum = max(maxSum, left + right + root->val);
    return root->val + max(left, right);
}

int maxPathSum(TreeNode* root) {
    int maxSum = INT_MIN;
    maxPathSumUtil(root, maxSum);
    return maxSum;
}
```

* **Time:** O(n)
* **Space:** O(h)

---

