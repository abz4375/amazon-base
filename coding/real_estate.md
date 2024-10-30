Code Question 1
Within the Amazon Real Estate Analytics Toolset, an investor is evaluating a series of buildings of varying heights. The investor aims to find a group of buildings that adhere to two key conditions:
1. The selected buildings must form a valid group.
A group of buildings is considered valid if it meets the following criteria.
• It is a contiguous subsegment of the original array with a size of at least 2.
The heights of the first and last buildings in this subsegment are the same.
2. The group variance should be as minimal as possible.
The investor's goal is to find such a valid group of buildings with the minimal possible variance in height among the buildings in the group.
The variance of a group is the difference between the size of the group and the occurrences of the first element height in the group, for example, the variance of the group [4, 2, 5, 4] is 4-2 = 2.
Formally, given an array height of size n, the height of each building, find the minimum possible variance of a valid group of buildings, it is guaranteed that there is at least one valid group.
An array a is a subsegment of an array b if a can be obtained from b by deletion of several (possibly, zero or all) elements from the beginning and several (possibly, zero or all) elements from the end. for example, [3, 1], and [4] is a subsegment of array [1, 3, 1, 4] while [1, 1], and [1, 4] is not.
Example
n=6
height[3, 6, 4, 6, 3, 3]

Let's try different groups with different variances:

| Group           | Validity                                      | Variance    |
|-----------------|----------------------------------------------|-------------|
| [3, 6, 4, 6, 3] | Valid (First and Last heights are equal)     | 5 - 2 = 3   |
| [6, 4, 6, 3]    | Invalid (First and Last heights are not equal)| 4 - 2 = 2   |
| [3, 6, 4, 6, 3, 3] | Valid (First and Last heights are equal) | 6 - 3 = 3   |
| [6, 4, 6]       | Valid (First and Last heights are equal)     | 3 - 2 = 1   |

The first, third, and fourth groups are valid with variances 3, 3, and 1 respectively, since the fourth group has the minimum variance the answer is 1.

Function Description
Complete the function findMinimumVariance in the editor below.
findMinimumVariance has the following parameter: 
int height[n]: the height of each building.
Returns
int: the variance of a valid group of buildings with minimum variance.
Constraints
2<=n<=2*10^5
1<=height[i]<=10^9
▼Input Format for Custom Testing
The first line contains an integer n, the size of the array height. Each of the next n lines contains an integer height[i].

Sample Case 0
Sample Input 0

4 
2 
2
2
2

Sample Output 0
0
Explanation
All groups of buildings have a variance of 0, for example, group [2, 2, 2] of the first three elements has a variance of 3-3=0.

▼Sample Case 1
Sample Input 1

5
1
2
3
4
1
Sample Output 1
3
Explanation
There are only one viäid group which is [1, 2, 3, 4, 1] with a variance of 5-2 = 3.

---

solution 1 : 14/15 testcases passing

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <climits>
using namespace std;

int findMinVariance(const vector<int>& height) {
    int n = height.size();
    unordered_map<int, int> firstIndex, frequency;
    int minVariance = INT_MAX;
    
    // Traverse the array to find the first index and frequency of each element
    for (int i = 0; i < n; ++i) {
        // Record the first occurrence of the element if not already recorded
        if (firstIndex.find(height[i]) == firstIndex.end()) {
            firstIndex[height[i]] = i;
        }
        // Increase the frequency of the element
        frequency[height[i]]++;
        
        // Calculate the variance for subarray ending at the current index
        int length = i - firstIndex[height[i]] + 1;
        if (length >= 2) {  // Subarray must have at least 2 elements
            int occurrences = frequency[height[i]];
            int variance = length - occurrences;
            minVariance = min(minVariance, variance);
        }
    }
    
    return minVariance;
}

int main() {
    int n;
    cin >> n;
    vector<int> height(n);
    
    for (int i = 0; i < n; ++i) {
        cin >> height[i];
    }
    
    int result = findMinVariance(height);
    cout << result << endl;
    
    return 0;
}
```
---
solution 2 : optimised by gpt
```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <climits>
using namespace std;

int findMinVariance(const vector<int>& height) {
    int n = height.size();
    unordered_map<int, vector<int>> positions;
    int minVariance = INT_MAX;

    // Store all positions of each height in the array
    for (int i = 0; i < n; ++i) {
        positions[height[i]].push_back(i);
    }

    // Iterate over each unique height that could be the start and end of a group
    for (const auto& entry : positions) {
        const vector<int>& pos = entry.second;
        
        // Check each pair of indices in this height's list to form valid subarrays
        for (int i = 0; i < pos.size(); i++) {
            int count = 0;
            for (int j = i; j < pos.size(); j++) {
                int start = pos[i];
                int end = pos[j];
                int groupSize = end - start + 1;

                // Count occurrences of this height in the current group
                count++;
                
                // Calculate variance and update minVariance if it is smaller
                if (groupSize >= 2) {
                    int variance = groupSize - count;
                    minVariance = min(minVariance, variance);
                }
            }
        }
    }

    return minVariance;
}

int main() {
    int n;
    cin >> n;
    vector<int> height(n);
    
    for (int i = 0; i < n; ++i) {
        cin >> height[i];
    }
    
    int result = findMinVariance(height);
    cout << result << endl;
    
    return 0;
}

```
---
solution 3 : O(N2)

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <limits.h>

int findMinimumVariance(std::vector<int>& height) {
    int n = height.size();
    int minVariance = INT_MAX;

    std::unordered_map<int, std::vector<int>> positions;
    
    // Store positions of each height in the array
    for (int i = 0; i < n; i++) {
        positions[height[i]].push_back(i);
    }

    // Iterate over each unique height that could be the start and end of a group
    for (const auto& entry : positions) {
        const std::vector<int>& pos = entry.second;
        
        // Check each pair of indices in this height list to form valid subarrays
        for (int i = 0; i < pos.size(); i++) {
            for (int j = i + 1; j < pos.size(); j++) {
                int start = pos[i];
                int end = pos[j];
                int groupSize = end - start + 1;

                // Count occurrences of this height in the current group
                int count = 0;
                for (int k = start; k <= end; k++) {
                    if (height[k] == entry.first) {
                        count++;
                    }
                }

                // Calculate variance and update minVariance if it is smaller
                int variance = groupSize - count;
                minVariance = std::min(minVariance, variance);
            }
        }
    }

    return minVariance;
}

int main() {
    int n;
    std::cin >> n;
    std::vector<int> height(n);
    for (int i = 0; i < n; i++) {
        std::cin >> height[i];
    }

    int result = findMinimumVariance(height);
    std::cout << result << std::endl;

    return 0;
}

```

---
reddit code: CORRECT CODE

```c++
#include <bits/stdc++.h>

using namespace std;

int findMinimumVariance(vector<int> height) {

unordered_map<int,int>mp;

unordered_map<int,int>freq;

int res=INT_MAX;

for(int i=0;i<height.size();i++)

{

//else mp[height[i]]=i;

freq[height[i]]++;

if(mp.find(height[i])!=mp.end() and i-mp[height[i]]+1>2)

{

res=min(res,(i-mp[height[i]]+1)-freq[height[i]]);

	 }
mp[height[i]]=i;

 }

 return res;   
}

int main()

{

cout<<findMinimumVariance({3, 6, 4, 6, 3, 3});
}
```