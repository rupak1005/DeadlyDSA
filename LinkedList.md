### Reverse a Linked List

#### Description

Given the head of a singly linked list, reverse the list and return the new head.

#### Example

Input: `head = [1,2,3,4,5]`
Output: `[5,4,3,2,1]`

Input: `head = [1,2]`
Output: `[2,1]`

#### Hint

Iteratively change the `next` pointers of each node to point to the previous node. Optionally, solve recursively.

#### Brute Force (Using Extra Space)

* Copy nodes into an array and rebuild the list in reverse.
* **TC:** O(n) | **SC:** O(n)

```cpp
ListNode* reverseList(ListNode* head) {
    vector<int> vals;
    ListNode* curr = head;
    while(curr){
        vals.push_back(curr->val);
        curr = curr->next;
    }
    curr = head;
    for(int i = vals.size()-1; i>=0; i--){
        curr->val = vals[i];
        curr = curr->next;
    }
    return head;
}
```

#### Optimized Approach (Iterative In-Place)

* Reverse pointers in-place using three pointers: `prev`, `curr`, `next`.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while(curr){
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

#### Optimized Approach (Recursive)

* Reverse rest of the list and attach head at the end.
* **TC:** O(n) | **SC:** O(n) recursion stack

```cpp
ListNode* reverseList(ListNode* head) {
    if(!head || !head->next) return head;
    ListNode* newHead = reverseList(head->next);
    head->next->next = head;
    head->next = nullptr;
    return newHead;
}
```

### Middle of the Linked List

#### Description

Given the head of a singly linked list, return the middle node. If there are two middle nodes, return the second middle node.

#### Example

Input: `head = [1,2,3,4,5]`
Output: `[3,4,5]`

Input: `head = [1,2,3,4,5,6]`
Output: `[4,5,6]`

#### Hint

Use two pointers (`slow` and `fast`). Move `slow` by one step and `fast` by two steps. When `fast` reaches the end, `slow` is at the middle.

#### Brute Force

* Count the number of nodes, then traverse to the middle.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* middleNode(ListNode* head) {
    int count = 0;
    ListNode* curr = head;
    while(curr){
        count++;
        curr = curr->next;
    }
    int mid = count/2;
    curr = head;
    for(int i=0;i<mid;i++) curr = curr->next;
    return curr;
}
```

#### Optimized Approach (Two Pointers)

* Use `slow` and `fast` pointers.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* middleNode(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

### Linked List Cycle

#### Description

Given a linked list, determine if it has a cycle in it. A cycle exists if a node's `next` pointer points to a previous node in the list.

#### Example

Input: `head = [3,2,0,-4]`, where the tail connects to node index 1
Output: `true`

Input: `head = [1,2]`, tail connects to node index 0
Output: `true`

Input: `head = [1]`, no cycle
Output: `false`

#### Hint

Use two pointers (`slow` and `fast`). Move `slow` by one step and `fast` by two steps. If they meet, a cycle exists.

#### Brute Force

* Use a hashmap/set to store visited nodes. If a node is revisited, there is a cycle.
* **TC:** O(n) | **SC:** O(n)

```cpp
bool hasCycle(ListNode *head) {
    unordered_set<ListNode*> visited;
    ListNode* curr = head;
    while(curr){
        if(visited.count(curr)) return true;
        visited.insert(curr);
        curr = curr->next;
    }
    return false;
}
```

#### Optimized Approach (Floyd’s Cycle Detection)

* Use `slow` and `fast` pointers.
* **TC:** O(n) | **SC:** O(1)

```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast) return true;
    }
    return false;
}
```


### Merge Two Sorted Lists

#### Description

Merge two sorted linked lists and return it as a new sorted list. The new list should be made by splicing together the nodes of the first two lists.

#### Example

Input: `l1 = [1,2,4], l2 = [1,3,4]`
Output: `[1,1,2,3,4,4]`

Input: `l1 = [], l2 = []`
Output: `[]`

Input: `l1 = [], l2 = [0]`
Output: `[0]`

#### Hint

Use a dummy node to simplify merging. Compare nodes from both lists and attach the smaller node to the merged list.

#### Brute Force

* Collect all nodes in a vector, sort them, and rebuild the list.
* **TC:** O(n log n) | **SC:** O(n)

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    vector<int> vals;
    while(l1){ vals.push_back(l1->val); l1 = l1->next; }
    while(l2){ vals.push_back(l2->val); l2 = l2->next; }
    sort(vals.begin(), vals.end());
    ListNode dummy(0), *curr = &dummy;
    for(int v : vals){
        curr->next = new ListNode(v);
        curr = curr->next;
    }
    return dummy.next;
}
```

