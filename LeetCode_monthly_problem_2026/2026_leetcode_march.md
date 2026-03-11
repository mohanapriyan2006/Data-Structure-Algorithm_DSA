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


# [1758. Minimum Changes To Make Alternating Binary String](https://leetcode.com/problems/minimum-changes-to-make-alternating-binary-string/)

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



# Code
```cpp []
class Solution {
public:
    int minOperations(string s) {
        char c_0 = s[0];
        int count1 = count(s, c_0);
        int count2 = count(s, c_0 == '0' ? '1' : '0') + 1;
        return min(count1, count2);
    }

private:
    int count(string s, char c_pre) {
        int count = 0;
        for (int i = 1; i < s.length(); i++) {
            char current = s[i];
            if (current == c_pre) {
                count++;
                c_pre = c_pre == '0' ? '1' : '0';
            } else {
                c_pre = current;
            }
        }
        return count;
    }
};
```

-----------------------------------------------------------------------------------------------------------------

# [1784. Check if Binary String Has at Most One Segment of Ones](https://leetcode.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)
 
Easy

Given a binary string s ​​​​​without leading zeros, return true​​​ if s contains at most one contiguous segment of ones. Otherwise, return false.

 

Example 1:

Input: s = "1001"

Output: false

Explanation: The ones do not form a contiguous segment.



Example 2:

Input: s = "110"

Output: true
 

Constraints:

1 <= s.length <= 100
s[i]​​​​ is either '0' or '1'.
s[0] is '1'.


# Code
```cpp []
class Solution {
public:
    bool checkOnesSegment(string s) {
        const int n = s.size();
        int indZero = s.find('0');
        if(indZero < 0) return true;
        int indLastOne = s.find_last_of('1');
        return indLastOne < indZero;
    }
};
```


--------------------------------------------------------------------------------------------------------------


# [1888. Minimum Number of Flips to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating)

Medium

You are given a binary string s. You are allowed to perform two types of operations on the string in any sequence:

Type-1: Remove the character at the start of the string s and append it to the end of the string.
Type-2: Pick any character in s and flip its value, i.e., if its value is '0' it becomes '1' and vice-versa.
Return the minimum number of type-2 operations you need to perform such that s becomes alternating.

The string is called alternating if no two adjacent characters are equal.

For example, the strings "010" and "1010" are alternating, while the string "0100" is not.
 

Example 1:

Input: s = "111000"

Output: 2

Explanation: Use the first operation two times to make s = "100011".
Then, use the second operation on the third and sixth elements to make s = "101010".



Example 2:

Input: s = "010"

Output: 0

Explanation: The string is already alternating.



Example 3:

Input: s = "1110"

Output: 1

Explanation: Use the second operation on the second element to make s = "1010".
 

Constraints:

1 <= s.length <= 105
s[i] is either '0' or '1'.


# Code
```cpp []
class Solution {
public:
    int minFlips(auto& s) {
        int n = s.length();
        int op[2] = {0, 0};

        for (int i = 0; i < n; i++)
            op[(s[i] ^ i) & 1]++;

        int res = min(op[0], op[1]);

        for (int i = 0; i < n - 1; i++) {
            op[(s[i] ^ i) & 1]--;
            op[(s[i] ^ (n + i)) & 1]++;
            res = min(res, min(op[0], op[1]));
        }

        return res;
    }
};
```

------------------------------------------------------------------------------------------------------------------------


# [1980. Find Unique Binary String](https://leetcode.com/problems/find-unique-binary-string/)

Medium
 
Given an array of strings nums containing n unique binary strings each of length n, return a binary string of length n that does not appear in nums. If there are multiple answers, you may return any of them.

 

Example 1:


Input: nums = ["01","10"]

Output: "11"

Explanation: "11" does not appear in nums. "00" would also be correct.




Example 2:

Input: nums = ["00","01"]

Output: "11"

Explanation: "11" does not appear in nums. "10" would also be correct.



Example 3:

