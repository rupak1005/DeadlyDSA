
### 1.Valid Parentheses

#### Description

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid. An input string is valid if:

* Open brackets are closed by the same type of brackets.
* Open brackets are closed in the correct order.

#### Example

Input: `s = "()"`
Output: `true`

Input: `s = "()[]{}"`
Output: `true`

Input: `s = "(]"`
Output: `false`

#### Hint

Use a stack to keep track of opening brackets. For each closing bracket, check if it matches the top of the stack.

#### Brute Force

* Check each pair manually using nested loops (inefficient for large strings).
* **TC:** O(n^2) | **SC:** O(n)

#### Optimized Approach (Stack)

* Traverse the string, push opening brackets onto a stack, and pop when a matching closing bracket is found.
* **TC:** O(n) | **SC:** O(n)

```cpp
bool isValid(string s) {
    stack<char> st;
    for(char c : s){
        if(c == '(' || c == '{' || c == '[') st.push(c);
        else {
            if(st.empty()) return false;
            if(c == ')' && st.top() != '(') return false;
            if(c == '}' && st.top() != '{') return false;
            if(c == ']' && st.top() != '[') return false;
            st.pop();
        }
    }
    return st.empty();
}
```

### 2.Min Stack

#### Description

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

#### Example

Input:

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // returns -3
minStack.pop();
minStack.top();    // returns 0
minStack.getMin(); // returns -2
```

#### Hint

Use an auxiliary stack to keep track of the minimum value at each stage.

#### Brute Force

* Traverse the entire stack to find the minimum for each `getMin()` call.
* **TC:** O(n) per getMin | **SC:** O(n)

#### Optimized Approach (Two Stacks)

* Maintain a main stack for values and a min stack for current minimums.
* **TC:** O(1) for all operations | **SC:** O(n)

```cpp
class MinStack {
    stack<int> st, minSt;
public:
    void push(int val) {
        st.push(val);
        if(minSt.empty() || val <= minSt.top()) minSt.push(val);
    }
    void pop() {
        if(st.top() == minSt.top()) minSt.pop();
        st.pop();
    }
    int top() {
        return st.top();
    }
    int getMin() {
        return minSt.top();
    }
};
```

---

### 3.Max Stack

#### Description

Design a stack that supports push, pop, top, and retrieving the maximum element in constant time.

#### Example

Input:

```
MaxStack maxStack = new MaxStack();
maxStack.push(2);
maxStack.push(0);
maxStack.push(3);
maxStack.getMax(); // returns 3
maxStack.pop();
maxStack.top();    // returns 0
maxStack.getMax(); // returns 2
```

#### Hint

Use an auxiliary stack to keep track of the maximum value at each stage.

#### Brute Force

* Traverse the entire stack to find the maximum for each `getMax()` call.
* **TC:** O(n) per getMax | **SC:** O(n)

#### Optimized Approach (Two Stacks)

* Maintain a main stack for values and a max stack for current maximums.
* **TC:** O(1) for all operations | **SC:** O(n)

```cpp
class MaxStack {
    stack<int> st, maxSt;
public:
    void push(int val) {
        st.push(val);
        if(maxSt.empty() || val >= maxSt.top()) maxSt.push(val);
    }
    void pop() {
        if(st.top() == maxSt.top()) maxSt.pop();
        st.pop();
    }
    int top() {
        return st.top();
    }
    int getMax() {
        return maxSt.top();
    }
};
```


### 4.Max Stack

#### Description

Design a stack that supports push, pop, top, peekMax, and popMax operations efficiently.

#### Example

Input:

```
MaxStack maxStack = new MaxStack();
maxStack.push(5);
maxStack.push(1);
maxStack.push(5);
maxStack.top();    // returns 5
maxStack.popMax(); // returns 5
maxStack.top();    // returns 1
maxStack.peekMax(); // returns 5
maxStack.pop();    // returns 1
maxStack.top();    // returns 5
```

#### Hint

Use a stack along with a multiset (or another stack) to keep track of maximums for efficient retrieval and deletion.

#### Brute Force

* Traverse the stack to find the maximum for each popMax() call.
* **TC:** O(n) per popMax | **SC:** O(n)

#### Optimized Approach (Stack + BST / Multiset)

* Maintain a stack for values and a multiset for fast max retrieval.
* **TC:** O(log n) for popMax, O(1) for push/pop/top/peekMax | **SC:** O(n)

```cpp
class MaxStack {
    stack<int> st;
    multiset<int> ms;
public:
    void push(int val) {
        st.push(val);
        ms.insert(val);
    }
    void pop() {
        int val = st.top(); st.pop();
        ms.erase(ms.find(val));
    }
    int top() {
        return st.top();
    }
    int peekMax() {
        return *ms.rbegin();
    }
    int popMax() {
        int maxVal = *ms.rbegin();
        ms.erase(prev(ms.end()));
        stack<int> buffer;
        while(st.top() != maxVal){
            buffer.push(st.top()); st.pop();
        }
        st.pop(); // remove max
        while(!buffer.empty()){ st.push(buffer.top()); buffer.pop(); }
        return maxVal;
    }
};
```

### 5.Daily Temperatures

#### Description

Given a list of daily temperatures `temperatures`, return a list `answer` such that `answer[i]` is the number of days you have to wait after the `i`-th day to get a warmer temperature. If there is no future day for which this is possible, put 0.

#### Example

Input: `temperatures = [73,74,75,71,69,72,76,73]`
Output: `[1,1,4,2,1,1,0,0]`

Input: `temperatures = [30,40,50,60]`
Output: `[1,1,1,0]`

Input: `temperatures = [30,60,90]`
Output: `[1,1,0]`

#### Hint

Use a stack to keep track of indices of temperatures. Iterate through the list, and for each day, pop from the stack until the current temperature is not warmer than the stack's top.

#### Brute Force

* For each day, check all future days to find a warmer temperature.
* **TC:** O(n^2) | **SC:** O(1)

```cpp
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> ans(n, 0);
    for(int i = 0; i < n; i++){
        for(int j = i+1; j < n; j++){
            if(temperatures[j] > temperatures[i]){
                ans[i] = j - i;
                break;
            }
        }
    }
    return ans;
}
```

#### Optimized Approach (Monotonic Stack)

* Use a stack to store indices of days with unresolved temperatures. Pop from the stack when a warmer temperature is found.
* **TC:** O(n) | **SC:** O(n)

```cpp
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> ans(n, 0);
    stack<int> st; // stores indices
    for(int i = 0; i < n; i++){
        while(!st.empty() && temperatures[i] > temperatures[st.top()]){
            int idx = st.top(); st.pop();
            ans[idx] = i - idx;
        }
        st.push(i);
    }
    return ans;
}
```

### 6.Car Fleet

#### Description

There are `n` cars going to the same destination along a one-lane road. You are given two integer arrays `position` and `speed`, where `position[i]` is the position of the `i`-th car and `speed[i]` is its speed. A car can never pass another car ahead of it, but it can catch up and form a fleet. Return the number of car fleets that will arrive at the destination.

#### Example

Input: `target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]`
Output: `3`

Input: `target = 10, position = [3], speed = [3]`
Output: `1`

Input: `target = 100, position = [0,2,4], speed = [4,2,1]`
Output: `1`

#### Hint

* Sort cars by starting position and compute the time each car takes to reach the destination. Compare times from the car closest to the destination to determine fleets.

#### Brute Force

* Simulate each second, update positions, and merge cars into fleets when they meet.
* **TC:** O(n^2) | **SC:** O(n)

```cpp
int carFleet(int target, vector<int>& position, vector<int>& speed) {
    int n = position.size();
    vector<pair<int,double>> cars;
    for(int i = 0; i < n; i++) cars.push_back({position[i], (double)(target - position[i]) / speed[i]});
    sort(cars.rbegin(), cars.rend()); // closest to target first
    int fleets = 0;
    double currTime = 0;
    for(auto& car : cars){
        if(car.second > currTime){
            currTime = car.second;
            fleets++;
        }
    }
    return fleets;
}
```

#### Optimized Approach

* Sort by position descending, calculate time to reach target, and count fleets by comparing times.
* **TC:** O(n log n) | **SC:** O(n)

### 7.Evaluate Reverse Polish Notation

#### Description

Evaluate the value of an arithmetic expression in Reverse Polish Notation (RPN). Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

#### Example

Input: `tokens = ["2","1","+","3","*"]`
Output: `9`

Input: `tokens = ["4","13","5","/","+"]`
Output: `6`

Input: `tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]`
Output: `22`

#### Hint

* Use a stack to store operands. When an operator is encountered, pop two operands, perform the operation, and push the result back.

#### Brute Force

* Evaluate from left to right with a stack, handling each operator manually.
* **TC:** O(n) | **SC:** O(n)

```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> st;
    for(string& token : tokens){
        if(token == "+" || token == "-" || token == "*" || token == "/"){
            int b = st.top(); st.pop();
            int a = st.top(); st.pop();
            if(token == "+") st.push(a+b);
            else if(token == "-") st.push(a-b);
            else if(token == "*") st.push(a*b);
            else st.push(a/b);
        } else {
            st.push(stoi(token));
        }
    }
    return st.top();
}
```

#### Optimized Approach

* Same as brute force; stack-based evaluation is already optimal.
* **TC:** O(n) | **SC:** O(n)


### 8.Minimum Remove to Make Valid Parentheses

#### Description

Given a string `s` consisting of lowercase letters and parentheses, remove the minimum number of parentheses to make the string valid. Return any valid string.

#### Example

Input: `s = "lee(t(c)o)de)"`
Output: `"lee(t(c)o)de"`

Input: `s = "a)b(c)d"`
Output: `"ab(c)d"`

Input: `s = "))(("`
Output: `""`

#### Hint

Use a stack to keep track of indices of unmatched parentheses. Remove all unmatched parentheses at the end.

#### Brute Force

* Try removing every parenthesis and check validity recursively (inefficient).
* **TC:** Exponential | **SC:** O(n)

#### Optimized Approach (Stack)

* Traverse the string and use a stack to store indices of '('.
* For each ')', pop from the stack if possible; otherwise mark it for removal.
* Remove all unmatched parentheses after traversal.
* **TC:** O(n) | **SC:** O(n)

```cpp
string minRemoveToMakeValid(string s) {
    stack<int> st;
    unordered_set<int> remove;
    for(int i = 0; i < s.size(); i++){
        if(s[i] == '(') st.push(i);
        else if(s[i] == ')'){
            if(st.empty()) remove.insert(i);
            else st.pop();
        }
    }
    while(!st.empty()){ remove.insert(st.top()); st.pop(); }
    string res;
    for(int i = 0; i < s.size(); i++){
        if(remove.find(i) == remove.end()) res += s[i];
    }
    return res;
}
```

### 9.Generate Parentheses

#### Description

Given n pairs of parentheses, generate all combinations of well-formed parentheses.

#### Example

Input: `n = 3`
Output: `["((()))","(()())","(())()","()(())","()()()"]`

Input: `n = 1`
Output: `["()"]`

#### Hint

Use backtracking. Keep track of the number of open and closed parentheses used and add '(' or ')' only if it does not violate the well-formed rule.

#### Brute Force

* Generate all sequences of '(' and ')' and check if they are valid.
* **TC:** O(2^(2n) * n) | **SC:** O(2^(2n))

#### Optimized Approach (Backtracking)

* Build the string incrementally, adding '(' if open < n and ')' if close < open.
* **TC:** Catalan number C_n | **SC:** O(n) for recursion stack

```cpp
void backtrack(vector<string>& res, string current, int open, int close, int maxPairs) {
    if(current.size() == maxPairs * 2){
        res.push_back(current);
        return;
    }
    if(open < maxPairs) backtrack(res, current + '(', open + 1, close, maxPairs);
    if(close < open) backtrack(res, current + ')', open, close + 1, maxPairs);
}

