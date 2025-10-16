### 1.Fizz Buzz

#### Description
Write a program that outputs the string representation of numbers from 1 to `n`. But for multiples of 3, output "Fizz" instead of the number, for multiples of 5 output "Buzz", and for multiples of both 3 and 5 output "FizzBuzz".

#### Example
Input: `n = 15`  
Output: `["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]`

#### Hint
Check divisibility for 3 and 5, and build the output string accordingly.

#### Brute Force
- Iterate from 1 to n, use if-else statements.
- **TC:** O(n) | **SC:** O(n)

```cpp
vector<string> fizzBuzz(int n) {
    vector<string> res;
    for(int i=1;i<=n;i++){
        if(i%15==0) res.push_back("FizzBuzz");
        else if(i%3==0) res.push_back("Fizz");
        else if(i%5==0) res.push_back("Buzz");
        else res.push_back(to_string(i));
    }
    return res;
}
```

#### Optimized Approach
- Brute force is already optimal for this problem.
- **TC:** O(n) | **SC:** O(n)

### 2.Longest Common Prefix

#### Description
Write a function to find the longest common prefix string among an array of strings. If there is no common prefix, return an empty string.

#### Example
Input: `strs = ["flower","flow","flight"]`  
Output: `"fl"`

Input: `strs = ["dog","racecar","car"]`  
Output: `""`

#### Hint
Compare characters of the strings one by one, or use divide and conquer.

#### Brute Force
- Compare characters of the first string with others.
- **TC:** O(n*m) | **SC:** O(1) (n = number of strings, m = length of first string)

```cpp
string longestCommonPrefix(vector<string>& strs) {
    if(strs.empty()) return "";
    string prefix = strs[0];
    for(int i=1;i<strs.size();i++){
        int j=0;
        while(j<prefix.size() && j<strs[i].size() && prefix[j]==strs[i][j]) j++;
        prefix = prefix.substr(0,j);
        if(prefix.empty()) return "";
    }
    return prefix;
}
```

#### Optimized Approach
- Vertical scanning, binary search, or divide and conquer can be used.
- **TC:** O(n*m) | **SC:** O(1)

### 3.Encode and Decode Strings

#### Description
Design an algorithm to encode a list of strings to a single string and decode it back to the original list of strings.

#### Example
Input: `strs = ["hello","world"]`  
Encoded Output: "5#hello5#world"  
Decoded Output: `["hello","world"]`

#### Hint
Use a delimiter or length-prefix to separate strings during encoding.

#### Brute Force
- Use a delimiter to join strings, then split using that delimiter.
- **TC:** O(n) | **SC:** O(n)

```cpp
string encode(vector<string>& strs) {
    string res = "";
    for(auto s : strs) res += to_string(s.size()) + '#' + s;
    return res;
}

vector<string> decode(string s) {
    vector<string> res;
    int i=0;
    while(i < s.size()){
        int j = i;
        while(s[j] != '#') j++;
        int len = stoi(s.substr(i, j-i));
        res.push_back(s.substr(j+1, len));
        i = j + 1 + len;
    }
    return res;
}
```

#### Optimized Approach
- Brute force using length-prefix is already efficient.
- **TC:** O(n) | **SC:** O(n)

### 5.Longest Palindromic Substring

#### Description
Given a string `s`, return the longest palindromic substring in `s`.

#### Example
Input: `s = "babad"`  
Output: `"bab"` (or "aba")

Input: `s = "cbbd"`  
Output: `"bb"`

#### Hint
Expand around center or use dynamic programming to check palindromes.

#### Brute Force
- Check all substrings if they are palindrome.
- **TC:** O(n³) | **SC:** O(1)

```cpp
string longestPalindrome(string s) {
    int n = s.size(), maxLen = 0;
    string res = "";
    for(int i=0;i<n;i++){
        for(int j=i;j<n;j++){
            string sub = s.substr(i,j-i+1);
            string rev = sub; reverse(rev.begin(), rev.end());
            if(sub == rev && sub.size() > maxLen){
                maxLen = sub.size();
                res = sub;
            }
        }
    }
    return res;
}
```

