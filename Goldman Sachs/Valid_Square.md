# Valid Square
## https://leetcode.com/problems/valid-square/

Given the coordinates of four points in 2D space p1, p2, p3 and p4, return true if the four points construct a square.
The coordinate of a point pi is represented as [xi, yi]. The input is not given in any order.
A valid square has four equal sides with positive length and four equal angles (90-degree angles).
- Example 1:
Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
Output: true
- Example 2:
Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,12]
Output: false
- Example 3:
Input: p1 = [1,0], p2 = [-1,0], p3 = [0,1], p4 = [0,-1]
Output: true

```cpp
class Solution {
public:
    /*
        P1[0] - x   P2[0] - x1
        P1[1] - y   P2[1] - y1
        (x - x1)^2 + (y - y2)^2
        
        
        (0,1)       (1,1)
            ----------
            |        |
            |        |
            |        |
            ----------
        (0,0)       (1,0)
        (0, 0), (0, 1), (1, 0), (1, 1)
    */
    
    int getDistance(vector<int>p1, vector<int>p2){
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
    }
    
    bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
        vector<vector<int>>p;
        p.push_back(p1); p.push_back(p2); p.push_back(p3); p.push_back(p4);
        sort(p.begin(), p.end());
        int s1 = getDistance(p[0], p[1]), s2 = getDistance(p[1], p[3]), 
        s3 = getDistance(p[2], p[3]), s4 = getDistance(p[0], p[2]), 
        d1 = getDistance(p[1], p[2]), d2 = getDistance(p[0], p[3]);
        
        if (s1==0 || s2==0 || s3==0 || s4==0 || d1==0 || d2==0)return false;
        if(s1 == s2 && s2 == s3 && s3 == s4 && d1 == d2)return true;
        return false;
        
    }
};
```