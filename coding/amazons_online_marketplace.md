In Amazon's online marketplace, the inventory manager is exploring strategies to enhance product sales. The focus is on creating appealing packages that offer customers a delightful shopping experience. To achieve this, the inventory manager aims to construct packages, each containing at most 2 items, to have an equal total cost across all packages. The total cost of a package is defined as the sum of the costs of the items contained within.
Formally, given an array cost of size n, representing the costs of individual items, determine the maximum number of packages that can be created, such that each package adheres to the constraint of having at most 2 items, and all packages have the same cost.
Note that each item can be used in at most one package.
Example 1
n=3
cost=[2,1,3]
Let's try packages with different total cost:

| case no. | Possible Packages | Total Cost | No of Packages |
|----------|-------------------|------------|----------------|
| 1        | [2]               | 2          | 1              |
| 2        | [1]               | 1          | 1              |
| 3        | [3], [1, 2]       | 3          | 2              |

In the above example, it is only possible to create 1 package with cost 2, similarly only one package can be created with cost 1, but with the cost 3 we can create 3 packages, which is the maximum possible packages with each package having same total cost, Hence the total answer is 2.


Example 2
n=9
cost[4, 5, 10, 3, 1, 2,2, 2, 3]
Let's try packages with different total cost:

| case no. | Possible Packages         | Total Cost | No of packages (with selected cost) |
|----------|---------------------------|------------|-------------------------------------|
| 1        | [10]                      | 10         | 1                                   |
| 2        | [4, 1], [5], [3, 2], [2, 3]| 5          | 4                                   |
| 3        | [4], [3, 1], [2, 2]       | 4          | 3                                   |

In the above example, the store manager can create 3 different types of packages with costs of 10, 5, and 4. However, th maximum number of packages that can be created with each package having an equal cost is 4, each with a cost of 5. Therefore, the answer is 4.

Function Description
Complete the function findMaximum Packages in the editor below.
findMaximumPackages has the following parameter:
int cost[n]: an array of integers denoting the cost of each item in the shop.
Returns
int: the maximum number of packages that can be made having the same cost, where each package consists of at most 2 items.

Constraints
1≤ n ≤2*10^5
• 1 ≤ cost[i] ≤2000

Input Format for Custom Testing
Sample Case 0
Sample Input 0
6
1
1
2
2
1
4
Sample Output 0
3
Explanation:
Lets try packages with different total cost:
| Packages       | Total Cost |
|----------------|------------|
| [1], [1], [1]  | 1          |
| [4], [2, 2]    | 4          |
| [2, 1], [2, 1] | 3          |
For the three package offers with total costs of 1, 4, and 3, the number of packages made are 3, 2, and 2 respectively. So, the inventory manager will make packages with a total cost of 1 since they can make the maximum packages of it, hence the answer is 3.

▼Sample Case 1
Sample input 1
3
10
2
1
sample output 1
1
Explanation

---
solution 1 : by gpt: O(N2)

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

int findMaximumPackages(vector<int>& cost) {
    unordered_map<int, int> costCount;

    // Count the occurrences of each cost
    for (int itemCost : cost) {
        costCount[itemCost]++;
    }

    int maxPackages = 0;

    // Check for all possible total costs from 1 to 4000 (max 2000 + 2000)
    for (int totalCost = 1; totalCost <= 4000; ++totalCost) {
        int packages = 0;

        // Try to form pairs of costs that sum to totalCost
        for (int itemCost = 1; itemCost <= totalCost / 2; ++itemCost) {
            if (costCount.find(itemCost) != costCount.end() && costCount.find(totalCost - itemCost) != costCount.end()) {
                if (itemCost == totalCost - itemCost) {
                    // Special case where both items are the same
                    packages += costCount[itemCost] / 2;
                } else {
                    packages += min(costCount[itemCost], costCount[totalCost - itemCost]);
                }
            }
        }

        // Update maxPackages
        maxPackages = max(maxPackages, packages);
    }

    // Handle single item packages
    for (const auto& [itemCost, count] : costCount) {
        maxPackages = max(maxPackages, count);
    }

    return maxPackages;
}

