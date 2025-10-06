### 1.Find First and Last Position of Element in Sorted Array

#### Description
Given an array of integers `nums` sorted in ascending order and a target value `target`, find the starting and ending position of a given target. If the target is not found, return `[-1, -1]`.

#### Example
Input: `nums = [5,7,7,8,8,10], target = 8`  
Output: `[3,4]`

Input: `nums = [5,7,7,8,8,10], target = 6`  
Output: `[-1,-1]`

#### Hint
Use binary search to find the first and last occurrence separately.

#### Brute Force
- Iterate through array to find first and last positions.
- **TC:** O(n) | **SC:** O(1)

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    int first = -1, last = -1;
    for(int i=0;i<nums.size();i++){
        if(nums[i]==target){
            if(first==-1) first=i;
            last=i;
        }
    }
    return {first,last};
}
```

#### Optimized Approach (Binary Search)
- Use binary search to find leftmost (first) and rightmost (last) indices.
- **TC:** O(log n) | **SC:** O(1)

```cpp
int findBound(vector<int>& nums, int target, bool isFirst){
    int left=0, right=nums.size()-1, bound=-1;
    while(left<=right){
        int mid = left + (right-left)/2;
        if(nums[mid]==target){
            bound = mid;
            if(isFirst) right = mid-1;
            else left = mid+1;
        } else if(nums[mid]<target) left=mid+1;
        else right=mid-1;
    }
    return bound;
}

vector<int> searchRange(vector<int>& nums, int target){
    return {findBound(nums,target,true), findBound(nums,target,false)};
}
```
### 2.Sort Colors

#### Description
Given an array `nums` with `n` objects colored red, white, or blue (represented as 0, 1, and 2), sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

#### Example
Input: `nums = [2,0,2,1,1,0]`  
Output: `[0,0,1,1,2,2]`

Input: `nums = [2,0,1]`  
Output: `[0,1,2]`

#### Hint
Use the Dutch National Flag algorithm: three pointers for low, mid, and high.

#### Brute Force
- Count occurrences of 0,1,2 and overwrite array.
- **TC:** O(n) | **SC:** O(n)

```cpp
void sortColors(vector<int>& nums) {
    int count[3] = {0,0,0};
    for(int num : nums) count[num]++;
    int idx = 0;
    for(int i=0;i<3;i++){
        for(int j=0;j<count[i];j++) nums[idx++] = i;
    }
}
```

#### Optimized Approach (Dutch National Flag)
- Use three pointers: `low`, `mid`, `high` to sort in-place.
- **TC:** O(n) | **SC:** O(1)

```cpp
void sortColors(vector<int>& nums) {
    int low=0, mid=0, high=nums.size()-1;
    while(mid <= high){
        if(nums[mid]==0) swap(nums[low++], nums[mid++]);
        else if(nums[mid]==1) mid++;
        else swap(nums[mid], nums[high--]);
    }
}
```
### 3.Majority Element

#### Description

Given an array `nums` of size `n`, return the majority element. The majority element is the element that appears more than `⌊n/2⌋` times. You may assume that the array is non-empty and the majority element always exists.

#### Example

Input: `nums = [3,2,3]`\
Output: `3`

Input: `nums = [2,2,1,1,1,2,2]`\
Output: `2`

#### Hint

Use a hashmap to count occurrences or use the Boyer-Moore Voting Algorithm.

#### Brute Force

- Count frequency using hashmap.
- **TC:** O(n) | **SC:** O(n)

```cpp
int majorityElement(vector<int>& nums) {
    unordered_map<int,int> freq;
    for(int num : nums) freq[num]++;
    for(auto &p : freq){
        if(p.second > nums.size()/2) return p.first;
    }
    return -1;
}
```

#### Optimized Approach (Boyer-Moore Voting)

- Maintain a candidate and a count.
- Increment count if same, decrement if different; reset candidate if count becomes 0.
- **TC:** O(n) | **SC:** O(1)

```cpp
int majorityElement(vector<int>& nums) {
    int count = 0, candidate = 0;
    for(int num : nums){
        if(count == 0) candidate = num;
        count += (num == candidate) ? 1 : -1;
    }
    return candidate;
}
```

### 4.Search a 2D Matrix

#### Description
Write an efficient algorithm that searches for a value `target` in an `m x n` matrix. This matrix has the following properties:
- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

#### Example
Input: `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3`  
Output: `true`

Input: `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13`  
Output: `false`

#### Hint
Treat the matrix as a flattened sorted array and use binary search.

#### Brute Force
- Iterate over each element and compare with target.
- **TC:** O(m*n) | **SC:** O(1)

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    for(auto &row : matrix){
        for(int val : row){
            if(val == target) return true;
        }
    }
    return false;
}
```

