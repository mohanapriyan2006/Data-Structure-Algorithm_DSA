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



# Code
```cpp []
class Solution {
public:
    int minSwaps(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> zeros(n);

        for (int i = 0; i < n; i++) {
            int count = 0;
            for (int j = n - 1; j >= 0 && grid[i][j] == 0; j--) count++;
            zeros[i] = count;
        }

        int swaps = 0;

        for (int i = 0; i < n; i++) {
            int needed = n - i - 1;
            int j = i;
            while (j < n && zeros[j] < needed) j++;
            if (j == n) return -1;
            while (j > i) {
                swap(zeros[j], zeros[j - 1]);
                j--;
                swaps++;
            }
        }

        return swaps;
    }
};
```


-----------------------------------------------------------------------------------------------------------------------


# [1545. Find Kth Bit in Nth Binary String](https://leetcode.com/problems/find-kth-bit-in-nth-binary-string)

Medium

Given two positive integers n and k, the binary string Sn is formed as follows:

S1 = "0"
Si = Si - 1 + "1" + reverse(invert(Si - 1)) for i > 1
Where + denotes the concatenation operation, reverse(x) returns the reversed string x, and invert(x) inverts all the bits in x (0 changes to 1 and 1 changes to 0).

For example, the first four strings in the above sequence are:

S1 = "0"
S2 = "011"
S3 = "0111001"
S4 = "011100110110001"
Return the kth bit in Sn. It is guaranteed that k is valid for the given n.

 

Example 1:

Input: n = 3, k = 1

Output: "0"

Explanation: S3 is "0111001".
The 1st bit is "0".



Example 2:

Input: n = 4, k = 11

Output: "1"

Explanation: S4 is "011100110110001".
The 11th bit is "1".
 

Constraints:

1 <= n <= 20
1 <= k <= 2n - 1


# Code
```cpp []
class Solution {
public:
    char findKthBit(int n, int k) {
        if (n == 1) return '0';
        
        int len = (1 << n) - 1;
        int mid = (len + 1) / 2;
        
        if (k == mid) return '1';
        if (k < mid) return findKthBit(n - 1, k);
        
        char c = findKthBit(n - 1, len - k + 1);
        return c == '0' ? '1' : '0';
    }
};
```

------------------------------------------------------------------------------------------------------


# [1582. Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix/)

Easy
 
Given an m x n binary matrix mat, return the number of special positions in mat.

A position (i, j) is called special if mat[i][j] == 1 and all other elements in row i and column j are 0 (rows and columns are 0-indexed).

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/12/23/special1.jpg)


Input: mat = [[1,0,0],[0,0,1],[1,0,0]]

Output: 1

Explanation: (1, 2) is a special position because mat[1][2] == 1 and all other elements in row 1 and column 2 are 0.



Example 2:

![img](https://assets.leetcode.com/uploads/2021/12/24/special-grid.jpg)

Input: mat = [[1,0,0],[0,1,0],[0,0,1]]

Output: 3

Explanation: (0, 0), (1, 1) and (2, 2) are special positions.
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 100
mat[i][j] is either 0 or 1.


# Code
```cpp []
class Solution {
public:
    int numSpecial(vector<vector<int>>& mat) {
        int res = 0;
        for(int i=0 ; i<mat.size(); ++i){
            int ind = checkRow(mat , i);
            if(ind >= 0 && checkCol(mat , ind , i)) res++;
        }
        return res;
    }
private:
    int checkRow(vector<vector<int>>& mat, int i){
        int ind = -1;
        for(int j=0 ; j<mat[0].size() ; ++j){
            if(mat[i][j] == 1){
                if(ind >= 0) return -1;
                else ind = j;
            }
        }
        return ind;
    }

    bool checkCol(vector<vector<int>>& mat,int ind , int j){
        
        for(int i=0 ; i<mat.size() ; ++i){
            if(mat[i][ind] == 1 && i != j) return false;
        }
        return true;
    }
};
```

---------------------------------------------------------------------------------------------------------------------


# 1758. Minimum Changes To Make Alternating Binary String

Easy
 
You are given a string s consisting only of the characters '0' and '1'. In one operation, you can change any '0' to '1' or vice versa.

The string is called alternating if no two adjacent characters are equal. For example, the string "010" is alternating, while the string "0100" is not.

Return the minimum number of operations needed to make s alternating.

 

Example 1:

Input: s = "0100"

Output: 1

Explanation: If you change the last character to '1', s will be "0101", which is alternating.




Example 2:

Input: s = "10"

Output: 0

Explanation: s is already alternating.



Example 3:

Input: s = "1111"

Output: 2

Explanation: You need two operations to reach "0101" or "1010".
 

Constraints:

1 <= s.length <= 104
s[i] is either '0' or '1'.




