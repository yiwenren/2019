### isSymmetric
O(n) 可以理解成遍历了整棵树？
https://leetcode.com/problems/symmetric-tree/

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }    
        
        return helper(root.left, root.right);
    }
    
    private boolean helper(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null || right == null) {
            return false;
        } else if (left.val != right.val) {
            return false;
        } 
        
        return helper(left.left, right.right) && helper(left.right, right.left);
    }
    
}
```
