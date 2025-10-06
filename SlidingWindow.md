### Best Time to Buy and Sell Stocks

#### Description
Given an array `prices` where `prices[i]` is the price of a stock on day `i`, find the maximum profit you can achieve by buying and selling once. You must buy before you sell.

#### Example
Input: `prices = [7,1,5,3,6,4]`  
Output: `5`  
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6).

Input: `prices = [7,6,4,3,1]`  
Output: `0`

#### Hint
Track the minimum price seen so far and compute potential profit for each day.

#### Brute Force
- Check all pairs of buy/sell days.
- **TC:** O(n²) | **SC:** O(1)

```cpp
int maxProfit(vector<int>& prices) {
    int maxProfit = 0;
    for(int i=0;i<prices.size();i++){
        for(int j=i+1;j<prices.size();j++){
            maxProfit = max(maxProfit, prices[j]-prices[i]);
        }
    }
    return maxProfit;
}
```

#### Optimized Approach (Single Pass)
- Keep track of the minimum price and maximum profit.
- **TC:** O(n) | **SC:** O(1)

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, maxProfit = 0;
    for(int price : prices){
        minPrice = min(minPrice, price);
        maxProfit = max(maxProfit, price - minPrice);
    }
    return maxProfit;
}
```

### Permutations in a String

#### Description
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, otherwise return `false`.

#### Example
Input: `s1 = "ab", s2 = "eidbaooo"`  
Output: `true`  
Explanation: s2 contains "ba", which is a permutation of s1.

Input: `s1 = "ab", s2 = "eidboaoo"`  
Output: `false`

#### Hint
Use a sliding window of size `s1.length()` and compare character counts.

#### Brute Force
- Generate all permutations of `s1` and check if any exists in `s2`.
- **TC:** O(n * n!) | **SC:** O(n!)

```cpp
bool checkInclusion(string s1, string s2) {
    int n = s1.size(), m = s2.size();
    sort(s1.begin(), s1.end());
    do {
        if(s2.find(s1) != string::npos) return true;
    } while(next_permutation(s1.begin(), s1.end()));
    return false;
}
```

#### Optimized Approach (Sliding Window)
- Use a fixed-size sliding window and character count arrays.
- **TC:** O(n) | **SC:** O(26) for lowercase letters

```cpp
bool checkInclusion(string s1, string s2) {
    int n = s1.size(), m = s2.size();
    if(n > m) return false;
    vector<int> count1(26,0), count2(26,0);
    for(int i=0;i<n;i++) {
        count1[s1[i]-'a']++;
        count2[s2[i]-'a']++;
    }
    for(int i=0;i<=m-n;i++) {
        if(count1 == count2) return true;
        if(i+n < m) {
            count2[s2[i]-'a']--;
            count2[s2[i+n]-'a']++;
        }
    }
    return false;
}
```

### Longest Substring Character Replacement

#### Description
Given a string `s` and an integer `k`, return the length of the longest substring containing the same letter after performing at most `k` character replacements.

#### Example
Input: `s = "ABAB", k = 2`  
Output: `4`

Input: `s = "AABABBA", k = 1`  
Output: `4`

#### Hint
Use a sliding window to keep track of the most frequent character count and window size.

#### Brute Force
- Check all substrings and count replacements needed.
- **TC:** O(n²) | **SC:** O(1)

```cpp
int characterReplacement(string s, int k) {
    int maxLen = 0;
    for(int i=0;i<s.size();i++){
        for(int j=i;j<s.size();j++){
            vector<int> count(26,0);
            for(int l=i;l<=j;l++) count[s[l]-'A']++;
            int maxCount = *max_element(count.begin(), count.end());
            if((j-i+1) - maxCount <= k) maxLen = max(maxLen, j-i+1);
        }
    }
    return maxLen;
}
```

#### Optimized Approach (Sliding Window)
- Maintain a window and track max frequency character.
- Expand window; shrink if replacements exceed `k`.
- **TC:** O(n) | **SC:** O(26)

```cpp
int characterReplacement(string s, int k) {
    vector<int> count(26,0);
    int maxCount = 0, left = 0, maxLen = 0;
    for(int right=0; right<s.size(); right++){
        maxCount = max(maxCount, ++count[s[right]-'A']);
        if((right-left+1) - maxCount > k) count[s[left++]-'A']--;
        maxLen = max(maxLen, right-left+1);
    }
    return maxLen;
}
```

### Longest Substring Without Repeating Characters

#### Description
Given a string `s`, find the length of the longest substring without repeating characters.

#### Example
Input: `s = "abcabcbb"`  
Output: `3`  
Explanation: The answer is "abc".

Input: `s = "bbbbb"`  
Output: `1`

#### Hint
Use a sliding window to track characters and their last positions.

#### Brute Force
- Check all substrings for uniqueness.
- **TC:** O(n²) | **SC:** O(min(n,m))

```cpp
int lengthOfLongestSubstring(string s) {
    int maxLen = 0;
    for(int i=0;i<s.size();i++){
        unordered_set<char> seen;
        int len = 0;
        for(int j=i;j<s.size();j++){
            if(seen.count(s[j])) break;
            seen.insert(s[j]);
            len++;
        }
        maxLen = max(maxLen, len);
    }
    return maxLen;
}
```

#### Optimized Approach (Sliding Window)
- Use two pointers and a map to track last seen positions.
- **TC:** O(n) | **SC:** O(min(n,m))

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char,int> lastIndex;
    int maxLen = 0, start = 0;
    for(int i=0;i<s.size();i++){
        if(lastIndex.count(s[i])) start = max(start, lastIndex[s[i]] + 1);
        lastIndex[s[i]] = i;
        maxLen = max(maxLen, i - start + 1);
    }
    return maxLen;
}
```
### Sliding Window Maximum

