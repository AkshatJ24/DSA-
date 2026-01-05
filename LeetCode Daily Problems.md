# Maximum Marriix Sum
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
