# Reschedule Meetings for Maximum Free Time I

## 🧩 Problem Statement

You are given:

- An integer `eventTime` denoting the total duration of an event, which occurs from time `t = 0` to `t = eventTime`.
- Two integer arrays `startTime` and `endTime`, each of length `n`. They represent the start and end times of `n` non-overlapping meetings. The `i-th` meeting occurs during the time `[startTime[i], endTime[i]]`.

You may **reschedule at most `k` meetings**, adjusting their start times **while preserving their durations**, to maximize the **longest continuous period of free time** during the event.

**Constraints:**

- Rescheduled meetings must:
  - Stay within the event duration.
  - Maintain the same order (no reordering).
  - Remain non-overlapping.

Return the **maximum possible amount of continuous free time** achievable after rescheduling.

---

## 💡 Intuition

The key to maximizing continuous free time is to focus on **gaps between meetings**:

- A "gap" is defined as the period of free time:
  - Before the first meeting.
  - Between any two consecutive meetings.
  - After the last meeting.

To create a longer free period, we can **merge multiple adjacent gaps** into one.  
However, merging two adjacent gaps requires **rescheduling** the single meeting that lies between them.

Hence:

- To merge `m` adjacent gaps, we must reschedule `m - 1` meetings.
- If we're allowed to reschedule at most `k` meetings, then we can merge up to `k + 1` gaps.
- The problem reduces to **finding the subarray of `k + 1` adjacent gaps with the largest total length**.

---

## 🛠️ Approach

1. **Build an array of gaps**:
   - Before the first meeting: `startTime[0] - 0`
   - Between meetings: `startTime[i+1] - endTime[i]`
   - After the last meeting: `eventTime - endTime[n-1]`
   - Total gaps: `n + 1`

2. **Sliding Window Technique**:
   - Use a window of size `k + 1` over the gap array.
   - For each such window, compute the sum of those `k + 1` gaps.
   - Track and return the maximum sum seen — that’s the largest continuous free time possible after rescheduling.

---

## ⏱️ Time and Space Complexity

- **Time Complexity:** `O(n)`  
  (One pass to compute the gaps, one pass for the sliding window.)

- **Space Complexity:** `O(n)`  
  (To store the `gaps` array.)

---

## 💻 Code

```cpp
class Solution {
public:
    int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
        int n = startTime.size();
        vector<int> v(n + 1);   
        
        // Gaps array
        v[0] = startTime[0];                         // Gap before the first meeting
        v[n] = eventTime - endTime[n - 1];           // Gap after the last meeting
        for (int i = 0; i < n - 1; i++) {
            v[i + 1] = startTime[i + 1] - endTime[i]; // Gaps between meetings
        }

        // Sliding window of size k + 1 to find max sum of k + 1 adjacent gaps
        int sum = 0, ans = 0;
        int l = 0, r = 0;
        while (r <= n) {
            sum += v[r];
            if (r - l + 1 == k + 1) {
                ans = max(ans, sum);
                sum -= v[l];
                l++;
            }
            r++;
        }

        return ans;
    }
};
