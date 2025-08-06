# Miscellaneous question
## DAY 26
### Trapping Rain Water
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int left = 0;
        int right = n - 1;
        int leftMax = 0;
        int rightMax = 0;
        int result = 0;

        // Traverse the array from both ends
        while (left <= right) {
            // Process the side with the smaller height
            if (height[left] <= height[right]) {
                // If current height is greater than or equal to leftMax, update leftMax
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    // Water can be trapped, add the difference
                    result += leftMax - height[left];
                }
                left++;
            } else {
                // If current height is greater than or equal to rightMax, update rightMax
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    // Water can be trapped, add the difference
                    result += rightMax - height[right];
                }
                right--;
            }
        }

        return result;
```
    }
}
