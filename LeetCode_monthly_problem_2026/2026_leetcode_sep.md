# 
# LeetCode Monthly Problems - September 2026

## [3876. Construct Uniform Parity Array II](https://leetcode.com/problems/construct-uniform-parity-array-ii)

Medium

You are given an array nums1 of n distinct integers.

You want to construct another array nums2 of length n such that the elements in nums2 are either all odd or all even.

For each index i, you must choose exactly one of the following (in any order):

nums2[i] = nums1[i]​​​​​​​
nums2[i] = nums1[i] - nums1[j], for an index j != i, such that nums1[i] - nums1[j] >= 1
Return true if it is possible to construct such an array, otherwise return false.

 

Example 1:

Input: nums1 = [1,4,7]

Output: true

Explanation:​​​​​​​​​​​​​​

Set nums2[0] = nums1[0] = 1.
Set nums2[1] = nums1[1] - nums1[0] = 4 - 1 = 3.
Set nums2[2] = nums1[2] = 7.
nums2 = [1, 3, 7], and all elements are odd. Thus, the answer is true.




Example 2:

Input: nums1 = [2,3]

Output: false

Explanation:

It is not possible to construct nums2 such that all elements have the same parity. Thus, the answer is false.




Example 3:

Input: nums1 = [4,6]

Output: true

Explanation:

Set nums2[0] = nums1[0] = 4.
Set nums2[1] = nums1[1] = 6.
nums2 = [4, 6], and all elements are even. Thus, the answer is true.


 

Constraints:

1 <= n == nums1.length <= 105
1 <= nums1[i] <= 109
nums1 consists of distinct integers.



# Code
```cpp []
class Solution {
public:
    bool uniformArray(vector<int>& A) {
        int xmin = A[0];
        bool odd = 0;

        for (auto& x : A) {
            xmin = min(xmin, x);
            odd |= x & 1;
        }

        return (xmin & 1) == odd;
    }
};
```

--------------------------------------------------------------------------------------------


# [3903. Smallest Stable Index I](https://leetcode.com/problems/smallest-stable-index-i)

Easy
 
You are given an integer array nums of length n and an integer k.

For each index i, define its instability score as max(nums[0..i]) - min(nums[i..n - 1]).

In other words:

max(nums[0..i]) is the largest value among the elements from index 0 to index i.
min(nums[i..n - 1]) is the smallest value among the elements from index i to index n - 1.
An index i is called stable if its instability score is less than or equal to k.

Return the smallest stable index. If no such index exists, return -1.

 


Example 1:

Input: nums = [5,0,1,4], k = 3

Output: 3

Explanation:

