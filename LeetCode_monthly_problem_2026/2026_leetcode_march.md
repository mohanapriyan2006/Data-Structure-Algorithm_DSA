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


# [1009. Complement of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer)

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



# Code
```cpp []
class Solution {
public:
    int bitwiseComplement(int n) {
        if (n == 0) return 1;
        int mask = n;
        for (int i = 0; i <= 4; i++)
            mask |= mask >> (1 << i);
        return ~n & mask;
    }
};
```


--------------------------------------------------------------------------------------------------------------



# [3600. Maximize Spanning Tree Stability with Upgrades](https://leetcode.com/problems/maximize-spanning-tree-stability-with-upgrades/)

Hard
 
You are given an integer n, representing n nodes numbered from 0 to n - 1 and a list of edges, where edges[i] = [ui, vi, si, musti]:

ui and vi indicates an undirected edge between nodes ui and vi.
si is the strength of the edge.
musti is an integer (0 or 1). If musti == 1, the edge must be included in the spanning tree. These edges cannot be upgraded.
You are also given an integer k, the maximum number of upgrades you can perform. Each upgrade doubles the strength of an edge, and each eligible edge (with musti == 0) can be upgraded at most once.

The stability of a spanning tree is defined as the minimum strength score among all edges included in it.

Return the maximum possible stability of any valid spanning tree. If it is impossible to connect all nodes, return -1.

Note: A spanning tree of a graph with n nodes is a subset of the edges that connects all nodes together (i.e. the graph is connected) without forming any cycles, and uses exactly n - 1 edges.

 

Example 1:

Input: n = 3, edges = [[0,1,2,1],[1,2,3,0]], k = 1

Output: 2

Explanation:

Edge [0,1] with strength = 2 must be included in the spanning tree.
Edge [1,2] is optional and can be upgraded from 3 to 6 using one upgrade.
The resulting spanning tree includes these two edges with strengths 2 and 6.
The minimum strength in the spanning tree is 2, which is the maximum possible stability.




Example 2:

Input: n = 3, edges = [[0,1,4,0],[1,2,3,0],[0,2,1,0]], k = 2

Output: 6

Explanation:

Since all edges are optional and up to k = 2 upgrades are allowed.
Upgrade edges [0,1] from 4 to 8 and [1,2] from 3 to 6.
The resulting spanning tree includes these two edges with strengths 8 and 6.
The minimum strength in the tree is 6, which is the maximum possible stability.




Example 3:

Input: n = 3, edges = [[0,1,1,1],[1,2,1,1],[2,0,1,1]], k = 0

Output: -1

Explanation:

All edges are mandatory and form a cycle, which violates the spanning tree property of acyclicity. Thus, the answer is -1.
 

Constraints:

2 <= n <= 105
1 <= edges.length <= 105
edges[i] = [ui, vi, si, musti]
0 <= ui, vi < n
ui != vi
1 <= si <= 105
musti is either 0 or 1.
0 <= k <= n
There are no duplicate edges.



# Code
```cpp []
class DSU {
public:
    vector<int> parent, rankv;
    int components;

    DSU(int n) {
        parent.resize(n);
        rankv.assign(n, 0);
        iota(parent.begin(), parent.end(), 0);
        components = n;
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }

    bool unite(int a, int b) {
        a = find(a);
        b = find(b);

        if (a == b) return false;

        if (rankv[a] < rankv[b]) swap(a, b);
        parent[b] = a;
        if (rankv[a] == rankv[b]) rankv[a]++;

        components--;
        return true;
    }
};

class Solution {
public:
    bool canAchieve(int n, vector<vector<int>>& edges, int k, int x) {
        DSU dsu(n);

        // 1. Mandatory edges must be included
        for (auto &e : edges) {
            int u = e[0], v = e[1], s = e[2], must = e[3];

            if (must == 1) {
                if (s < x) return false;          // mandatory edge too weak
                if (!dsu.unite(u, v)) return false; // mandatory cycle
            }
        }

        // 2. Use all free optional edges
        for (auto &e : edges) {
            int u = e[0], v = e[1], s = e[2], must = e[3];

            if (must == 0 && s >= x) {
                dsu.unite(u, v);
            }
        }

        // 3. Use upgradeable optional edges if needed
        int usedUpgrades = 0;

        for (auto &e : edges) {
            int u = e[0], v = e[1], s = e[2], must = e[3];

            if (must == 0 && s < x && 2 * s >= x) {
                if (dsu.unite(u, v)) {
                    usedUpgrades++;
                    if (usedUpgrades > k) return false;
                }
            }
        }

        return dsu.components == 1;
    }

    int maxStability(int n, vector<vector<int>>& edges, int k) {
        // Early check: mandatory edges alone must not form a cycle
        {
            DSU dsu(n);
            for (auto &e : edges) {
                if (e[3] == 1) {
                    if (!dsu.unite(e[0], e[1])) return -1;
                }
            }
        }

        int low = 1, high = 200000, ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canAchieve(n, edges, k, mid)) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return ans;
    }
};
```


