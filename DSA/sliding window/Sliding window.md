
In Sliding window we make a window that we consistently expand skrink or slide based on our conditions over our array :

The main idea is to use the results of previous window to do computations for the next window.

Question 1:
### Maximum Sum of a Subarray with K Elements

### ****Sliding Window Technique - O(n) Time and O(1) Space****

- We compute the sum of the first k elements out of n terms using a linear loop and store the sum in variable ****window_sum****.
- Then we will traverse linearly over the array till it reaches the end and simultaneously keep track of the maximum sum.
- To get the current sum of a block of k elements just subtract the first element from the previous block and add the last element of the current block.


There are basically two types of sliding window:

***Fixed Sized Sliding window:***
The general steps to solve these questions by following below steps:

- Find the size of the window required, say K.
- Compute the result for 1st window, i.e. include the first K elements of the data structure.
- Then use a loop to slide the window by 1 and keep computing the result window by window.

**Variable Size Sliding Window:***

The general steps to solve these questions by following below steps:

- In this type of sliding window problem, we increase our right pointer one by one till our condition is true.
- At any step if our condition does not match, we shrink the size of our window by increasing left pointer.
- Again, when our condition satisfies, we start increasing the right pointer and follow step 1.
- We follow these steps until we reach to the end of the array.

### How to Identify Sliding Window Problems?

- These problems generally require Finding Maximum/Minimum ****Subarray****, ****Substrings**** which satisfy some specific condition.
- The size of the subarray or substring ‘k****’**** will be given in some of the problems.
- These problems can easily be solved in O(n2) time complexity using nested loops, using sliding window we can solve these in ****O(n)**** Time Complexity.
- ****Required Time Complexity:**** O(n) or O(n log n)
- ****Constraints:**** n <= 106

Complete Easy problems by today:

April 15th.

