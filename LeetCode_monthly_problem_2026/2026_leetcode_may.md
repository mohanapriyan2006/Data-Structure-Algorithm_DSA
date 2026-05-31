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


 ![img](https://assets.leetcode.com/users/images/1eebda8a-86c5-439f-9bf6-4ddf3dc36489_1778890239.7109902.png)

 <br/>

 ![img](https://assets.leetcode.com/users/images/14356d5c-5dd0-43eb-af30-e4cd8744ca1e_1778894140.5373573.gif)



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
 
# [1306. Jump Game III](https://leetcode.com/problems/jump-game-iii/)

Medium
 
Given an array of non-negative integers arr, you are initially positioned at start index of the array. When you are at index i, you can jump to i + arr[i] or i - arr[i], check if you can reach any index with value 0.

Notice that you can not jump outside of the array at any time.

 ![img]()


Example 1:

Input: arr = [4,2,3,0,3,1,2], start = 5

Output: true

Explanation: 
All possible ways to reach at index 3 with value 0 are: 
index 5 -> index 4 -> index 1 -> index 3 
index 5 -> index 6 -> index 4 -> index 1 -> index 3 




Example 2:

Input: arr = [4,2,3,0,3,1,2], start = 0

Output: true 

Explanation: 

One possible way to reach at index 3 with value 0 is: 
index 0 -> index 4 -> index 1 -> index 3



Example 3:

Input: arr = [3,0,2,1,2], start = 2

Output: false

Explanation: There is no way to reach at index 1 with value 0.


 

Constraints:

1 <= arr.length <= 5 * 104
0 <= arr[i] < arr.length
0 <= start < arr.length


# Code
```cpp []
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        int n = arr.size();

        vector<bool> visited(n, false);
        queue<int> q;

        q.push(start);

        while (!q.empty()) {
            int i = q.front();
            q.pop();

            if (i < 0 || i >= n || visited[i])
                continue;

            if (arr[i] == 0)
                return true;

            visited[i] = true;

            q.push(i + arr[i]);
            q.push(i - arr[i]);
        }

        return false;
    }
};
```

------------------------------------------------------------------------------------------------------------


# [1345. Jump Game IV](https://leetcode.com/problems/jump-game-iv/description)
 
Hard
 
Given an array of integers arr, you are initially positioned at the first index of the array.

In one step you can jump from index i to index:

i + 1 where: i + 1 < arr.length.
i - 1 where: i - 1 >= 0.
j where: arr[i] == arr[j] and i != j.
Return the minimum number of steps to reach the last index of the array.

Notice that you can not jump outside of the array at any time.

 


Example 1:

Input: arr = [100,-23,-23,404,100,23,23,23,3,404]

Output: 3

Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.



Example 2:

Input: arr = [7]

Output: 0

Explanation: Start index is the last index. You do not need to jump.



Example 3:

Input: arr = [7,6,9,6,9,6,9,7]

Output: 1

Explanation: You can jump directly from index 0 to index 7 which is last index of the array.
 


Constraints:

1 <= arr.length <= 5 * 104
-108 <= arr[i] <= 108



# Code
```cpp []
class Solution {
public:
    int minJumps(vector<int>& arr) {
        int n=arr.size();
        if(n==1)return 0;
        unordered_map<int,vector<int>>mp;
        for (int i=0;i<n;i++){
            mp[arr[i]].push_back(i);
        }
        queue<pair<int,int>>q;
        q.push({0,0});
        vector<int>vis(n,0);
        vis[0]=1;
        while(!q.empty()){
            int node=q.front().first;
            int dist=q.front().second;
            q.pop();
            if(node==n-1)return dist;
            if(node-1>=0 && !vis[node-1]){
                vis[node-1]=1;
                q.push({node-1,dist+1});
            }
            if(node+1<n && !vis[node+1]){
                vis[node+1]=1;
                q.push({node+1,dist+1});
            }
            for (int next:mp[arr[node]]){
                if (!vis[next]){
                    vis[next]=1;
                    q.push({next,dist+1});
                }
            }
            mp[arr[node]].clear();
        }
        return -1;
    }
};
```


----------------------------------------------------------------------------

# [2540. Minimum Common Value](https://leetcode.com/problems/minimum-common-value/)

Easy
 
Given two integer arrays nums1 and nums2, sorted in non-decreasing order, return the minimum integer common to both arrays. If there is no common integer amongst nums1 and nums2, return -1.

Note that an integer is said to be common to nums1 and nums2 if both arrays have at least one occurrence of that integer.

 

Example 1:

Input: nums1 = [1,2,3], nums2 = [2,4]


Output: 2

Explanation: The smallest element common to both arrays is 2, so we return 2.



Example 2:

Input: nums1 = [1,2,3,6], nums2 = [2,3,4,5]

Output: 2

Explanation: There are two common elements in the array 2 and 3 out of which 2 is the smallest, so 2 is returned.
 


Constraints:

1 <= nums1.length, nums2.length <= 105
1 <= nums1[i], nums2[j] <= 109
Both nums1 and nums2 are sorted in non-decreasing order.



# Code
```cpp []
class Solution {
public:
    static int getCommon(vector<int>& nums1, vector<int>& nums2) {
        int n1=nums1.size(), n2=nums2.size();
        if (n1>n2){
            swap(nums1, nums2);
            swap(n1, n2);
        }
        int prev=0;
        for(int i=0; i<n1; i++){
            int x=nums1[i];
            int j=lower_bound(nums2.begin()+prev, nums2.end(), x)-nums2.begin();
            if (j==n2) return -1;
            else if (x==nums2[j]) return x;
            else// x<nums2[j]
                prev=j;
        }
        return -1;
    }
};
```


----------------------------------------------------------------------------------


# [2657. Find the Prefix Common Array of Two Arrays](https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays/)

Medium
 
You are given two 0-indexed integer permutations A and B of length n.

A prefix common array of A and B is an array C such that C[i] is equal to the count of numbers that are present at or before the index i in both A and B.

Return the prefix common array of A and B.

A sequence of n integers is called a permutation if it contains all integers from 1 to n exactly once.

![img](https://assets.leetcode.com/users/images/e6e5d85c-b631-4430-8b3e-ac04417742e6_1779238436.586387.gif)



Example 1:

Input: A = [1,3,2,4], B = [3,1,2,4]

Output: [0,2,3,4]

Explanation: At i = 0: no number is common, so C[0] = 0.
At i = 1: 1 and 3 are common in A and B, so C[1] = 2.
At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3.
At i = 3: 1, 2, 3, and 4 are common in A and B, so C[3] = 4.




Example 2:

Input: A = [2,3,1], B = [3,1,2]

Output: [0,1,3]

Explanation: At i = 0: no number is common, so C[0] = 0.
At i = 1: only 3 is common in A and B, so C[1] = 1.
At i = 2: 1, 2, and 3 are common in A and B, so C[2] = 3.
 


Constraints:

1 <= A.length == B.length == n <= 50
1 <= A[i], B[i] <= n
It is guaranteed that A and B are both a permutation of n integers.



# Code
```cpp []
class Solution {
public:
    vector<int> findThePrefixCommonArray(vector<int>& A, vector<int>& B) {
        int n = A.size(), count = 0;
        vector<int> res(n);
        bitset<51> seen;

        for (int i = 0; i < n; i++) {
            count += seen[A[i]];
            seen.set(A[i]);

            count += seen[B[i]];
            seen.set(B[i]);

            res[i] = count;
        }

        return res;
    }
};

```


--------------------------------------------------------------------------------------------------------------


# [3043. Find the Length of the Longest Common Prefix](https://leetcode.com/problems/find-the-length-of-the-longest-common-prefix/)

Medium
 
You are given two arrays with positive integers arr1 and arr2.

A prefix of a positive integer is an integer formed by one or more of its digits, starting from its leftmost digit. For example, 123 is a prefix of the integer 12345, while 234 is not.

A common prefix of two integers a and b is an integer c, such that c is a prefix of both a and b. For example, 5655359 and 56554 have common prefixes 565 and 5655 while 1223 and 43456 do not have a common prefix.

You need to find the length of the longest common prefix between all pairs of integers (x, y) such that x belongs to arr1 and y belongs to arr2.

Return the length of the longest common prefix among all pairs. If no common prefix exists among them, return 0.

 



Example 1:

Input: arr1 = [1,10,100], arr2 = [1000]

Output: 3

Explanation: There are 3 pairs (arr1[i], arr2[j]):
- The longest common prefix of (1, 1000) is 1.
- The longest common prefix of (10, 1000) is 10.
- The longest common prefix of (100, 1000) is 100.
The longest common prefix is 100 with a length of 3.



Example 2:

Input: arr1 = [1,2,3], arr2 = [4,4,4]

Output: 0

Explanation: There exists no common prefix for any pair (arr1[i], arr2[j]), hence we return 0.
Note that common prefixes between elements of the same array do not count.
 


Constraints:

1 <= arr1.length, arr2.length <= 5 * 104
1 <= arr1[i], arr2[i] <= 108


# Code
```cpp []
class Solution {
public:
    struct Node {
        Node* child[10];
        bool isEnd;
        Node(){
            for(int i=0;i<10;i++)child[i]=nullptr;
            isEnd=false;
        }
    };
    void insert(string word, Node* root){
        Node* temp=root;
        for(char ch:word){
            int idx=ch-'0';
            if (temp->child[idx] == nullptr){
                temp->child[idx] = new Node();
            }
            temp=temp->child[idx];
        }
        temp->isEnd=true;
    }

    int check(string str, Node* root) {
        Node* temp = root;
        int idx = 0;
        while (idx < str.size()) {
            int i = str[idx] - '0';
            if (temp->child[i] != nullptr) {
                temp = temp->child[i];
                idx++;
            } else {
                break;
            }
        }
        return idx;
    }
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) {
        Node* root = new Node();
        for(int x:arr2){
            string str=to_string(x);
            insert(str,root);
        }
        int ans=0;
        for(int x:arr1){
            string str=to_string(x);
            ans=max(ans,check(str, root));
        }
        return ans;
    }
};
```

------------------------------------------------------------------------------------------------------------

# [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description)

Medium
 
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly left rotated at an unknown index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be left rotated by 3 indices and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0

Output: 4



Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3

Output: -1



Example 3:

Input: nums = [1], target = 0

Output: -1


![img](https://assets.leetcode.com/users/images/5b34e34f-3584-48f3-a6b4-b13e37ec54c6_1779409083.0322373.png)

![img](https://assets.leetcode.com/users/images/1f87da75-ecf4-48da-b142-d4fd928009fb_1778801700.4621232.png)

![img](https://assets.leetcode.com/users/images/89f87aad-5b0f-46f1-819a-a6f018a72495_1778800100.8111465.png)

![img](https://assets.leetcode.com/users/images/5e6e8b81-c931-44a5-b0ec-090b57ed45ad_1779408960.2074807.png)
 


Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104



# Code
```cpp []
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int n = nums.size();
        int l = 0 , h = n - 1;

        while(l<=h){
            int m = (l+h) / 2;
            if(nums[m] == target) return m;

            if(nums[l] <= nums[m]){
                if(nums[l] <= target && target <= nums[m]){
                    h = m - 1;
                }
                else{
                    l = m + 1;
                }
            }
            else{
                if(nums[m] <= target && target <= nums[h]){
                    l = m + 1;
                }
                else{
                    h = m - 1;
                }
            }
        }

        return -1;
    }
};
```

--------------------------------------------------------------------------------------------------

# [1752. Check if Array Is Sorted and Rotated](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/)
 
Easy
 
Given an array nums, return true if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return false.

There may be duplicates in the original array.

Note: An array A rotated by x positions results in an array B of the same length such that B[i] == A[(i+x) % A.length] for every valid index i.

![img](https://assets.leetcode.com/users/images/4f4c4dff-f5af-4eb8-8257-0495e564b2c4_1779491334.3749745.png)

![img](https://assets.leetcode.com/users/images/93e741d7-34bb-4163-a6dd-16c9b9aa9983_1779493000.6066852.png)
 

Example 1:

Input: nums = [3,4,5,1,2]

Output: true

Explanation: [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 2 positions to begin on the element of value 3: [3,4,5,1,2].




Example 2:

Input: nums = [2,1,3,4]

Output: false

Explanation: There is no sorted array once rotated that can make nums.




Example 3:

Input: nums = [1,2,3]

Output: true

Explanation: [1,2,3] is the original sorted array.
You can rotate the array by x = 0 positions (i.e. no rotation) to make nums.
 



Constraints:

1 <= nums.length <= 100
1 <= nums[i] <= 100



# Code
```cpp []
class Solution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int r = 0;
        for(int i=0 ; i<n ; ++i){
            if(nums[i] > nums[(i+1) % n] && ++r > 1) return false;
        }
        return true;
    }
};
```

-----------------------------------------------------------------------------------------------------------------


# [2126. Destroying Asteroids](https://leetcode.com/problems/destroying-asteroids/description/)
 
Medium
 
You are given an integer mass, which represents the original mass of a planet. You are further given an integer array asteroids, where asteroids[i] is the mass of the ith asteroid.

You can arrange for the planet to collide with the asteroids in any arbitrary order. If the mass of the planet is greater than or equal to the mass of the asteroid, the asteroid is destroyed and the planet gains the mass of the asteroid. Otherwise, the planet is destroyed.

Return true if all asteroids can be destroyed. Otherwise, return false.

 

Example 1:


Input: mass = 10, asteroids = [3,9,19,5,21]

Output: true

Explanation: One way to order the asteroids is [9,19,5,3,21]:
- The planet collides with the asteroid with a mass of 9. New planet mass: 10 + 9 = 19
- The planet collides with the asteroid with a mass of 19. New planet mass: 19 + 19 = 38
- The planet collides with the asteroid with a mass of 5. New planet mass: 38 + 5 = 43
- The planet collides with the asteroid with a mass of 3. New planet mass: 43 + 3 = 46
- The planet collides with the asteroid with a mass of 21. New planet mass: 46 + 21 = 67
All asteroids are destroyed.




Example 2:


Input: mass = 5, asteroids = [4,9,23,4]

Output: false

Explanation: 
The planet cannot ever gain enough mass to destroy the asteroid with a mass of 23.
After the planet destroys the other asteroids, it will have a mass of 5 + 4 + 9 + 4 = 22.
This is less than 23, so a collision would not destroy the last asteroid.
 


Constraints:

1 <= mass <= 105
1 <= asteroids.length <= 105
1 <= asteroids[i] <= 105



# Code
```cpp []
unsigned freq[100001]={0};
class Solution {
public:
    static bool asteroidsDestroyed(int mass, vector<int>& asteroids) {
        unsigned xmax=0, xmin=1e5;
        for(unsigned x: asteroids){
            freq[x]++;
            xmin=min(xmin, x);
            xmax=max(xmax, x);
        }
        long long planet=mass;// careful for overflow
        for(int x=xmin; x<=xmax; x++){
            if (freq[x]==0) continue;
            if (x>planet) {
                memset(freq+x, 0, (xmax-x+1)*sizeof(unsigned));
                return 0;
            }
            planet+=(long long)x*freq[x];
            freq[x]=0;
        }
        return 1;
    }
};
auto init = []() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    return 'c';
}();
```

----------------------------------------------------------------------------------------------------------------






