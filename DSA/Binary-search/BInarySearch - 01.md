

In every binary search, you maintain a "search space" defined by two pointers: low (or left) and high (or right).
Midpoint calculation:
$$mid = low + \frac{high - low}{2}$$The Decision: At each step, you check the middle element and discard half of the remaining array.

### Question 1: SQRT 

Given a non-negative integer `x`, return _the square root of_ `x` _rounded down to the nearest integer_. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.
The intuition behind this one is to start from 0 to x and check at each iteration of the mid * mid == x then we have a possible ans also as we are using integers and sqrts can be decimals a number like 2 can be the ans so we will have to take care of case like  sqrt is 2.232 but we have 2 so a lesser number is always a candidate for being the ans.

Edge cases: Integer overflows occuring during mid calculation and sq calculation like mid * mid
which can be handled like the implementation below.

### Implementation

```java
class Solution {

    public int mySqrt(int x) {
        if(x < 2){
            return x;
        }
        int left = 0;
        int right = x;
        int result = 0;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (mid <= x/mid){
                result = mid;
                left = mid +1 ;
            } else {
                right = mid - 1;
            }
        }
        return result;
    }

}

```

### Question 2: Search in 2D matrix:

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.

You must write a solution in `O(log(m * n))` time complexity.

Intuition:

Approach it as a standard binary search problem in two steps first problem is to find the row in which your ans might me lying as we have this property "The first integer of each row is greater than the last integer of the previous row." we can easily find the row index then the second part is finding col which is straight forward a plain binary search. remember do not overcomplicate.

```java
class Solution {

    public boolean searchMatrix(int[][] matrix, int target) {
        int leftRow = 0;
        int rightRow = matrix.length-1;
        int targetRow = 0;
        while(leftRow <= rightRow){
            int mid = leftRow + (rightRow - leftRow)/2;
            if(matrix[mid][0] <= target ){
                targetRow = mid;
                leftRow = mid + 1;
            }else{
                rightRow = mid - 1;
            }
        }
        int leftCol = 0;
        int rightCol = matrix[0].length-1;
        while(leftCol <= rightCol){
            int midCol = leftCol + (rightCol - leftCol)/2;
            int possibleAns = matrix[targetRow][midCol];
            if(possibleAns == target){
                return true;
            }else if (possibleAns < target){
                leftCol = midCol + 1;
            }else{
                rightCol = midCol - 1;
            }
        }
        return false;
    }
}

```

Next : [[BInarySearch - 02]]