---------------------------------------------------------------------------------------------


# [3296. Minimum Number of Seconds to Make Mountain Height Zero](https://leetcode.com/problems/minimum-number-of-seconds-to-make-mountain-height-zero)

Medium
 
You are given an integer mountainHeight denoting the height of a mountain.

You are also given an integer array workerTimes representing the work time of workers in seconds.

The workers work simultaneously to reduce the height of the mountain. For worker i:

To decrease the mountain's height by x, it takes workerTimes[i] + workerTimes[i] * 2 + ... + workerTimes[i] * x seconds. For example:
To reduce the height of the mountain by 1, it takes workerTimes[i] seconds.
To reduce the height of the mountain by 2, it takes workerTimes[i] + workerTimes[i] * 2 seconds, and so on.
Return an integer representing the minimum number of seconds required for the workers to make the height of the mountain 0.

 


Example 1:

Input: mountainHeight = 4, workerTimes = [2,1,1]

Output: 3

Explanation:

One way the height of the mountain can be reduced to 0 is:

Worker 0 reduces the height by 1, taking workerTimes[0] = 2 seconds.
Worker 1 reduces the height by 2, taking workerTimes[1] + workerTimes[1] * 2 = 3 seconds.
Worker 2 reduces the height by 1, taking workerTimes[2] = 1 second.
Since they work simultaneously, the minimum time needed is max(2, 3, 1) = 3 seconds.



Example 2:

Input: mountainHeight = 10, workerTimes = [3,2,2,4]

Output: 12

Explanation:

Worker 0 reduces the height by 2, taking workerTimes[0] + workerTimes[0] * 2 = 9 seconds.
Worker 1 reduces the height by 3, taking workerTimes[1] + workerTimes[1] * 2 + workerTimes[1] * 3 = 12 seconds.
Worker 2 reduces the height by 3, taking workerTimes[2] + workerTimes[2] * 2 + workerTimes[2] * 3 = 12 seconds.
Worker 3 reduces the height by 2, taking workerTimes[3] + workerTimes[3] * 2 = 12 seconds.
The number of seconds needed is max(9, 12, 12, 12) = 12 seconds.



Example 3:

Input: mountainHeight = 5, workerTimes = [1]

Output: 15

Explanation:

There is only one worker in this example, so the answer is workerTimes[0] + workerTimes[0] * 2 + workerTimes[0] * 3 + workerTimes[0] * 4 + workerTimes[0] * 5 = 15.

 

Constraints:

1 <= mountainHeight <= 105
1 <= workerTimes.length <= 104
1 <= workerTimes[i] <= 106



# Code
```cpp []
using ll = long long;
class Solution {
public:
    ll minNumberOfSeconds(int height, vector<int>& times) {
        ll lo = 1, hi = 1e16;

        while (lo < hi) {
            ll mid = (lo + hi) >> 1;
            ll tot = 0;
            for (int i = 0; i < times.size() && tot < height; i++)
                tot += (ll)(sqrt((double)mid / times[i] * 2 + 0.25) - 0.5);
            if (tot >= height)
                hi = mid;
            else
                lo = mid + 1;
        }

        return lo;
    }
};
```


---------------------------------------------------------------------------------------------------------------------


# [1415. The k-th Lexicographical String of All Happy Strings of Length n](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n)
 
Medium
 
A happy string is a string that:

consists only of letters of the set ['a', 'b', 'c'].
s[i] != s[i + 1] for all values of i from 1 to s.length - 1 (string is 1-indexed).
For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers n and k, consider a list of all happy strings of length n sorted in lexicographical order.

Return the kth string of this list or return an empty string if there are less than k happy strings of length n.

 

Example 1:

Input: n = 1, k = 3

Output: "c"

Explanation: The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".



Example 2:

Input: n = 1, k = 4

Output: ""

Explanation: There are only 3 happy strings of length 1.



Example 3:

Input: n = 3, k = 9

Output: "cab"

Explanation: There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9th string = "cab"
 

Constraints:

1 <= n <= 10
1 <= k <= 100




# Code
```cpp []
class Solution {
public:
    string getHappyString(int n, int k) {
        int perCharCount=pow(2,n-1);
        if(3*perCharCount<k)return "";
        string ans="";
        if(k<=perCharCount){
            ans.push_back('a');
        }else if(k<=2*perCharCount){
            ans.push_back('b');
            k-=perCharCount;
        }else{
            ans.push_back('c');
            k-=2*perCharCount;
        }
        vector<string>options{"bc","ac","ab"};
        for(int i=1;i<n;i++){
            perCharCount/=2;
            string option=options[ans.back()-'a'];
            if(k<=perCharCount){
                ans.push_back(option[0]);
            }else{
                ans.push_back(option[1]);
                k-=perCharCount;
            }
        }
        return ans;
    }
};
```