#### Description
Given an integer array `nums` and an integer `k`, find the maximum value in each sliding window of size `k`.

#### Example
Input: `nums = [1,3,-1,-3,5,3,6,7], k = 3`  
Output: `[3,3,5,5,6,7]`

Input: `nums = [1], k = 1`  
Output: `[1]`

#### Hint
Use a deque to keep track of indices of useful elements for each window.

#### Brute Force
- For each window, find the maximum.
- **TC:** O(n*k) | **SC:** O(1)

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> res;
    for(int i=0;i<=nums.size()-k;i++){
        int mx = nums[i];
        for(int j=i;j<i+k;j++) mx = max(mx, nums[j]);
        res.push_back(mx);
    }
    return res;
}
```

#### Optimized Approach (Deque)
- Use a deque to store indices of elements in descending order.
- Remove indices out of window and smaller elements.
- **TC:** O(n) | **SC:** O(k)

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> res;
    for(int i=0;i<nums.size();i++){
        while(!dq.empty() && dq.front() <= i-k) dq.pop_front();
        while(!dq.empty() && nums[dq.back()] < nums[i]) dq.pop_back();
        dq.push_back(i);
        if(i >= k-1) res.push_back(nums[dq.front()]);
    }
    return res;
}
```
### Minimum Window Substring

#### Description
Given two strings `s` and `t`, return the minimum window in `s` which contains all the characters of `t`. If no such window exists, return an empty string.

#### Example
Input: `s = "ADOBECODEBANC", t = "ABC"`  
Output: `"BANC"`

Input: `s = "a", t = "a"`  
Output: `"a"`

#### Hint
Use a sliding window and two hash maps to track required and current characters.

#### Brute Force
- Check all substrings of `s` and see if they contain `t`.
- **TC:** O(n² * m) | **SC:** O(1) (n = s length, m = t length)

```cpp
string minWindow(string s, string t) {
    int n = s.size();
    string res = "";
    int minLen = INT_MAX;
    for(int i=0;i<n;i++){
        for(int j=i;j<n;j++){
            string sub = s.substr(i, j-i+1);
            unordered_map<char,int> mp;
            for(char c : sub) mp[c]++;
            bool ok = true;
            for(char c : t){
                if(mp[c] < count(t.begin(), t.end(), c)){ok=false; break;}
            }
            if(ok && sub.size() < minLen){
                minLen = sub.size();
                res = sub;
            }
        }
    }
    return res;
}
```

#### Optimized Approach (Sliding Window)
- Use two pointers, a required map and a formed map to track counts.
- Expand window until all required characters are covered, then shrink from left.
- **TC:** O(n + m) | **SC:** O(1) (for character counts)

```cpp
string minWindow(string s, string t) {
    if(s.empty() || t.empty()) return "";
    unordered_map<char,int> required;
    for(char c : t) required[c]++;
    unordered_map<char,int> window;
    int left = 0, right = 0, formed = 0, minLen = INT_MAX, start = 0;
    while(right < s.size()){
        char c = s[right];
        window[c]++;
        if(required.count(c) && window[c] == required[c]) formed++;
        while(left <= right && formed == required.size()){
            if(right - left +1 < minLen){
                minLen = right - left +1;
                start = left;
            }
            char l = s[left];
            window[l]--;
            if(required.count(l) && window[l] < required[l]) formed--;
            left++;
        }
        right++;
    }
    return minLen == INT_MAX ? "" : s.substr(start, minLen);
}
```

