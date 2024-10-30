Code Question 1
In an Amazon computing environment, there is a critical sequence of n processes that must be executed in a specific order for accurate results. The prescribed order is represented by the array processOrder, where processOrder[i] (1<=i<=n) denotes the process to be executed at the ith position. Accurate results require the execution of all preceding processes.
Facing a challenge, an individual, guided by the array executionOrder, mistakenly runs the processes in a different order. Here, execution Order[i] (1 ≤ i ≤ n) signifies the process executed at the ith position. Both processOrder and executionOrder are permutations of size n.
Determine the count of processes that fail to produce accurate results due to the deviation from the specified order.
A permutation is a sequence of integers from 1 to n of length n containing each number exactly once, for example [1, 3, 2] is a permutation while [1, 2, 1] is not.
Example
n = 5 processOrder = [2, 3, 5, 1, 4] executionOrder = [5, 2, 3, 4, 1]
We see that 2 processes would give inaccurate results and these would be the process number 5 and 4.

Process 5 gives inaccurate results because in the original order, it needs processes 2 and 3 to run successfully, and the individual executes both of them after process 5. Also, process 4 needs all the other processes to be executed before it runs properly. But the individual executes process 1 afterward, resulting in 4 giving inaccurate results
Therefore, 2 processes give inaccurate results.
Function Description
Complete the function getInaccurateProcesses in the editor below.
getInaccurate Processes has the following parameters:
int processOrder[n]: Order in which the processes should be executed.
int executionOrder[n]:Order in which the processes were actually executed.
Returns
int: the count of processes that fail to produce accurate results due to the deviation from the specified order.
Constraints
1<=n<=2*10^5
1 ≤ processOrder[i], executionOrder[i] ≤ n
processOrder and executionOrder are permutations of integers from 1 to n

---
laest solution

```c++
#include <vector>
#include <unordered_map>
#include <unordered_set>
using namespace std;

int getInaccurateProcesses(vector<int>& processOrder, vector<int>& executionOrder) {
    int n = processOrder.size();
    
    // Create a map to store the position of each process in the correct order
    unordered_map<int, int> processPosition;
    for(int i = 0; i < n; i++) {
        processPosition[processOrder[i]] = i;
    }
    
    // Create a map to store required processes for each process
    unordered_map<int, unordered_set<int>> requirements;
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < i; j++) {
            requirements[processOrder[i]].insert(processOrder[j]);
        }
    }
    
    // Track executed processes
    unordered_set<int> executed;
    int inaccurateCount = 0;
    
    // Check each process in execution order
    for(int process : executionOrder) {
        // Check if all required processes have been executed
        bool isAccurate = true;
        for(int req : requirements[process]) {
            if(executed.find(req) == executed.end()) {
                isAccurate = false;
                break;
            }
        }
        
        if(!isAccurate) {
            inaccurateCount++;
        }
        
        executed.insert(process);
    }
    
    return inaccurateCount;
}

#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <iostream>
using namespace std;

// Previous getInaccurateProcesses function remains the same

int main() {
    // Test case from the problem
    vector<int> processOrder = {2, 3, 5, 1, 4};
    vector<int> executionOrder = {5, 2, 3, 4, 1};
    
    int result = getInaccurateProcesses(processOrder, executionOrder);
    
    cout << "Number of inaccurate processes: " << result << endl;
    // Expected output: Number of inaccurate processes: 2
    
    return 0;
}

```

---
solution 1 : telegram - python

```py
def getInaccurateProcesses(processOrder, executionOrder):
    class FenwickTree:
        def init(self, size):
            self.size = size
            self.tree = [0] * (size + 2)  

        def update(self, index, delta=1):
            while index <= self.size:
                self.tree[index] += delta
                index += index & -index

        def query(self, index):
            res = 0
            while index > 0:
                res += self.tree[index]
                index -= index & -index
            return res

        def range_query(self, left, right):
            if left > right:
                return 0
            return self.query(right) - self.query(left -1)

    n = len(processOrder)
    pid = {}
    for idx, process in enumerate(processOrder):
        pid[process] = idx

    ft = FenwickTree(n)

    inaccurate =0

    for process in executionOrder:
        index = pid[process] 
        fti = index +1
        count = ft.query(fti -1)
        if count != index:
            inaccurate +=1
        ft.update(fti)
    
    return inaccurate
```
---
OPTIMISED BY CLAUDE

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>

class Solution {
public:
    int countFailedProcesses(int n, std::vector<int>& processOrder, std::vector<int>& executionOrder) {
        std::unordered_map<int, int> processPosition;
        std::vector<bool> executed(n + 1, false);
        std::vector<int> lastRequiredProcess(n + 1, 0);
        
        // Precompute the last required process for each process
        for (int i = 0; i < n; i++) {
            processPosition[processOrder[i]] = i;
            if (i > 0) {
                lastRequiredProcess[processOrder[i]] = processOrder[i-1];
            }
        }
        
        int failCount = 0;
        for (int i = 0; i < n; i++) {
            int currentProcess = executionOrder[i];
            int pos = processPosition[currentProcess];
            
            if (pos > 0 && !executed[lastRequiredProcess[currentProcess]]) {
                failCount++;
            }
            
            executed[currentProcess] = true;
        }
        
        return failCount;
    }
};


int main() {
    // Create instance of Solution class
    Solution solution;
    
    // Test case from the example
    int n = 5;
    std::vector<int> processOrder = {2, 3, 5, 1, 4};
    std::vector<int> executionOrder = {5, 2, 3, 4, 1};
    
    // Call the function and print result
    int result = solution.countFailedProcesses(n, processOrder, executionOrder);
    std::cout << "Number of failed processes: " << result << std::endl;
    
    return 0;
}

```

---
solution 2: gpt O(N2):

```c++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>

int countInaccurateResults(int n, const std::vector<int>& processOrder, const std::vector<int>& executionOrder) {
    // Map to store the position of each process in processOrder
    std::unordered_map<int, int> positionMap;
    for (int i = 0; i < n; ++i) {
        positionMap[processOrder[i]] = i;
    }

    // Set to keep track of processes executed so far
    std::unordered_set<int> executed;
    int inaccurateCount = 0;

    // Traverse each process in the executionOrder
    for (const auto& process : executionOrder) {
        int requiredPos = positionMap[process];
        
        // Check if any prior process (as per processOrder) has not been executed yet
        for (int i = 0; i < requiredPos; ++i) {
            if (executed.find(processOrder[i]) == executed.end()) {
                ++inaccurateCount;
                break;
            }
        }

        // Add the current process to the executed set
        executed.insert(process);
    }

    return inaccurateCount;
}

int main() {
    int n = 5;
    std::vector<int> processOrder = {2, 3, 5, 1, 4};
    std::vector<int> executionOrder = {5, 2, 3, 4, 1};
    
    int result = countInaccurateResults(n, processOrder, executionOrder);
    std::cout << "Number of inaccurate processes: " << result << std::endl;
    
    return 0;
}

```