### 1.Valid Palindrome

#### Description
Given a string `s`, return `true` if it is a palindrome, considering only alphanumeric characters and ignoring cases.

#### Example
Input: `s = "A man, a plan, a canal: Panama"`  
Output: `true`

Input: `s = "race a car"`  
Output: `false`

#### Hint
Use two pointers from the start and end, skip non-alphanumeric characters.

#### Brute Force
- Clean the string, then check if reversed string equals original.
- **TC:** O(n) | **SC:** O(n)

```cpp
bool isPalindrome(string s) {
    string filtered = "";
    for(char c : s){
        if(isalnum(c)) filtered += tolower(c);
    }
    string rev = filtered;
    reverse(rev.begin(), rev.end());
    return filtered == rev;
}
```

#### Optimized Approach (Two Pointers)
- Use two pointers without extra space.
- **TC:** O(n) | **SC:** O(1)

```cpp
bool isPalindrome(string s) {
    int left = 0, right = s.size()-1;
    while(left < right){
        while(left < right && !isalnum(s[left])) left++;
        while(left < right && !isalnum(s[right])) right--;
        if(tolower(s[left]) != tolower(s[right])) return false;
        left++; right--;
    }
    return true;
}
```
### 2.Two Sum II

#### Description
Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target`. Return the indices of the two numbers as an array `[index1, index2]` of length 2.

#### Example
Input: `numbers = [2,7,11,15], target = 9`  
Output: `[1,2]`

Input: `numbers = [2,3,4], target = 6`  
Output: `[1,3]`

#### Hint
Since the array is sorted, use two pointers from the start and end.

#### Brute Force
- Check all pairs.
- **TC:** O(n²) | **SC:** O(1)

```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    for(int i=0;i<numbers.size();i++){
        for(int j=i+1;j<numbers.size();j++){
            if(numbers[i]+numbers[j] == target) return {i+1,j+1};
        }
    }
    return {};
}
```

#### Optimized Approach (Two Pointers)
- Start one pointer at the beginning and one at the end.
- Move pointers based on sum.
- **TC:** O(n) | **SC:** O(1)

```cpp
vector<int> twoSum(vector<int>& numbers, int target) {
    int left = 0, right = numbers.size()-1;
    while(left < right){
        int sum = numbers[left] + numbers[right];
        if(sum == target) return {left+1, right+1};
        else if(sum < target) left++;
        else right--;
    }
    return {};
}
```
### 3.3Sum

#### Description
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j != k` and `nums[i] + nums[j] + nums[k] == 0`. The solution set must not contain duplicate triplets.

#### Example
Input: `nums = [-1,0,1,2,-1,-4]`  
Output: `[[-1,-1,2],[-1,0,1]]`

Input: `nums = [0,1,1]`  
Output: `[]`

#### Hint
Sort the array, fix one element, and use two pointers for the other two.

#### Brute Force
- Check all triplets.
- **TC:** O(n³) | **SC:** O(1)

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> res;
    sort(nums.begin(), nums.end());
    int n = nums.size();
    for(int i=0;i<n-2;i++){
        if(i>0 && nums[i] == nums[i-1]) continue;
        for(int j=i+1;j<n-1;j++){
            if(j>i+1 && nums[j]==nums[j-1]) continue;
            for(int k=j+1;k<n;k++){
                if(k>j+1 && nums[k]==nums[k-1]) continue;
                if(nums[i]+nums[j]+nums[k]==0) res.push_back({nums[i], nums[j], nums[k]});
            }
        }
    }
    return res;
}
```

#### Optimized Approach (Sorting + Two Pointers)
- Sort array.
- Fix `i`, use two pointers `left` and `right` to find pairs summing to `-nums[i]`.
- **TC:** O(n²) | **SC:** O(1) ignoring output

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> res;
    sort(nums.begin(), nums.end());
    int n = nums.size();
    for(int i=0;i<n-2;i++){
        if(i>0 && nums[i]==nums[i-1]) continue;
        int left=i+1, right=n-1;
        while(left<right){
            int sum = nums[i]+nums[left]+nums[right];
            if(sum==0){
                res.push_back({nums[i], nums[left], nums[right]});
                while(left<right && nums[left]==nums[left+1]) left++;
                while(left<right && nums[right]==nums[right-1]) right--;
                left++; right--;
            } else if(sum<0) left++;
            else right--;
        }
    }
    return res;
}
```
### 4.Container With Most Water

#### Description
Given an array `height` where each element represents the height of a vertical line on the x-axis, find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum area.

#### Example
Input: `height = [1,8,6,2,5,4,8,3,7]`  
Output: `49`

Input: `height = [1,1]`  
Output: `1`

#### Hint
Use two pointers at the start and end, move the shorter line inward.

#### Brute Force
- Check all pairs of lines.
- **TC:** O(n²) | **SC:** O(1)

