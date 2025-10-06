### 1.Two Sum

#### Description
Find indices of two numbers in `nums` that add up to `target`. Each input has one unique solution.

#### Example
Input: `nums = [2,7,11,15], target = 9`  
Output: `[0,1]` → `2 + 7 = 9`

#### Hint
For each element, check if its complement `(target - nums[i])` exists.

#### Brute Force
- Use nested loops to check all pairs.
- **TC:** O(n²) | **SC:** O(1)

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    for(int i=0;i<nums.size();i++){
        for(int j=i+1;j<nums.size();j++){
            if(nums[i]+nums[j]==target) return {i,j};
        }
    }
    return {};
}
```

#### Optimized (Hash Map)
- Store numbers in a map while iterating.
- Check if `target - nums[i]` exists.
- **TC:** O(n) | **SC:** O(n)

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> mp;
    for(int i=0;i<nums.size();i++){
        int c = target - nums[i];
        if(mp.count(c)) return {mp[c], i};
        mp[nums[i]] = i;
    }
    return {};
}
```

### 2.Contains Duplicate

#### Description
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and `false` if every element is distinct.

#### Example
Input: `nums = [1,2,3,1]`  
Output: `true`

Input: `nums = [1,2,3,4]`  
Output: `false`

#### Hint
Check if an element has been seen before.

#### Brute Force
- Compare each pair of elements.
- **TC:** O(n²) | **SC:** O(1)

```cpp
bool containsDuplicate(vector<int>& nums) {
    for(int i=0;i<nums.size();i++){
        for(int j=i+1;j<nums.size();j++){
            if(nums[i]==nums[j]) return true;
        }
    }
    return false;
}
```

#### Optimized (Hash Set)
- Use a set to track seen numbers.
- If an element is already in the set, return `true`.
- **TC:** O(n) | **SC:** O(n)

```cpp
bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;
    for(int num : nums) {
        if(seen.count(num)) return true;
        seen.insert(num);
    }
    return false;
}
```


### 3.Contains Duplicate II

#### Description
Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

#### Example
Input: `nums = [1,2,3,1], k = 3`  
Output: `true`

Input: `nums = [1,0,1,1], k = 1`  
Output: `true`

Input: `nums = [1,2,3,1,2,3], k = 2`  
Output: `false`

#### Hint
Keep track of the last seen index of each number.

#### Brute Force
- Check all pairs `(i, j)` with `|i - j| <= k`.
- **TC:** O(n * k) | **SC:** O(1)

```cpp
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    for(int i=0;i<nums.size();i++){
        for(int j=i+1;j<nums.size() && j<=i+k;j++){
            if(nums[i]==nums[j]) return true;
        }
    }
    return false;
}
```

#### Optimized (Hash Map)
- Store each number with its last index.
- If the number reappears and the distance between indices is ≤ k, return `true`.
- **TC:** O(n) | **SC:** O(n)

```cpp
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_map<int,int> mp;
    for(int i=0;i<nums.size();i++){
        if(mp.count(nums[i]) && i - mp[nums[i]] <= k) return true;
        mp[nums[i]] = i;
    }
    return false;
}
```
### 4.Valid Anagram

#### Description
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

#### Example
Input: `s = "anagram", t = "nagaram"`  
Output: `true`

Input: `s = "rat", t = "car"`  
Output: `false`

#### Hint
An anagram has the same character counts for all letters.

#### Brute Force
- Sort both strings and compare.
- **TC:** O(n log n) | **SC:** O(1) or O(n) for sorting

```cpp
bool isAnagram(string s, string t) {
    if(s.size() != t.size()) return false;
    sort(s.begin(), s.end());
    sort(t.begin(), t.end());
    return s == t;
}
```

#### Optimized (Hash Map / Count Array)
- Count frequency of characters in `s` and `t` and compare.
- **TC:** O(n) | **SC:** O(1) (26 letters for lowercase English)

```cpp
bool isAnagram(string s, string t) {
    if(s.size() != t.size()) return false;
    vector<int> count(26,0);
    for(int i=0;i<s.size();i++) {
        count[s[i]-'a']++;
        count[t[i]-'a']--;
    }
    for(int c : count) if(c != 0) return false;
    return true;
}
```
### 5.Group Anagrams

