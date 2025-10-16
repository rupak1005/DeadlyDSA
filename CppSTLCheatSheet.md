# 🧠 C++ STL & Inbuilt Functions — Complete Cheatsheet with Examples

---

## 🧩 1. Strings

### 🔹 Common Functions

| Function                       | Description                                           | Example                                            |
| ------------------------------ | ----------------------------------------------------- | -------------------------------------------------- |
| `s.length()`                   | Returns the length of the string                      | `cout << s.length();`                              |
| `s.substr(pos, len)`           | Extracts substring starting from `pos`                | `cout << s.substr(2,3); // from index 2, length 3` |
| `s.find("abc")`                | Finds substring; returns index or `npos` if not found | `if(s.find("abc") != string::npos)`                |
| `s.erase(pos, len)`            | Erases portion of string                              | `s.erase(2, 3);`                                   |
| `s.insert(pos, str)`           | Inserts string at position                            | `s.insert(1, "hello");`                            |
| `reverse(s.begin(), s.end())`  | Reverses the string                                   | `reverse(s.begin(), s.end());`                     |
| `stoi(str)` / `to_string(num)` | Converts string ↔ integer                             | `int x = stoi("123");`                             |
| `toupper(c)` / `tolower(c)`    | Convert char to upper/lower                           | `c = tolower('A');`                                |

### 🧠 Example

```cpp
string s = "Hello World";
cout << s.substr(0,5) << endl; // "Hello"
reverse(s.begin(), s.end());
cout << s; // "dlroW olleH"
```

---

## 🧩 2. Vectors

### 🔹 Common Functions

| Function                           | Description            | Example                                     |
| ---------------------------------- | ---------------------- | ------------------------------------------- |
| `v.push_back(x)`                   | Add element at end     | `v.push_back(10);`                          |
| `v.pop_back()`                     | Remove last element    | `v.pop_back();`                             |
| `v.size()`                         | Get number of elements | `cout << v.size();`                         |
| `v.empty()`                        | Check if empty         | `if(v.empty())`                             |
| `v.clear()`                        | Remove all elements    | `v.clear();`                                |
| `sort(v.begin(), v.end())`         | Sort ascending         | `sort(v.begin(), v.end());`                 |
| `reverse(v.begin(), v.end())`      | Reverse elements       | `reverse(v.begin(), v.end());`              |
| `*max_element(v.begin(), v.end())` | Largest element        | `cout << *max_element(v.begin(), v.end());` |
| `*min_element(v.begin(), v.end())` | Smallest element       | `cout << *min_element(v.begin(), v.end());` |

### 🧠 Example

```cpp
vector<int> v = {4, 2, 8, 1};
sort(v.begin(), v.end()); // [1, 2, 4, 8]
v.push_back(10);
cout << v.back(); // 10
```

---

## 🧩 3. Maps (Associative Containers)

### 🔹 Common Functions

| Function           | Description                 | Example                          |
| ------------------ | --------------------------- | -------------------------------- |
| `m[key] = value`   | Insert or update value      | `m["apple"] = 3;`                |
| `m.find(key)`      | Returns iterator to element | `if(m.find("apple") != m.end())` |
| `m.erase(key)`     | Removes element             | `m.erase("apple");`              |
| `m.size()`         | Returns size                | `cout << m.size();`              |
| `for(auto &p : m)` | Iterate key-value pairs     | `cout << p.first << p.second;`   |

### 🧠 Example

```cpp
unordered_map<string, int> freq;
string s = "banana";
for(char c : s) freq[string(1, c)]++;
for(auto &p : freq) cout << p.first << " => " << p.second << endl;
```

---

## 🧩 4. Sets

### 🔹 Common Functions

| Function      | Description             | Example                    |
| ------------- | ----------------------- | -------------------------- |
| `s.insert(x)` | Insert element          | `s.insert(5);`             |
| `s.erase(x)`  | Remove element          | `s.erase(5);`              |
| `s.count(x)`  | Check if element exists | `if(s.count(5))`           |
| `s.find(x)`   | Get iterator to element | `if(s.find(3) != s.end())` |
| `s.size()`    | Get size                | `cout << s.size();`        |

### 🧠 Example

```cpp
set<int> s = {2, 3, 1};
s.insert(4);
s.erase(2);
for(int x : s) cout << x << " "; // 1 3 4
```

---

## 🧩 5. Stack / Queue / Deque

### 🟣 Stack

| Function  | Description     |
| --------- | --------------- |
| `push(x)` | Add to top      |
| `pop()`   | Remove from top |
| `top()`   | Access top      |
| `empty()` | Check if empty  |

```cpp
stack<int> st;
st.push(1); st.push(2);
cout << st.top(); // 2
st.pop();
```