----------------------------------------------------------------------------------


# [1622. Fancy Sequence](https://leetcode.com/problems/fancy-sequence/)

Hard
 
Write an API that generates fancy sequences using the append, addAll, and multAll operations.

Implement the Fancy class:

Fancy() Initializes the object with an empty sequence.
void append(val) Appends an integer val to the end of the sequence.
void addAll(inc) Increments all existing values in the sequence by an integer inc.
void multAll(m) Multiplies all existing values in the sequence by an integer m.
int getIndex(idx) Gets the current value at index idx (0-indexed) of the sequence modulo 109 + 7. If the index is greater or equal than the length of the sequence, return -1.


 

Example 1:

Input
["Fancy", "append", "addAll", "append", "multAll", "getIndex", "addAll", "append", "multAll", "getIndex", "getIndex", "getIndex"]
[[], [2], [3], [7], [2], [0], [3], [10], [2], [0], [1], [2]]

Output

[null, null, null, null, null, 10, null, null, null, 26, 34, 20]

Explanation

Fancy fancy = new Fancy();
fancy.append(2);   // fancy sequence: [2]
fancy.addAll(3);   // fancy sequence: [2+3] -> [5]
fancy.append(7);   // fancy sequence: [5, 7]
fancy.multAll(2);  // fancy sequence: [5*2, 7*2] -> [10, 14]
fancy.getIndex(0); // return 10
fancy.addAll(3);   // fancy sequence: [10+3, 14+3] -> [13, 17]
fancy.append(10);  // fancy sequence: [13, 17, 10]
fancy.multAll(2);  // fancy sequence: [13*2, 17*2, 10*2] -> [26, 34, 20]
fancy.getIndex(0); // return 26
fancy.getIndex(1); // return 34
fancy.getIndex(2); // return 20
 


Constraints:

1 <= val, inc, m <= 100
0 <= idx <= 105
At most 105 calls total will be made to append, addAll, multAll, and getIndex.



# Code
```cpp []
class Fancy {
private:
    const int mod = 1e9 + 7;
    vector<long long> val;
    long long a, b;
    long long modPow(long long x, long long y, long long mod) {
        long long res = 1;
        x = x % mod;
        while (y > 0) {
            if (y % 2 == 1) {
                res = (res * x) % mod;
            }
            y = y / 2;
            x = (x * x) % mod;
        }
        return res;
    }
public:
    Fancy() : a(1), b(0) {
    }
    
    void append(int val) {
        long long x = (val - b + mod) % mod;
        this->val.push_back((x * modPow(a, mod - 2, mod)) % mod);
    }
    
    void addAll(int inc) {
        b = (b + inc) % mod;
    }
    
    void multAll(int m) {
        a = (a * m) % mod;
        b = (b * m) % mod;
    }
    
    int getIndex(int idx) {
        if (idx >= val.size()) 
            return -1;
        return (a * val[idx] + b) % mod;
    }
};

/**
 * Your Fancy object will be instantiated and called as such:
 * Fancy* obj = new Fancy();
 * obj->append(val);
 * obj->addAll(inc);
 * obj->multAll(m);
 * int param_4 = obj->getIndex(idx);
 */
```


-------------------------------------------------------------------------------------------------------

# [1878. Get Biggest Three Rhombus Sums in a Grid](https://leetcode.com/problems/get-biggest-three-rhombus-sums-in-a-grid/)

Medium
 
You are given an m x n integer matrix grid​​​.

A rhombus sum is the sum of the elements that form the border of a regular rhombus shape in grid​​​. The rhombus must have the shape of a square rotated 45 degrees with each of the corners centered in a grid cell. Below is an image of four valid rhombus shapes with the corresponding colored cells that should be included in each rhombus sum:


Note that the rhombus can have an area of 0, which is depicted by the purple rhombus in the bottom right corner.

Return the biggest three distinct rhombus sums in the grid in descending order. If there are less than three distinct values, return all of them.


Example 1:

![img](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-desc-2.png)

Input: grid = [[3,4,5,1,3],[3,3,4,2,3],[20,30,200,40,10],[1,5,5,4,1],[4,3,2,2,5]]

Output: [228,216,211]

Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.
- Blue: 20 + 3 + 200 + 5 = 228
- Red: 200 + 2 + 10 + 4 = 216
- Green: 5 + 200 + 4 + 2 = 211




Example 2:

![img](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex1.png)

Input: grid = [[1,2,3],[4,5,6],[7,8,9]]

Output: [20,9,8]

Explanation: The rhombus shapes for the three biggest distinct rhombus sums are depicted above.