#### Optimized Approach (Binary Search)
- Treat matrix as a 1D sorted array of size `m*n`.
- Map index `i` to `matrix[i/n][i%n]`.
- **TC:** O(log(m*n)) | **SC:** O(1)

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if(matrix.empty() || matrix[0].empty()) return false;
    int m = matrix.size(), n = matrix[0].size();
    int left = 0, right = m*n - 1;
    while(left <= right){
        int mid = left + (right-left)/2;
        int mid_val = matrix[mid/n][mid%n];
        if(mid_val == target) return true;
        else if(mid_val < target) left = mid + 1;
        else right = mid - 1;
    }
    return false;
}
```
### 5.Find Minimum in Rotated Sorted Array

#### Description
Suppose an array of unique integers `nums` is sorted in ascending order and then rotated at some pivot unknown to you. Find the minimum element in this rotated sorted array.

#### Example
Input: `nums = [3,4,5,1,2]`  
Output: `1`

Input: `nums = [4,5,6,7,0,1,2]`  
Output: `0`

Input: `nums = [11,13,15,17]`  
Output: `11`

#### Hint
Use binary search to find the point where the rotation occurs; the minimum is the only element smaller than its previous.

#### Brute Force
- Iterate through the array to find the minimum element.
- **TC:** O(n) | **SC:** O(1)

```cpp
int findMin(vector<int>& nums) {
    int minVal = nums[0];
    for(int i=1;i<nums.size();i++){
        if(nums[i] < minVal) minVal = nums[i];
    }
    return minVal;
}
```

#### Optimized Approach (Binary Search)
- Use two pointers to find the minimum in O(log n) time.
- Compare mid element with the rightmost element to decide which half to search.
- **TC:** O(log n) | **SC:** O(1)

```cpp
int findMin(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    while(left < right){
        int mid = left + (right - left)/2;
        if(nums[mid] > nums[right]) left = mid + 1;
        else right = mid;
    }
    return nums[left];
}
```
### 6.Search in Rotated Sorted Array

#### Description
Given a rotated sorted array `nums` and a target value `target`, return the index of `target` if it exists in the array; otherwise, return `-1`. The array was originally sorted in ascending order and then rotated.

#### Example
Input: `nums = [4,5,6,7,0,1,2], target = 0`  
Output: `4`

Input: `nums = [4,5,6,7,0,1,2], target = 3`  
Output: `-1`

Input: `nums = [1], target = 0`  
Output: `-1`

#### Hint
Use modified binary search: check which side is sorted and decide which half to continue searching.

#### Brute Force
- Iterate through the array to find the target.
- **TC:** O(n) | **SC:** O(1)

```cpp
int search(vector<int>& nums, int target) {
    for(int i=0;i<nums.size();i++){
        if(nums[i]==target) return i;
    }
    return -1;
}
```

#### Optimized Approach (Binary Search)
- Use binary search while checking which half is sorted.
- If target lies in the sorted half, adjust pointers accordingly.
- **TC:** O(log n) | **SC:** O(1)

```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] == target) return mid;
        
        if(nums[left] <= nums[mid]){ // left half sorted
            if(nums[left] <= target && target < nums[mid]) right = mid - 1;
            else left = mid + 1;
        } else { // right half sorted
            if(nums[mid] < target && target <= nums[right]) left = mid + 1;
            else right = mid - 1;
        }
    }
    return -1;
}
```
### 7.Largest Number

#### Description
Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it as a string.

#### Example
Input: `nums = [10,2]`  
Output: `"210"`

Input: `nums = [3,30,34,5,9]`  
Output: `"9534330"`

#### Hint
Sort numbers as strings, using a custom comparator: `a+b > b+a`.

#### Brute Force
- Try all permutations and pick the largest.
- **TC:** O(n!) | **SC:** O(n)

```cpp
string largestNumber(vector<int>& nums) {
    vector<string> arr;
    for(int num : nums) arr.push_back(to_string(num));
    sort(arr.begin(), arr.end(), [](string &a, string &b){ return a+b > b+a; });
    string res = "";
    for(string s : arr) res += s;
    if(res[0]=='0') return "0";
    return res;
}
```

#### Optimized Approach
- Custom sort strings as above is already optimal.
- **TC:** O(n log n * k) | **SC:** O(n) (k = max digits in number)


### 8.Sort List

#### Description
Given the head of a linked list, sort the list in ascending order and return the sorted list. You must sort it in O(n log n) time and use constant space for the list nodes.

#### Example
Input: `head = [4,2,1,3]`  
Output: `[1,2,3,4]`

Input: `head = [-1,5,3,4,0]`  
Output: `[-1,0,3,4,5]`

#### Hint
Use merge sort for linked lists because it provides O(n log n) time complexity with constant extra space.

#### Brute Force
- Copy nodes to an array, sort the array, and rebuild the linked list.
- **TC:** O(n log n) | **SC:** O(n)

```cpp
ListNode* sortList(ListNode* head) {
    vector<int> vals;
    ListNode* curr = head;
    while(curr){
        vals.push_back(curr->val);
        curr = curr->next;
    }
    sort(vals.begin(), vals.end());
    curr = head;
    for(int v : vals){
        curr->val = v;
        curr = curr->next;
    }
    return head;
}
```

#### Optimized Approach (Merge Sort on Linked List)
- Recursively split the list into halves, sort, and merge.
- **TC:** O(n log n) | **SC:** O(log n) due to recursion stack

```cpp
ListNode* merge(ListNode* l1, ListNode* l2){
    ListNode dummy(0), *tail = &dummy;
    while(l1 && l2){
        if(l1->val < l2->val){ tail->next = l1; l1 = l1->next; }
        else { tail->next = l2; l2 = l2->next; }
        tail = tail->next;
    }
    tail->next = l1 ? l1 : l2;
    return dummy.next;
}

