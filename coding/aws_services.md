AWS provides many services for outlier detection. A prototype small service to detect an outlier in an array of integers is under development.
Given an array of n integers, there are (n-2) normal numbers and the sum of the (n-2) numbers originally in this array. If a number is neither in the original numbers nor is it their sum, it is an outlier. Discover the potential outliers and return the greatest of them.
Note: It is guaranteed that the answer exists.
Example
arr = [4, 1,3,16, 2, 10]
There are two potential outliers:
Remove 16 - arr = [4,1,3,2, 10] the sum of [4, 1,3, 2] is 10, so 16 is a potential outlier, (it is not an ongmal number or their sum)

Remove 4 - arr = [1,3,16, 2, 10]. The sum of [1,3, 2, 10] is 16. 50-4 s a potential outher
The outlier is the greater of the two potential outliers, so 16 is the outlier

Function Description
Complete the function getOutlierValue in the editor below 
gerOutliervalue has the following parameterts:
int arr[n]: value of (n-2) numbers, their sum, and outlier value

Returns
int: the outlier value

Constraints
3 <= n <= 10^5
1 <= arr[i] <= 10^9

► Input Format For Custom Testing
▼ Sample Case 0
Sample Input For Custom Testing
6
4
1
2
1
10
3
Sample Output
1
Explanation
The original list is [1,2,3,4] with a sum of 10.

▼ Sample Case 1
Sample Input For Custom Testing
4
2 
2
4
2
Sample Output
2
Explanation
The original list is [2, 2] with a sum of 4

---
solution 1 : O(N^2)

```c++
#include <vector>
#include <algorithm>
#include <numeric>

class Solution {
public:
    long long getOutlierValue(std::vector<int>& arr) {
        int n = arr.size();
        long long maxOutlier = 0;
        
        // Try removing each number and check if remaining numbers satisfy the condition
        for (int i = 0; i < n; i++) {
            // Create a vector without current element
            std::vector<int> remaining;
            for (int j = 0; j < n; j++) {
                if (j != i) {
                    remaining.push_back(arr[j]);
                }
            }
            
            // Try each remaining number as potential sum
            for (int j = 0; j < remaining.size(); j++) {
                int potentialSum = remaining[j];
                std::vector<int> withoutSum;
                
                // Create vector without the potential sum
                for (int k = 0; k < remaining.size(); k++) {
                    if (k != j) {
                        withoutSum.push_back(remaining[k]);
                    }
                }
                
                // Calculate actual sum of remaining numbers
                long long actualSum = 0;
                for (int num : withoutSum) {
                    actualSum += num;
                }
                
                // If actual sum matches potential sum, we found a valid combination
                if (actualSum == potentialSum) {
                    maxOutlier = std::max(maxOutlier, (long long)arr[i]);
                }
            }
        }
        
        return maxOutlier;
    }
};
```
---
claude solution (O(N2)) [add long long wherever necessary]

```c++
#include <vector>
#include <algorithm>
#include <iostream>
#include <bits/stdc++.h>


int getOutlierValue(std::vector<int>& arr) {
    int n = arr.size();
    int maxOutlier = 0;
    
    // Try removing each number as potential outlier
    for (int i = 0; i < n; i++) {
        long long totalSum = 0;
        std::vector<long long> remaining;
        
        // Create array without current element
        for (int j = 0; j < n; j++) {
            if (j != i) {
                totalSum += arr[j];
                remaining.push_back(arr[j]);
            }
        }
        
        // Try each remaining number as potential sum
        for (int j = 0; j < remaining.size(); j++) {
            long long sumWithoutCurrent = totalSum - remaining[j];
            // If current number equals sum of others (excluding it)
            if (remaining[j] == sumWithoutCurrent) {
                maxOutlier = std::max(maxOutlier, arr[i]);
                break;
            }
        }
    }
    
    return maxOutlier;
}


int main() {
    // Sample test case 0
    std::vector<int> arr1 = {4, 1, 2, 1, 10, 3};
    std::cout << "Test case 1 result: " << getOutlierValue(arr1) << std::endl;  // Expected: 1
    
    // Sample test case 1
    std::vector<int> arr2 = {2, 2, 4, 2};
    std::cout << "Test case 2 result: " << getOutlierValue(arr2) << std::endl;  // Expected: 2
    
    return 0;
}

```
