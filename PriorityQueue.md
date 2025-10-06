### 1.Priority Queue

#### Description
A priority queue is an abstract data type where each element has a "priority". Elements with higher priority are dequeued before elements with lower priority. If priorities are equal, the order can be arbitrary or follow FIFO.

#### Example
```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    // Max-heap priority queue (default)
    priority_queue<int> pq;
    pq.push(3);
    pq.push(1);
    pq.push(5);
    cout << pq.top() << endl; // 5
    pq.pop();
    cout << pq.top() << endl; // 3

    // Min-heap priority queue
    priority_queue<int, vector<int>, greater<int>> minPq;
    minPq.push(3);
    minPq.push(1);
    minPq.push(5);
    cout << minPq.top() << endl; // 1
    minPq.pop();
    cout << minPq.top() << endl; // 3

    return 0;
}
```

#### Operations
- `push(x)`: Insert element `x` into the priority queue.
- `pop()`: Remove the element with the highest priority.
- `top()`: Return the element with the highest priority.
- `empty()`: Check if the queue is empty.

#### Complexity
- **Time:** O(log n) for push and pop, O(1) for top
- **Space:** O(n)

### 2.Last Stone Weight

#### Description
You are given a list of stones with positive integer weights. On each turn, choose the two heaviest stones and smash them together. If they are equal, both are destroyed; if unequal, the difference remains. Return the weight of the last remaining stone or 0 if none remain.

#### Example
Input: `stones = [2,7,4,1,8,1]`  
Output: `1`

Input: `stones = [1]`  
Output: `1`

#### Hint
Use a max-heap (priority queue) to always access the two heaviest stones efficiently.

#### Brute Force
- Sort the array in each iteration and remove the last two elements. Repeat until one or no stones remain.
- **TC:** O(n^2 log n) due to repeated sorting | **SC:** O(1)

#### Optimized Approach (Priority Queue / Max-Heap)
- Push all stones into a max-heap.
- While the heap has more than one stone:
  - Pop the two largest stones.
  - If they are unequal, push the difference back.
- Return 0 if the heap is empty, else return the remaining stone.

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int lastStoneWeight(vector<int>& stones) {
    priority_queue<int> pq(stones.begin(), stones.end());
    while(pq.size() > 1) {
        int first = pq.top(); pq.pop();
        int second = pq.top(); pq.pop();
        if(first != second) pq.push(first - second);
    }
    return pq.empty() ? 0 : pq.top();
}

int main() {
    vector<int> stones = {2,7,4,1,8,1};
    cout << lastStoneWeight(stones) << endl; // 1
    return 0;
}
```

#### Complexity
- **Time:** O(n log n) because each push/pop operation is O(log n)
- **Space:** O(n) for the heap

### 3.Kth Largest Element in an Array

#### Description
Find the k-th largest element in an unsorted array. Note that it is the k-th largest element in sorted order, not the k-th distinct element.

#### Example
Input: `nums = [3,2,1,5,6,4], k = 2`  
Output: `5`

Input: `nums = [3,2,3,1,2,4,5,5,6], k = 4`  
Output: `4`

#### Hint
Use a min-heap of size k to maintain the k largest elements or use the QuickSelect algorithm for average O(n) time.

#### Brute Force
- Sort the array in descending order and return the element at index `k-1`.
- **TC:** O(n log n) | **SC:** O(1)

#### Optimized Approach (Min-Heap)
- Maintain a min-heap of size k.
- Push elements into the heap. If the heap exceeds size k, remove the smallest.
- After processing all elements, the top of the heap is the k-th largest.
- **TC:** O(n log k) | **SC:** O(k)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    for(int num : nums) {
        minHeap.push(num);
        if(minHeap.size() > k) minHeap.pop();
    }
    return minHeap.top();
}

int main() {
    vector<int> nums = {3,2,1,5,6,4};
    cout << findKthLargest(nums, 2) << endl; // 5
    return 0;
}
```

#### Optimized Approach (QuickSelect)
- Partition the array around a pivot such that elements greater than pivot are on one side.
- Recurse on the side containing the k-th largest element.
- **Average TC:** O(n) | **Worst TC:** O(n^2) | **SC:** O(1)

### 4.K Closest Points to Origin

#### Description
Given a list of points in a 2D plane, find the k points closest to the origin (0,0) using Euclidean distance.

