Data Scientists at Amazon are working on cleansing a machine learning dataset. The dataset is represented as a string dataset consisting of an even number of lowercase English letters. The goal is to clean the dataset efficiently by performing specific operations.
Here's how the operations work:
In each operation, two characters from the dataset are selected and removed.
• Each operation has an associated cost:
x the cost of removing two identical characters.
y the cost of removing two different characters.
The task is to determine the optimal strategy that minimizes the total cost to completely clean up the dataset. In other words, find the minimum cost required to
remove all characters and make the dataset empty.
Example
dataset="ouio" 
x=2
y=4
Initial String: dataset "ouio"
• Operation 1:
Action: Remove the first and last characters of the dataset, resulting in the string

---
solution 1 : whatsapp

```c++
#include <bits/stdc++.h>
using namespace std;

int minCleanupCost(string dataset, int x, int y) {
    // Step 1: Frequency map for counting occurrences of each character
    unordered_map<char, int> freq;
    vector<int>cntmap;
    for (char c : dataset) {
        freq[c]++;
    }
    for(auto it:freq){
        cntmap.push_back(it.second);
    }
    sort(cntmap.begin(),cntmap.end(),greater<int>());
    double ans=0;

    if(x<y){
        for(int it=0;it<cntmap.size();it++){
            ans+=((cntmap[it]/2)*x);
            if(cntmap[it]%2==1){
                double temp=y;
                ans+=(temp/2);
            }
        }
        int fans=ans;
        return fans;
    }

    else{

        int prev=-1;
        for(int it=0;it<cntmap.size();it++){
            if(prev==-1){
                prev=cntmap[it];
            }
            else{
                int mini=min(prev,cntmap[it]);
                ans+=mini*(y);
                prev=max(prev,cntmap[it])-min(prev,cntmap[it]);
            }

        }
        if(prev>0){
            ans+=(prev/2)*x;
        }
        int fans=ans;
        return ans;
    }





}

int main() {
    string dataset = "ouio";
    int ox = 2; // Cost of removing two identical characters
    int oy = 4; // Cost of removing two different characters

    int result = minCleanupCost(dataset, ox, oy);
    cout << result << endl;

    return 0;
}
```
---
POSSIBLY OPTIMISED SOLUTION

```c++
#include <bits/stdc++.h>
using namespace std;

int minCleanupCost(string dataset, int x, int y) {
    int n = dataset.length();
    
    // Base case: empty string or odd length string
    if (n == 0) return 0;
    if (n % 2 != 0) return -1;  // Invalid case
    
    // Create frequency map
    vector<int> freq(26, 0);
    for (char c : dataset) {
        freq[c - 'a']++;
    }
    
    // Count characters with odd frequency
    int oddCount = 0;
    for (int i = 0; i < 26; i++) {
        if (freq[i] % 2 != 0) oddCount++;
    }
    
    // If odd count is odd, it's impossible to pair all characters
    if (oddCount % 2 != 0) return -1;
    
    int totalCost = 0;
    
    if (x <= y) {
        // If removing same characters costs less
        // First try to pair same characters
        for (int i = 0; i < 26; i++) {
            totalCost += (freq[i] / 2) * x;
            freq[i] %= 2;
        }
        
        // Remaining characters must be paired with different ones
        int remainingPairs = 0;
        for (int i = 0; i < 26; i++) {
            remainingPairs += freq[i];
        }
        totalCost += (remainingPairs / 2) * y;
    } else {
        // If removing different characters costs less
        // Try to pair different characters first
        vector<int> sortedFreq;
        for (int i = 0; i < 26; i++) {
            if (freq[i] > 0) {
                sortedFreq.push_back(freq[i]);
            }
        }
        sort(sortedFreq.begin(), sortedFreq.end(), greater<int>());
        
        while (sortedFreq.size() > 1) {
            int min_val = min(sortedFreq[0], sortedFreq[1]);
            totalCost += min_val * y;
            
            sortedFreq[0] -= min_val;
            sortedFreq[1] -= min_val;
            
            // Remove frequencies that became 0
            sortedFreq.erase(
                remove_if(sortedFreq.begin(), sortedFreq.end(), 
                         [](int x) { return x == 0; }),
                sortedFreq.end());
        }
        
        // If any characters remain, they must be paired with same type
        if (!sortedFreq.empty()) {
            totalCost += (sortedFreq[0] / 2) * x;
        }
    }
    
    return totalCost;
}

int main() {
    // Test cases
    vector<pair<string, pair<int, int>>> testCases = {
        {"ouio", {2, 4}},
        {"aabb", {2, 4}},
        {"abcd", {2, 4}},
        {"aaaa", {2, 4}},
        {"aabbcc", {1, 3}}
    };
    
    for (const auto& test : testCases) {
        string dataset = test.first;
        int x = test.second.first;
        int y = test.second.second;
        cout << "Dataset: " << dataset << ", x=" << x << ", y=" << y << "\n";
        cout << "Result: " << minCleanupCost(dataset, x, y) << "\n\n";
    }
    
    return 0;
}

```