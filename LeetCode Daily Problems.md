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
# Max Dot Product of Two Subsequences
```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[][] dp = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                int product = nums1[i] * nums2[j];
                if(i > 0 && j > 0){
                    dp[i][j] = product + Math.max(0, dp[i-1][j-1]);
                } else {
                    dp[i][j] = product;
                }
                if(i > 0){
                    dp[i][j] = Math.max(dp[i][j], dp[i-1][j]);
                }
                if(j > 0){
                    dp[i][j] = Math.max(dp[i][j], dp[i][j-1]);
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```