#### Optimized Approach (Expand Around Center)
- For each character, expand around it and between i and i+1.
- **TC:** O(n²) | **SC:** O(1)

```cpp
string expandFromCenter(const string &s, int left, int right){
    while(left>=0 && right<s.size() && s[left]==s[right]){
        left--; right++;
    }
    return s.substr(left+1, right-left-1);
}

string longestPalindrome(string s) {
    if(s.empty()) return "";
    string res = s.substr(0,1);
    for(int i=0;i<s.size();i++){
        string s1 = expandFromCenter(s,i,i);
        string s2 = expandFromCenter(s,i,i+1);
        if(s1.size() > res.size()) res = s1;
        if(s2.size() > res.size()) res = s2;
    }
    return res;
}
```

### 6.Text Justification

#### Description
Given an array of words and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully justified. Extra spaces should be distributed as evenly as possible.

#### Example
Input: `words = ["This","is","an","example","of","text","justification."], maxWidth = 16`  
Output:
```
["This    is    an",
 "example  of text",
 "justification.  "]
```

#### Hint
Greedily pack words into a line. Distribute extra spaces, and left-justify the last line.

#### Brute Force
- Build lines word by word and pad spaces.
- **TC:** O(n) | **SC:** O(n)

```cpp
vector<string> fullJustify(vector<string>& words, int maxWidth) {
    vector<string> res;
    int n = words.size(), i = 0;
    while(i < n){
        int lineLen = words[i].size(), j = i+1;
        while(j<n && lineLen + 1 + words[j].size() <= maxWidth){
            lineLen += 1 + words[j].size();
            j++;
        }
        int spaces = maxWidth - lineLen + (j-i-1);
        string line = words[i];
        for(int k=i+1;k<j;k++){
            int space = (j-i-1==0) ? spaces : spaces/(j-i-1) + (k-i <= spaces%(j-i-1) ? 1 : 0);
            line += string(space, ' ') + words[k];
        }
        line += string(maxWidth-line.size(), ' ');
        res.push_back(line);
        i = j;
    }
    return res;
}
```

#### Optimized Approach
- Greedy line building with careful space distribution is already efficient.
- **TC:** O(n) | **SC:** O(n)

### 7.Palindromic Substrings

#### Description
Given a string `s`, return the number of palindromic substrings in it. A substring is palindromic if it reads the same backward as forward.

#### Example
Input: `s = "abc"`  
Output: `3`  ("a", "b", "c")

Input: `s = "aaa"`  
Output: `6`  ("a", "a", "a", "aa", "aa", "aaa")

#### Hint
Expand around each center to count palindromes.

#### Brute Force
- Check all substrings if they are palindromes.
- **TC:** O(n³) | **SC:** O(1)

```cpp
int countSubstrings(string s) {
    int n = s.size(), count = 0;
    for(int i=0;i<n;i++){
        for(int j=i;j<n;j++){
            string sub = s.substr(i,j-i+1);
            string rev = sub; reverse(rev.begin(), rev.end());
            if(sub == rev) count++;
        }
    }
    return count;
}
```

#### Optimized Approach (Expand Around Center)
- Treat each character and gap between characters as center, expand to count palindromes.
- **TC:** O(n²) | **SC:** O(1)

```cpp
int expandFromCenter(const string &s, int left, int right){
    int count = 0;
    while(left>=0 && right<s.size() && s[left]==s[right]){
        count++; left--; right++;
    }
    return count;
}

int countSubstrings(string s) {
    int res = 0;
    for(int i=0;i<s.size();i++){
        res += expandFromCenter(s,i,i);   // odd length
        res += expandFromCenter(s,i,i+1); // even length
    }
    return res;
}
```

