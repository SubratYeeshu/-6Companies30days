# Course Schedule
## https://leetcode.com/problems/course-schedule/

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.
    For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.
- Example 1:
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

- Example 2:
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

```cpp
class Solution {
public:
    /*
        The intution to solve this question is that storing the points in a map, the bottom left point and top right is incremented and bottom right 
		and top left is decremented which as whole cancels the inner points. If the absolute sum of all the points comes out to be 4 the return true
            
        P[0][3]       P[2][3]
            ---------
            |       |
            |       |   
            |       |
            ---------
        P[0][1]       P[2][1]
    */
    bool isRectangleCover(vector<vector<int>>& Pts) {
        map<pair<int, int>, int> mp;
        int count = 0;
        for(auto P : Pts){
            mp[{P[0], P[1]}]++;
            mp[{P[2], P[3]}]++;
            mp[{P[0], P[3]}]--;
            mp[{P[2], P[1]}]--;
        }
        for(auto itr : mp)count += abs(itr.second);
        return count == 4;
    }
};
```
