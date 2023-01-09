# Number of Ways to Arrive at Destination
## https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/

You are in a city that consists of n intersections numbered from 0 to n - 1 with bi-directional roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.
You are given an integer n and a 2D integer array roads where roads[i] = [ui, vi, timei] means that there is a road between intersections ui and vi that takes timei minutes to travel. You want to know in how many ways you can travel from intersection 0 to intersection n - 1 in the shortest amount of time.
Return the number of ways you can arrive at your destination in the shortest amount of time. Since the answer may be large, return it modulo 109 + 7.
- Example 1:
Input: n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
Output: 4
Explanation: The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes.
The four ways to get there in 7 minutes are:
- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6
- Example 2:
Input: n = 2, roads = [[1,0,10]]
Output: 1
Explanation: There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.

# Approach 1: Using Priority Queue
```cpp
class Solution {
public:
    /*
        Approach: We will use the dijsktra algorithm to find out the shortest distance then calculate number of ways you can arrive at your destination in the shortest time. Take distance as long long
        1. Using a Priority Queue
        2. Using a Set
        3. Using a queue (not preferred)
    */
    #define P pair<long long,long long> 
    long long mod = 1e9 + 7;
    int dijkstra(vector<pair<int, int>> adj[], int n) {
        // Priority queue for relaxing nodes (distance, nodes)
        priority_queue<P, vector<P>, greater<P>> pq;
        
        // Distance vector
        vector<long long>distance(n, 1e15), path(n, 0);
        distance[0] = 0;
        pq.push({0, 0});
        path[0] = 1;

        while(!pq.empty()){
            int node = pq.top().second;
            long long dist = pq.top().first;
            pq.pop();
            
            for(auto it : adj[node]){
                int adjNode = it.first;
                long long edgeW = it.second;
                
                if(dist + edgeW < distance[adjNode]){
                    // Relax
                    distance[adjNode] = dist + edgeW;
                    path[adjNode] = path[node];
                    pq.push({distance[adjNode], adjNode});
                }else if(dist + edgeW == distance[adjNode]){
                    // Node is already visited with the shortes path, sum of parent and the current node path
                    path[adjNode] = (1LL*path[node] + 1LL*path[adjNode]) % mod;
                }
            }
        }
        return path[n - 1];
    }
    int countPaths(int n, vector<vector<int>>& roads) {
        
        
         // First step for grpah question is to create the adjacency list
        vector<pair<int, int>> adj[n];

        // Graph Creation
        for (auto &v : roads) {
            adj[v[0]].push_back({v[1], v[2]});
            adj[v[1]].push_back({v[0], v[2]});
        }
        
        return dijkstra(adj, n);
    }
};
```

# Approach 2: Using Set
```cpp
class Solution {
public:
    /*
        Approach: We will use the dijsktra algorithm to find out the shortest distance then calculate number of ways you can arrive at your destination in the shortest time. Take distance as long long
        1. Using a Priority Queue
        2. Using a Set
        3. Using a queue (not preferred)
    */
    long long mod = 1e9 + 7;
    int dijkstra2(vector<pair<int, int>> adj[], int n) {
     // Set for relaxing nodes (distance, nodes), in set we can delete if we found dist != INF but it will also takes up O(logn)
        set<pair <long, long>> st;
        
        // Distance vector
        vector<long long>distance(n, 1e15), path(n, 0);
        distance[0] = 0;
        st.insert({0, 0});
        path[0] = 1;

        while(!st.empty()){
            auto itr = *(st.begin());
            int node = itr.second;
            long long dist = itr.first;
            st.erase(itr);
            
            for(auto it : adj[node]){
                int adjNode = it.first;
                long long edgeW = it.second;
                
                if(dist + edgeW < distance[adjNode]){
                    if(distance[adjNode] != 1e9)st.erase({distance[adjNode], adjNode});
                    
                    // Relax
                    distance[adjNode] = dist + edgeW;
                    
                    // Path Update
                    path[adjNode] = path[node];
                    
                    // Inserting Back 
                    st.insert({distance[adjNode], adjNode});
                }else if(dist + edgeW == distance[adjNode]){
                    // Node is already visited with the shortes path, sum of parent and the current node path
                    path[adjNode] = (1LL*path[node] + 1LL*path[adjNode]) % mod;
                }
            }
        }
        return path[n - 1];
    }
    
    int countPaths(int n, vector<vector<int>>& roads) {
         // First step for grpah question is to create the adjacency list
        vector<pair<int, int>> adj[n];

        // Graph Creation
        for (auto &v : roads) {
            adj[v[0]].push_back({v[1], v[2]});
            adj[v[1]].push_back({v[0], v[2]});
        }
        
        return dijkstra2(adj, n);
    }
};
```