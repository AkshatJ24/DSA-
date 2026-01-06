# Maximum Matrix Sum
```java
class Solution {
    public long maxMatrixSum(int[][] matrix) {
        long absSum = 0;
        int minValue = Integer.MAX_VALUE;
        int count = 0;
        for(int[] row : matrix){
            for(int i : row){
                absSum += Math.abs(i);
                if(i < 0){
                    count++;
                }
                minValue = Math.min(minValue, Math.abs(i));
            }    
        }
        return (count % 2 == 0) ? absSum : (absSum - 2 * minValue);
    }
}
```
# Maximum Level Sum of a Binary Tree
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxLevelSum(TreeNode root) {
        int maxSum = Integer.MIN_VALUE;
        int ans = 0;
        int level = 0;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            level++;
            int sumAtCurrentLevel = 0;
            for(int i = q.size(); i > 0; --i){
                TreeNode node = q.poll();
                sumAtCurrentLevel += node.val;
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            if(maxSum < sumAtCurrentLevel){
                maxSum = sumAtCurrentLevel;
                ans = level;
            }
        }
        return ans;
    }
}
```