#### Example
Input: `points = [[1,3],[-2,2]], k = 1`  
Output: `[[-2,2]]`

Input: `points = [[3,3],[5,-1],[-2,4]], k = 2`  
Output: `[[3,3],[-2,4]]`

#### Hint
Use a max-heap (priority queue) to maintain the k closest points. Push points with their squared distance from origin; remove the farthest if heap size exceeds k.

#### Brute Force
- Compute distance for all points, sort them, and return the first k.
- **TC:** O(n log n) | **SC:** O(n)

#### Optimized Approach (Max-Heap)
- Use a max-heap of size k.
- Push points with their distance into the heap.
- If heap exceeds size k, pop the farthest.
- After processing all points, heap contains k closest points.
- **TC:** O(n log k) | **SC:** O(k)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>
using namespace std;

vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
    auto cmp = [](vector<int>& a, vector<int>& b){
        return (a[0]*a[0] + a[1]*a[1]) < (b[0]*b[0] + b[1]*b[1]);
    };
    priority_queue<vector<int>, vector<vector<int>>, decltype(cmp)> pq(cmp);
    for(auto& p : points){
        pq.push(p);
        if(pq.size() > k) pq.pop();
    }
    vector<vector<int>> res;
    while(!pq.empty()){ res.push_back(pq.top()); pq.pop(); }
    return res;
}

int main() {
    vector<vector<int>> points = {{1,3},{-2,2}};
    int k = 1;
    vector<vector<int>> res = kClosest(points, k);
    for(auto &p : res) cout << '[' << p[0] << ',' << p[1] << ']' << endl;
    return 0;
}
```

#### Optimized Approach (QuickSelect)
- Partition points around a pivot distance from origin.
- Recurse on the side containing k closest points.
- **Average TC:** O(n) | **Worst TC:** O(n^2) | **SC:** O(1)
### 5.High Five

#### Description
Given a list of scores of different students, return the average of each student's top five scores. Each item is represented as `[studentId, score]`.

#### Example
Input: `items = [[1,91],[1,92],[2,93],[2,97],[1,60],[2,77],[1,65],[1,87],[1,100],[2,100],[2,76]]`  
Output: `[[1,87],[2,88]]`

#### Hint
Use a map to store scores for each student. For each student, maintain a min-heap of size 5 to track the top five scores.

#### Brute Force
- Collect all scores for each student, sort, and take the top five to compute the average.
- **TC:** O(n log n) | **SC:** O(n)

#### Optimized Approach (Min-Heap)
- Use a min-heap of size 5 for each student.
- Push scores into the heap; if heap exceeds size 5, remove the smallest.
- Compute the average of remaining 5 scores.
- **TC:** O(n log 5) => O(n) | **SC:** O(n)

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

vector<vector<int>> highFive(vector<vector<int>>& items) {
    unordered_map<int, priority_queue<int, vector<int>, greater<int>>> mp;
    for(auto &it : items) {
        int id = it[0], score = it[1];
        mp[id].push(score);
        if(mp[id].size() > 5) mp[id].pop();
    }
    vector<vector<int>> res;
    for(auto &p : mp) {
        int sum = 0;
        auto pq = p.second;
        while(!pq.empty()){ sum += pq.top(); pq.pop(); }
        res.push_back({p.first, sum/5});
    }
    sort(res.begin(), res.end());
    return res;
}

int main() {
    vector<vector<int>> items = {{1,91},{1,92},{2,93},{2,97},{1,60},{2,77},{1,65},{1,87},{1,100},{2,100},{2,76}};
    vector<vector<int>> res = highFive(items);
    for(auto &r : res) cout << '[' << r[0] << ',' << r[1] << ']' << endl;
    return 0;
}
```

### 6.Kth Largest Element in a Stream

#### Description
Design a class that finds the k-th largest element in a stream of integers. The class should support adding new numbers and querying the k-th largest number at any point.

#### Example
```cpp
KthLargest kthLargest(3, [4,5,8,2]);
kthLargest.add(3);  // returns 4
kthLargest.add(5);  // returns 5
kthLargest.add(10); // returns 5
kthLargest.add(9);  // returns 8
kthLargest.add(4);  // returns 8
```

#### Hint
Use a min-heap of size k to maintain the k largest elements seen so far. The top of the heap is the k-th largest.

#### Brute Force
- Keep all numbers in a list, sort each time `add` is called, and return the k-th largest.
- **TC:** O(n log n) per add | **SC:** O(n)

