## **Problem Statement**

Given two arrays `nums1` and `nums2` consisting of positive integers, replace all the `0`s in both arrays with strictly positive integers such that the sum of elements of both arrays becomes equal.  
Return the **minimum possible equal sum**, or **-1** if it's impossible to make the sums equal.

---

## **Approach**

### 1. **Calculate Minimum Sums**
For each array, compute the minimum possible sum by treating all `0`s as `1` (the smallest strictly positive integer).

### 2. **Count Zeros**
Keep track of how many zeros are in each array.

### 3. **Compare Sums**

- If the minimum sums are already equal, that's our answer.  
- If one sum is smaller than the other, check if the array with the smaller sum has at least one zero  
  (so we can increase its sum by replacing zeros with larger numbers).  
- If the smaller sum array has no zeros, it's impossible to make the sums equal.

---

## **C++ Code**

```cpp
using ll = long long;

class Solution {
public:
    long long minSum(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), cntZ1 = 0;
        ll sum1 = 0;
        for (int i = 0; i < n1; i++) {
            if (nums1[i] == 0) {
                cntZ1 += 1;
                sum1 += 1;
            }
            else sum1 += nums1[i];
        }

        int n2 = nums2.size(), cntZ2 = 0;
        ll sum2 = 0;
        for (int i = 0; i < n2; i++) {
            if (nums2[i] == 0) {
                cntZ2 += 1;
                sum2 += 1;
            }
            else sum2 += nums2[i];
        }

        ll minSum = max(sum1, sum2);
        if (sum1 < sum2) {
            if (cntZ1 == 0) return -1;
            else return minSum;
        } 
        else if (sum2 < sum1) {
            if (cntZ2 == 0) return -1;
            else return minSum;
        }
        else return minSum;
    }
};