Input: nums = ["111","011","001"]

Output: "101"

Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
 

Constraints:

n == nums.length
1 <= n <= 16
nums[i].length == n
nums[i] is either '0' or '1'.
All the strings of nums are unique.


# Code
```cpp []
class Solution {
public:
    string findDifferentBinaryString(vector<string>& nums) {
        int n = nums.size();
        int size = pow(2, n);

        vector<int> nu(size, 0);

        for(string num : nums){
            int val = stoi(num, nullptr, 2);
            nu[val]++;
        }

        for(int i = 0; i < size; i++){
            if(nu[i] == 0){
                string ans = bitset<32>(i).to_string();
                ans = ans.substr(32 - n);
                return ans;
            }
        }

        return string(n, '0');
    }
};
```

--------------------------------------------------------------------------------------------------------------------------------

# [3129. Find All Possible Stable Binary Arrays I](https://leetcode.com/problems/find-all-possible-stable-binary-arrays-i/)

Medium
 
You are given 3 positive integers zero, one, and limit.

A binary array arr is called stable if:

The number of occurrences of 0 in arr is exactly zero.
The number of occurrences of 1 in arr is exactly one.
Each subarray of arr with a size greater than limit must contain both 0 and 1.
Return the total number of stable binary arrays.

Since the answer may be very large, return it modulo 109 + 7.

 


Example 1:

Input: zero = 1, one = 1, limit = 2

Output: 2

Explanation:

The two possible stable binary arrays are [1,0] and [0,1], as both arrays have a single 0 and a single 1, and no subarray has a length greater than 2.



Example 2:

Input: zero = 1, one = 2, limit = 1

Output: 1

Explanation:

The only possible stable binary array is [1,0,1].

Note that the binary arrays [1,1,0] and [0,1,1] have subarrays of length 2 with identical elements, hence, they are not stable.



Example 3:

Input: zero = 3, one = 3, limit = 2

Output: 14

Explanation:

All the possible stable binary arrays are [0,0,1,0,1,1], [0,0,1,1,0,1], [0,1,0,0,1,1], [0,1,0,1,0,1], [0,1,0,1,1,0], [0,1,1,0,0,1], [0,1,1,0,1,0], [1,0,0,1,0,1], [1,0,0,1,1,0], [1,0,1,0,0,1], [1,0,1,0,1,0], [1,0,1,1,0,0], [1,1,0,0,1,0], and [1,1,0,1,0,0].

 

Constraints:

1 <= zero, one, limit <= 200



# Code
```cpp []
class Solution {
public:
    int numberOfStableArrays(int zero, int one, int limit) {
        long long MOD = 1000000007;
        int maxN = zero + one;
        
        std::vector<long long> fact(maxN + 1, 0);
        std::vector<long long> invFact(maxN + 1, 0);
        
        fact[0] = 1;
        invFact[0] = 1;
        for (int i = 1; i <= maxN; i++) {
            fact[i] = (fact[i - 1] * i) % MOD;
        }
        
        auto power = [&](long long baseVal, long long exp) {
            long long res = 1;
            baseVal %= MOD;
            while (exp > 0) {
                if (exp & 1) res = (res * baseVal) % MOD;
                baseVal = (baseVal * baseVal) % MOD;
                exp >>= 1;
            }
            return res;
        };
        
        invFact[maxN] = power(fact[maxN], MOD - 2);
        for (int i = maxN - 1; i >= 1; i--) {
            invFact[i] = (invFact[i + 1] * (i + 1)) % MOD;
        }
        
        auto C = [&](int n, int k) -> long long {
            if (k < 0 || k > n) return 0;
            return fact[n] * invFact[k] % MOD * invFact[n - k] % MOD;
        };
        
        auto F = [&](int N, int K, int L) -> long long {
            if (K <= 0 || K > N) return 0;
            long long ans = 0;
            int maxJ = (N - K) / L;
            for (int j = 0; j <= maxJ; j++) {
                long long term = C(K, j) * C(N - j * L - 1, K - 1) % MOD;
                if (j & 1) {
                    ans = (ans - term + MOD) % MOD;
                } else {
                    ans = (ans + term) % MOD;
                }
            }
            return ans;
        };

        int maxK = std::min(zero, one + 1);
        std::vector<long long> fOne(maxK + 2, 0);
        for (int k = 1; k <= maxK + 1; k++) {
            fOne[k] = F(one, k, limit);
        }
        
        long long ans = 0;
        for (int k = 1; k <= maxK; k++) {
            long long fz = F(zero, k, limit);
            if (fz == 0) continue;
            long long fo = (fOne[k - 1] + 2 * fOne[k] + fOne[k + 1]) % MOD;
            ans = (ans + fz * fo) % MOD;
        }
        
        return static_cast<int>(ans);
    }
};
```


