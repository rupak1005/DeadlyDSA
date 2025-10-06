## 1. Implement Trie (Prefix Tree)

### Problem Visualization

Insert words: ["apple", "app", "bat"]

```
root
 ├─ a
 │  └─ p
 │     └─ p
 │        ├─ l
 │        │  └─ e (isEnd=true)
 │        └─ (isEnd=true)
 └─ b
    └─ a
       └─ t (isEnd=true)
```

### Input / Output

* **Input:** List of words to insert
* **Output:** Trie supporting insert, search, and prefix search

### Hints

1. Each node contains children map and boolean flag for end-of-word.
2. Insert by creating nodes character by character.
3. Search or startsWith traverse nodes according to characters.

### Optimal Solution (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    bool isEnd;
    unordered_map<char, TrieNode*> children;
    TrieNode() : isEnd(false) {}
};

class Trie {
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c)) node->children[c] = new TrieNode();
            node = node->children[c];
        }
        node->isEnd = true;
    }
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c)) return false;
            node = node->children[c];
        }
        return node->isEnd;
    }
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (!node->children.count(c)) return false;
            node = node->children[c];
        }
        return true;
    }
};
```

* **Time:** O(L) per operation, L = word length
* **Space:** O(N * L)

---

## 2. Word Search II

### Problem Visualization

Board:

```
[['o','a','a','n'],
 ['e','t','a','e'],
 ['i','h','k','r'],
 ['i','f','l','v']]
```

Words: ["oath", "pea", "eat", "rain"]

### Input / Output

* **Input:** 2D board and list of words
* **Output:** List of words present on board

### Hints

1. Use Trie to store words.
2. DFS from each cell, backtracking and marking visited.
3. Stop DFS early if current prefix not in Trie.

### Optimal Solution (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    bool isEnd;
    unordered_map<char, TrieNode*> children;
    TrieNode() : isEnd(false) {}
};

class Trie {
public:
    TrieNode* root;
    Trie() { root = new TrieNode(); }
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c)) node->children[c] = new TrieNode();
            node = node->children[c];
        }
        node->isEnd = true;
    }
};

void dfs(vector<vector<char>>& board, int i, int j, TrieNode* node, string path, set<string>& res) {
    if (i<0 || i>=board.size() || j<0 || j>=board[0].size() || !node->children.count(board[i][j])) return;
    char c = board[i][j];
    node = node->children[c];
    path += c;
    if (node->isEnd) res.insert(path);
    board[i][j] = '#'; // mark visited
    dfs(board,i+1,j,node,path,res);
    dfs(board,i-1,j,node,path,res);
    dfs(board,i,j+1,node,path,res);
    dfs(board,i,j-1,node,path,res);
    board[i][j] = c; // restore
}

vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
    Trie trie;
    for (auto &w : words) trie.insert(w);
    set<string> res;
    for (int i=0;i<board.size();i++)
        for (int j=0;j<board[0].size();j++)
            dfs(board,i,j,trie.root,"",res);
    return vector<string>(res.begin(),res.end());
}
```

* **Time:** O(M*N*4^L), M*N board size, L = max word length
* **Space:** O(W*L + recursion stack)

---

## 3. Design Add & Search Words

### Problem Visualization

Words added: ["bad","dad","mad"]

Search examples:

* search("pad") → false
* search("bad") → true
* search(".ad") → true (dot matches any letter)

### Input / Output

* **Input:** Words to add, search queries (may contain '.')
* **Output:** Boolean for each search

### Hints

1. Use Trie to store words.
2. DFS search with recursion to handle '.' wildcard.

### Optimal Solution (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    bool isEnd;
    unordered_map<char, TrieNode*> children;
    TrieNode() : isEnd(false) {}
};

class WordDictionary {
    TrieNode* root;
public:
    WordDictionary() { root = new TrieNode(); }
    void addWord(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children.count(c)) node->children[c] = new TrieNode();
            node = node->children[c];
        }
        node->isEnd = true;
    }

    bool search(string word) {
        return dfs(root, word, 0);
    }

    bool dfs(TrieNode* node, string &word, int idx) {
        if (!node) return false;
        if (idx == word.size()) return node->isEnd;
        char c = word[idx];
        if (c == '.') {
            for (auto &[k, child] : node->children)
                if (dfs(child, word, idx+1)) return true;
            return false;
        } else {
            return node->children.count(c) && dfs(node->children[c], word, idx+1);
        }
    }
};
```

* **Time:** O(L) per add/search in best case, O(26^L) with wildcards
* **Space:** O(N*L)