ListNode* sortList(ListNode* head) {
    if(!head || !head->next) return head;
    ListNode *slow = head, *fast = head, *prev = nullptr;
    while(fast && fast->next){
        prev = slow;
        slow = slow->next;
        fast = fast->next->next;
    }
    prev->next = nullptr;
    ListNode* l1 = sortList(head);
    ListNode* l2 = sortList(slow);
    return merge(l1,l2);
}
```
### 9.Koko Eating Bananas

#### Description
Koko loves to eat bananas. There are `n` piles of bananas, the `i`-th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours. Koko can decide her eating speed `k` (bananas per hour). Each hour, she chooses a pile and eats `k` bananas from it. If the pile has less than `k` bananas, she eats all of them and will not eat any more bananas during that hour. Find the minimum integer `k` such that she can eat all bananas within `h` hours.

#### Example
Input: `piles = [3,6,7,11], h = 8`  
Output: `4`

Input: `piles = [30,11,23,4,20], h = 5`  
Output: `30`

#### Hint
Use binary search on the possible eating speeds. Check if a given speed allows finishing within `h` hours.

#### Brute Force
- Try all possible speeds from 1 to max(piles). Check if Koko can finish.
- **TC:** O(max(piles) * n) | **SC:** O(1)

```cpp
bool canFinish(vector<int>& piles, int k, int h){
    int hours = 0;
    for(int pile : piles){
        hours += (pile + k - 1)/k;
    }
    return hours <= h;
}

