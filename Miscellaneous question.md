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
    }
}
```

## DAY 27
### Sum of SUbarray Minimum
#### Brute Force
```java
class Solution {
    public int sumSubarrayMins(int[] arr) {
        int sum = 0;
        int n = arr.length;
        int mod = (int)(1e9 + 7);
        for(int i = 0; i<n ; i++){
            int mini = arr[i];
            for(int j = i; j<n; j++){
                mini = Math.min(mini, arr[j]);
                sum = (sum + mini)%(mod);
            }
        }
        return sum;
    }
}
```
#### Optimal Approach
```java
import java.util.*;

class Solution {
    public int sumSubarrayMins(int[] arr) {
        int n = arr.length;
        int[] nse = nextSmaller(arr);
        int[] pse = prevSmaller(arr);
        int total = 0;
        int mod = (int)(1e9 + 7);

        for (int i = 0; i < n; i++) {
            int left = i - pse[i];
            int right = nse[i] - i;
            long contrib = ((long)arr[i] * left * right) % mod;
            total = (int)((total + contrib) % mod);
        }

        return total;
    }

    // Next Smaller Element indices (strictly smaller)
    public int[] nextSmaller(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> stack = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && arr[stack.peek()] > arr[i]) {
                stack.pop();
            }
            result[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
        }

        return result;
    }

    // Previous Smaller Element indices (strictly smaller)
    public int[] prevSmaller(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
                stack.pop();
            }
            result[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }

        return result;
    }
}
```
## DAY 28
### Asteroid Collision
```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        int n = asteroids.length;
        Stack<Integer> st = new Stack<>();
        
        for (int i = 0; i < n; i++) {
            if (asteroids[i] > 0) {
                st.push(asteroids[i]);
            } else {
                while (!st.isEmpty() && st.peek() > 0 && st.peek() < Math.abs(asteroids[i])) {
                    st.pop();
                }
                if (!st.isEmpty() && st.peek() == Math.abs(asteroids[i])) {
                    st.pop();
                } else if (st.isEmpty() || st.peek() < 0) {
                    st.push(asteroids[i]);
                }
            }
        }
        
        int[] result = new int[st.size()];
        for (int i = st.size() - 1; i >= 0; i--) {
            result[i] = st.pop();
        }
        return result;
    }
}
```
