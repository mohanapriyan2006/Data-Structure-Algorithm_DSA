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



# Code
```cpp []
class Solution {
public:
    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) {
        int T = 0, L = 0;
        int B = grid.size() - 1, R = grid[0].size() - 1;

        while (T < B && L < R) {
            int len = B - T, wid = R - L;
            int perimeter = 2 * len + 2 * wid;
            int r = k % perimeter;

            while (r--) {
                int tmp = grid[T][L];

                for (int i = L; i < R; i++)
                    grid[T][i] = grid[T][i + 1];

                for (int i = T; i < B; i++)
                    grid[i][R] = grid[i + 1][R];

                for (int i = R; i > L; i--)
                    grid[B][i] = grid[B][i - 1];

                for (int i = B; i > T; i--)
                    grid[i][L] = grid[i - 1][L];

                grid[T + 1][L] = tmp;
            }

            T++; L++;
            B--; R--;
        }

        return grid;
    }
};
```


---------------------------------------------------------------------------------------------------------

# [2770. Maximum Number of Jumps to Reach the Last Index](https://leetcode.com/problems/maximum-number-of-jumps-to-reach-the-last-index/)

Medium
 
You are given a 0-indexed array nums of n integers and an integer target.

You are initially positioned at index 0. In one step, you can jump from index i to any index j such that:

0 <= i < j < n
-target <= nums[j] - nums[i] <= target
Return the maximum number of jumps you can make to reach index n - 1.

If there is no way to reach index n - 1, return -1.

 



Example 1:

Input: nums = [1,3,6,4,1,2], target = 2

Output: 3

Explanation: To go from index 0 to index n - 1 with the maximum number of jumps, you can perform the following jumping sequence:
- Jump from index 0 to index 1. 
- Jump from index 1 to index 3.
- Jump from index 3 to index 5.
It can be proven that there is no other jumping sequence that goes from 0 to n - 1 with more than 3 jumps. Hence, the answer is 3. 




Example 2:

Input: nums = [1,3,6,4,1,2], target = 3

Output: 5

Explanation: To go from index 0 to index n - 1 with the maximum number of jumps, you can perform the following jumping 
sequence:
- Jump from index 0 to index 1.
- Jump from index 1 to index 2.
- Jump from index 2 to index 3.
- Jump from index 3 to index 4.
- Jump from index 4 to index 5.
It can be proven that there is no other jumping sequence that goes from 0 to n - 1 with more than 5 jumps. Hence, the answer is 5. 



Example 3:

Input: nums = [1,3,6,4,1,2], target = 0

Output: -1

Explanation: It can be proven that there is no jumping sequence that goes from 0 to n - 1. Hence, the answer is -1. 
 

Constraints:

2 <= nums.length == n <= 1000
-109 <= nums[i] <= 109
0 <= target <= 2 * 109



# Code
```cpp []
class Solution {
public:
    int maximumJumps(vector<int>& nums, int target) {
        int n = nums.size();
        vector<int> dp(n, -1);

        dp[0]=0;

        for(int i=1; i<n; i++) {
            for(int j=0; j<i; j++) {
                if(abs(nums[i]-nums[j]) <= target && dp[j]>-1) {
                    dp[i] = max(dp[i], 1+dp[j]);
                }
            }
        }

        return dp[n-1];
    }
};
```



--------------------------------------------------------------------------------------------------------------

# [2553. Separate the Digits in an Array](https://leetcode.com/problems/separate-the-digits-in-an-array/description)

Easy
 
Given an array of positive integers nums, return an array answer that consists of the digits of each integer in nums after separating them in the same order they appear in nums.

To separate the digits of an integer is to get all the digits it has in the same order.

For example, for the integer 10921, the separation of its digits is [1,0,9,2,1].
 




Example 1:

Input: nums = [13,25,83,77]

Output: [1,3,2,5,8,3,7,7]

Explanation: 
- The separation of 13 is [1,3].
- The separation of 25 is [2,5].
- The separation of 83 is [8,3].
- The separation of 77 is [7,7].
answer = [1,3,2,5,8,3,7,7]. Note that answer contains the separations in the same order.




Example 2:

Input: nums = [7,1,3,9]

Output: [7,1,3,9]

Explanation: The separation of each integer in nums is itself.
answer = [7,1,3,9].
 



Constraints:

1 <= nums.length <= 1000
1 <= nums[i] <= 105



# Code
```cpp []
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        
        vector<int> result;

        for (int num : nums) {

            string s = to_string(num);

            for (char ch : s) {

                result.push_back(ch - '0');
            }
        }

        return result;
    }
};
```


