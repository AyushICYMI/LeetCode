# Sum of Largest Prime Substrings

## Problem Statement
Given a string `s`, find the sum of the 3 largest unique prime numbers that can be formed from its substrings. A substring is a contiguous sequence of characters. Leading zeros in substrings should be ignored when converting to numbers.

**Rules:**
- Return sum of top 3 largest unique primes
- If fewer than 3 primes exist, return sum of all available
- If no primes, return 0

## Approach
1. **Generate all possible substrings** of the input string
2. **Convert each substring** to a number (ignoring leading zeros)
3. **Check for primality** using trial division
4. **Store unique primes** in a sorted set (descending order)
5. **Sum the top 3 largest** primes from the set

### Time and Space Complexity
- **Time Complexity**: O(n² * √m)  
  (n = string length, m = maximum number value)
  - O(n²) for substring generation
  - O(√m) for primality check of each number
- **Space Complexity**: O(k)  
  (k = number of unique primes found)

## Solution Code
```cpp
#include <vector>
#include <string>
#include <set>
#include <algorithm>

using namespace std;

class Solution {
public:
    bool isPrime(long long num) {
        if (num <= 1) return false;
        if (num == 2) return true;
        if (num % 2 == 0) return false;
        for (long long i = 3; i * i <= num; i += 2) {
            if (num % i == 0) return false;
        }
        return true;
    }

    long long sumOfLargestPrimes(string s) {
        set<long long, greater<long long>> primes;
        
        // Generate all substrings
        for (int i = 0; i < s.size(); i++) {
            string current;
            for (int j = i; j < s.size(); j++) {
                current += s[j];
                // Skip leading zeros except for "0" itself
                if (current.size() > 1 && current[0] == '0') continue;
                
                long long num = stoll(current);
                if (isPrime(num)) {
                    primes.insert(num);
                }
            }
        }
        
        // Sum top 3 largest primes
        long long sum = 0;
        auto it = primes.begin();
        for (int i = 0; i < 3 && it != primes.end(); i++, it++) {
            sum += *it;
        }
        
        return sum;
    }
};
