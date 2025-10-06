### 1.Common Queue Implementation

#### Description

A queue is a data structure that follows First-In-First-Out (FIFO) principle. Elements are added at the back (enqueue) and removed from the front (dequeue).

#### Operations

* `enqueue(x)`: Add element `x` to the back of the queue.
* `dequeue()`: Remove and return the front element.
* `front()`: Return the front element without removing it.
* `size()`: Return the number of elements.
* `empty()`: Check if the queue is empty.

#### Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;
    q.push(1); // enqueue 1
    q.push(2); // enqueue 2
    q.push(3); // enqueue 3

    cout << q.front() << endl; // 1
    q.pop(); // dequeue 1
    cout << q.front() << endl; // 2

    cout << "Queue size: " << q.size() << endl; // 2
    cout << "Is empty? " << q.empty() << endl; // 0 (false)
    return 0;
}
```

#### Complexity

* **Time:** O(1) for enqueue, dequeue, and front.
* **Space:** O(n)

### 2.Implement Queue Using Stacks

#### Description

Implement a queue using two stacks. The queue should support `enqueue` and `dequeue` operations using only stack operations (push, pop, top).

#### Approach

* Use two stacks, `s1` for enqueue and `s2` for dequeue.
* For `enqueue(x)`: push `x` into `s1`.
* For `dequeue()`: if `s2` is empty, move all elements from `s1` to `s2` (reversing order), then pop from `s2`. Otherwise, pop directly from `s2`.
* `front()`: similar to `dequeue()`, but return the top of `s2` instead of popping.

#### Example

```cpp
#include <iostream>
#include <stack>
using namespace std;

class MyQueue {
    stack<int> s1, s2;
public:
    void enqueue(int x) {
        s1.push(x);
    }
    void dequeue() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        if(!s2.empty()) s2.pop();
    }
    int front() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        return s2.top();
    }
    bool empty() {
        return s1.empty() && s2.empty();
    }
};

int main() {
    MyQueue q;
    q.enqueue(1);
    q.enqueue(2);
    cout << q.front() << endl; // 1
    q.dequeue();
    cout << q.front() << endl; // 2
    return 0;
}
```

#### Complexity

* **Time:** Amortized O(1) per operation
* **Space:** O(n)

---

### 3.Implement Stack Using Queues

#### Description

Implement a stack using one or two queues. The stack should support `push`, `pop`, `top`, and `empty` operations using only queue operations (enqueue, dequeue).

#### Approach (Two Queues)

* Use two queues `q1` and `q2`. Keep the newest element at the front of `q1` by pushing into `q2` and moving elements from `q1`.
* `push(x)`: enqueue into `q2`, then move all elements from `q1` to `q2`, and swap `q1` and `q2`.
* `pop()`: dequeue from `q1`.
* `top()`: return front of `q1`.

#### Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

class MyStack {
    queue<int> q1, q2;
public:
    void push(int x) {
        q2.push(x);
        while(!q1.empty()){
            q2.push(q1.front());
            q1.pop();
        }
        swap(q1, q2);
    }
    void pop() {
        if(!q1.empty()) q1.pop();
    }
    int top() {
        return q1.front();
    }
    bool empty() {
        return q1.empty();
    }
};

int main() {
    MyStack s;
    s.push(1);
    s.push(2);
    cout << s.top() << endl; // 2
    s.pop();
    cout << s.top() << endl; // 1
    return 0;
}
```

#### Complexity

* **Time:** O(n) for push, O(1) for pop/top
* **Space:** O(n)

### 4.Implement Stack Using Queues (Single Queue Approach)

#### Description

Implement a stack using a single queue. The stack should support `push`, `pop`, `top`, and `empty` operations using only queue operations (enqueue, dequeue).

#### Approach (Single Queue)

* Use a single queue `q`. For `push(x)`, enqueue `x` and then rotate all previous elements to the back to maintain LIFO order.
* `pop()`: dequeue from the front.
* `top()`: return the front element.
* `empty()`: check if the queue is empty.

#### Example

```cpp
#include <iostream>
#include <queue>
using namespace std;

class MyStack {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        int sz = q.size();
        for(int i = 0; i < sz - 1; i++){
            q.push(q.front());
            q.pop();
        }
    }
    void pop() {
        if(!q.empty()) q.pop();
    }
    int top() {
        return q.front();
    }
    bool empty() {
        return q.empty();
    }
};

int main() {
    MyStack s;
    s.push(1);
    s.push(2);
    cout << s.top() << endl; // 2
    s.pop();
    cout << s.top() << endl; // 1
    return 0;
}
```

#### Complexity

* **Time:** O(n) for push, O(1) for pop/top
* **Space:** O(n)

### 5.Design Circular Queue

#### Description

Design a circular queue with fixed size. The queue should support `enqueue`, `dequeue`, `Front`, `Rear`, `isEmpty`, and `isFull` operations.

#### Approach

* Use an array of size `k` and two pointers `front` and `rear`.
* `enqueue(value)`: add the value at `rear` and move `rear` forward in a circular manner if the queue is not full.
* `dequeue()`: remove the value at `front` and move `front` forward in a circular manner if the queue is not empty.
* `Front()`: return the value at `front`.
* `Rear()`: return the value at `(rear - 1 + capacity) % capacity`.
* `isEmpty()`: check if `size == 0`.
* `isFull()`: check if `size == capacity`.

#### Example