#### Description
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

#### Example
Input: `strs = ["eat","tea","tan","ate","nat","bat"]`  
Output: `[["eat","tea","ate"],["tan","nat"],["bat"]]`

#### Hint
Anagrams have the same character counts or sorted string.

#### Brute Force
- Compare each string with all others to form groups.
- **TC:** O(n² * k) | **SC:** O(n * k) where k is max string length

#### Optimized (Hash Map)
- Sort each string or use a character count as a key in a map to group anagrams.
- **TC:** O(n * k log k) for sorting each string, **SC:** O(n * k)

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for(string s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        mp[key].push_back(s);
    }
    vector<vector<string>> result;
    for(auto &p : mp) result.push_back(p.second);
    return result;
}
```
### 6.Product of Array Except Self

#### Description
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all elements of `nums` except `nums[i]`. Do not use division and solve in O(n) time.

#### Example
Input: `nums = [1,2,3,4]`  
Output: `[24,12,8,6]`

#### Hint
Use prefix and suffix products.

#### Brute Force
- Compute the product for each index by multiplying all other elements.
- **TC:** O(n²) | **SC:** O(1)

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n,1);
    for(int i=0;i<n;i++) {
        for(int j=0;j<n;j++) {
            if(i!=j) res[i]*=nums[j];
        }
    }
    return res;
}
```

#### Optimized (Prefix and Suffix)
- Compute prefix product from left and suffix product from right.
- Multiply prefix[i-1] * suffix[i+1] for each index.
- **TC:** O(n) | **SC:** O(1) extra space (excluding output)

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n,1);
    int prefix = 1;
    for(int i=0;i<n;i++) {
        res[i] = prefix;
        prefix *= nums[i];
    }
    int suffix = 1;
    for(int i=n-1;i>=0;i--) {
        res[i] *= suffix;
        suffix *= nums[i];
    }
    return res;
}
```
### 7.Top K Elements

#### Description
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

#### Example
Input: `nums = [1,1,1,2,2,3], k = 2`  
Output: `[1,2]`

Input: `nums = [1], k = 1`  
Output: `[1]`

#### Hint
Count frequency of elements, then retrieve the top k.

#### Brute Force
- Count frequency, then sort elements by frequency.
- **TC:** O(n log n) | **SC:** O(n)

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int,int> freq;
    for(int n : nums) freq[n]++;
    vector<pair<int,int>> vec(freq.begin(), freq.end());
    sort(vec.begin(), vec.end(), [](auto &a, auto &b){ return a.second > b.second; });
    vector<int> res;
    for(int i=0;i<k;i++) res.push_back(vec[i].first);
    return res;
}
```

#### Optimized (Heap)
- Use a min-heap (priority queue) of size k to maintain top k frequent elements.
- **TC:** O(n log k) | **SC:** O(n)

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int,int> freq;
    for(int n : nums) freq[n]++;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    for(auto &p : freq) {
        pq.push({p.second, p.first});
        if(pq.size() > k) pq.pop();
    }
    vector<int> res;
    while(!pq.empty()) {
        res.push_back(pq.top().second);
        pq.pop();
    }
    reverse(res.begin(), res.end());
    return res;
}
```
### 8.Roman to Integer

#### Description
Convert a Roman numeral string `s` to an integer.

#### Example
Input: `s = "III"`  
Output: `3`

Input: `s = "IV"`  
Output: `4`

Input: `s = "LVIII"`  
Output: `58`

#### Hint
Process from left to right. If a smaller value comes before a larger one, subtract it; otherwise, add it.

#### Brute Force
- Check every character and apply subtraction rule manually.
- **TC:** O(n) | **SC:** O(1)

```cpp
int romanToInt(string s) {
    int res = 0;
    unordered_map<char,int> mp{{'I',1},{'V',5},{'X',10},{'L',50},{'C',100},{'D',500},{'M',1000}};
    for(int i=0;i<s.size();i++){
        if(i+1<s.size() && mp[s[i]] < mp[s[i+1]]) res -= mp[s[i]];
        else res += mp[s[i]];
    }
    return res;
}
```

#### Optimized Approach
- Same as brute force since the approach is already O(n).
- **TC:** O(n) | **SC:** O(1)

### 9.Verify Alien Dictionary

#### Description
Given a list of words and a string `order` representing the alphabet order of an alien language, return `true` if the words are sorted lexicographically according to this order.

#### Example
Input: `words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"`  
Output: `true`