#### Optimized Approach (Iterative Merge)

* Merge in-place by comparing nodes from both lists.
* **TC:** O(n + m) | **SC:** O(1)

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0), *curr = &dummy;
    while(l1 && l2){
        if(l1->val < l2->val){ curr->next = l1; l1 = l1->next; }
        else { curr->next = l2; l2 = l2->next; }
        curr = curr->next;
    }
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}
```

#### Optimized Approach (Recursive Merge)

* Recursively merge nodes.
* **TC:** O(n + m) | **SC:** O(n + m) recursion stack

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if(!l1) return l2;
    if(!l2) return l1;
    if(l1->val < l2->val){
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    } else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```

### Remove Nth Node From End of List

#### Description

Given the head of a linked list, remove the n-th node from the end of the list and return its head.

#### Example

Input: `head = [1,2,3,4,5], n = 2`
Output: `[1,2,3,5]`

Input: `head = [1], n = 1`
Output: `[]`

Input: `head = [1,2], n = 1`
Output: `[1]`

#### Hint

Use two pointers (`fast` and `slow`) to find the node to remove. Move `fast` n steps ahead first.

#### Brute Force

* Count the number of nodes to find the target node, then remove it.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    int count = 0;
    ListNode* curr = head;
    while(curr){ count++; curr = curr->next; }
    int pos = count - n;
    ListNode dummy(0); dummy.next = head;
    curr = &dummy;
    for(int i = 0; i < pos; i++) curr = curr->next;
    curr->next = curr->next->next;
    return dummy.next;
}
```

#### Optimized Approach (Two Pointers)

* Move `fast` pointer n steps ahead, then move both `fast` and `slow` together until `fast` reaches the end. Remove `slow->next`.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0); dummy.next = head;
    ListNode *fast = &dummy, *slow = &dummy;
    for(int i = 0; i <= n; i++) fast = fast->next;
    while(fast){
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    return dummy.next;
}
```

### Add Two Numbers

#### Description

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

#### Example

Input: `l1 = [2,4,3], l2 = [5,6,4]`
Output: `[7,0,8]`  // 342 + 465 = 807

Input: `l1 = [0], l2 = [0]`
Output: `[0]`

Input: `l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]`
Output: `[8,9,9,9,0,0,0,1]`

#### Hint

Traverse both lists simultaneously, adding corresponding digits along with carry. Create a new node for each sum digit.

#### Brute Force

* Convert linked lists to integers, add them, and rebuild the list. This can fail for very large numbers.
* **TC:** O(n + m) | **SC:** O(n + m)

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    long long num1 = 0, num2 = 0, multiplier = 1;
    while(l1){ num1 += l1->val * multiplier; multiplier *= 10; l1 = l1->next; }
    multiplier = 1;
    while(l2){ num2 += l2->val * multiplier; multiplier *= 10; l2 = l2->next; }
    long long sum = num1 + num2;
    if(sum == 0) return new ListNode(0);
    ListNode* dummy = new ListNode(0), *curr = dummy;
    while(sum){
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
        sum /= 10;
    }
    return dummy->next;
}
```

#### Optimized Approach (Digit-by-Digit)

* Traverse both lists, add digits and carry, create nodes directly.
* **TC:** O(max(n, m)) | **SC:** O(max(n, m))

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode dummy(0), *curr = &dummy;
    int carry = 0;
    while(l1 || l2 || carry){
        int sum = carry;
        if(l1){ sum += l1->val; l1 = l1->next; }
        if(l2){ sum += l2->val; l2 = l2->next; }
        carry = sum / 10;
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
    }
    return dummy.next;
}
```

### Reorder List

#### Description

Given a singly linked list `L: L0→L1→…→Ln-1→Ln`, reorder it to: `L0→Ln→L1→Ln-1→L2→Ln-2→…` without altering the node values, only changing the pointers.

#### Example

Input: `head = [1,2,3,4]`
Output: `[1,4,2,3]`

Input: `head = [1,2,3,4,5]`
Output: `[1,5,2,4,3]`

#### Hint

* Find the middle of the list.
* Reverse the second half.
* Merge the two halves alternatingly.

#### Brute Force

* Use a vector to store nodes, then reorder using indices.
* **TC:** O(n) | **SC:** O(n)

```cpp
void reorderList(ListNode* head) {
    vector<ListNode*> nodes;
    ListNode* curr = head;
    while(curr){ nodes.push_back(curr); curr = curr->next; }
    int i = 0, j = nodes.size() - 1;
    while(i < j){
        nodes[i]->next = nodes[j];
        i++;
        if(i == j) break;
        nodes[j]->next = nodes[i];
        j--;
    }
    nodes[i]->next = nullptr;
}
```