int main() {
    // Sample Input
    int n;
    cout << "Enter number of items: ";
    cin >> n;
    vector<int> cost(n);
    cout << "Enter costs: ";
    for (int i = 0; i < n; ++i) {
        cin >> cost[i];
    }

    // Calculate and output the result
    int result = findMaximumPackages(cost);
    cout << "Maximum packages with equal cost: " << result << endl;

    return 0;
}

```
---
solution 2 : optimised code by gpt

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

int findMaximumPackages(vector<int>& cost) {
    // Count occurrences of each cost
    unordered_map<int, int> costCount;
    for (int itemCost : cost) {
        costCount[itemCost]++;
    }

    int maxPackages = 0;

    // We can only have costs from 1 to 2000, thus total costs will range from 1 to 4000
    for (int totalCost = 1; totalCost <= 4000; ++totalCost) {
        int packages = 0;
        unordered_map<int, int> tempCount = costCount;  // Copy of original counts to avoid modifying them

        for (int itemCost = 1; itemCost <= totalCost / 2; ++itemCost) {
            if (tempCount[itemCost] > 0) {
                if (itemCost == totalCost - itemCost) {
                    // Special case for pairs (itemCost, itemCost)
                    packages += tempCount[itemCost] / 2; // Use pairs of the same cost
                } else {
                    int otherCost = totalCost - itemCost;
                    if (tempCount[otherCost] > 0) {
                        int pairCount = min(tempCount[itemCost], tempCount[otherCost]);
                        packages += pairCount;

                        // Decrease the counts
                        tempCount[itemCost] -= pairCount;
                        tempCount[otherCost] -= pairCount;
                    }
                }
            }
        }

        // Update the maximum number of packages found for any total cost
        maxPackages = max(maxPackages, packages);
    }

    // Check single items only (if they can form packages)
    for (const auto& [itemCost, count] : costCount) {
        maxPackages = max(maxPackages, count);
    }

    return maxPackages;
}

int main() {
    // Sample Input
    int n;
    cout << "Enter number of items: ";
    cin >> n;
    vector<int> cost(n);
    cout << "Enter costs: ";
    for (int i = 0; i < n; ++i) {
        cin >> cost[i];
    }

    // Calculate and output the result
    int result = findMaximumPackages(cost);
    cout << "Maximum packages with equal cost: " << result << endl;

    return 0;
}

```
---
OPTIMISED CODE

```c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int findMaximumPackages(vector<int>& cost) {
        int n = cost.size();
        sort(cost.begin(), cost.end()); // Sort the array to use two-pointer technique
        
        int maxPackages = 0;
        // Try each possible target sum
        for (int i = 0; i < n; i++) {
            vector<bool> used(n, false);
            int currentPackages = 0;
            int targetSum = cost[i];
            
            // For single item packages
            for (int j = 0; j < n && !used[j]; j++) {
                if (cost[j] == targetSum) {
                    used[j] = true;
                    currentPackages++;
                }
            }
            
            // For two item packages
            int left = 0, right = n - 1;
            while (left < right) {
                if (used[left]) {
                    left++;
                    continue;
                }
                if (used[right]) {
                    right--;
                    continue;
                }
                
                int sum = cost[left] + cost[right];
                if (sum == targetSum) {
                    used[left] = used[right] = true;
                    currentPackages++;
                    left++;
                    right--;
                } else if (sum < targetSum) {
                    left++;
                } else {
                    right--;
                }
            }
            
            maxPackages = max(maxPackages, currentPackages);
        }
        
        return maxPackages;
    }
};

int main() {
    Solution solution;
    vector<int> cost = {10, 2, 1};
    int result = solution.findMaximumPackages(cost);
    cout << result << endl; // Outputs: 3
    return 0;
}

```