Input: `words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"`  
Output: `false`

#### Hint
Compare each pair of adjacent words. For the first differing character, ensure it is in correct order.

#### Brute Force
- Compare all pairs of words character by character.
- **TC:** O(n * m) | **SC:** O(1) (n = number of words, m = max word length)

```cpp
bool isAlienSorted(vector<string>& words, string order) {
    unordered_map<char,int> mp;
    for(int i=0;i<order.size();i++) mp[order[i]] = i;
    for(int i=0;i<words.size()-1;i++){
        string a = words[i], b = words[i+1];
        for(int j=0;j<min(a.size(),b.size());j++){
            if(a[j]!=b[j]){
                if(mp[a[j]] > mp[b[j]]) return false;
                goto next;
            }
        }
        if(a.size() > b.size()) return false;
        next:;
    }
    return true;
}
```

#### Optimized Approach
- Same as brute force; comparing adjacent words is already efficient.
- **TC:** O(n * m) | **SC:** O(1)

### 10.Longest Consecutive Sequence

#### Description
Given an unsorted integer array `nums`, return the length of the longest consecutive elements sequence.

#### Example
Input: `nums = [100,4,200,1,3,2]`  
Output: `4`  
Explanation: The longest consecutive sequence is `[1,2,3,4]`.

Input: `nums = [0,3,7,2,5,8,4,6,0,1]`  
Output: `9`

#### Hint
Use a set to quickly check for the start of a sequence.

#### Brute Force
- Sort the array and count consecutive sequences.
- **TC:** O(n log n) | **SC:** O(1)

```cpp
int longestConsecutive(vector<int>& nums) {
    if(nums.empty()) return 0;
    sort(nums.begin(), nums.end());
    int longest = 1, count = 1;
    for(int i=1;i<nums.size();i++){
        if(nums[i] != nums[i-1]){
            if(nums[i] == nums[i-1] + 1) count++;
            else count = 1;
            longest = max(longest, count);
        }
    }
    return longest;
}
```

#### Optimized Approach (Hash Set)
- Insert all numbers into a set.
- For each number, check if it's the start of a sequence (`num - 1` not in set).
- Count the length of the consecutive sequence starting from it.
- **TC:** O(n) | **SC:** O(n)

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> s(nums.begin(), nums.end());
    int longest = 0;
    for(int num : s){
        if(!s.count(num - 1)){
            int current = num, length = 1;
            while(s.count(current + 1)){
                current++; length++;
            }
            longest = max(longest, length);
        }
    }
    return longest;
}
```

### 11.First Missing Positive

#### Description
Given an unsorted integer array `nums`, return the smallest missing positive integer.

#### Example
Input: `nums = [1,2,0]`  
Output: `3`

Input: `nums = [3,4,-1,1]`  
Output: `2`

#### Hint
Place each number `x` at index `x-1` if possible.

#### Brute Force
- Check every positive integer starting from 1.
- **TC:** O(n²) | **SC:** O(1)

```cpp
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    for(int i=1;;i++){
        bool found = false;
        for(int num : nums){
            if(num == i){
                found = true;
                break;
            }
        }
        if(!found) return i;
    }
}
```
or 

```cpp
 int firstMissingPositive(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int missing=1;
        for(auto i:nums){
            if(missing==i)missing++;
        }
        return missing;
    }
```

#### Optimized Approach (Index Placement)
- Place each number `x` at `nums[x-1]` if in range.
- First index `i` where `nums[i] != i+1` gives missing positive `i+1`.
- **TC:** O(n) | **SC:** O(1)

```cpp
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    for(int i=0;i<n;i++){
        while(nums[i]>0 && nums[i]<=n && nums[nums[i]-1]!=nums[i])
            swap(nums[i], nums[nums[i]-1]);
    }
    for(int i=0;i<n;i++){
        if(nums[i] != i+1) return i+1;
    }
    return n+1;
}
```