#### Optimized Approach (Min-Heap)
- Maintain a min-heap of size k.
- For each `add(val)`, push `val` into the heap. If heap size exceeds k, pop the smallest.
- Return the top of the heap as the k-th largest.
- **TC:** O(log k) per add | **SC:** O(k)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

class KthLargest {
    priority_queue<int, vector<int>, greater<int>> pq;
    int k;
public:
    KthLargest(int k_, vector<int>& nums) {
        k = k_;
        for(int num : nums) {
            pq.push(num);
            if(pq.size() > k) pq.pop();
        }
    }

    int add(int val) {
        pq.push(val);
        if(pq.size() > k) pq.pop();
        return pq.top();
    }
};

int main() {
    vector<int> nums = {4,5,8,2};
    KthLargest kthLargest(3, nums);
    cout << kthLargest.add(3) << endl;  // 4
    cout << kthLargest.add(5) << endl;  // 5
    cout << kthLargest.add(10) << endl; // 5
    cout << kthLargest.add(9) << endl;  // 8
    cout << kthLargest.add(4) << endl;  // 8
    return 0;
}
```

### 7.Task Scheduler

#### Description
Given a list of tasks represented by characters and a non-negative integer `n` representing the cooldown period between the same task, determine the minimum time required to execute all tasks. You can execute tasks in any order, but the same task must have at least `n` intervals between them.

#### Example
Input: `tasks = ['A','A','A','B','B','B'], n = 2`  
Output: `8`  
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B

#### Hint
Count the frequency of each task. The minimum time is determined by the task with the highest frequency. Arrange tasks by max frequency first, filling cooldowns with other tasks or idle slots.

#### Brute Force
- Simulate the task execution step by step, keeping track of cooldowns.
- **TC:** O(n * len(tasks)) | **SC:** O(26) for task count

#### Optimized Approach
- Count the frequency of each task.
- Let `maxFreq` be the frequency of the most frequent task.
- Let `maxCount` be the number of tasks with frequency `maxFreq`.
- Minimum intervals = `(maxFreq - 1) * (n + 1) + maxCount`
- Take the maximum of this value and total number of tasks.
- **TC:** O(n) | **SC:** O(26)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int leastInterval(vector<char>& tasks, int n) {
    vector<int> freq(26, 0);
    for(char t : tasks) freq[t - 'A']++;
    int maxFreq = *max_element(freq.begin(), freq.end());
    int maxCount = count(freq.begin(), freq.end(), maxFreq);
    int minIntervals = (maxFreq - 1) * (n + 1) + maxCount;
    return max((int)tasks.size(), minIntervals);
}

int main() {
    vector<char> tasks = {'A','A','A','B','B','B'};
    int n = 2;
    cout << leastInterval(tasks, n) << endl; // 8
    return 0;
}
```

### 8.Employee Free Time

#### Description
Given a list of employees' schedules where each schedule is a list of non-overlapping intervals `[start, end]` sorted by start time, return the list of intervals representing the common free time across all employees.

#### Example
Input: `schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]`  
Output: `[[3,4]]`

Input: `schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]`  
Output: `[[5,6],[7,9]]`

#### Hint
Merge all employees' intervals into a single list and sort by start time. Then, traverse the merged list to find gaps between consecutive intervals.

#### Brute Force
- Compare each employee's schedule pairwise to find common free slots.
- **TC:** O(n^2) | **SC:** O(n)

#### Optimized Approach
- Flatten all intervals into one list and sort by start time.
- Merge overlapping intervals to get the combined busy times.
- The gaps between consecutive merged intervals are the free times.
- **TC:** O(n log n) | **SC:** O(n)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> employeeFreeTime(vector<vector<vector<int>>>& schedule) {
    vector<pair<int,int>> intervals;
    for(auto& emp : schedule)
        for(auto& itv : emp) intervals.push_back({itv[0], itv[1]});
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> res;
    int end = intervals[0].second;
    for(int i=1;i<intervals.size();i++) {
        if(intervals[i].first > end) res.push_back({end, intervals[i].first});
        end = max(end, intervals[i].second);
    }
    return res;
}

