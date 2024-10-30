At the Amazon Fulfillment Center, a software engineer is tasked with optimizing the performance of a large inventory system consisting of several complex components.
There are n components, and the performance level of each component is represented by performance[0], performance[1],... performance[n-1].
Let's define a partition (l,r) as the group of components indexed from l to r. To identify and resolve potential bottlenecks in the system, the engineer needs to compute the bitwise AND operation for all components within a partition. The bottleneck level of a partition (l, r) is expressed as:
f(l,r) = performance[l] & performance[l+1] &... & performance[r]
Here, the & operator represents the bitwise AND operation.
The goal of the engineers is to divide the components into contiguous partitions, ensuring each
component belongs to exactly one partition. The objective is to minimize the total bottleneck level of the partitions while maximizing the number of partitions.
Given an integer array performance of size n, determine the maximum number of partitions that can be achieved while minimizing the total bottleneck levels of the partitions.
Example
n=6
performance [5, 2, 6, 1, 1, 4]

| Index Range (l, r) for the partitions | Bottleneck Level |
|---------------------------------------|------------------|
| (0, 1)                                | (performance[0] & performance[1]) = (5 & 2) = 0 |
| (2, 3)                                | (performance[2] & performance[3]) = (6 & 1) = 0 |
| (4, 5)                                | (performance[4] & performance[5]) = (1 & 4) = 0 |

The sum of bottleneck levels is 0+0+0=0, which is the minimum possible. Therefore, the maximum number of partitions with the minimum total bottleneck level is 3.

Function Description
Complete the function getMaximumPartitions in the editor below.
getMaximum Partitions has the following parameters: int performance[i]: The performance of all components
Returns
int: the maximum number of partitions among all possible ways to divide the components with the smallest sum of bottleneck levels
Constraints
1<=n<=2*10^5
1<=performance[i] <2^30
l < r

► Input Format For Custom Testing
▼ Sample Case 0
Sample Input For Custom Testing
5
2
3
1
5
2

Sample Output
2

Explanation

| Index Range (l, r) for the partitions | Bottleneck Level                                      |
|---------------------------------------|-------------------------------------------------------|
| (0, 2)                                | (performance[0] & performance[1] & performance[2]) = (2 & 3 & 1) = 0 |
| (3, 4)                                | (performance[3] & performance[4]) = (5 & 2) = 0       |

The sum of bottleneck levels is 0+0=0, which is the minimum possible. Therefore, the maximum number of partitions with the minimum total bottleneck level is 2

---
solution 1 : gpt

```c++
#include <iostream>
#include <vector>

using namespace std;

int getMaximumPartitions(vector<int>& performance) {
    int n = performance.size();
    int partitions = 0;  // To count the number of partitions
    int currentAND = performance[0];  // Initialize with the first component's performance

    for (int i = 0; i < n; i++) {
        // Perform the bitwise AND with the current component
        currentAND &= performance[i];
        
        // If the current AND becomes zero, we can create a new partition
        if (currentAND == 0) {
            partitions++;  // Increment the partition count
            if (i + 1 < n) {
                currentAND = performance[i + 1]; // Start new AND with next component
                i++; // Move to the next component as it belongs to the new partition
            }
        }
    }

    // Increment for the last partition if currentAND is not zero
    if (currentAND != 0) {
        partitions++;
    }
    
    return partitions;
}

int main() {
    // Sample Input
    vector<int> performance = {5, 2, 6, 1, 1, 4};
    cout << "Maximum number of partitions: " << getMaximumPartitions(performance) << endl;

    // You can also test with the custom test case
    vector<int> performance2 = {2, 3, 1, 5, 2};
    cout << "Maximum number of partitions for second case: " << getMaximumPartitions(performance2) << endl;

    return 0;
}

```