At index 0: The maximum in [5] is 5, and the minimum in [5, 0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 1: The maximum in [5, 0] is 5, and the minimum in [0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 2: The maximum in [5, 0, 1] is 5, and the minimum in [1, 4] is 1, so the instability score is 5 - 1 = 4.
At index 3: The maximum in [5, 0, 1, 4] is 5, and the minimum in [4] is 4, so the instability score is 5 - 4 = 1.
This is the first index with an instability score less than or equal to k = 3. Thus, the answer is 3.



Example 2:

Input: nums = [3,2,1], k = 1

Output: -1

Explanation:

At index 0, the instability score is 3 - 1 = 2.
At index 1, the instability score is 3 - 1 = 2.
At index 2, the instability score is 3 - 1 = 2.
None of these values is less than or equal to k = 1, so the answer is -1.



Example 3:

Input: nums = [0], k = 0

Output: 0

Explanation:

At index 0, the instability score is 0 - 0 = 0, which is less than or equal to k = 0. Therefore, the answer is 0.

 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 109
0 <= k <= 109


# Code
```cpp []
class Solution {
public:
    inline static int suf[100];
    int firstStableIndex(vector<int>& nums, int k) {
        int n = nums.size();
        suf[n - 1] = nums.back();

        for (int i = n - 2; i >= 0; i--)
            suf[i] = min(suf[i + 1], nums[i]);        

        int maxSoFar = 0;
        for (int i = 0; i < n; i++) {
            maxSoFar = max(maxSoFar, nums[i]);
            if (maxSoFar - suf[i] <= k)
                return i;
        }

        return -1;
    }
};
```

---------------------------------------------------------------------------------------------------------------------


# [3904. Smallest Stable Index II](https://leetcode.com/problems/smallest-stable-index-ii/)

Medium

You are given an integer array nums of length n and an integer k.

For each index i, define its instability score as max(nums[0..i]) - min(nums[i..n - 1]).

In other words:

max(nums[0..i]) is the largest value among the elements from index 0 to index i.
min(nums[i..n - 1]) is the smallest value among the elements from index i to index n - 1.
An index i is called stable if its instability score is less than or equal to k.

Return the smallest stable index. If no such index exists, return -1.

 

Example 1:

Input: nums = [5,0,1,4], k = 3

Output: 3

Explanation:

At index 0: The maximum in [5] is 5, and the minimum in [5, 0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 1: The maximum in [5, 0] is 5, and the minimum in [0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 2: The maximum in [5, 0, 1] is 5, and the minimum in [1, 4] is 1, so the instability score is 5 - 1 = 4.
At index 3: The maximum in [5, 0, 1, 4] is 5, and the minimum in [4] is 4, so the instability score is 5 - 4 = 1.
This is the first index with an instability score less than or equal to k = 3. Thus, the answer is 3.





Example 2:

Input: nums = [3,2,1], k = 1

Output: -1

Explanation:

At index 0, the instability score is 3 - 1 = 2.
At index 1, the instability score is 3 - 1 = 2.
At index 2, the instability score is 3 - 1 = 2.
None of these values is less than or equal to k = 1, so the answer is -1.




Example 3:

Input: nums = [0], k = 0

Output: 0

Explanation:

At index 0, the instability score is 0 - 0 = 0, which is less than or equal to k = 0. Therefore, the answer is 0.


 

Constraints:

1 <= nums.length <= 105
0 <= nums[i] <= 109
0 <= k <= 109




# Code
```cpp []
class Solution {
public:
    int firstStableIndex(vector<int>& nums, int k) {
        int maxSoFar = -1;
        int cand = 0, cMax = 0;

        for (int i = 0; i < nums.size(); i++) {
            maxSoFar = max(maxSoFar, nums[i]);

            if (i == cand) cMax = maxSoFar;

            if (nums[i] < cMax - k)
                cand = i + 1;
        }

        return cand < nums.size() ? cand : -1;
    }
};
```

-----------------------------------------------------------------------------

# [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

Hard
 
Given two strings s and t, return the number of distinct subsequences of s which equals t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

 

Example 1:

Input: s = "rabbbit", t = "rabbit"

Output: 3

Explanation:
As shown below, there are 3 ways you can generate "rabbit" from s.
rabbbit
rabbbit
rabbbit



Example 2:

Input: s = "babgbag", t = "bag"

Output: 5

Explanation:
As shown below, there are 5 ways you can generate "bag" from s.
babgbag
babgbag
babgbag
babgbag
babgbag
 



Constraints:

1 <= s.length, t.length <= 1000
s and t consist of English letters.


# Code
```cpp []
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = s.length(), n = t.length();
        if (m < n) {
            return 0;
        }
        vector<vector<unsigned long long>> dp(m + 1, vector<unsigned long long>(n + 1));
        for (int i = 0; i <= m; i++) {
            dp[i][n] = 1;
        }
        for (int i = m - 1; i >= 0; i--) {
            char sChar = s.at(i);
            for (int j = n - 1; j >= 0; j--) {
                char tChar = t.at(j);
                if (sChar == tChar) {
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                } else {
                    dp[i][j] = dp[i + 1][j];
                }
            }
        }
        return dp[0][0];
    }
};
```

-------------------------------------------------------------------------------------------------


# [940. Distinct Subsequences II](https://leetcode.com/problems/distinct-subsequences-ii/)

Hard

Given a string s, return the number of distinct non-empty subsequences of s. Since the answer may be very large, return it modulo 109 + 7.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not.
 



Example 1:

Input: s = "abc"

Output: 7

Explanation: The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".




Example 2:

Input: s = "aba"

Output: 6

Explanation: The 6 distinct subsequences are "a", "b", "ab", "aa", "ba", and "aba".




Example 3:

Input: s = "aaa"

Output: 3

Explanation: The 3 distinct subsequences are "a", "aa" and "aaa".
 



Constraints:

1 <= s.length <= 2000
s consists of lowercase English letters.


# Code
```cpp []
class Solution {
    static constexpr int MOD = 1e9 + 7;

public:
    int distinctSubseqII(string s) {
        int tot = 0, dp[26]{};

        for (auto& c : s) {
            c -= 'a';
            int add = (tot - dp[c] + MOD) % MOD;
            dp[c] = 1 + tot;
            tot = (dp[c] + add) % MOD;
        }

        return tot;
    }
};
```

---------------------------------------------------------------------------------------------------------------------

# [3870. Count Commas in Range](https://leetcode.com/problems/count-commas-in-range/)

Easy

You are given an integer n.

Return the total number of commas used when writing all integers from [1, n] (inclusive) in standard number formatting.

In standard formatting:

A comma is inserted after every three digits from the right.
Numbers with fewer than 4 digits contain no commas.
 

Example 1:

Input: n = 1002

Output: 3

Explanation:

The numbers "1,000", "1,001", and "1,002" each contain one comma, giving a total of 3.




Example 2:

Input: n = 998

Output: 0

Explanation:

All numbers from 1 to 998 have fewer than four digits. Therefore, no commas are used.

 

Constraints:

1 <= n <= 105



# Code
```cpp []
class Solution {
public:
    int countCommas(int n) {
        return (n > 999) * (n - 999);
    }
};
```

----------------------------------------------------------------------------------------------------

# [3871. Count Commas in Range II](https://leetcode.com/problems/count-commas-in-range-ii/)

Medium

You are given an integer n.

Return the total number of commas used when writing all integers from [1, n] (inclusive) in standard number formatting.

In standard formatting:

A comma is inserted after every three digits from the right.
Numbers with fewer than 4 digits contain no commas.
 

Example 1:

Input: n = 1002

Output: 3

Explanation:

The numbers "1,000", "1,001", and "1,002" each contain one comma, giving a total of 3.

Example 2:

Input: n = 998

Output: 0

Explanation:

​​​​​​​All numbers from 1 to 998 have fewer than four digits. Therefore, no commas are used.

 

Constraints:

1 <= n <= 1015