```cpp
#include <iostream>
using namespace std;

class MyCircularQueue {
    vector<int> q;
    int front, rear, size, capacity;
public:
    MyCircularQueue(int k) : q(k), front(0), rear(0), size(0), capacity(k) {}

    bool enqueue(int value) {
        if(size == capacity) return false;
        q[rear] = value;
        rear = (rear + 1) % capacity;
        size++;
        return true;
    }

    bool dequeue() {
        if(size == 0) return false;
        front = (front + 1) % capacity;
        size--;
        return true;
    }

    int Front() {
        return size == 0 ? -1 : q[front];
    }

    int Rear() {
        return size == 0 ? -1 : q[(rear - 1 + capacity) % capacity];
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }
};

int main() {
    MyCircularQueue q(3);
    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);
    cout << q.Rear() << endl; // 3
    q.dequeue();
    q.enqueue(4);
    cout << q.Rear() << endl; // 4
    return 0;
}
```

#### Complexity

* **Time:** O(1) for all operations
* **Space:** O(k)

### 6.Open the Lock

#### Description

You have a lock represented by 4 wheels, each with digits '0'–'9'. The lock starts at "0000". Given a list of deadends and a target combination, return the minimum number of turns to open the lock, or -1 if it is impossible. Each move increments or decrements a single wheel by 1 (with wrap-around).

#### Example

Input: `deadends = ["0201","0101","0102","1212","2002"], target = "0202"`
Output: `6`

Input: `deadends = ["8888"], target = "0009"`
Output: `1`

#### Hint

Treat each combination as a node in an implicit graph; edges connect combinations one move apart. Use BFS from "0000" to find the shortest path to `target`, skipping deadends.

#### Brute Force

* DFS exploring all combinations may find a path but does not guarantee shortest and can be exponential.
* **TC:** O(10^4) worst-case | **SC:** O(10^4)

```cpp
int openLock(vector<string>& deadends, string target) {
    unordered_set<string> dead(deadends.begin(), deadends.end());
    if(dead.count("0000")) return -1;
    unordered_set<string> seen;
    queue<pair<string,int>> q;
    q.push({"0000", 0});
    seen.insert("0000");
    while(!q.empty()){
        auto [cur, steps] = q.front(); q.pop();
        if(cur == target) return steps;
        if(dead.count(cur)) continue;
        for(int i=0;i<4;i++){
            string s = cur;
            char c = s[i];
            // move up
            s[i] = (c == '9') ? '0' : c + 1;
            if(!seen.count(s) && !dead.count(s)){ seen.insert(s); q.push({s, steps+1}); }
            // move down
            s[i] = (c == '0') ? '9' : c - 1;
            if(!seen.count(s) && !dead.count(s)){ seen.insert(s); q.push({s, steps+1}); }
        }
    }
    return -1;
}
```

#### Optimized Approach (Bidirectional BFS)

* Run BFS from both `start` and `target`, expanding the smaller frontier each step to reduce search space. Useful when target is far from start.
* **TC:** O(10^4) worst-case but typically faster | **SC:** O(10^4)

```cpp
int openLock(vector<string>& deadends, string target) {
    unordered_set<string> dead(deadends.begin(), deadends.end());
    if(dead.count("0000")) return -1;
    string start = "0000";
    if(start == target) return 0;
    unordered_set<string> begin{start}, end{target}, visited{start};
    int steps = 0;
    while(!begin.empty() && !end.empty()){
        if(begin.size() > end.size()) swap(begin, end);
        unordered_set<string> next;
        steps++;
        for(auto cur : begin){
            if(dead.count(cur)) continue;
            for(int i=0;i<4;i++){
                string s = cur;
                char c = s[i];
                s[i] = (c == '9') ? '0' : c + 1;
                if(end.count(s)) return steps;
                if(!visited.count(s) && !dead.count(s)){ visited.insert(s); next.insert(s); }
                s[i] = (c == '0') ? '9' : c - 1;
                if(end.count(s)) return steps;
                if(!visited.count(s) && !dead.count(s)){ visited.insert(s); next.insert(s); }
            }
        }
        begin = move(next);
    }
    return -1;
}
```


### 7.Dota2 Senate

#### Description

In the Dota2 Senate game, each senator can ban one senator of the opposite party (Radiant or Dire) in the current round. Given a string of senators where 'R' = Radiant and 'D' = Dire, predict which party will eventually win if every senator acts optimally and takes turns in order.

#### Example

Input: `senate = "RD"`
Output: `R`

Input: `senate = "RDD"`
Output: `D`

#### Hint

Use queues to simulate the senators of each party. Track the order of turns and ban the opposing senator accordingly. Continue until one party has no senators left.

#### Approach (Queue Simulation)

* Create two queues, one for Radiant indices and one for Dire indices.
* For each round, compare the front of both queues:

  * The smaller index gets to ban the other party's senator.
  * Move the winning senator to the back of its queue with index incremented by `n` (length of senate).
* Repeat until one queue is empty.
* Return the winning party.

#### Brute Force

* Naive simulation by scanning and banning senators in place can work but is inefficient for large strings.
* **TC:** O(n^2) | **SC:** O(n)

#### Optimized Approach (Queue)

* Use two queues to efficiently manage turns.
* **TC:** O(n) | **SC:** O(n)

```cpp
string predictPartyVictory(string senate) {
    int n = senate.size();
    queue<int> radiant, dire;
    for(int i = 0; i < n; i++) {
        if(senate[i] == 'R') radiant.push(i);
        else dire.push(i);
    }
    while(!radiant.empty() && !dire.empty()) {
        int r = radiant.front(); radiant.pop();
        int d = dire.front(); dire.pop();
        if(r < d) radiant.push(r + n);
        else dire.push(d + n);
    }
    return radiant.empty() ? "Dire" : "Radiant";
}
```