### 🟢 Queue

| Function  | Description  |
| --------- | ------------ |
| `push(x)` | Add at back  |
| `pop()`   | Remove front |
| `front()` | Access front |
| `back()`  | Access back  |

```cpp
queue<int> q;
q.push(10); q.push(20);
cout << q.front(); // 10
q.pop();
```

### 🔵 Deque

| Function             | Description     |
| -------------------- | --------------- |
| `push_front(x)`      | Add at front    |
| `push_back(x)`       | Add at back     |
| `pop_front()`        | Remove front    |
| `pop_back()`         | Remove back     |
| `front()` / `back()` | Access elements |

```cpp
deque<int> dq;
dq.push_front(1);
dq.push_back(2);
cout << dq.back(); // 2
```

---

## 🧩 6. Algorithms Header&#x20;

| Function                               | Description                             | Example                                 |
| -------------------------------------- | --------------------------------------- | --------------------------------------- |
| `sort(v.begin(), v.end())`             | Sort ascending                          | `sort(v.begin(), v.end());`             |
| `reverse(v.begin(), v.end())`          | Reverse                                 | `reverse(v.begin(), v.end());`          |
| `count(v.begin(), v.end(), x)`         | Count occurrences                       | `count(v.begin(), v.end(), 2);`         |
| `find(v.begin(), v.end(), x)`          | Find element                            | `find(v.begin(), v.end(), 3);`          |
| `accumulate(v.begin(), v.end(), 0)`    | Sum elements                            | `accumulate(v.begin(), v.end(), 0);`    |
| `max_element(v.begin(), v.end())`      | Largest                                 | `*max_element(v.begin(), v.end());`     |
| `min_element(v.begin(), v.end())`      | Smallest                                | `*min_element(v.begin(), v.end());`     |
| `binary_search(v.begin(), v.end(), x)` | Search (sorted)                         | `binary_search(v.begin(), v.end(), 3);` |
| `next_permutation(v.begin(), v.end())` | Generate next lexicographic permutation | `next_permutation(s.begin(), s.end());` |

### 🧠 Example

```cpp
vector<int> v = {1, 3, 5, 7};
if(binary_search(v.begin(), v.end(), 3)) cout << "Found";
cout << accumulate(v.begin(), v.end(), 0); // 16
```

---

## 🧩 7. Math & Miscellaneous

| Function                    | Description              | Example                       |
| --------------------------- | ------------------------ | ----------------------------- |
| `abs(x)`                    | Absolute value           | `abs(-5);`                    |
| `pow(a,b)`                  | Power                    | `pow(2,3); // 8`              |
| `sqrt(x)`                   | Square root              | `sqrt(9);`                    |
| `ceil(x)` / `floor(x)`      | Round up/down            | `ceil(2.3);`                  |
| `max(a,b)` / `min(a,b)`     | Compare                  | `max(3,7);`                   |
| `swap(a,b)`                 | Swap values              | `swap(x,y);`                  |
| `__builtin_popcount(x)`     | Count set bits (for int) | `__builtin_popcount(7); // 3` |
| `rand()` / `srand(time(0))` | Random numbers           | `rand() % 10;`                |

### 🧠 Example

```cpp
int x = -5;
cout << abs(x); // 5
cout << max(3, 9); // 9
cout << __builtin_popcount(15); // 4
```

---

## 🧩 8. Stringstream (Parsing & Tokenizing)

| Function                 | Description                      |
| ------------------------ | -------------------------------- |
| `stringstream ss(str)`   | Create stream from string        |
| `ss >> word`             | Extract tokens (space separated) |
| `getline(ss, word, ',')` | Extract till delimiter           |

### 🧠 Example

```cpp
string line = "apple,banana,grape";
stringstream ss(line);
string word;
while(getline(ss, word, ',')) cout << word << endl;
```

---

## 🧩 9. Priority Queue

| Type                                             | Description |
| ------------------------------------------------ | ----------- |
| `priority_queue<int>`                            | Max heap    |
| `priority_queue<int, vector<int>, greater<int>>` | Min heap    |

### 🧠 Example

```cpp
priority_queue<int> pq;
pq.push(10); pq.push(5);
cout << pq.top(); // 10
pq.pop();
```

---

## 🧩 10. Pair & Tuple

| Function             | Description     | Example                        |
| -------------------- | --------------- | ------------------------------ |
| `make_pair(a,b)`     | Create pair     | `auto p = make_pair(1, "hi");` |
| `p.first / p.second` | Access elements | `cout << p.first;`             |
| `tie(a,b) = t;`      | Unpack tuple    | `tie(x,y) = t;`                |

```cpp
pair<int,string> p = {1, "apple"};
cout << p.first << " " << p.second;
```

---