- Blue: 4 + 2 + 6 + 8 = 20
- Red: 9 (area 0 rhombus in the bottom right corner)
- Green: 8 (area 0 rhombus in the bottom middle)




Example 3:

![img](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex2.png)

Input: grid = [[7,7,7]]

Output: [7]

Explanation: All three possible rhombus sums are the same, so return [7].
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j] <= 105



# Code
```cpp []
class Solution {
public:
    vector<int> getBiggestThree(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        set<int> st;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {

                // size 0 rhombus
                st.insert(grid[i][j]);

                for(int k = 1; ; k++) {

                    int r = i + 2*k;
                    int left = j - k;
                    int right = j + k;

                    if(r >= m || left < 0 || right >= n) break;

                    int sum = 0;

                    int x = i, y = j;

                    // top -> right
                    for(int t = 0; t < k; t++) {
                        sum += grid[x + t][y + t];
                    }

                    // right -> bottom
                    for(int t = 0; t < k; t++) {
                        sum += grid[x + k + t][y + k - t];
                    }

                    // bottom -> left
                    for(int t = 0; t < k; t++) {
                        sum += grid[x + 2*k - t][y - t];
                    }

                    // left -> top
                    for(int t = 0; t < k; t++) {
                        sum += grid[x + k - t][y - k + t];
                    }

                    st.insert(sum);
                }
            }
        }

        vector<int> ans;
        for(auto it = st.rbegin(); it != st.rend() && ans.size() < 3; ++it) {
            ans.push_back(*it);
        }

        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------------------

# [1727. Largest Submatrix With Rearrangements](https://leetcode.com/problems/largest-submatrix-with-rearrangements/)

Medium
 
You are given a binary matrix matrix of size m x n, and you are allowed to rearrange the columns of the matrix in any order.

Return the area of the largest submatrix within matrix where every element of the submatrix is 1 after reordering the columns optimally.

 

Example 1:


Input: matrix = [[0,0,1],[1,1,1],[1,0,1]]

Output: 4

Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.





Example 2:


Input: matrix = [[1,0,1,0,1]]

Output: 3

Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 3.



Example 3:

Input: matrix = [[1,1,0],[1,0,1]]

Output: 2

Explanation: Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.
 

Constraints:

m == matrix.length
n == matrix[i].length
1 <= m * n <= 105
matrix[i][j] is either 0 or 1.




# Code
```cpp []
class Solution {
public:
    int largestSubmatrix(vector<vector<int>>& matrix) {
        auto m = matrix.size(), n = matrix[0].size();
        int res = 0;

        for (int i = 1; i < m; i++)
            for (int j = 0; j < n; j++)
                if (matrix[i][j] == 1)
                    matrix[i][j] += matrix[i - 1][j];

        for (int i = 0; i < m; i++) {
            sort(matrix[i].rbegin(), matrix[i].rend());
            for (int j = 0; j < n; j++)
                res = max(res, matrix[i][j] * (j + 1));
        }

        return res;
    }
};
```


------------------------------------------------------------------------------------------------------------------


# [3070. Count Submatrices with Top-Left Element and Sum Less Than k](https://leetcode.com/problems/count-submatrices-with-top-left-element-and-sum-less-than-k/)

Medium
 
You are given a 0-indexed integer matrix grid and an integer k.

Return the number of submatrices that contain the top-left element of the grid, and have a sum less than or equal to k.

 


Example 1:

![img](https://assets.leetcode.com/uploads/2024/01/01/example1.png)

Input: grid = [[7,6,3],[6,6,1]], k = 18

Output: 4

Explanation: There are only 4 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 18.



Example 2:

![img](https://assets.leetcode.com/uploads/2024/01/01/example21.png)

Input: grid = [[7,2,9],[1,5,0],[2,6,6]], k = 20

Output: 6

Explanation: There are only 6 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 20.
 

Constraints:

m == grid.length 
n == grid[i].length
1 <= n, m <= 1000 
0 <= grid[i][j] <= 1000
1 <= k <= 109




# Code
```cpp []
class Solution {
public:
    static int countSubmatrices(vector<vector<int>>& grid, int k) {
        const int r=grid.size(), c=grid[0].size();
        int cnt=0, brCol=c;
        if (grid[0][0]>k) return 0;// early stop
        cnt++;
        for(int j=1; j<c; j++){
            int& x=grid[0][j];
            x+=grid[0][j-1];
            if(x>k)// no need for computing for the rest cols
                { brCol=j; break;}
            cnt++;
        }
        for(int i=1; i<r; i++){
            grid[i][0]+=grid[i-1][0];
            if (grid[i][0]>k) break;
            cnt++;
            for(int j=1; j<brCol; j++){
                int& x=grid[i][j];
                x+=grid[i-1][j]+grid[i][j-1]-grid[i-1][j-1];
                if (x>k){
                    brCol=j; break;
                }
                cnt++;
            }
        }
        return cnt;
    }
};
```


-----------------------------------------------------------------------------------------------------------------


# [3212. Count Submatrices With Equal Frequency of X and Y](https://leetcode.com/problems/count-submatrices-with-equal-frequency-of-x-and-y/)

Medium
 
Given a 2D character matrix grid, where grid[i][j] is either 'X', 'Y', or '.', return the number of submatrices that contain:

grid[0][0]
an equal frequency of 'X' and 'Y'.
at least one 'X'.
 


Example 1:

Input: grid = [["X","Y","."],["Y",".","."]]

Output: 3

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/07/examplems.png)


Example 2:

Input: grid = [["X","X"],["X","Y"]]

Output: 0

Explanation:

No submatrix has an equal frequency of 'X' and 'Y'.




Example 3:

Input: grid = [[".","."],[".","."]]

Output: 0

Explanation:

No submatrix has at least one 'X'.

 

Constraints:

1 <= grid.length, grid[i].length <= 1000
grid[i][j] is either 'X', 'Y', or '.'.




# Code
```cpp []
class Solution {
public:
    int numberOfSubmatrices(vector<vector<char>>& grid) {
        int rows = grid.size(), cols = grid[0].size();
        vector<int> sumX(cols, 0);
        vector<int> sumY(cols, 0);
        int res = 0;

        for (int i = 0; i < rows; i++) {
            int rx = 0, ry = 0;

            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 'X') rx++;
                else if (grid[i][j] == 'Y') ry++;
                sumX[j] += rx;
                sumY[j] += ry;
                if (sumX[j] > 0 && sumX[j] == sumY[j]) res++;
            }
        }

        return res;
    }
};
```


--------------------------------------------------------------------------------------------------------------------


# [3567. Minimum Absolute Difference in Sliding Submatrix](https://leetcode.com/problems/minimum-absolute-difference-in-sliding-submatrix)

Medium
 
You are given an m x n integer matrix grid and an integer k.

For every contiguous k x k submatrix of grid, compute the minimum absolute difference between any two distinct values within that submatrix.

Return a 2D array ans of size (m - k + 1) x (n - k + 1), where ans[i][j] is the minimum absolute difference in the submatrix whose top-left corner is (i, j) in grid.

Note: If all elements in the submatrix have the same value, the answer will be 0.

A submatrix (x1, y1, x2, y2) is a matrix that is formed by choosing all cells matrix[x][y] where x1 <= x <= x2 and y1 <= y <= y2.


 

Example 1:

Input: grid = [[1,8],[3,-2]], k = 2

Output: [[2]]

Explanation:

There is only one possible k x k submatrix: [[1, 8], [3, -2]].
Distinct values in the submatrix are [1, 8, 3, -2].
The minimum absolute difference in the submatrix is |1 - 3| = 2. Thus, the answer is [[2]].




Example 2:

Input: grid = [[3,-1]], k = 1

Output: [[0,0]]

Explanation:

Both k x k submatrix has only one distinct element.
Thus, the answer is [[0, 0]].



Example 3:

Input: grid = [[1,-2,3],[2,3,5]], k = 2

Output: [[1,2]]

Explanation:

There are two possible k × k submatrix:
Starting at (0, 0): [[1, -2], [2, 3]].
Distinct values in the submatrix are [1, -2, 2, 3].
The minimum absolute difference in the submatrix is |1 - 2| = 1.
Starting at (0, 1): [[-2, 3], [3, 5]].
Distinct values in the submatrix are [-2, 3, 5].
The minimum absolute difference in the submatrix is |3 - 5| = 2.
Thus, the answer is [[1, 2]].
 

Constraints:

1 <= m == grid.length <= 30
1 <= n == grid[i].length <= 30
-105 <= grid[i][j] <= 105
1 <= k <= min(m, n)



# Code
```cpp []
class Solution {
public:
    vector<vector<int>> minAbsDiff(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> ans(m - k + 1, vector<int>(n - k + 1));

        for (int i = 0; i <= m - k; i++) {
            for (int j = 0; j <= n - k; j++) {
                vector<int> v;
                for (int x = i; x < i + k; x++)
                    for (int y = j; y < j + k; y++)
                        v.push_back(grid[x][y]);

                sort(v.begin(), v.end());
                v.erase(unique(v.begin(), v.end()), v.end());

                if (v.size() <= 1) {
                    ans[i][j] = 0;
                } else {
                    int mn = INT_MAX;
                    for (int p = 0; p < (int)v.size() - 1; p++)
                        mn = min(mn, v[p+1] - v[p]);
                    ans[i][j] = mn;
                }
            }
        }
        return ans;
    }
};
```


----------------------------------------------------------------------------------------------------------------------------------


# [3643. Flip Square Submatrix Vertically](https://leetcode.com/problems/flip-square-submatrix-vertically/)

Easy
 
You are given an m x n integer matrix grid, and three integers x, y, and k.

The integers x and y represent the row and column indices of the top-left corner of a square submatrix and the integer k represents the size (side length) of the square submatrix.

Your task is to flip the submatrix by reversing the order of its rows vertically.

Return the updated matrix.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2025/07/20/gridexmdrawio.png)


Input: grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], x = 1, y = 0, k = 3

Output: [[1,2,3,4],[13,14,15,8],[9,10,11,12],[5,6,7,16]]

Explanation:

The diagram above shows the grid before and after the transformation.




Example 2:

![img](https://assets.leetcode.com/uploads/2025/07/20/gridexm2drawio.png)
​​​​​​​
Input: grid = [[3,4,2,3],[2,3,4,2]], x = 0, y = 2, k = 2

Output: [[3,4,4,2],[2,3,2,3]]

Explanation:

The diagram above shows the grid before and after the transformation.

 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j] <= 100
0 <= x < m
0 <= y < n
1 <= k <= min(m - x, n - y)



# Code
```cpp []
class Solution {
public:
    vector<vector<int>>& reverseSubmatrix(vector<vector<int>>& grid, int x, int y, int k) {
        for (int i = 0; i < k; i++)
            for (int j = 0; j < k >> 1; j++)
                swap(grid[x + j][y + i], grid[x + k - j - 1][y + i]);

        return grid;
    }
};
```


---------------------------------------------------------------------------------------------------------------------------


# [1886. Determine Whether Matrix Can Be Obtained By Rotation](https://leetcode.com/problems/determine-whether-matrix-can-be-obtained-by-rotation)

Easy
 
Given two n x n binary matrices mat and target, return true if it is possible to make mat equal to target by rotating mat in 90-degree increments, or false otherwise.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/05/20/grid3.png)

Input: mat = [[0,1],[1,0]], target = [[1,0],[0,1]]

Output: true

Explanation: We can rotate mat 90 degrees clockwise to make mat equal target.



Example 2:

![img](https://assets.leetcode.com/uploads/2021/05/20/grid4.png)

Input: mat = [[0,1],[1,1]], target = [[1,0],[0,1]]

Output: false

Explanation: It is impossible to make mat equal to target by rotating mat.



Example 3:

![img](https://assets.leetcode.com/uploads/2021/05/26/grid4.png)

Input: mat = [[0,0,0],[0,1,0],[1,1,1]], target = [[1,1,1],[0,1,0],[0,0,0]]

Output: true

Explanation: We can rotate mat 90 degrees clockwise two times to make mat equal target.
 

Constraints:

n == mat.length == target.length
n == mat[i].length == target[i].length
1 <= n <= 10
mat[i][j] and target[i][j] are either 0 or 1.


# Code
```cpp []
class Solution {
public:
    void rotate(vector<vector<int>>& mat) {
        int n = mat.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                swap(mat[i][j], mat[j][i]);
            }
        }
        for (int i = 0; i < n; i++) {
            reverse(mat[i].begin(), mat[i].end());
        }
    }
    bool findRotation(vector<vector<int>>& mat, vector<vector<int>>& target) {
        for (int i = 0; i < 4; i++) {
            if (mat == target) return true;
            rotate(mat);
        }
        return false;
    }
};
```


--------------------------------------------------------------------------------------------------------------------------


# [1594. Maximum Non Negative Product in a Matrix](https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix)

Medium
 
You are given a m x n matrix grid. Initially, you are located at the top-left corner (0, 0), and in each step, you can only move right or down in the matrix.

Among all possible paths starting from the top-left corner (0, 0) and ending in the bottom-right corner (m - 1, n - 1), find the path with the maximum non-negative product. The product of a path is the product of all integers in the grid cells visited along the path.

Return the maximum non-negative product modulo 109 + 7. If the maximum product is negative, return -1.

Notice that the modulo is performed after getting the maximum product.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/12/23/product1.jpg)

Input: grid = [[-1,-2,-3],[-2,-3,-3],[-3,-3,-2]]

Output: -1

Explanation: It is not possible to get non-negative product in the path from (0, 0) to (2, 2), so return -1.





Example 2:

![img](https://assets.leetcode.com/uploads/2021/12/23/product2.jpg)

Input: grid = [[1,-2,1],[1,-2,1],[3,-4,1]]

Output: 8

Explanation: Maximum non-negative product is shown (1 * 1 * -2 * -4 * 1 = 8).




Example 3:

![img](https://assets.leetcode.com/uploads/2021/12/23/product3.jpg)

Input: grid = [[1,3],[0,-4]]

Output: 0

Explanation: Maximum non-negative product is shown (1 * 0 * -4 = 0).
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 15
-4 <= grid[i][j] <= 4




# Code
```cpp []
int64_t dp[15][15][2];
class Solution {
public:
    static int maxProductPath(vector<vector<int>>& grid) {
        const int r=grid.size(), c=grid[0].size(), MOD=1e9+7;
        int64_t p=dp[0][0][0]=dp[0][0][1]=grid[0][0];
        for(int j=1; j<c; j++){
            p*=grid[0][j];
            dp[0][j][0]=dp[0][j][1]=p;
        }
        p=grid[0][0];
        for(int i=1; i<r; i++){
            p*=grid[i][0];
            dp[i][0][0]=dp[i][0][1]=p;
            for(int j=1; j<c; j++){
                int x=grid[i][j];
                auto [minP, maxP]=minmax({x*dp[i][j-1][0], x*dp[i][j-1][1], x*dp[i-1][j][0], x*dp[i-1][j][1]});
                dp[i][j][0]=minP, dp[i][j][1]=maxP;
            }
        }
        int64_t ans=dp[r-1][c-1][1];
        return ans<0?-1: ans%MOD;
    }
};
```



--------------------------------------------------------------------------------------------------------------------


# [2906. Construct Product Matrix](https://leetcode.com/problems/construct-product-matrix)

Medium

Given a 0-indexed 2D integer matrix grid of size n * m, we define a 0-indexed 2D matrix p of size n * m as the product matrix of grid if the following condition is met:

Each element p[i][j] is calculated as the product of all elements in grid except for the element grid[i][j]. This product is then taken modulo 12345.
Return the product matrix of grid.

 


Example 1:

Input: grid = [[1,2],[3,4]]

Output: [[24,12],[8,6]]

Explanation: p[0][0] = grid[0][1] * grid[1][0] * grid[1][1] = 2 * 3 * 4 = 24
p[0][1] = grid[0][0] * grid[1][0] * grid[1][1] = 1 * 3 * 4 = 12
p[1][0] = grid[0][0] * grid[0][1] * grid[1][1] = 1 * 2 * 4 = 8
p[1][1] = grid[0][0] * grid[0][1] * grid[1][0] = 1 * 2 * 3 = 6
So the answer is [[24,12],[8,6]].




Example 2:

Input: grid = [[12345],[2],[1]]

Output: [[2],[0],[0]]

Explanation: p[0][0] = grid[0][1] * grid[0][2] = 2 * 1 = 2.
p[0][1] = grid[0][0] * grid[0][2] = 12345 * 1 = 12345. 12345 % 12345 = 0. So p[0][1] = 0.
p[0][2] = grid[0][0] * grid[0][1] = 12345 * 2 = 24690. 24690 % 12345 = 0. So p[0][2] = 0.
So the answer is [[2],[0],[0]].
 



Constraints:

1 <= n == grid.length <= 105
1 <= m == grid[i].length <= 105
2 <= n * m <= 105
1 <= grid[i][j] <= 109





# Code
```cpp []
class Solution {
public:
    vector<vector<int>> constructProductMatrix(vector<vector<int>>& grid) {
        const int MOD = 12345;
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> p(n, vector<int>(m));

        long long suffix = 1;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                p[i][j] = suffix;
                suffix = (suffix * (grid[i][j] % MOD)) % MOD;
            }
        }

        long long prefix = 1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                p[i][j] = (p[i][j] * prefix) % MOD;
                prefix = (prefix * (grid[i][j] % MOD)) % MOD;
            }
        }

        return p;
    }
};
```


------------------------------------------------------------------------------------------------------------------------


# [3546. Equal Sum Grid Partition I](https://leetcode.com/problems/equal-sum-grid-partition-i)

Medium
 
You are given an m x n matrix grid of positive integers. Your task is to determine if it is possible to make either one horizontal or one vertical cut on the grid such that:

Each of the two resulting sections formed by the cut is non-empty.
The sum of the elements in both sections is equal.
Return true if such a partition exists; otherwise return false.

 

Example 1:

Input: grid = [[1,4],[2,3]]

Output: true

Explanation:

![img](https://assets.leetcode.com/uploads/2025/03/30/lc.jpeg)

A horizontal cut between row 0 and row 1 results in two non-empty sections, each with a sum of 5. Thus, the answer is true.





Example 2:

Input: grid = [[1,3],[2,4]]

Output: false

Explanation:

No horizontal or vertical cut results in two non-empty sections with equal sums. Thus, the answer is false.

 

Constraints:

1 <= m == grid.length <= 105
1 <= n == grid[i].length <= 105
2 <= m * n <= 105
1 <= grid[i][j] <= 105



# Code
```cpp []
class Solution {
public:
    bool canPartitionGrid(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        long long total = 0;
        
        for (auto &row : grid)
            for (int x : row)
                total += x;
        
        if (total % 2) return false;
        
        long long target = total / 2, sum = 0;
        
        for (int i = 0; i < m - 1; i++) {
            for (int j = 0; j < n; j++)
                sum += grid[i][j];
            if (sum == target) return true;
        }
        
        sum = 0;
        
        for (int j = 0; j < n - 1; j++) {
            for (int i = 0; i < m; i++)
                sum += grid[i][j];
            if (sum == target) return true;
        }
        
        return false;
    }
};
```


-----------------------------------------------------------------------------------------------------------

# [3548. Equal Sum Grid Partition II](https://leetcode.com/problems/equal-sum-grid-partition-ii/)

Hard
 
You are given an m x n matrix grid of positive integers. Your task is to determine if it is possible to make either one horizontal or one vertical cut on the grid such that:

Each of the two resulting sections formed by the cut is non-empty.
The sum of elements in both sections is equal, or can be made equal by discounting at most one single cell in total (from either section).
If a cell is discounted, the rest of the section must remain connected.
Return true if such a partition exists; otherwise, return false.

Note: A section is connected if every cell in it can be reached from any other cell by moving up, down, left, or right through other cells in the section.

 

Example 1:

Input: grid = [[1,4],[2,3]]

Output: true

Explanation:

![img](https://assets.leetcode.com/uploads/2025/03/30/lc.jpeg)

A horizontal cut after the first row gives sums 1 + 4 = 5 and 2 + 3 = 5, which are equal. Thus, the answer is true.




Example 2:

Input: grid = [[1,2],[3,4]]

Output: true

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/01/chatgpt-image-apr-1-2025-at-05_28_12-pm.png)

A vertical cut after the first column gives sums 1 + 3 = 4 and 2 + 4 = 6.
By discounting 2 from the right section (6 - 2 = 4), both sections have equal sums and remain connected. Thus, the answer is true.




Example 3:

Input: grid = [[1,2,4],[2,3,5]]

Output: false

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/01/chatgpt-image-apr-2-2025-at-02_50_29-am.png)

A horizontal cut after the first row gives 1 + 2 + 4 = 7 and 2 + 3 + 5 = 10.
By discounting 3 from the bottom section (10 - 3 = 7), both sections have equal sums, but they do not remain connected as it splits the bottom section into two parts ([2] and [5]). Thus, the answer is false.



Example 4:

Input: grid = [[4,1,8],[3,2,6]]

Output: false

Explanation:

No valid cut exists, so the answer is false.

 

Constraints:

1 <= m == grid.length <= 105
1 <= n == grid[i].length <= 105
2 <= m * n <= 105
1 <= grid[i][j] <= 105



# Code
```cpp []
class Solution {
public:
    bool canPartitionGrid(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();

        long long total = 0;
        unordered_map<long long,int> bottomMap,topMap, leftMap, rightMap;

        for (auto &row : grid) {
            for (int x : row) {
                total += x;
                bottomMap[x]++;
                rightMap[x]++;
            }
        }

        long long sumTop = 0;

        for (int i = 0; i < m - 1; i++) {
            for (int j = 0; j < n; j++) {
                int val = grid[i][j];
                sumTop += val;

                topMap[val]++;
                bottomMap[val]--;
            }

            long long sumBottom = total - sumTop;

            if (sumTop == sumBottom) return true;

            long long diff = abs(sumTop - sumBottom);

            if (sumTop > sumBottom) {
                if (check(topMap, grid, 0, i, 0, n-1, diff)) return true;
            } else {
                if (check(bottomMap, grid, i+1, m-1, 0, n-1, diff)) return true;
            }
        }

        long long sumLeft = 0;
        for (int j = 0; j < n - 1; j++) {
            for (int i = 0; i < m; i++) {
                int val = grid[i][j];
                sumLeft += val;

                leftMap[val]++;
                rightMap[val]--;
            }

            long long sumRight = total - sumLeft;
            if (sumLeft == sumRight) return true;

            long long diff = abs(sumLeft - sumRight);

            if (sumLeft > sumRight) {
                if (check(leftMap, grid, 0, m-1, 0, j, diff)) return true;
            } else {
                if (check(rightMap, grid, 0, m-1, j+1, n-1, diff)) return true;
            }
        }

        return false;
    }

    bool check(unordered_map<long long,int>& mp, vector<vector<int>>& grid,
           int r1, int r2, int c1, int c2, long long diff) {

        int rows = r2 - r1 + 1;
        int cols = c2 - c1 + 1;

        if (rows * cols == 1) return false;

        if (rows == 1) {
            return (grid[r1][c1] == diff || grid[r1][c2] == diff);
        }

        if (cols == 1) {
            return (grid[r1][c1] == diff || grid[r2][c1] == diff);
        }

        return mp[diff]>0;
    }
};
```



--------------------------------------------------------------------------------------------------------------




