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


# [3660. Jump Game IX](https://leetcode.com/problems/jump-game-ix/)

Medium
 
You are given an integer array nums.

From any index i, you can jump to another index j under the following rules:

Jump to index j where j > i is allowed only if nums[j] < nums[i].
Jump to index j where j < i is allowed only if nums[j] > nums[i].
For each index i, find the maximum value in nums that can be reached by following any sequence of valid jumps starting at i.

Return an array ans where ans[i] is the maximum value reachable starting from index i.


 ![img](https://assets.leetcode.com/users/images/d1b41787-c88d-490b-a611-f34fa10d4dd9_1778114546.1877158.png)

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




# Code
```cpp []
class Solution {
public:
    vector<int> maxValue(vector<int>& nums) {
        int n = nums.size();

        vector<int> pre(n), suf(n), res(n);

         pre[0] = nums[0];
        for (int i = 1; i < n; i++) {
            pre[i] = max(pre[i - 1], nums[i]);
        }

         suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            suf[i] = min(suf[i + 1], nums[i]);
        }

        res[n - 1] = pre[n - 1];

         for (int i = n - 2; i >= 0; i--) {

             if (pre[i] > suf[i + 1]) {
                res[i] = res[i + 1];
            }

             else {
                res[i] = pre[i];
            }
        }

        return res;
    }
};
```


-----------------------------------------------------------------------------------------------------------------


# [3629. Minimum Jumps to Reach End via Prime Teleportation](https://leetcode.com/problems/minimum-jumps-to-reach-end-via-prime-teleportation)

Medium
 
You are given an integer array nums of length n.

You start at index 0, and your goal is to reach index n - 1.

From any index i, you may perform one of the following operations:

Adjacent Step: Jump to index i + 1 or i - 1, if the index is within bounds.
Prime Teleportation: If nums[i] is a prime number p, you may instantly jump to any index j != i such that nums[j] % p == 0.
Return the minimum number of jumps required to reach index n - 1.

 

Example 1:

Input: nums = [1,2,4,6]

Output: 2

Explanation:

One optimal sequence of jumps is:

Start at index i = 0. Take an adjacent step to index 1.
At index i = 1, nums[1] = 2 is a prime number. Therefore, we teleport to index i = 3 as nums[3] = 6 is divisible by 2.
Thus, the answer is 2.




Example 2:

Input: nums = [2,3,4,7,9]

Output: 2

Explanation:

One optimal sequence of jumps is:

Start at index i = 0. Take an adjacent step to index i = 1.
At index i = 1, nums[1] = 3 is a prime number. Therefore, we teleport to index i = 4 since nums[4] = 9 is divisible by 3.
Thus, the answer is 2.





Example 3:

Input: nums = [4,6,5,8]

Output: 3

Explanation:

Since no teleportation is possible, we move through 0 → 1 → 2 → 3. Thus, the answer is 3.
 




Constraints:

1 <= n == nums.length <= 105
1 <= nums[i] <= 106




# Code
```cpp []
class Solution {
    static const int n = 1000005;
    static inline int prime[n];
    static inline int version = 0;
    static inline bool init = []() {
        prime[0] = -1;
        prime[1] = -1;
        for (int i = 2; i <= 1000; i++)
            if (prime[i] == 0)
                for (int j = i * i; j < n; j += i)
                    prime[j] = -1;
        return true;
    }();

public:
    int minJumps(vector<int>& nums) {
        int len = nums.size();
        int max = *max_element(nums.begin(), nums.end());

        vector<int> head(max + 1, -1);
        vector<int> next(len);

        for (int i = 0; i < len; i++) {
            next[i] = head[nums[i]];
            head[nums[i]] = i;
        }

        vector<int> dp(len, -1);
        dp[0] = 0;

        queue<int> queue;
        queue.push(0);

        version++;

        while (!queue.empty()) {
            int dq = queue.front();
            queue.pop();

            if (dq == len - 1)
                return dp[dq];

            int right = dq + 1;
            if (right < len && dp[right] == -1) {
                dp[right] = dp[dq] + 1;
                queue.push(right);
            }

            int left = dq - 1;
            if (left >= 0 && dp[left] == -1) {
                dp[left] = dp[dq] + 1;
                queue.push(left);
            }

            int val = nums[dq];
            if (prime[val] != -1 && prime[val] != version) {
                prime[val] = version;
                for (int i = val; i <= max; i += val) {
                    for (int j = head[i]; j != -1; j = next[j]) {
                        if (dp[j] == -1) {
                            dp[j] = dp[dq] + 1;
                            queue.push(j);
                        }
                    }
                    head[i] = -1;
                }
            }
        }

        return -1;
    }
};
```


-------------------------------------------------------------------------------------------


# [1914. Cyclically Rotating a Grid](https://leetcode.com/problems/cyclically-rotating-a-grid/)

Medium
 
You are given an m x n integer matrix grid​​​, where m and n are both even integers, and an integer k.

The matrix is composed of several layers, which is shown in the below image, where each color is its own layer:

![img](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid.png)

A cyclic rotation of the matrix is done by cyclically rotating each layer in the matrix. To cyclically rotate a layer once, each element in the layer will take the place of the adjacent element in the counter-clockwise direction. An example rotation is shown below:

![img](https://assets.leetcode.com/uploads/2021/06/22/explanation_grid.jpg)

Return the matrix after applying k cyclic rotations to it.

 


Example 1:

![img](https://assets.leetcode.com/uploads/2021/06/19/rod2.png)

Input: grid = [[40,10],[30,20]], k = 1

Output: [[10,20],[40,30]]

Explanation: The figures above represent the grid at every state.





Example 2:


Input: grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2

Output: [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]

Explanation: The figures above represent the grid at every state.
 
![img](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid5.png)

![img](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid6.png)

![img](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid7.png)



Constraints:

m == grid.length
n == grid[i].length
2 <= m, n <= 50
Both m and n are even integers.
1 <= grid[i][j] <= 5000
1 <= k <= 109