#### Optimized Approach (In-Place)

* **Step 1:** Find the middle using slow and fast pointers.
* **Step 2:** Reverse the second half.
* **Step 3:** Merge two halves.
* **TC:** O(n) | **SC:** O(1)

```cpp
void reorderList(ListNode* head) {
    if(!head || !head->next) return;
    // Find middle
    ListNode *slow = head, *fast = head;
    while(fast->next && fast->next->next){
        slow = slow->next;
        fast = fast->next->next;
    }
    // Reverse second half
    ListNode* prev = nullptr, *curr = slow->next;
    while(curr){
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    slow->next = nullptr;
    // Merge two halves
    ListNode* first = head, *second = prev;
    while(second){
        ListNode* tmp1 = first->next;
        ListNode* tmp2 = second->next;
        first->next = second;
        second->next = tmp1;
        first = tmp1;
        second = tmp2;
    }
}
```

### Find the Duplicate Number

#### Description

Given an array `nums` containing `n + 1` integers where each integer is between 1 and n (inclusive), there is only one repeated number. Find the duplicate number without modifying the array and using constant extra space.

#### Example

Input: `nums = [1,3,4,2,2]`
Output: `2`

Input: `nums = [3,1,3,4,2]`
Output: `3`

#### Hint

Use cycle detection (Floyd’s Tortoise and Hare) since the array can be interpreted as a linked list.

#### Brute Force

* Sort the array and check consecutive elements.
* **TC:** O(n log n) | **SC:** O(1) if sorting in place

```cpp
int findDuplicate(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    for(int i = 1; i < nums.size(); i++){
        if(nums[i] == nums[i-1]) return nums[i];
    }
    return -1;
}
```

#### Optimized Approach (Floyd’s Tortoise and Hare)

* Treat numbers as pointers to next indices, detect the cycle.
* **TC:** O(n) | **SC:** O(1)

```cpp
int findDuplicate(vector<int>& nums) {
    int slow = nums[0], fast = nums[0];
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while(slow != fast);
    slow = nums[0];
    while(slow != fast){
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

### Copy List With Random Pointer

#### Description

Given a linked list where each node contains an additional random pointer which could point to any node in the list or null, return a deep copy of the list.

#### Example

Input: `head = [[7,null],[13,0],[11,4],[10,2],[1,0]]`
Output: `[[7,null],[13,0],[11,4],[10,2],[1,0]]`

#### Hint

Use a hashmap to store the mapping from original nodes to copied nodes, or interleave copied nodes with original nodes to reduce space.

#### Brute Force (HashMap)

* Create a copy of each node and store mapping in a hashmap. Then set `next` and `random` pointers.
* **TC:** O(n) | **SC:** O(n)

```cpp
Node* copyRandomList(Node* head) {
    if(!head) return nullptr;
    unordered_map<Node*, Node*> m;
    Node* curr = head;
    while(curr){
        m[curr] = new Node(curr->val);
        curr = curr->next;
    }
    curr = head;
    while(curr){
        m[curr]->next = m[curr->next];
        m[curr]->random = m[curr->random];
        curr = curr->next;
    }
    return m[head];
}
```

#### Optimized Approach (Interleaving)

* Interleave copied nodes with original nodes, then separate.
* **TC:** O(n) | **SC:** O(1)

```cpp
Node* copyRandomList(Node* head) {
    if(!head) return nullptr;
    Node* curr = head;
    while(curr){
        Node* copy = new Node(curr->val);
        copy->next = curr->next;
        curr->next = copy;
        curr = copy->next;
    }
    curr = head;
    while(curr){
        if(curr->random) curr->next->random = curr->random->next;
        curr = curr->next->next;
    }
    Node* dummy = new Node(0), *copyCurr = dummy;
    curr = head;
    while(curr){
        copyCurr->next = curr->next;
        copyCurr = copyCurr->next;
        curr->next = curr->next->next;
        curr = curr->next;
    }
    return dummy->next;
}
```

### Merge K Sorted Lists

#### Description

You are given an array of `k` linked lists, each sorted in ascending order. Merge all the linked lists into one sorted linked list and return it.

#### Example

Input: `lists = [[1,4,5],[1,3,4],[2,6]]`
Output: `[1,1,2,3,4,4,5,6]`

Input: `lists = []`
Output: `[]`

Input: `lists = [[]]`
Output: `[]`

#### Hint

Use a min-heap (priority queue) to always select the smallest head among the k lists.

#### Brute Force

* Collect all nodes into a vector, sort them, and rebuild the list.
* **TC:** O(n log n) where n is total nodes | **SC:** O(n)

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    vector<int> vals;
    for(auto l : lists){
        while(l){ vals.push_back(l->val); l = l->next; }
    }
    sort(vals.begin(), vals.end());
    ListNode dummy(0), *curr = &dummy;
    for(int v : vals){
        curr->next = new ListNode(v);
        curr = curr->next;
    }
    return dummy.next;
}
```

