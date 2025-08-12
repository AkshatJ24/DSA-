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
## DAY 28
### Remove K Digits
```java
class Solution {
    public String removeKdigits(String num, int k) {
        Stack<Character> st = new Stack<>();

        // Step 1: Build the stack with greedy removal
        for (int i = 0; i < num.length(); i++) {
            while (!st.empty() && k > 0 && st.peek() > num.charAt(i)) {
                st.pop();
                --k;
            }
            st.push(num.charAt(i));
        }

        // Step 2: Remove remaining digits from the end if k > 0
        while (k > 0) {
            st.pop();
            --k;
        }

        // Step 3: Build the result string from the stack
        StringBuilder res = new StringBuilder();
        while (!st.empty()) {
            res.append(st.pop());
        }

        // Step 4: Reverse the result since we built it backwards
        res.reverse();

        // Step 5: Remove leading zeroes
        while (res.length() > 0 && res.charAt(0) == '0') {
            res.deleteCharAt(0);
        }

        // Step 6: Return "0" if result is empty
        return res.length() == 0 ? "0" : res.toString();
    }
}
```
## DAY 29
### Largest Rectangle in Histogram
```java
class Solution {
    public int largestRectangleArea(int[] nums) {
        Stack <Integer> st = new Stack<>();
        int n = nums.length;
        int maxA = 0;
        for(int i = 0; i<=n ; i++){
            while(!st.empty() && (i==n || nums[st.peek()] >= nums[i])){
                int height = nums[st.peek()];
                st.pop();
                int width;
                if(st.empty()){
                    width = i;
                } else {
                    width = i - st.peek() - 1;
                }
                maxA = Math.max(maxA, width * height);
            }
            st.push(i);
        }
        return maxA;
    }
}

## DAY 30
### Maximal Rectangle
```java
import java.util.Stack;

class Solution {
    // Helper method to compute largest rectangle in histogram
    public int LHist(int[] arr) {
        Stack<Integer> st = new Stack<>();
        int maxA = 0;
        int n = arr.length;

        for (int i = 0; i <= n; i++) {
            while (!st.isEmpty() && (i == n || arr[st.peek()] >= (i < n ? arr[i] : 0))) {
                int height = arr[st.pop()];
                int width = st.isEmpty() ? i : i - st.peek() - 1;
                maxA = Math.max(maxA, width * height);
            }
            if (i < n) {
                st.push(i);
            }
        }

        return maxA;
    }

    public int maximalRectangle(char[][] matrix) {
        // Edge case: empty matrix
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }

        int row = matrix.length;
        int col = matrix[0].length;
        int maxA = 0;

        // Prefix sum matrix to build histogram rows
        int[][] psum = new int[row][col];

        for (int j = 0; j < col; j++) {
            int sum = 0;
            for (int i = 0; i < row; i++) {
                sum = (matrix[i][j] == '1') ? sum + 1 : 0;
                psum[i][j] = sum;
            }
        }

        // Apply histogram logic row by row
        for (int i = 0; i < row; i++) {
            maxA = Math.max(maxA, LHist(psum[i]));
        }

        return maxA;
    }
}
```
```