int main() {
    vector<vector<vector<int>>> schedule = {{{1,2},{5,6}},{{1,3}},{{4,10}}};
    vector<vector<int>> res = employeeFreeTime(schedule);
    for(auto &v : res) cout << '[' << v[0] << ',' << v[1] << ']' << endl;
    return 0;
}
```
### 9.Find Median From Data Stream

#### Description
Design a data structure that supports adding integers from a data stream and finding the median of all elements so far.

#### Example
```cpp
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3)
findMedian() -> 2
```

#### Hint
Use two heaps:
- Max-heap for the smaller half of numbers.
- Min-heap for the larger half of numbers.
- Balance the heaps so their sizes differ by at most 1. Median is top of max-heap (odd) or average of tops (even).

#### Brute Force
- Store all numbers in a list and sort each time `findMedian` is called.
- **TC:** O(n log n) per find | **SC:** O(n)

#### Optimized Approach (Two Heaps)
- Max-heap `low` stores smaller half.
- Min-heap `high` stores larger half.
- Maintain size property: `low.size() >= high.size()`.
- Median: if sizes equal, average tops; else, top of `low`.
- **TC:** O(log n) per add | O(1) per find | **SC:** O(n)

```cpp
#include <iostream>
#include <queue>
using namespace std;

class MedianFinder {
    priority_queue<int> low; // max-heap
    priority_queue<int, vector<int>, greater<int>> high; // min-heap
public:
    void addNum(int num) {
        low.push(num);
        high.push(low.top()); low.pop();
        if(low.size() < high.size()) { low.push(high.top()); high.pop(); }
    }

    double findMedian() {
        if(low.size() == high.size()) return (low.top() + high.top()) / 2.0;
        return low.top();
    }
};

int main() {
    MedianFinder mf;
    mf.addNum(1);
    mf.addNum(2);
    cout << mf.findMedian() << endl; // 1.5
    mf.addNum(3);
    cout << mf.findMedian() << endl; // 2
    return 0;
}
```
### 10.Design Twitter

#### Description
Design a simplified Twitter system with the following features:
- Users can post tweets.
- Users can follow/unfollow other users.
- Retrieve the 10 most recent tweets in the user's news feed, which should include tweets from the user and people they follow.

#### Example
```cpp
Twitter twitter;
twitter.postTweet(1, 5);
twitter.getNewsFeed(1); // returns [5]
twitter.follow(1, 2);
twitter.postTweet(2, 6);
twitter.getNewsFeed(1); // returns [6, 5]
twitter.unfollow(1, 2);
twitter.getNewsFeed(1); // returns [5]
```

#### Hint
- Maintain a mapping from user to their tweets (with timestamps).
- Maintain a mapping from user to the set of followed users.
- Use a max-heap or priority queue to merge tweets for the news feed efficiently.

#### Brute Force
- Scan all tweets in the system and filter by follow relationships each time `getNewsFeed` is called.
- **TC:** O(total tweets) per query | **SC:** O(total tweets)

#### Optimized Approach (Heap + Maps)
- Map: `userId -> set of followees`.
- Map: `userId -> list of tweets with timestamps`.
- For `getNewsFeed`, use a max-heap to merge tweets from the user and followees, picking the top 10 most recent.
- **TC:** O(N log k) where N = total tweets of user + followees, k = 10 | **SC:** O(N)

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>
using namespace std;

struct Tweet {
    int id, time;
    Tweet(int tid, int t) : id(tid), time(t) {}
};

class Twitter {
    unordered_map<int, unordered_set<int>> followees;
    unordered_map<int, vector<Tweet>> tweets;
    int timestamp = 0;
public:
    void postTweet(int userId, int tweetId) {
        tweets[userId].emplace_back(tweetId, timestamp++);
    }

    vector<int> getNewsFeed(int userId) {
        auto cmp = [](Tweet &a, Tweet &b){ return a.time < b.time; };
        priority_queue<Tweet, vector<Tweet>, decltype(cmp)> pq(cmp);
        for(auto &t : tweets[userId]) pq.push(t);
        for(int f : followees[userId])
            for(auto &t : tweets[f]) pq.push(t);
        vector<int> res;
        int count = 0;
        while(!pq.empty() && count < 10){ res.push_back(pq.top().id); pq.pop(); count++; }
        return res;
    }

    void follow(int followerId, int followeeId) {
        if(followerId != followeeId) followees[followerId].insert(followeeId);
    }

    void unfollow(int followerId, int followeeId) {
        followees[followerId].erase(followeeId);
    }
};
```