vector<string> generateParenthesis(int n) {
    vector<string> res;
    backtrack(res, "", 0, 0, n);
    return res;
}
```

### 10.Longest Valid Parentheses

#### Description

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

#### Example

Input: `s = "(()"`
Output: `2`

Input: `s = ")()())"`
Output: `4`

Input: `s = ""`
Output: `0`

#### Hint

Use a stack to keep track of indices of unmatched '(' or a DP array to store lengths of valid substrings ending at each index.

#### Brute Force

* Check every substring and validate if it's well-formed.
* **TC:** O(n^3) | **SC:** O(1)

#### Optimized Approach (Stack)

* Use a stack to store indices of '(' and the last unmatched ')' index. Update the max length when a valid substring is found.
* **TC:** O(n) | **SC:** O(n)

```cpp
int longestValidParentheses(string s) {
    stack<int> st;
    st.push(-1);
    int maxLen = 0;
    for(int i = 0; i < s.size(); i++){
        if(s[i] == '(') st.push(i);
        else {
            st.pop();
            if(st.empty()) st.push(i);
            else maxLen = max(maxLen, i - st.top());
        }
    }
    return maxLen;
}
```

#### Optimized Approach (DP)

* Use a DP array where `dp[i]` stores the length of the longest valid substring ending at index `i`.
* **TC:** O(n) | **SC:** O(n)

```cpp
int longestValidParentheses(string s) {
    int n = s.size(), maxLen = 0;
    vector<int> dp(n, 0);
    for(int i = 1; i < n; i++){
        if(s[i] == ')'){
            if(s[i-1] == '(') dp[i] = (i >= 2 ? dp[i-2] : 0) + 2;
            else if(i - dp[i-1] - 1 >= 0 && s[i - dp[i-1] - 1] == '(')
                dp[i] = dp[i-1] + 2 + (i - dp[i-1] - 2 >= 0 ? dp[i - dp[i-1] - 2] : 0);
            maxLen = max(maxLen, dp[i]);
        }
    }
    return maxLen;
}
```

### 11.Largest Rectangle In Histogram

#### Description

Given an array of non-negative integers representing the heights of bars in a histogram where the width of each bar is 1, find the area of the largest rectangle that can be formed in the histogram.

#### Example

Input: `heights = [2,1,5,6,2,3]`
Output: `10`

Input: `heights = [2,4]`
Output: `4`

#### Hint

Use a stack to keep track of indices of bars. Calculate areas when a bar shorter than the top of the stack is encountered.

#### Brute Force

* Check each bar as the smallest bar of a rectangle and expand to left and right until a shorter bar is found.
* **TC:** O(n^2) | **SC:** O(1)

```cpp
int largestRectangleArea(vector<int>& heights) {
    int n = heights.size(), maxArea = 0;
    for(int i = 0; i < n; i++){
        int height = heights[i];
        int left = i, right = i;
        while(left > 0 && heights[left-1] >= height) left--;
        while(right < n-1 && heights[right+1] >= height) right++;
        maxArea = max(maxArea, height * (right - left + 1));
    }
    return maxArea;
}
```

#### Optimized Approach (Stack)

* Use a stack to maintain indices of bars in increasing order of height. Compute area when a bar shorter than the top is found.
* **TC:** O(n) | **SC:** O(n)

```cpp
int largestRectangleArea(vector<int>& heights) {
    stack<int> st;
    heights.push_back(0); // Sentinel to pop all remaining bars
    int maxArea = 0;
    for(int i = 0; i < heights.size(); i++){
        while(!st.empty() && heights[i] < heights[st.top()]){
            int h = heights[st.top()]; st.pop();
            int w = st.empty() ? i : i - st.top() - 1;
            maxArea = max(maxArea, h * w);
        }
        st.push(i);
    }
    return maxArea;
}
```

