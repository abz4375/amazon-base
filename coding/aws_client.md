An AWS client has brought servers and databases from data centers in different parts of the world for their application. For simplicity, let's assume all the servers and data centers are located on a 1-dimensional line.
You have been given the task of optimizing the network connection. Each data center must be connected to a server. The positions of n data centers and n servers are given in the form of arrays. Any particular data center, center[i], can deliver to any particular server destination, destination[j]. The lag is defined distance between a data center at location x and a server destination at location y is |x - y| i.e., the absolute difference between x and y. Determine the minimum lag to establish the entire network.
Example
There are n = 3 connections, the positions of data centers, center = [1, 2, 2] , and the positions of the server destinations, destination = [5, 2, 4].
The most efficient deliveries are:
• The center at location 1 makes the first connection to the server at location 2.
• The center at location 2 makes the second connection to the server at location 4.
• The center at location 2 makes the third connection to the server at location 5.
The minimum total lag is = abs(1-2)+abs(2-4)+abs(2-5)=1+2 +3=6.

Function Description
Complete the function getMin Distance in the editor below.
getMinDistance has the following parameter(s): 
int center[n]: the positions of data centers 
int destination[n]: the positions of server destinations
Returns
long int: the minimum lag
Constraints
1≤ n ≤10^5
1 ≤ center[i], destination[i] ≤ 10^9
► Input Format For Custom Testing
▼ Sample Case 0
Sample Input For Custom Testing
5
3
1
6
8
9
5
2
3
1
7
9
Sample output
5
Explanation
You may create the connections as:
center = [1, 3, 6, 8, 9]
destination = [1, 2, 3, 7, 9]
Minimum total distance abs(1-1)+abs(2-3)+abs(3-6)+abs(7-8)+abs(9-9)=0+1+3+1+0=5

---
solution 1: gpt

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

long long getMinDistance(std::vector<int>& center, std::vector<int>& destination) {
    // Sort both arrays
    std::sort(center.begin(), center.end());
    std::sort(destination.begin(), destination.end());
    
    long long totalLag = 0;
    int n = center.size();
    
    // Calculate the total lag
    for (int i = 0; i < n; ++i) {
        totalLag += std::abs(center[i] - destination[i]);
    }
    
    return totalLag;
}

int main() {
    // Example input
    int n = 5;
    std::vector<int> center = {1,2,2};
    std::vector<int> destination = {5,2,4};
    
    long long result = getMinDistance(center, destination);
    std::cout << result << std::endl; // Output the minimum total lag

    return 0;
}

```

---
