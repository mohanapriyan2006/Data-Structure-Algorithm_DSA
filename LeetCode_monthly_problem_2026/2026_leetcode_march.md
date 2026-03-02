# -----------------------------------------
# LeetCode Problems - MARCH 2026
# -----------------------------------------


# [1689. Partitioning Into Minimum Number Of Deci-Binary Numbers](https://leetcode.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers/)

Medium
 
A decimal number is called deci-binary if each of its digits is either 0 or 1 without any leading zeros. For example, 101 and 1100 are deci-binary, while 112 and 3001 are not.

Given a string n that represents a positive decimal integer, return the minimum number of positive deci-binary numbers needed so that they sum up to n.

 

Example 1:

Input: n = "32"

Output: 3

Explanation: 10 + 11 + 11 = 32



Example 2:

Input: n = "82734"

Output: 8



Example 3:

Input: n = "27346209830709182346"

Output: 9
 

Constraints:

1 <= n.length <= 105
n consists of only digits.
n does not contain any leading zeros and represents a positive integer.


# Code
```cpp []
class Solution {
public:
    int minPartitions(string n) {
        int ans = 0;
        for (char& c : n) ans = max(ans, c - '0');
        return ans;
    }
};
```

-------------------------------------------------------------------------------------------------

# [1536. Minimum Swaps to Arrange a Binary Grid](https://leetcode.com/problems/minimum-swaps-to-arrange-a-binary-grid)

Medium
 
Given an n x n binary grid, in one step you can choose two adjacent rows of the grid and swap them.

A grid is said to be valid if all the cells above the main diagonal are zeros.

Return the minimum number of steps needed to make the grid valid, or -1 if the grid cannot be valid.

The main diagonal of a grid is the diagonal that starts at cell (1, 1) and ends at cell (n, n).


 ![img](https://assets.leetcode.com/uploads/2020/07/28/fw.jpg)

Example 1:


Input: grid = [[0,0,1],[1,1,0],[1,0,0]]

Output: 3

![img](https://assets.leetcode.com/uploads/2020/07/16/e2.jpg)


Example 2:

Input: grid = [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]

Output: -1

Explanation: All rows are similar, swaps have no effect on the grid.

![IMG](https://assets.leetcode.com/uploads/2020/07/16/e3.jpg)

Example 3:

Input: grid = [[1,0,0],[1,1,0],[1,1,1]]

Output: 0
 

Constraints:

n == grid.length == grid[i].length
1 <= n <= 200
grid[i][j] is either 0 or 1