```cpp
int maxArea(vector<int>& height) {
    int maxArea = 0;
    for(int i=0;i<height.size();i++){
        for(int j=i+1;j<height.size();j++){
            int area = min(height[i], height[j]) * (j-i);
            maxArea = max(maxArea, area);
        }
    }
    return maxArea;
}
```

#### Optimized Approach (Two Pointers)
- Use two pointers from start and end.
- Move the pointer with smaller height inward.
- **TC:** O(n) | **SC:** O(1)

```cpp
int maxArea(vector<int>& height) {
    int left = 0, right = height.size()-1;
    int maxArea = 0;
    while(left < right){
        int area = min(height[left], height[right]) * (right-left);
        maxArea = max(maxArea, area);
        if(height[left] < height[right]) left++;
        else right--;
    }
    return maxArea;
}
```
### 5.Trapping Rain Water

#### Description
Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

#### Example
Input: `height = [0,1,0,2,1,0,1,3,2,1,2,1]`  
Output: `6`

Input: `height = [4,2,0,3,2,5]`  
Output: `9`

#### Hint
Use the concept of left max and right max for each position.

#### Brute Force
- For each position, find the max height on left and right, then min(leftMax, rightMax) - height[i].
- **TC:** O(n²) | **SC:** O(1)

```cpp
int trap(vector<int>& height) {
    int n = height.size(), water = 0;
    for(int i=0;i<n;i++){
        int leftMax = 0, rightMax = 0;
        for(int j=0;j<=i;j++) leftMax = max(leftMax, height[j]);
        for(int j=i;j<n;j++) rightMax = max(rightMax, height[j]);
        water += min(leftMax, rightMax) - height[i];
    }
    return water;
}
```

#### Optimized Approach (Two Pointers)
- Use two pointers, track leftMax and rightMax, move pointers inward.
- **TC:** O(n) | **SC:** O(1)

```cpp
int trap(vector<int>& height) {
    int left = 0, right = height.size()-1;
    int leftMax = 0, rightMax = 0, water = 0;
    while(left < right){
        if(height[left] < height[right]){
            height[left] >= leftMax ? (leftMax = height[left]) : water += leftMax - height[left];
            left++;
        } else {
            height[right] >= rightMax ? (rightMax = height[right]) : water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```
### 6.Remove Duplicates from Sorted Array

#### Description
Given a sorted array `nums`, remove the duplicates in-place such that each element appears only once and return the new length.

#### Example
Input: `nums = [1,1,2]`  
Output: `2`, `nums = [1,2]`

Input: `nums = [0,0,1,1,1,2,2,3,3,4]`  
Output: `5`, `nums = [0,1,2,3,4]`

#### Hint
Use two pointers: one to iterate, one to place the next unique element.

#### Brute Force
- Use extra array to store unique elements.
- **TC:** O(n) | **SC:** O(n)

```cpp
int removeDuplicates(vector<int>& nums) {
    if(nums.empty()) return 0;
    vector<int> temp;
    temp.push_back(nums[0]);
    for(int i=1;i<nums.size();i++){
        if(nums[i]!=nums[i-1]) temp.push_back(nums[i]);
    }
    for(int i=0;i<temp.size();i++) nums[i]=temp[i];
    return temp.size();
}
```

#### Optimized Approach (Two Pointers)
- Use slow pointer for placement, fast pointer for iteration.
- **TC:** O(n) | **SC:** O(1)

```cpp
int removeDuplicates(vector<int>& nums) {
    if(nums.empty()) return 0;
    int j = 0;
    for(int i=1;i<nums.size();i++){
        if(nums[i]!=nums[j]) nums[++j] = nums[i];
    }
    return j+1;
}
```
### 7.Next Permutation

#### Description
Implement `nextPermutation`, which rearranges numbers into the lexicographically next greater permutation of numbers. If such an arrangement is not possible, it rearranges it as the lowest possible order (sorted in ascending order).

#### Example
Input: `nums = [1,2,3]`  
Output: `[1,3,2]`

Input: `nums = [3,2,1]`  
Output: `[1,2,3]`

#### Hint
Find the first decreasing element from the end, swap it with the smallest larger element on its right, then reverse the suffix.

#### Brute Force
- Generate all permutations, find the next one.
- **TC:** O(n!) | **SC:** O(n!)

```cpp
void nextPermutation(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    do{
        if(nums > original) break;
    }while(next_permutation(nums.begin(), nums.end()));
}
```

#### Optimized Approach (In-place)
- Traverse from end to find first decreasing element `i`.
- Find smallest element > nums[i] to swap.
- Reverse from i+1 to end.
- **TC:** O(n) | **SC:** O(1)

```cpp
void nextPermutation(vector<int>& nums) {
    int n = nums.size(), i = n - 2;
    while(i >= 0 && nums[i] >= nums[i+1]) i--;
    if(i >= 0){
        int j = n - 1;
        while(nums[j] <= nums[i]) j--;
        swap(nums[i], nums[j]);
    }
    reverse(nums.begin()+i+1, nums.end());
}
```