int minEatingSpeed(vector<int>& piles, int h){
    int left = 1, right = *max_element(piles.begin(), piles.end());
    while(left < right){
        int mid = left + (right-left)/2;
        if(canFinish(piles, mid, h)) right = mid;
        else left = mid + 1;
    }
    return left;
}
```

#### Optimized Approach
- Binary search on speed is already optimal.
- **TC:** O(n log(max(piles))) | **SC:** O(1)

### 10.Time Based Key Value Store

#### Description
Design a time-based key-value data structure that can store multiple values for the same key at different timestamps and retrieve the value of a key at a given timestamp.

#### Example
```cpp
TimeMap kv;
kv.set("foo", "bar", 1);
kv.get("foo", 1); // returns "bar"
kv.get("foo", 3); // returns "bar" since latest <= 3
kv.set("foo", "bar2", 4);
kv.get("foo", 4); // returns "bar2"
kv.get("foo", 5); // returns "bar2"
```

#### Hint
Use a hashmap with key mapping to a vector of (timestamp, value) pairs. Use binary search to retrieve the latest value at a timestamp.

#### Brute Force
- Iterate through the vector of timestamps to find the latest one <= query timestamp.
- **TC:** O(n) per `get` | **SC:** O(n) per key

```cpp
class TimeMap {
    unordered_map<string, vector<pair<int,string>>> store;
public:
    TimeMap() {}
    
    void set(string key, string value, int timestamp){
        store[key].push_back({timestamp, value});
    }
    
    string get(string key, int timestamp){
        if(store.find(key) == store.end()) return "";
        auto &vec = store[key];
        string res = "";
        for(auto &p : vec){
            if(p.first <= timestamp) res = p.second;
            else break;
        }
        return res;
    }
};
```

#### Optimized Approach
- Use binary search on timestamps to find the latest value <= timestamp.
- **TC:** O(log n) per `get` | **SC:** O(n) per key

```cpp
string get(string key, int timestamp){
    if(store.find(key) == store.end()) return "";
    auto &vec = store[key];
    int left = 0, right = vec.size()-1;
    string res = "";
    while(left <= right){
        int mid = left + (right-left)/2;
        if(vec[mid].first <= timestamp){
            res = vec[mid].second;
            left = mid + 1;
        } else right = mid - 1;
    }
    return res;
}
```
### 11.Median of Two Sorted Arrays

#### Description
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

#### Example
Input: `nums1 = [1,3], nums2 = [2]`  
Output: `2.0`

Input: `nums1 = [1,2], nums2 = [3,4]`  
Output: `2.5`

#### Hint
Use binary search on the smaller array to partition both arrays such that the left partition contains half of the elements.

#### Brute Force
- Merge both arrays and find median.
- **TC:** O(m+n) | **SC:** O(m+n)

```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    vector<int> merged;
    int i=0, j=0;
    while(i<nums1.size() && j<nums2.size()){
        if(nums1[i] < nums2[j]) merged.push_back(nums1[i++]);
        else merged.push_back(nums2[j++]);
    }
    while(i<nums1.size()) merged.push_back(nums1[i++]);
    while(j<nums2.size()) merged.push_back(nums2[j++]);
    int n = merged.size();
    if(n % 2 == 1) return merged[n/2];
    return (merged[n/2-1] + merged[n/2]) / 2.0;
}
```

#### Optimized Approach (Binary Search)
- Perform binary search on the smaller array to find the correct partition.
- **TC:** O(log min(m,n)) | **SC:** O(1)

```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    if(nums1.size() > nums2.size()) return findMedianSortedArrays(nums2, nums1);
    int m = nums1.size(), n = nums2.size();
    int left = 0, right = m;
    while(left <= right){
        int i = left + (right-left)/2;
        int j = (m+n+1)/2 - i;
        int maxLeftX = (i==0) ? INT_MIN : nums1[i-1];
        int minRightX = (i==m) ? INT_MAX : nums1[i];
        int maxLeftY = (j==0) ? INT_MIN : nums2[j-1];
        int minRightY = (j==n) ? INT_MAX : nums2[j];
        
        if(maxLeftX <= minRightY && maxLeftY <= minRightX){
            if((m+n)%2==0) return (max(maxLeftX,maxLeftY)+min(minRightX,minRightY))/2.0;
            return max(maxLeftX,maxLeftY);
        } else if(maxLeftX > minRightY) right = i-1;
        else left = i+1;
    }
    return 0.0;
}
```