#### Optimized Approach (Min-Heap / Priority Queue)

* Push the head of each list into a min-heap.
* Pop the smallest node and push its next node into the heap.
* **TC:** O(n log k) | **SC:** O(k)

```cpp
struct compare {
    bool operator()(ListNode* a, ListNode* b){
        return a->val > b->val;
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, compare> pq;
    for(auto l : lists) if(l) pq.push(l);
    ListNode dummy(0), *curr = &dummy;
    while(!pq.empty()){
        ListNode* node = pq.top(); pq.pop();
        curr->next = node;
        curr = curr->next;
        if(node->next) pq.push(node->next);
    }
    return dummy.next;
}
```

#### Optimized Approach (Divide and Conquer)

* Repeatedly merge pairs of lists.
* **TC:** O(n log k) | **SC:** O(1)

### Reverse Nodes In K Group

#### Description

Given a linked list, reverse the nodes of the list k at a time and return its modified list. Nodes that are not a multiple of k should remain in the original order.

#### Example

Input: `head = [1,2,3,4,5], k = 2`
Output: `[2,1,4,3,5]`

Input: `head = [1,2,3,4,5], k = 3`
Output: `[3,2,1,4,5]`

#### Hint

* Reverse the first k nodes, then recursively reverse the rest.
* Make sure to connect the reversed part to the remaining list.

#### Brute Force

* Collect nodes in a vector, reverse every k nodes, and rebuild the list.
* **TC:** O(n) | **SC:** O(n)

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
    vector<ListNode*> nodes;
    ListNode* curr = head;
    while(curr){ nodes.push_back(curr); curr = curr->next; }
    for(int i = 0; i + k <= nodes.size(); i += k){
        reverse(nodes.begin() + i, nodes.begin() + i + k);
    }
    for(int i = 0; i < nodes.size() - 1; i++) nodes[i]->next = nodes[i+1];
    nodes.back()->next = nullptr;
    return nodes[0];
}
```

#### Optimized Approach (In-Place)

* Reverse k nodes iteratively and connect each reversed group.
* **TC:** O(n) | **SC:** O(1)

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
    ListNode dummy(0); dummy.next = head;
    ListNode *prev = &dummy, *curr = head, *next;
    int count = 0;
    while(curr){ count++; curr = curr->next; }
    curr = head;
    while(count >= k){
        ListNode* tail = curr;
        ListNode* prevNode = nullptr;
        for(int i = 0; i < k; i++){
            next = curr->next;
            curr->next = prevNode;
            prevNode = curr;
            curr = next;
        }
        prev->next = prevNode;
        tail->next = curr;
        prev = tail;
        count -= k;
    }
    return dummy.next;
}
```

### Flatten Binary Tree to Linked List

#### Description

Given the root of a binary tree, flatten it into a linked list in-place following the preorder traversal. The left child of all nodes should be null and the right child should point to the next node in preorder.

#### Example

Input: `root = [1,2,5,3,4,null,6]`
Output: `[1,null,2,null,3,null,4,null,5,null,6]`

Input: `root = []`
Output: `[]`

#### Hint

Use recursion or iteration to traverse the tree and rewire the pointers. You can flatten the left subtree and attach it between the root and the right subtree.

#### Brute Force

* Store preorder traversal in a vector, then rebuild the right-skewed list.
* **TC:** O(n) | **SC:** O(n)

```cpp
void flatten(TreeNode* root) {
    if(!root) return;
    vector<TreeNode*> nodes;
    function<void(TreeNode*)> preorder = [&](TreeNode* node){
        if(!node) return;
        nodes.push_back(node);
        preorder(node->left);
        preorder(node->right);
    };
    preorder(root);
    for(int i = 1; i < nodes.size(); i++){
        nodes[i-1]->left = nullptr;
        nodes[i-1]->right = nodes[i];
    }
}
```

#### Optimized Approach (In-Place, O(1) Space)

* Use a pointer to traverse and rewire nodes directly.
* **TC:** O(n) | **SC:** O(1)

```cpp
void flatten(TreeNode* root) {
    TreeNode* curr = root;
    while(curr){
        if(curr->left){
            TreeNode* prev = curr->left;
            while(prev->right) prev = prev->right;
            prev->right = curr->right;
            curr->right = curr->left;
            curr->left = nullptr;
        }
        curr = curr->right;
    }
}
```