------------------------------------------------------------------------------------------------


# [1665. Minimum Initial Energy to Finish Tasks](https://leetcode.com/problems/minimum-initial-energy-to-finish-tasks)

Hard
 
You are given an array tasks where tasks[i] = [actuali, minimumi]:

actuali is the actual amount of energy you spend to finish the ith task.
minimumi is the minimum amount of energy you require to begin the ith task.
For example, if the task is [10, 12] and your current energy is 11, you cannot start this task. However, if your current energy is 13, you can complete this task, and your energy will be 3 after finishing it.

You can finish the tasks in any order you like.

Return the minimum initial amount of energy you will need to finish all the tasks.

 


Example 1:

Input: tasks = [[1,2],[2,4],[4,8]]

Output: 8

Explanation:

Starting with 8 energy, we finish the tasks in the following order:
    - 3rd task. Now energy = 8 - 4 = 4.
    - 2nd task. Now energy = 4 - 2 = 2.
    - 1st task. Now energy = 2 - 1 = 1.
Notice that even though we have leftover energy, starting with 7 energy does not work because we cannot do the 3rd task.




Example 2:

Input: tasks = [[1,3],[2,4],[10,11],[10,12],[8,9]]

Output: 32

Explanation:

Starting with 32 energy, we finish the tasks in the following order:
    - 1st task. Now energy = 32 - 1 = 31.
    - 2nd task. Now energy = 31 - 2 = 29.
    - 3rd task. Now energy = 29 - 10 = 19.
    - 4th task. Now energy = 19 - 10 = 9.
    - 5th task. Now energy = 9 - 8 = 1.




Example 3:

Input: tasks = [[1,7],[2,8],[3,9],[4,10],[5,11],[6,12]]

Output: 27

Explanation:
Starting with 27 energy, we finish the tasks in the following order:
    - 5th task. Now energy = 27 - 5 = 22.
    - 2nd task. Now energy = 22 - 2 = 20.
    - 3rd task. Now energy = 20 - 3 = 17.
    - 1st task. Now energy = 17 - 1 = 16.
    - 4th task. Now energy = 16 - 4 = 12.
    - 6th task. Now energy = 12 - 6 = 6.


 

Constraints:

1 <= tasks.length <= 105
1 <= actual​i <= minimumi <= 104


# Code
```cpp []
class Solution {
public:
    int minimumEffort(vector<vector<int>>& shop) {
        sort(shop.begin(), shop.end(), [&](vector<int>& a, vector<int>& b) {
            return a[1] - a[0] > b[1] - b[0];
        });

        int start = shop[0][1];
        int bal = shop[0][1] - shop[0][0];
        int loan = 0;

        for (int i = 1; i < shop.size(); i++) {
            int cost = shop[i][0];
            int thresh = shop[i][1];

            if (bal < thresh) {
                loan += thresh - bal;
                bal = thresh;
            }

            bal -= cost;
        }

        return start + loan;
    }
};
```


-------------------------------------------------------------------------------------------------

# [1674. Minimum Moves to Make Array Complementary](https://leetcode.com/problems/minimum-moves-to-make-array-complementary)

Medium
 
You are given an integer array nums of even length n and an integer limit. In one move, you can replace any integer from nums with another integer between 1 and limit, inclusive.

The array nums is complementary if for all indices i (0-indexed), nums[i] + nums[n - 1 - i] equals the same number. For example, the array [1,2,3,4] is complementary because for all indices i, nums[i] + nums[n - 1 - i] = 5.

Return the minimum number of moves required to make nums complementary.

 



Example 1:


Input: nums = [1,2,4,3], limit = 4

Output: 1

Explanation: In 1 move, you can change nums to [1,2,2,3] (underlined elements are changed).
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
Therefore, nums[i] + nums[n-1-i] = 4 for every i, so nums is complementary.



Example 2:

Input: nums = [1,2,2,1], limit = 2

Output: 2

Explanation: In 2 moves, you can change nums to [2,2,2,2]. You cannot change any number to 3 since 3 > limit.




Example 3:

Input: nums = [1,2,1,2], limit = 2

Output: 0

Explanation: nums is already complementary.
 



Constraints:

n == nums.length
2 <= n <= 105
1 <= nums[i] <= limit <= 105
n is even.




# Code
```cpp []
class Solution {
public:
    int minMoves(vector<int>& nums, int limit) {
        int n = nums.size();
        vector<int> delta((limit << 1) + 2, 0);

        for (int i = 0; i < n >> 1; i++) {
            int min = nums[i];
            int max = nums[n - 1 - i];
            if (min > max) swap(min, max);

            delta[2] += 2;
            delta[min + 1]--;
            delta[min + max]--;
            delta[min + max + 1]++;
            delta[max + limit + 1]++;
        }

        int res = n, moves = 0;

        for (int targ = 2; targ <= limit * 2; targ++) {
            moves += delta[targ];
            res = min(res, moves);
        }

        return res;
    }
};
```

