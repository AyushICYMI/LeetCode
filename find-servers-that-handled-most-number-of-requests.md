# Find Servers That Handled Most Number of Requests

## Problem Statement

You are given `k` servers numbered from `0` to `k - 1`. Each server can handle **only one request at a time**, but has infinite computing capacity otherwise.

You're also given:

- A strictly increasing array `arrival[]`, where `arrival[i]` is the time the `i-th` request arrives.
- An array `load[]`, where `load[i]` is the processing time for the `i-th` request.

**Assignment Rules:**

- A request `i` should be assigned to server `(i % k)` if it is free.
- If that server is busy, find the next available server `(i+1) % k`, `(i+2) % k`, ... (wrap around circularly).
- If all servers are busy at the time of the request, **drop the request**.

Your task is to **return the list of server indices that handled the most number of requests**.

---

## Intuition

This is a simulation problem where we track which servers are busy and which are free over time. The challenge is:

- Efficiently finding the next available server.
- Keeping track of when servers will become free (using a min-heap).
- Ensuring we assign the request to the correct server as per the rules.

We use a `set` to store the available servers in sorted order, allowing efficient lookup for the "next available" one.

---

## Approach

1. Use a `set` to maintain the IDs of available servers.
2. Use a `min-heap` (priority queue) to track currently busy servers and when they'll become free: `(end_time, server_id)`.
3. For each request:
   - Free up servers whose end time ≤ current time.
   - Try to assign the request to `(i % k)` or the next available server (wrapping around).
   - If assigned:
     - Increment that server's request count.
     - Mark it busy by pushing into the heap with its end time.
4. Track the maximum number of requests handled.
5. Return all server IDs with the maximum request count.

---

## Time and Space Complexity

- **Time Complexity:** `O(n log k)`  
  - For each of `n` requests, heap and set operations are `O(log k)`.

- **Space Complexity:** `O(k + n)`  
  - For storing server status and heap of busy servers.

---

## Code

```cpp
using ll = long long;

class Solution {
public:
    vector<int> busiestServers(int k, vector<int>& arrival, vector<int>& load) {
        vector<int> request_counts(k, 0);
        priority_queue<pair<ll, int>, vector<pair<ll, int>>, greater<>> busy_servers;
        set<int> available_servers;
        
        for (int i = 0; i < k; ++i) {
            available_servers.insert(i);
        }

        int max_requests = 0;

        for (int i = 0; i < arrival.size(); ++i) {
            ll current_time = arrival[i];

            // Free up servers
            while (!busy_servers.empty() && busy_servers.top().first <= current_time) {
                int freed_server_id = busy_servers.top().second;
                busy_servers.pop();
                available_servers.insert(freed_server_id);
            }

            if (available_servers.empty()) continue;

            int preferred_server = i % k;
            auto it = available_servers.lower_bound(preferred_server);

            if (it == available_servers.end()) {
                it = available_servers.begin();
            }

            int server_to_use = *it;
            request_counts[server_to_use]++;
            max_requests = max(max_requests, request_counts[server_to_use]);

            available_servers.erase(it);
            ll end_time = current_time + load[i];
            busy_servers.emplace(end_time, server_to_use);
        }

        vector<int> result;
        for (int i = 0; i < k; ++i) {
            if (request_counts[i] == max_requests) {
                result.push_back(i);
            }
        }

        return result;
    }
};
