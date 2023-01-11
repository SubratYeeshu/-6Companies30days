# All Elements in Two Binary Search Trees
## https://leetcode.com/problems/all-elements-in-two-binary-search-trees/

Given two binary search trees root1 and root2, return a list containing all the integers from both trees sorted in ascending order.
- Example 1:
Input: root1 = [2,1,4], root2 = [1,0,3]
Output: [0,1,1,2,3,4]
- Example 2:
Input: root1 = [1,null,8], root2 = [8,1]
Output: [1,1,8,8]


```cpp
class Solution {
private:
    vector<int>vec;
    void inorder(TreeNode* root, vector<int>&vec){
        if(!root)return;
        inorder(root -> left, vec);
        vec.push_back(root -> val);
        inorder(root -> right, vec);
    }
    
    vector<int> merge(vector<int> vec1, vector<int> vec2){
        int i = 0, j = 0, k = 0, n = vec1.size(), m = vec2.size(); 
        vector<int> merged (n + m, 0);
        while (i < n && j < m){ 
            if (vec1[i] < vec2[j]) merged[k++] = vec1[i++]; 
            else merged[k++] = vec2[j++];
        }    
        while (i < n)merged[k++] = vec1[i++];
        while (j < m)merged[k++] = vec2[j++];
        
        return merged;
    }
public:
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        vector<int>vec1, vec2;
        inorder(root1, vec1);
        inorder(root2, vec2);
        return merge(vec1 ,vec2);
    }
};
```