--------------------------------------------------------------------------------

#  [2784. Check if Array is Good](https://leetcode.com/problems/check-if-array-is-good)

Easy
 
You are given an integer array nums. We consider an array good if it is a permutation of an array base[n].

base[n] = [1, 2, ..., n - 1, n, n] (in other words, it is an array of length n + 1 which contains 1 to n - 1 exactly once, plus two occurrences of n). For example, base[1] = [1, 1] and base[3] = [1, 2, 3, 3].

Return true if the given array is good, otherwise return false.

Note: A permutation of integers represents an arrangement of these numbers.

 
![img](https://assets.leetcode.com/users/images/40430b82-20ef-4073-8e5c-29bbb27b17b1_1778720447.3829114.png)


Example 1:

Input: nums = [2, 1, 3]

Output: false

Explanation: Since the maximum element of the array is 3, the only candidate n for which this array could be a permutation of base[n], is n = 3. However, base[3] has four elements but array nums has three. Therefore, it can not be a permutation of base[3] = [1, 2, 3, 3]. So the answer is false.





Example 2:

Input: nums = [1, 3, 3, 2]

Output: true

Explanation: Since the maximum element of the array is 3, the only candidate n for which this array could be a permutation 
of base[n], is n = 3. It can be seen that nums is a permutation of base[3] = [1, 2, 3, 3] (by swapping the second and fourth elements in nums, we reach base[3]). Therefore, the answer is true.




Example 3:

Input: nums = [1, 1]

Output: true

Explanation: Since the maximum element of the array is 1, the only candidate n for which this array could be a permutation of base[n], is n = 1. It can be seen that nums is a permutation of base[1] = [1, 1]. Therefore, the answer is true.




Example 4:

Input: nums = [3, 4, 4, 1, 2, 1]

Output: false

Explanation: Since the maximum element of the array is 4, the only candidate n for which this array could be a permutation of base[n], is n = 4. However, base[4] has five elements but array nums has six. Therefore, it can not be a permutation of base[4] = [1, 2, 3, 4, 4]. So the answer is false.
 


Constraints:

1 <= nums.length <= 100
1 <= num[i] <= 200



# Code
```cpp []
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int n = nums.size() - 1;
        bitset<201> seen;
        bool dup = 0;

        for (auto& num : nums) {
            if (num > n) return false;

            if (seen.test(num)) {
                if (num < n || dup) return false;
                dup |= 1;
                continue;
            }

            seen.set(num);
        }

        return true;
    }
};
```

---------------------------------------------------------------------------------------------------------------------


# [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description)
 
Medium
 
Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.


 

Example 1:


Input: nums = [3,4,5,1,2]

Output: 1

Explanation: The original array was [1,2,3,4,5] rotated 3 times.



Example 2:

Input: nums = [4,5,6,7,0,1,2]

Output: 0

Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.




Example 3:

Input: nums = [11,13,15,17]

Output: 11

Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
 


Constraints:

n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
All the integers of nums are unique.
nums is sorted and rotated between 1 and n times.


# Code
```cpp []
class Solution {
public:
    int findMin(vector<int>& nums) {

        int l = 0;
        int h = nums.size() - 1;

        while(l<h){
            int mid = l + (h-l)/2;
            if(nums[mid] > nums[h]){
                l = mid + 1;
            }else{
                h = mid;
            }
        }
      
        return nums[l];
    }
};
```

-------------------------------------------------------------------------------------------------------------

# [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
 
Hard
 
Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:

[4,5,6,7,0,1,4] if it was rotated 4 times.
[0,1,4,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.

You must decrease the overall operation steps as much as possible.

 



Example 1:

Input: nums = [1,3,5]

Output: 1



Example 2:

Input: nums = [2,2,2,0,1]

Output: 0
 


Constraints:

n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
nums is sorted and rotated between 1 and n times.




# Code
```cpp []
class Solution {
public:
    int findMin(vector<int>& nums) { return dnc(0, nums.size() - 1, nums); }

    int dnc(int left, int right, vector<int>& nums) {
        if (left == right)
            return nums[left];

        if (nums[left] < nums[right])
            return nums[left];

        int m = (left + right) >> 1;

        return min(dnc(left, m, nums), dnc(m + 1, right, nums));
    }
};
```

----------------------------------------------------------------------------------------------------------
 