----------------------------------------------------------------------------------------------------------

# [3130. Find All Possible Stable Binary Arrays II](https://leetcode.com/problems/find-all-possible-stable-binary-arrays-ii)

Hard
 
You are given 3 positive integers zero, one, and limit.

A binary array arr is called stable if:

The number of occurrences of 0 in arr is exactly zero.
The number of occurrences of 1 in arr is exactly one.
Each subarray of arr with a size greater than limit must contain both 0 and 1.
Return the total number of stable binary arrays.

Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: zero = 1, one = 1, limit = 2

Output: 2

Explanation:

The two possible stable binary arrays are [1,0] and [0,1].



Example 2:

Input: zero = 1, one = 2, limit = 1

Output: 1

Explanation:

The only possible stable binary array is [1,0,1].



Example 3:

Input: zero = 3, one = 3, limit = 2

Output: 14

Explanation:

All the possible stable binary arrays are [0,0,1,0,1,1], [0,0,1,1,0,1], [0,1,0,0,1,1], [0,1,0,1,0,1], [0,1,0,1,1,0], [0,1,1,0,0,1], [0,1,1,0,1,0], [1,0,0,1,0,1], [1,0,0,1,1,0], [1,0,1,0,0,1], [1,0,1,0,1,0], [1,0,1,1,0,0], [1,1,0,0,1,0], and [1,1,0,1,0,0].

 

Constraints:

1 <= zero, one, limit <= 1000



# Code
```cpp []
class Solution {
public:
    int numberOfStableArrays(int zero, int one, int limit) {
        const int MOD = 1e9 + 7;
        vector<vector<array<long,2>>> dp(
            zero+1, vector<array<long,2>>(one+1, {0LL,0LL}));

        for (int i = 1; i <= min(zero,limit); i++) dp[i][0][0] = 1;
        for (int j = 1; j <= min(one, limit); j++) dp[0][j][1] = 1;

        for (int i = 1; i <= zero; i++) {
            for (int j = 1; j <= one; j++) {
                long over0 = (i-limit >= 1) ? dp[i-limit-1][j][1] : 0;
                long over1 = (j-limit >= 1) ? dp[i][j-limit-1][0] : 0;
                dp[i][j][0] = (dp[i-1][j][0] + dp[i-1][j][1] - over0 + MOD) % MOD;
                dp[i][j][1] = (dp[i][j-1][0] + dp[i][j-1][1] - over1 + MOD) % MOD;
            }
        }
        return (dp[zero][one][0] + dp[zero][one][1]) % MOD;
    }
};
```


---------------------------------------------------------------------------------------------------


# 1009. Complement of Base 10 Integer

Easy
 
The complement of an integer is the integer you get when you flip all the 0's to 1's and all the 1's to 0's in its binary representation.

For example, The integer 5 is "101" in binary and its complement is "010" which is the integer 2.
Given an integer n, return its complement.

 

Example 1:

Input: n = 5

Output: 2

Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.




Example 2:

Input: n = 7

Output: 0

Explanation: 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.





Example 3:

Input: n = 10

Output: 5

Explanation: 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.

 

Constraints:

0 <= n < 109





