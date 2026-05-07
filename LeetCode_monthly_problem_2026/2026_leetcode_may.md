# --------------------------------------------

# LeetCode Problems  - May 2026

# --------------------------------------------

# [788. Rotated Digits](https://leetcode.com/problems/rotated-digits/description)

Medium
 
An integer x is a good if after rotating each digit individually by 180 degrees, we get a valid number that is different from x. Each digit must be rotated - we cannot choose to leave it alone.

A number is valid if each digit remains a digit after rotation. For example:

0, 1, and 8 rotate to themselves,
2 and 5 rotate to each other (in this case they are rotated in a different direction, in other words, 2 or 5 gets mirrored),
6 and 9 rotate to each other, and
the rest of the numbers do not rotate to any other number and become invalid.
Given an integer n, return the number of good integers in the range [1, n].

 

Example 1:


Input: n = 10

Output: 4

Explanation: There are four good numbers in the range [1, 10] : 2, 5, 6, 9.

Note that 1 and 10 are not good numbers, since they remain unchanged after rotating.



Example 2:

Input: n = 1

Output: 0




Example 3:

Input: n = 2

Output: 1
 




Constraints:

1 <= n <= 104



# Code
```cpp []
class Solution {
public:
    int rotatedDigits(int n) {
        vector<int> dp(n + 1, 0);
        int count = 0;

        for(int i = 0; i <= n; i++){
            if(i < 10){
                if(i == 0 || i == 1 || i == 8) dp[i] = 1;
                else if(i == 2 || i == 5 || i == 6 || i == 9){
                    dp[i] = 2;
                    count++;
                }
                else dp[i] = 0;
            }
            else{
                int a = dp[i / 10];
                int b = dp[i % 10];

                if(a == 1 && b == 1) dp[i] = 1;
                else if(a >= 1 && b >= 1){
                    dp[i] = 2;
                    count++;
                }
                else dp[i] = 0;
            }
        }

        return count;
    }
};
```

---------------------------------------------------------------------------------------------------

# [796. Rotate String](https://leetcode.com/problems/rotate-string/description/)
 
Easy
 
Given two strings s and goal, return true if and only if s can become goal after some number of shifts on s.

A shift on s consists of moving the leftmost character of s to the rightmost position.

For example, if s = "abcde", then it will be "bcdea" after one shift.
 


Example 1:

Input: s = "abcde", goal = "cdeab"

Output: true




Example 2:

Input: s = "abcde", goal = "abced"

Output: false

 

Constraints:

1 <= s.length, goal.length <= 100
s and goal consist of lowercase English letters.




# Code
```cpp []
class Solution {
public:
    bool rotateString(string s, string goal) {
        
        if(s.size() != goal.size()) return false;

        return ( (s + s).find(goal) < s.size() );
    }
};
```

------------------------------------------------------------------------------------


# [48. Rotate Image](https://leetcode.com/problems/rotate-image/description)
 
Medium
 
You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]

Output: [[7,4,1],[8,5,2],[9,6,3]]




Example 2:

![img](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]

Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
 

Constraints:

n == matrix.length == matrix[i].length
1 <= n <= 20
-1000 <= matrix[i][j] <= 1000



# Code
```cpp []
class Solution {
public:
    void rotate(vector<vector<int>>& mat) {
        int n = mat.size(), k = n - 1;
        for (int i = 0; i < n >> 1; i++)
            for (int j = i; j < k - i; j++) {
                int t = mat[i][j];
                mat[i][j] = mat[k - j][i];
                mat[k - j][i] = mat[k - i][k - j];
                mat[k - i][k - j] = mat[j][k - i];
                mat[j][k - i] = t;
            }
    }
};
```

-------------------------------------------------------------------------------------------------------------------



# 1861. Rotating the Box

Medium
 
You are given an m x n matrix of characters boxGrid representing a side-view of a box. Each cell of the box is one of the following:

A stone '#'
A stationary obstacle '*'
Empty '.'
The box is rotated 90 degrees clockwise, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity does not affect the obstacles' positions, and the inertia from the box's rotation does not affect the stones' horizontal positions.

It is guaranteed that each stone in boxGrid rests on an obstacle, another stone, or the bottom of the box.

Return an n x m matrix representing the box after the rotation described above.

 

Example 1:


![img](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

Input: boxGrid = [["#",".","#"]]

Output: [["."],
         ["#"],
         ["#"]]




Example 2:

![img](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

Input: boxGrid = [["#",".","*","."],
              ["#","#","*","."]]

Output: [["#","."],
         ["#","#"],
         ["*","*"],
         [".","."]]




Example 3:

![img](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)


Input: boxGrid = [["#","#","*",".","*","."],
              ["#","#","#","*",".","."],
              ["#","#","#",".","#","."]]


Output: [[".","#","#"],
         [".","#","#"],
         ["#","#","*"],
         ["#","*","."],
         ["#",".","*"],
         ["#",".","."]]
 



Constraints:

m == boxGrid.length
n == boxGrid[i].length
1 <= m, n <= 500
boxGrid[i][j] is either '#', '*', or '.'.



# Code
```cpp []
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& grid) {
        int rows = grid.size(), cols = grid[0].size();
        for (int r = 0; r < rows; r++) {
            int p = 0;
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == '.') {
                    swap(grid[r][c], grid[r][p]);
                    p++;
                } else if (grid[r][c] == '*')
                    p = c + 1;
            }
        }
        
        vector<vector<char>> res(cols, vector<char>(rows));
        for (int r = 0; r < rows; r++)
            for (int c = 0; c < cols; c++)
                res[c][rows - 1 - r] = grid[r][c];
                
        return res;
    }
};
```


-----------------------------------------------------------------------------------------------------------


# 3660. Jump Game IX

Medium
 
You are given an integer array nums.

From any index i, you can jump to another index j under the following rules:

Jump to index j where j > i is allowed only if nums[j] < nums[i].
Jump to index j where j < i is allowed only if nums[j] > nums[i].
For each index i, find the maximum value in nums that can be reached by following any sequence of valid jumps starting at i.

Return an array ans where ans[i] is the maximum value reachable starting from index i.


 

Example 1:

Input: nums = [2,1,3]

Output: [2,2,3]

Explanation:

For i = 0: No jump increases the value.
For i = 1: Jump to j = 0 as nums[j] = 2 is greater than nums[i].
For i = 2: Since nums[2] = 3 is the maximum value in nums, no jump increases the value.
Thus, ans = [2, 2, 3].



Example 2:

Input: nums = [2,3,1]

Output: [3,3,3]

Explanation:

For i = 0: Jump forward to j = 2 as nums[j] = 1 is less than nums[i] = 2, then from i = 2 jump to j = 1 as nums[j] = 3 is greater than nums[2].
For i = 1: Since nums[1] = 3 is the maximum value in nums, no jump increases the value.
For i = 2: Jump to j = 1 as nums[j] = 3 is greater than nums[2] = 1.
Thus, ans = [3, 3, 3].

 


Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109











