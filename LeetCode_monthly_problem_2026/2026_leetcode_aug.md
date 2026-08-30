# -------------------------------------------

# LeetCode Monthly Problems - August 2026

# -------------------------------------------

--------------------------------------------

# [1406. Stone Game III](https://leetcode.com/problems/stone-game-iii/)

Hard


Alice and Bob continue their games with piles of stones. There are several stones arranged in a row, and each stone has an associated value which is an integer given in the array stoneValue.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take 1, 2, or 3 stones from the first remaining stones in the row.

The score of each player is the sum of the values of the stones taken. The score of each player is 0 initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume Alice and Bob play optimally.

Return "Alice" if Alice will win, "Bob" if Bob will win, or "Tie" if they will end the game with the same score.

 


Example 1:

Input: stoneValue = [1,2,3,7]

Output: "Bob"

Explanation: Alice will always lose. Her best move will be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.




Example 2:

Input: stoneValue = [1,2,3,-9]

Output: "Alice"

Explanation: Alice must choose all the three piles at the first move to win and leave Bob with negative score.
If Alice chooses one pile her score will be 1 and the next move Bob's score becomes 5. In the next move, Alice will take the pile with value = -9 and lose.
If Alice chooses two piles her score will be 3 and the next move Bob's score becomes 3. In the next move, Alice will take the pile with value = -9 and also lose.
Remember that both play optimally so here Alice will choose the scenario that makes her win.



Example 3:

Input: stoneValue = [1,2,3,6]

Output: "Tie"

Explanation: Alice cannot win this game. She can end the game in a draw if she decided to choose all the first three piles, otherwise she will lose.
 


Constraints:

1 <= stoneValue.length <= 5 * 104
-1000 <= stoneValue[i] <= 1000


# Code
```cpp []
class Solution {
public:
    static constexpr int MIN = -50000001;
    static inline string s[] = {"Bob", "Tie", "Alice"};

    string stoneGameIII(vector<int>& A) {
        int n = A.size();
        vector<int> dp(n, MIN);

        auto maxDiff = [&](this auto&& maxDiff, int i) -> int {
            if (i == n) return 0;

            int& res = dp[i];
            if (res != MIN) return res;

            int sum = 0;

            for (int j = 1; j <= 3 && i + j <= n; j++) {
                sum += A[i + j - 1];
                res = max(res, sum - maxDiff(i + j));
            }

            return res;
        };

        int d = maxDiff(0);
        return s[(d > 0) - (d < 0) + 1];
    }
};
```

--------------------------------------------------------------------------------------------------------

# [3310. Remove Methods From Project](https://leetcode.com/problems/remove-methods-from-project/description)

Medium
 
You are maintaining a project that has n methods numbered from 0 to n - 1.

You are given two integers n and k, and a 2D integer array invocations, where invocations[i] = [ai, bi] indicates that method ai invokes method bi.

There is a known bug in method k. Method k, along with any method invoked by it, either directly or indirectly, are considered suspicious and we aim to remove them.

A group of methods can only be removed if no method outside the group invokes any methods within it.

Return an array containing all the remaining methods after removing all the suspicious methods. You may return the answer in any order. If it is not possible to remove all the suspicious methods, none should be removed.

 


Example 1:

Input: n = 4, k = 1, invocations = [[1,2],[0,1],[3,2]]

Output: [0,1,2,3]

Explanation:

![img](https://assets.leetcode.com/uploads/2024/07/18/graph-2.png)

Method 2 and method 1 are suspicious, but they are directly invoked by methods 3 and 0, which are not suspicious. We return all elements without removing anything.





Example 2:

Input: n = 5, k = 0, invocations = [[1,2],[0,2],[0,1],[3,4]]

Output: [3,4]

Explanation:

![img](https://assets.leetcode.com/uploads/2024/07/18/graph-3.png)


Methods 0, 1, and 2 are suspicious and they are not directly invoked by any other method. We can remove them.





Example 3:

Input: n = 3, k = 2, invocations = [[1,2],[0,1],[2,0]]

Output: []

Explanation:

![img](https://assets.leetcode.com/uploads/2024/07/20/graph.png)


All methods are suspicious. We can remove them.

 

Constraints:

1 <= n <= 105
0 <= k <= n - 1
0 <= invocations.length <= 2 * 105
invocations[i] == [ai, bi]
0 <= ai, bi <= n - 1
ai != bi
invocations[i] != invocations[j]


# Code
```cpp []
constexpr int MAXN = 100005;

class Solution {
public:
    vector<int> remainingMethods(int n, int k, vector<vector<int>>& invocations) {
        vector<vector<int>> edges(n);
        vector<int> inDegree(n, 0);

        bitset<MAXN> sus;

        for (const auto& inv : invocations) {
            edges[inv[0]].push_back(inv[1]);
            inDegree[inv[1]]++;
        }

        queue<int> q;
        q.push(k);

        sus.set(k);

        while (!q.empty()) {
            int u = q.front();
            q.pop();
            for (int v : edges[u]) {
                inDegree[v]--;

                if (!sus.test(v)) {
                    q.push(v);
                    sus.set(v);
                }
            }
        }

        bool canRemoveAll = true;
        vector<int> rem;

        for (int i = 0; i < n; i++) {
            if (sus.test(i) && inDegree[i] > 0) {
                canRemoveAll = false;
                break;
            } else if (!sus.test(i)) {
                rem.push_back(i);
            }
        }

        if (!canRemoveAll) {
            vector<int> allNodes(n);
            iota(allNodes.begin(), allNodes.end(), 0);
            return allNodes;
        }

        return rem;
    }
};
```

----------------------------------------------------------------------------------------------------

# [3345. Smallest Divisible Digit Product I](https://leetcode.com/problems/smallest-divisible-digit-product-i)

Easy
 
You are given two integers n and t. Return the smallest number greater than or equal to n such that the product of its digits is divisible by t.


 

Example 1:

Input: n = 10, t = 2

Output: 10

Explanation:

The digit product of 10 is 0, which is divisible by 2, making it the smallest number greater than or equal to 10 that satisfies the condition.




Example 2:

Input: n = 15, t = 3

Output: 16

Explanation:

The digit product of 16 is 6, which is divisible by 3, making it the smallest number greater than or equal to 15 that satisfies the condition.


 

Constraints:

1 <= n <= 100
1 <= t <= 10


# Code
```cpp []
class Solution {
public:
    int smallestNumber(int n, int t) {
        auto [q, r] = div(n, 10);

        int req = t / gcd(q + (10 - q) / 10, t);
        int nxt = ((r + req - 1) / req) * req;
        int x = nxt - (nxt - 10) * (nxt / 10);

        return q * 10 + x;
    }
};
```

-----------------------------------------------------------------------------------------------------------------

# [3348. Smallest Divisible Digit Product II](https://leetcode.com/problems/smallest-divisible-digit-product-ii/)

Hard

You are given a string num which represents a positive integer, and an integer t.

A number is called zero-free if none of its digits are 0.

Return a string representing the smallest zero-free number greater than or equal to num such that the product of its digits is divisible by t. If no such number exists, return "-1".

 

Example 1:

Input: num = "1234", t = 256

Output: "1488"

Explanation:

The smallest zero-free number that is greater than 1234 and has the product of its digits divisible by 256 is 1488, with the product of its digits equal to 256.




Example 2:

Input: num = "12355", t = 50

Output: "12355"

Explanation:

12355 is already zero-free and has the product of its digits divisible by 50, with the product of its digits equal to 150.




Example 3:

Input: num = "11111", t = 26

Output: "-1"

Explanation:

No number greater than 11111 has the product of its digits divisible by 26.

 

Constraints:

2 <= num.length <= 2 * 105
num consists only of digits in the range ['0', '9'].
num does not contain leading zeros.
1 <= t <= 1014



# Code
```cpp []
class Solution {
public:
    vector<int> allowedPrimes = {2, 3, 5, 7};

    // optimization
    int contrib[10][4] = {
        {0,0,0,0},{0,0,0,0},{1,0,0,0},{0,1,0,0},{2,0,0,0},
        {0,0,1,0},{1,1,0,0},{0,0,0,1},{3,0,0,0},{0,2,0,0}
    };

    int maxE2, maxE3, maxE5, maxE7;
    vector<vector<vector<vector<int>>>> dp; // dp[e2][e3][e5][e7] = min digits

    void buildDP(int E2, int E3, int E5, int E7) {
        maxE2 = E2; maxE3 = E3; maxE5 = E5; maxE7 = E7;
        dp.assign(E2 + 1, vector<vector<vector<int>>>(
                  E3 + 1, vector<vector<int>>(
                  E5 + 1, vector<int>(E7 + 1, INT_MAX))));

        dp[0][0][0][0] = 0;

        // iterate in increasing order of total exponent sum so we prepare teh dependencies first
        for (int s = 1; s <= E2 + E3 + E5 + E7; s++) {
            for (int e2 = 0; e2 <= E2; e2++)
            for (int e3 = 0; e3 <= E3; e3++)
            for (int e5 = 0; e5 <= E5; e5++)
            for (int e7 = 0; e7 <= E7; e7++) {
                if (e2 + e3 + e5 + e7 != s) continue;
                int best = INT_MAX;
                for (int d = 2; d <= 9; d++) {
                    // can I add this digit
                    // What it is going to contribute?
                    // means if it's 8 -> it contributes 3 2's.
                    // if 6 -> 1-2 & 1-3.
                    // and so on.
                    int ne2 = max(0, e2 - contrib[d][0]);
                    int ne3 = max(0, e3 - contrib[d][1]);
                    int ne5 = max(0, e5 - contrib[d][2]);
                    int ne7 = max(0, e7 - contrib[d][3]);
                    if (dp[ne2][ne3][ne5][ne7] != INT_MAX)
                        best = min(best, 1 + dp[ne2][ne3][ne5][ne7]);
                }
                // update the memo here.
                dp[e2][e3][e5][e7] = best;
            }
        }
    }

    int minDigits(int e2, int e3, int e5, int e7) {
        return dp[min(e2, maxE2)][min(e3, maxE3)][min(e5, maxE5)][min(e7, maxE7)];
    }

    void applyDigit(vector<int>& freq, int d) {
        // remove the contribution
        freq[2] = max(0, freq[2] - contrib[d][0]);
        freq[3] = max(0, freq[3] - contrib[d][1]);
        freq[5] = max(0, freq[5] - contrib[d][2]);
        freq[7] = max(0, freq[7] - contrib[d][3]);
    }

    bool isReqMet(vector<int>& freq) {
        for (int p : allowedPrimes) if (freq[p] > 0) return false;
        return true;
    }

    // Smallest suffix of exactly length L satisfying freq. Caller must ensure feasibility.
    string greedyFill(vector<int> freq, int L) {
        string res;
        res.reserve(L);
        for (int pos = 0; pos < L; pos++) {
            int slotsAfter = L - pos - 1;
            for (int d = 1; d <= 9; d++) {
                // Save old values
                vector<int> nf = freq;
                applyDigit(nf, d);
                if (minDigits(nf[2], nf[3], nf[5], nf[7]) <= slotsAfter) {
                    freq = nf;
                    res.push_back('0' + d);
                    break;
                }
            }
        }
        return res;
    }

    string smallestNumber(string num, long long t) {

        vector<int> freqFull(10, 0);
        for (int p : allowedPrimes) {
            while (t % p == 0) { freqFull[p]++; t /= p; }
        }
        if (t > 1) return "-1"; // not possible.

        buildDP(freqFull[2], freqFull[3], freqFull[5], freqFull[7]);

        int len = num.size();
        bool hasZero = false;
        for (char c : num) if (c == '0') { hasZero = true; break; }

        if (!hasZero) {
            vector<int> freq = freqFull;
            for (char c : num) applyDigit(freq, c - '0');
            if (isReqMet(freq)) return num;
        }

        vector<vector<int>> prefixFreq(len + 1);
        prefixFreq[0] = freqFull;
        for (int i = 0; i < len; i++) {
            prefixFreq[i + 1] = prefixFreq[i];
            if (num[i] != '0') applyDigit(prefixFreq[i + 1], num[i] - '0');
        }

        int limit = hasZero ? (int)num.find('0') : len - 1;

        string answer;
        for (int pos = limit; pos >= 0 && answer.empty(); pos--) {
            vector<int>& freqBefore = prefixFreq[pos];
            int origDigit = num[pos] - '0';
            for (int d = origDigit + 1; d <= 9; d++) {
                vector<int> nf = freqBefore;
                applyDigit(nf, d);
                int slotsAfter = len - pos - 1;
                if (minDigits(nf[2], nf[3], nf[5], nf[7]) <= slotsAfter) {
                    answer = num.substr(0, pos) + char('0' + d) + greedyFill(nf, slotsAfter);
                    break;
                }
            }
        }

        if (!answer.empty()) return answer;

        int totalNeeded = minDigits(freqFull[2], freqFull[3], freqFull[5], freqFull[7]);
        int L = max(len + 1, totalNeeded);
        return greedyFill(freqFull, L);
    }
};
```

------------------------------------------------------------------------------------------------------------------


# [3302. Find the Lexicographically Smallest Valid Sequence](https://leetcode.com/problems/find-the-lexicographically-smallest-valid-sequence/)

Medium
 
You are given two strings word1 and word2.

A string x is called almost equal to y if you can change at most one character in x to make it identical to y.

A sequence of indices seq is called valid if:

The indices are sorted in ascending order.
Concatenating the characters at these indices in word1 in the same order results in a string that is almost equal to word2.
Return an array of size word2.length representing the lexicographically smallest valid sequence of indices. If no such sequence of indices exists, return an empty array.

Note that the answer must represent the lexicographically smallest array, not the corresponding string formed by those indices.

 

Example 1:

Input: word1 = "vbcca", word2 = "abc"

Output: [0,1,2]

Explanation:

The lexicographically smallest valid sequence of indices is [0, 1, 2]:

Change word1[0] to 'a'.
word1[1] is already 'b'.
word1[2] is already 'c'.



Example 2:

Input: word1 = "bacdc", word2 = "abc"

Output: [1,2,4]

Explanation:

The lexicographically smallest valid sequence of indices is [1, 2, 4]:

word1[1] is already 'a'.
Change word1[2] to 'b'.
word1[4] is already 'c'.



Example 3:

Input: word1 = "aaaaaa", word2 = "aaabc"

Output: []

Explanation:

There is no valid sequence of indices.




Example 4:

Input: word1 = "abc", word2 = "ab"

Output: [0,1]

 

Constraints:

1 <= word2.length < word1.length <= 3 * 105
word1 and word2 consist only of lowercase English letters.


# Code
```cpp []
class Solution {
public:
    vector<int> validSequence(string word1, string word2) {
        int N = word1.size();
        int M = word2.size();
        int R = M - 1;
        int C = 0;
        vector<int> Right(N);
        for (int i = N - 1; i >= 0; i--) {
            Right[i] = C;
            if (R >= 0 && word1[i] == word2[R]) {
                R--;
                C++;
            }
        }

        vector<int> ans;
        bool changed = false;
        int j = 0;

        for (int i = 0; i < N && j < M; i++) {
            if (word1[i] == word2[j]) {
                ans.push_back(i);
                j++;
            } else if (!changed && Right[i] >= M - 1 - j) {
                ans.push_back(i);
                j++;
                changed = true;
            }
        }

        if (j == M) {
            return ans;
        }
        return {};
    }
};
```

--------------------------------------------------------------------------------------------------------------------

# [1140. Stone Game II](https://leetcode.com/problems/stone-game-ii)

Medium
 
Alice and Bob continue their games with piles of stones. There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i]. The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M. Then, we set M = max(M, X). Initially, M = 1.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

 

Example 1:

Input: piles = [2,7,9,4,4]

Output: 10

Explanation:

If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 stones in total.
If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 stones in total.
So we return 10 since it's larger.



Example 2:

Input: piles = [1,2,3,4,5,100]

Output: 104

 

Constraints:

1 <= piles.length <= 100
1 <= piles[i] <= 104


# Code
```cpp []
class Solution {
    int dfs(int i, int m, vector<int>& piles, unordered_map<int, int>& memo) {
        int n = piles.size();
        if (i + m * 2 >= n)
            return piles[i];

        int key = (i << 8) | m;
        if (memo.count(key))
            return memo[key];

        int res = 2000000000;
        for (int k = 1; k <= m * 2; k++)
            res = min(res, dfs(i + k, max(m, k), piles, memo));

        memo[key] = piles[i] - res;

        return memo[key];
    }

public:
    int stoneGameII(vector<int>& piles) {
        int n = piles.size();
        for (int i = n - 2; i >= 0; i--)
            piles[i] += piles[i + 1];

        unordered_map<int, int> memo;

        return dfs(0, 1, piles, memo);
    }
};
```

-----------------------------------------------------------------------------------------------------------------------

# [1510. Stone Game IV](https://leetcode.com/problems/stone-game-iv/description)

Hard
 
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are n stones in a pile. On each player's turn, that player makes a move consisting of removing any non-zero square number of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer n, return true if and only if Alice wins the game otherwise return false, assuming both players play optimally.

 

Example 1:

Input: n = 1

Output: true

Explanation: Alice can remove 1 stone winning the game because Bob doesn't have any moves.




Example 2:

Input: n = 2

Output: false

Explanation: Alice can only remove 1 stone, after that Bob removes the last one winning the game (2 -> 1 -> 0).



Example 3:

Input: n = 4

Output: true

Explanation: n is already a perfect square, Alice can win with one move, removing 4 stones (4 -> 0).

 

Constraints:

1 <= n <= 105


# Code
```cpp []
class Solution {
public:
    bool winnerSquareGame(int n) {
         vector<bool> dp(n + 1, false);

        for (int i = 0; i <= n; i++) {

            if (!dp[i]) {

                for (int j = 1; i + j * j <= n; j++) {
                    dp[i + j * j] = true;
                }
                if (dp[n]) {
                    return true;
                }
            }
        }

        return false;
    }
};
```

-----------------------------------------------------------------------------------------------------------------------

# 996. [Smallest Missing Integer Greater Than Sequential Prefix Sum](https://leetcode.com/problems/smallest-missing-integer-greater-than-sequential-prefix-sum)

Easy
 
You are given a 0-indexed array of integers nums.

A prefix nums[0..i] is sequential if, for all 1 <= j <= i, nums[j] = nums[j - 1] + 1. In particular, the prefix consisting only of nums[0] is sequential.

Return the smallest integer x missing from nums such that x is greater than or equal to the sum of the longest sequential prefix.

 

Example 1:

Input: nums = [1,2,3,2,5]

Output: 6

Explanation: The longest sequential prefix of nums is [1,2,3] with a sum of 6. 6 is not in the array, therefore 6 is the smallest missing integer greater than or equal to the sum of the longest sequential prefix.



Example 2:

Input: nums = [3,4,5,1,12,14,13]

Output: 15

Explanation: The longest sequential prefix of nums is [3,4,5] with a sum of 12. 12, 13, and 14 belong to the array while 15 does not. Therefore 15 is the smallest missing integer greater than or equal to the sum of the longest sequential prefix.
 


Constraints:

1 <= nums.length <= 50
1 <= nums[i] <= 50


# Code
```cpp []
class Solution {
public:
    int missingInteger(vector<int>& A) {
        int n = A.size();
        unordered_set<int> seen(A.begin(), A.end());
        int sum = A[0];

        for (int i = 1; i < n; i++) {
            if (A[i] == A[i - 1] + 1) sum += A[i];
            else break;
        }

        while (seen.count(sum))
            sum++;

        return sum;
    }
};
```

------------------------------------------------------------------------------------------


# [2958. Length of Longest Subarray With at Most K Frequency](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/)

Medium
 
You are given an integer array nums and an integer k.

The frequency of an element x is the number of times it occurs in an array.

An array is called good if the frequency of each element in this array is less than or equal to k.

Return the length of the longest good subarray of nums.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,2,3,1,2,3,1,2], k = 2

Output: 6

Explanation: The longest possible good subarray is [1,2,3,1,2,3] since the values 1, 2, and 3 occur at most twice in this subarray. Note that the subarrays [2,3,1,2,3,1] and [3,1,2,3,1,2] are also good.
It can be shown that there are no good subarrays with length more than 6.




Example 2:

Input: nums = [1,2,1,2,1,2,1,2], k = 1

Output: 2

Explanation: The longest possible good subarray is [1,2] since the values 1 and 2 occur at most once in this subarray. Note that the subarray [2,1] is also good.
It can be shown that there are no good subarrays with length more than 2.




Example 3:

Input: nums = [5,5,5,5,5,5,5], k = 4

Output: 4

Explanation: The longest possible good subarray is [5,5,5,5] since the value 5 occurs 4 times in this subarray.
It can be shown that there are no good subarrays with length more than 4.
 


Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
1 <= k <= nums.length


# Code
```cpp []
class Solution {
public:
    int maxSubarrayLength(vector<int>& nums, int k) {
        const int n=nums.size();
        int cnt=0;
        unordered_map<int, int> freq;
        freq.reserve(n);
        for (int l=0, r=0; r<n; r++){
            int x=nums[r];
            auto it=freq.find(x);
            int& f=(it==freq.end())?freq[x]=1:++(it->second);
            while (f>k)
                freq[nums[l++]]--;
        
            cnt=max(cnt,r-l+1);
        }
        return cnt;
    }
};


auto init = []() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    return 'c';
}();
```


---------------------------------------------------------------------------------------------------------------


# [2213. Longest Substring of One Repeating Character](https://leetcode.com/problems/longest-substring-of-one-repeating-character)

Hard
 
You are given a 0-indexed string s. You are also given a 0-indexed string queryCharacters of length k and a 0-indexed array of integer indices queryIndices of length k, both of which are used to describe k queries.

The ith query updates the character in s at index queryIndices[i] to the character queryCharacters[i].

Return an array lengths of length k where lengths[i] is the length of the longest substring of s consisting of only one repeating character after the ith query is performed.

 

Example 1:

Input: s = "babacc", queryCharacters = "bcb", queryIndices = [1,3,3]

Output: [3,3,4]

Explanation: 
- 1st query updates s = "bbbacc". The longest substring consisting of one repeating character is "bbb" with length 3.
- 2nd query updates s = "bbbccc". 
  The longest substring consisting of one repeating character can be "bbb" or "ccc" with length 3.
- 3rd query updates s = "bbbbcc". The longest substring consisting of one repeating character is "bbbb" with length 4.
Thus, we return [3,3,4].




Example 2:

Input: s = "abyzz", queryCharacters = "aa", queryIndices = [2,1]

Output: [2,3]

Explanation:
- 1st query updates s = "abazz". The longest substring consisting of one repeating character is "zz" with length 2.
- 2nd query updates s = "aaazz". The longest substring consisting of one repeating character is "aaa" with length 3.
Thus, we return [2,3].

 

Constraints:

1 <= s.length <= 105
s consists of lowercase English letters.
k == queryCharacters.length == queryIndices.length
1 <= k <= 105
queryCharacters consists of lowercase English letters.
0 <= queryIndices[i] < s.length



# Code
```cpp []
class Solution {
private:
    struct Node {char leftChar; char rightChar; int length; int prefix; int suffix; int best;
};

    vector<Node> tree;

    Node mergeNodes(const Node& left, const Node& right) {
        Node res;

        res.leftChar = left.leftChar;
        res.rightChar = right.rightChar;
        res.length = left.length + right.length;

        res.prefix = left.prefix;

        if (
            left.rightChar == right.leftChar &&
            left.prefix == left.length
        ) {
            res.prefix = left.length + right.prefix;
        }

        res.suffix = right.suffix;

        if (
            left.rightChar == right.leftChar &&
            right.suffix == right.length
        ) {
            res.suffix = right.length + left.suffix;
        }

        res.best = max(left.best, right.best);

        if (left.rightChar == right.leftChar) {
            res.best = max(
                res.best,
                left.suffix + right.prefix
            );
        }

        return res;
    }

    void build( int node, int start, int end, const string& s
    ) {
        if (start == end) {
            tree[node] = {s[start], s[start], 1, 1, 1, 1};
            return;
        }

        int mid = (start + end) / 2;

        build(node * 2, start, mid, s);
        build(node * 2 + 1, mid + 1, end, s);

        tree[node] = mergeNodes(
            tree[node * 2],
            tree[node * 2 + 1]
        );
    }

    void update( int node, int start, int end, int index, char ch ) {
        if (start == end) {
            tree[node] = {ch, ch, 1, 1, 1, 1};
            return;
        }

        int mid = (start + end) / 2;

        if (index <= mid) {
            update(node * 2, start, mid, index, ch);
        } else {
            update(node * 2 + 1, mid + 1, end, index, ch);
        }

        tree[node] = mergeNodes(
            tree[node * 2],
            tree[node * 2 + 1]
        );
    }

public:
    vector<int> longestRepeating( string s, string queryCharacters, vector<int>& queryIndices) {
        int n = s.size();
        tree.resize(4 * n);
        build(1, 0, n - 1, s);
        vector<int> answer;

        for (int i = 0; i < queryIndices.size(); i++) {
            update(1, 0, n - 1, queryIndices[i], queryCharacters[i]);
            answer.push_back(tree[1].best);
        }

        return answer;
    }
};
```

-----------------------------------------------------------------------------------------------------------

# [3090. Maximum Length Substring With Two Occurrences](https://leetcode.com/problems/maximum-length-substring-with-two-occurrences)

Easy
 
Given a string s, return the maximum length of a substring such that it contains at most two occurrences of each character.
 


Example 1:

Input: s = "bcbbbcba"

Output: 4

Explanation:

The following substring has a length of 4 and contains at most two occurrences of each character: "bcbbbcba".




Example 2:

Input: s = "aaaa"

Output: 2

Explanation:

The following substring has a length of 2 and contains at most two occurrences of each character: "aaaa".
 


Constraints:

2 <= s.length <= 100
s consists only of lowercase English letters.



# Code
```cpp []
class Solution {
public:
    int maximumLengthSubstring(string s) {
        int res = 0;
        int fq[26] = {0};

        for (int l = 0, r = 0; r < s.length(); r++) {
            fq[(s[r] & 31) - 1]++;

            while (fq[(s[r] & 31) - 1] > 2)
                fq[(s[l++] & 31) - 1]--;

            res = max(res, r - l + 1);
        }

        return res;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------

# [2029. Stone Game IX](https://leetcode.com/problems/stone-game-ix/)

Medium
 
Alice and Bob continue their games with stones. There is a row of n stones, and each stone has an associated value. You are given an integer array stones, where stones[i] is the value of the ith stone.

Alice and Bob take turns, with Alice starting first. On each turn, the player may remove any stone from stones. The player who removes a stone loses if the sum of the values of all removed stones is divisible by 3. Bob will win automatically if there are no remaining stones (even if it is Alice's turn).

Assuming both players play optimally, return true if Alice wins and false if Bob wins.

 

Example 1:

Input: stones = [2,1]

Output: true

Explanation: The game will be played as follows:
- Turn 1: Alice can remove either stone.
- Turn 2: Bob removes the remaining stone. 
The sum of the removed stones is 1 + 2 = 3 and is divisible by 3. Therefore, Bob loses and Alice wins the game.




Example 2:

Input: stones = [2]

Output: false

Explanation: Alice will remove the only stone, and the sum of the values on the removed stones is 2. 
Since all the stones are removed and the sum of values is not divisible by 3, Bob wins the game.



Example 3:

Input: stones = [5,1,2,4,3]

Output: false

Explanation: Bob will always win. One possible way for Bob to win is shown below:
- Turn 1: Alice can remove the second stone with value 1. Sum of removed stones = 1.
- Turn 2: Bob removes the fifth stone with value 3. Sum of removed stones = 1 + 3 = 4.
- Turn 3: Alices removes the fourth stone with value 4. Sum of removed stones = 1 + 3 + 4 = 8.
- Turn 4: Bob removes the third stone with value 2. Sum of removed stones = 1 + 3 + 4 + 2 = 10.
- Turn 5: Alice removes the first stone with value 5. Sum of removed stones = 1 + 3 + 4 + 2 + 5 = 15.
Alice loses the game because the sum of the removed stones (15) is divisible by 3. Bob wins the game.
 


Constraints:

1 <= stones.length <= 105
1 <= stones[i] <= 104


# Code
```cpp []
class Solution {
public:
    bool stoneGameIX(vector<int>& stones) {
        int f[3] = {0, 0, 0};

        for (auto& s : stones)
            f[s % 3]++;

        if (~f[0] & 1)
            return min(f[1], f[2]) >= 1;

        return abs(f[1] - f[2]) >= 3;
    }
};
```

--------------------------------------------------------------------------------------

# [3471. Find the Largest Almost Missing Integer](https://leetcode.com/problems/find-the-largest-almost-missing-integer/)

Easy

You are given an integer array nums and an integer k.

An integer x is almost missing from nums if x appears in exactly one subarray of size k within nums.

Return the largest almost missing integer from nums. If no such integer exists, return -1.

A subarray is a contiguous sequence of elements within an array.


 

Example 1:

Input: nums = [3,9,2,1,7], k = 3

Output: 7

Explanation:

1 appears in 2 subarrays of size 3: [9, 2, 1] and [2, 1, 7].
2 appears in 3 subarrays of size 3: [3, 9, 2], [9, 2, 1], [2, 1, 7].
3 appears in 1 subarray of size 3: [3, 9, 2].
7 appears in 1 subarray of size 3: [2, 1, 7].
9 appears in 2 subarrays of size 3: [3, 9, 2], and [9, 2, 1].
We return 7 since it is the largest integer that appears in exactly one subarray of size k.




Example 2:

Input: nums = [3,9,7,2,1,7], k = 4

Output: 3

Explanation:

1 appears in 2 subarrays of size 4: [9, 7, 2, 1], [7, 2, 1, 7].
2 appears in 3 subarrays of size 4: [3, 9, 7, 2], [9, 7, 2, 1], [7, 2, 1, 7].
3 appears in 1 subarray of size 4: [3, 9, 7, 2].
7 appears in 3 subarrays of size 4: [3, 9, 7, 2], [9, 7, 2, 1], [7, 2, 1, 7].
9 appears in 2 subarrays of size 4: [3, 9, 7, 2], [9, 7, 2, 1].
We return 3 since it is the largest and only integer that appears in exactly one subarray of size k.




Example 3:

Input: nums = [0,0], k = 1

Output: -1

Explanation:

There is no integer that appears in only one subarray of size 1.


 

Constraints:

1 <= nums.length <= 50
0 <= nums[i] <= 50
1 <= k <= nums.length

## code

```cpp[]
class Solution {
public:
    int largestInteger(vector<int>& A, int k) {
        int f[51] = {0};
        for (auto& x : A)
            f[x]++;

        int res = -1, n = A.size();
        for (int i = 0; i < n; i++)
            if (k == n || (f[A[i]]==1 && (k==1||!i||i==n-1)))
                res = max(res, A[i]);

        return res;
    }
};
```

---------------------------------------------------------------------------------------------------------------


# [1386. Cinema Seat Allocation](https://leetcode.com/problems/cinema-seat-allocation/description)

Medium



A cinema has n rows of seats, numbered from 1 to n. Each row has 10 seats, numbered from 1 to 10.

You are given a 2D integer array reservedSeats, where reservedSeats[i] = [rowi, seati] means that seat seati in row rowi is already reserved.

A four-person group must be assigned to four seats in the same row. The group can be seated in one of the following seat blocks:

seats 2, 3, 4, 5
seats 4, 5, 6, 7
seats 6, 7, 8, 9
A block can be used only if none of its seats are reserved. Each seat can be assigned to at most one group.

Return an integer denoting the maximum number of four-person groups that can be assigned.

 

Example 1:

Input: n = 3, reservedSeats = [[1,2],[1,3],[1,8],[2,6],[3,1],[3,10]]

Output: 4

Explanation: The figure above shows an optimal allocation of four groups. Seats marked in blue are already reserved, and each set of four contiguous seats marked in orange is assigned to one group.





Example 2:

Input: n = 2, reservedSeats = [[2,1],[1,8],[2,6]]

Output: 2




Example 3:

Input: n = 4, reservedSeats = [[4,3],[1,4],[4,6],[1,7]]

Output: 4
 

Constraints:

1 <= n <= 109
1 <= reservedSeats.length <= min(10 * n, 104)
reservedSeats[i] == [rowi, seati]
1 <= rowi <= n
1 <= seati <= 10
All reservedSeats[i] are distinct.


# Code
```cpp []
class Solution {
public:
    static int maxNumberOfFamilies(int n, vector<vector<int>>& reservedSeats) {
        const int m=reservedSeats.size();
        unordered_map<int, uint8_t> seat;
        seat.reserve(m);
        for(auto& r: reservedSeats){
            const int i=r[0]-1, j=r[1]-2;
            if (j<0 || j>=8) continue;
            seat[i]|=1<<j;
        }
        int sz=seat.size(), cnt=(n-sz)*2;
        const uint8_t A=15, B=15<<2, C=15<<4, D=A|C;
        for(auto [_, S]: seat){
            S=~S;
            bool has2=(S&D)==D, 
            has1=(!has2)&& ((S&A)==A||(S&B)==B ||(S&C)==C);
            cnt+=has2<<1;
            cnt+=has1;
        }
        return cnt;
    }
};


auto init = []() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    return 'c';
}();
```

------------------------------------------------------------------------------------------------

# [3069. Distribute Elements Into Two Arrays I](https://leetcode.com/problems/distribute-elements-into-two-arrays-i)

Easy
 
You are given a 1-indexed array of distinct integers nums of length n.

You need to distribute all the elements of nums between two arrays arr1 and arr2 using n operations. In the first operation, append nums[1] to arr1. In the second operation, append nums[2] to arr2. Afterwards, in the ith operation:

If the last element of arr1 is greater than the last element of arr2, append nums[i] to arr1. Otherwise, append nums[i] to arr2.
The array result is formed by concatenating the arrays arr1 and arr2. For example, if arr1 == [1,2,3] and arr2 == [4,5,6], then result = [1,2,3,4,5,6].

Return the array result.

 

Example 1:

Input: nums = [2,1,3]

Output: [2,3,1]

Explanation: After the first 2 operations, arr1 = [2] and arr2 = [1].
In the 3rd operation, as the last element of arr1 is greater than the last element of arr2 (2 > 1), append nums[3] to arr1.
After 3 operations, arr1 = [2,3] and arr2 = [1].
Hence, the array result formed by concatenation is [2,3,1].




Example 2:

Input: nums = [5,4,3,8]

Output: [5,3,4,8]

Explanation: After the first 2 operations, arr1 = [5] and arr2 = [4].
In the 3rd operation, as the last element of arr1 is greater than the last element of arr2 (5 > 4), append nums[3] to arr1, hence arr1 becomes [5,3].
In the 4th operation, as the last element of arr2 is greater than the last element of arr1 (4 > 3), append nums[4] to arr2, hence arr2 becomes [4,8].
After 4 operations, arr1 = [5,3] and arr2 = [4,8].
Hence, the array result formed by concatenation is [5,3,4,8].
 


Constraints:

3 <= n <= 50
1 <= nums[i] <= 100
All elements in nums are distinct.



# Code
```cpp []
class Solution {
public:
    vector<int> resultArray(vector<int>& nums) {
        vector<int> A[2]={{nums[0]}, {nums[1]}};
        const int n=nums.size();
        for(int i=2; i<n; i++){
            A[A[0].back()<=A[1].back()].push_back(nums[i]);
        }
        A[0].insert(A[0].end(), A[1].begin(), A[1].end());
        return A[0];
    }
};


auto init = []() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    return 'c';
}();
```

-----------------------------------------------------------------------------------------------------------------

# [3116. Kth Smallest Amount With Single Denomination Combination](https://leetcode.com/problems/kth-smallest-amount-with-single-denomination-combination/)

Hard

You are given an integer array coins representing coins of different denominations and an integer k.

You have an infinite number of coins of each denomination. However, you are not allowed to combine coins of different denominations.

Return the kth smallest amount that can be made using these coins.

 

Example 1:

Input: coins = [3,6,9], k = 3

Output: 9

Explanation: The given coins can make the following amounts:
Coin 3 produces multiples of 3: 3, 6, 9, 12, 15, etc.
Coin 6 produces multiples of 6: 6, 12, 18, 24, etc.
Coin 9 produces multiples of 9: 9, 18, 27, 36, etc.
All of the coins combined produce: 3, 6, 9, 12, 15, etc.



Example 2:

Input: coins = [5,2], k = 7

Output: 12

Explanation: The given coins can make the following amounts:
Coin 5 produces multiples of 5: 5, 10, 15, 20, etc.
Coin 2 produces multiples of 2: 2, 4, 6, 8, 10, 12, etc.
All of the coins combined produce: 2, 4, 5, 6, 8, 10, 12, 14, 15, etc.


 

Constraints:

1 <= coins.length <= 15
1 <= coins[i] <= 25
1 <= k <= 2 * 109
coins contains pairwise distinct integers.



# Code
```cpp []
class Solution {
public:
    using ll = long long;
    long long findKthSmallest(vector<int>& coins, int k) {
        ranges::sort(coins);
        vector<int> A;

        for (auto& c : coins)
            if (ranges::none_of(A, [&](int x) { return !(c % x); }))
                A.push_back(c);

        int n = A.size();

        auto check = [&](ll mid) {
            ll tot = 0;
            for (int i = 1; i <= n; i++) {
                int q = (1 << i) - 1;

                while (q < 1 << n) {
                    ll x = 1;
                    for (int j = 0; j < n; j++)
                        if ((q >> j) & 1)
                            x = lcm(x, A[j]);

                    tot += (mid / x) * (((i & 1) << 1) - 1);

                    int c = q & -q;
                    int r = q + c;
                    q = (((r ^ q) >> 2) / c) | r;
                }
            }
            return tot >= k;
        };

        ll low = k, high = 1ll * A[0] * k;
        return *ranges::lower_bound(views::iota(low, high + 1), true, {},
                                    [&](ll mid) { return check(mid); });
    }
};
```

-----------------------------------------------------------------------------------------------------------------


# [3622. Check Divisibility by Digit Sum and Product](https://leetcode.com/problems/check-divisibility-by-digit-sum-and-product/)

Easy
 
You are given a positive integer n. Determine whether n is divisible by the sum of the following two values:

The digit sum of n (the sum of its digits).

The digit product of n (the product of its digits).

Return true if n is divisible by this sum; otherwise, return false.

 

Example 1:

Input: n = 99

Output: true

Explanation:

Since 99 is divisible by the sum (9 + 9 = 18) plus product (9 * 9 = 81) of its digits (total 99), the output is true.

Example 2:

Input: n = 23

Output: false

Explanation:

Since 23 is not divisible by the sum (2 + 3 = 5) plus product (2 * 3 = 6) of its digits (total 11), the output is false.

 

Constraints:

1 <= n <= 106



# Code
```cpp []
class Solution {
public:
    bool checkDivisibility(int n) {
        int s=0, p=1;
        for(int x=n; x>0; x/=10){
            const int r=x%10;
            s+=r;
            p*=r;
        }
        return n%(s+p)==0;
    }
};
```

---------------------------------------------------------------------

# [1927. Sum Game](https://leetcode.com/problems/sum-game)

Medium

Alice and Bob take turns playing a game, with Alice starting first.

You are given a string num of even length consisting of digits and '?' characters. On each turn, a player will do the following if there is still at least one '?' in num:

Choose an index i where num[i] == '?'.
Replace num[i] with any digit between '0' and '9'.
The game ends when there are no more '?' characters in num.

For Bob to win, the sum of the digits in the first half of num must be equal to the sum of the digits in the second half. For Alice to win, the sums must not be equal.

For example, if the game ended with num = "243801", then Bob wins because 2+4+3 = 8+0+1. If the game ended with num = "243803", then Alice wins because 2+4+3 != 8+0+3.
Assuming Alice and Bob play optimally, return true if Alice will win and false if Bob will win.

 


Example 1:

Input: num = "5023"

Output: false

Explanation: There are no moves to be made.
The sum of the first half is equal to the sum of the second half: 5 + 0 = 2 + 3.



Example 2:

Input: num = "25??"

Output: true

Explanation: Alice can replace one of the '?'s with '9' and it will be impossible for Bob to make the sums equal.



Example 3:

Input: num = "?3295???"

Output: false

Explanation: It can be proven that Bob will always win. One possible outcome is:
- Alice replaces the first '?' with '9'. num = "93295???".
- Bob replaces one of the '?' in the right half with '9'. num = "932959??".
- Alice replaces one of the '?' in the right half with '2'. num = "9329592?".
- Bob replaces the last '?' in the right half with '7'. num = "93295927".
Bob wins because 9 + 3 + 2 + 9 = 5 + 9 + 2 + 7.

 

Constraints:

2 <= num.length <= 105
num.length is even.
num consists of only digits and '?'.


# Code
```cpp []
class Solution {
public:
    bool sumGame(string A) {
        int sum[2] = {0, 0}, q[2] = {0, 0};
        int n = A.length();

        for (int i = 0; i < n; i++) {
            int j = i / (n >> 1);
            if (A[i] == '?')
                q[j]++;
            else
                sum[j] += A[i] - '0';
        }

        if ((q[0] + q[1]) & 1) return 1;

        return (sum[0] - sum[1]) != ((q[1] - q[0]) >> 1) * 9;
    }
};
```

---------------------------------------------------------------------------------------------------------


# [1872. Stone Game VIII](https://leetcode.com/problems/stone-game-viii/)

Hard
 
Alice and Bob take turns playing a game, with Alice starting first.

There are n stones arranged in a row. On each player's turn, while the number of stones is more than one, they will do the following:

Choose an integer x > 1, and remove the leftmost x stones from the row.
Add the sum of the removed stones' values to the player's score.
Place a new stone, whose value is equal to that sum, on the left side of the row.
The game stops when only one stone is left in the row.

The score difference between Alice and Bob is (Alice's score - Bob's score). Alice's goal is to maximize the score difference, and Bob's goal is the minimize the score difference.

Given an integer array stones of length n where stones[i] represents the value of the ith stone from the left, return the score difference between Alice and Bob if they both play optimally.

 

Example 1:

Input: stones = [-1,2,-3,4,-5]

Output: 5

Explanation:
- Alice removes the first 4 stones, adds (-1) + 2 + (-3) + 4 = 2 to her score, and places a stone of
  value 2 on the left. stones = [2,-5].
- Bob removes the first 2 stones, adds 2 + (-5) = -3 to his score, and places a stone of value -3 on
  the left. stones = [-3].
The difference between their scores is 2 - (-3) = 5.



Example 2:

Input: stones = [7,-6,5,10,5,-2,-6]

Output: 13

Explanation:
- Alice removes all stones, adds 7 + (-6) + 5 + 10 + 5 + (-2) + (-6) = 13 to her score, and places a
  stone of value 13 on the left. stones = [13].
The difference between their scores is 13 - 0 = 13.



Example 3:

Input: stones = [-10,-12]

Output: -22

Explanation:
- Alice can only make one move, which is to remove both stones. She adds (-10) + (-12) = -22 to her
  score and places a stone of value -22 on the left. stones = [-22].
The difference between their scores is (-22) - 0 = -22.
 



Constraints:

n == stones.length
2 <= n <= 105
-104 <= stones[i] <= 104


# Code
```cpp []
class Solution {
public:
    int stoneGameVIII(vector<int>& A) {
        int n = A.size();
        for (int i = 1; i < n; i++)
            A[i] += A[i - 1];

        int ans = A.back();
        for (int i = n - 2; i > 0; i--)
            ans = max(ans, A[i] - ans);

        return ans;
    }
};
```

-----------------------------------------------------------------------------------------------------------------


# [3734. Lexicographically Smallest Palindromic Permutation Greater Than Target](https://leetcode.com/problems/lexicographically-smallest-palindromic-permutation-greater-than-target)

Hard
 
You are given two strings s and target, each of length n, consisting of lowercase English letters.

Return the lexicographically smallest string that is both a palindromic permutation of s and strictly greater than target. If no such permutation exists, return an empty string.

 

Example 1:

Input: s = "baba", target = "abba"

Output: "baab"

Explanation:

The palindromic permutations of s (in lexicographical order) are "abba" and "baab".
The lexicographically smallest permutation that is strictly greater than target is "baab".
Example 2:

Input: s = "baba", target = "bbaa"

Output: ""

Explanation:

The palindromic permutations of s (in lexicographical order) are "abba" and "baab".
None of them is lexicographically strictly greater than target. Therefore, the answer is "".
Example 3:

Input: s = "abc", target = "abb"

Output: ""

Explanation:

s has no palindromic permutations. Therefore, the answer is "".

Example 4:

Input: s = "aac", target = "abb"

Output: "aca"

Explanation:

The only palindromic permutation of s is "aca".
"aca" is strictly greater than target. Therefore, the answer is "aca".
 

Constraints:

1 <= n == s.length == target.length <= 300
s and target consist of only lowercase English letters.


# Code
```cpp []
class Solution {
public:
    string lexPalindromicPermutation(string s, string target) {
        int n = s.length();
        if (n == 1) {
            return s > target ? s : "";
        }

        vector<int> cnt(26, 0);
        for (char c : s) {
            cnt[c - 'a']++;
        }

      
        string oddChar = "";
        for (int i = 0; i < 26; i++) {
            if (cnt[i] % 2 == 1) {
               
                if (oddChar != "") {
                    return "";
                }
                oddChar = string(1, 'a' + i);
            }
            cnt[i] /= 2;  
        }

        string prefix = "";

        auto check = [&](char c) -> bool {
            string left = prefix;
            left.push_back(c);
            for (int i = 25; i >= 0; i--) {
                left.append(cnt[i], 'a' + i);
            }

            string palindrome = left + oddChar;
            string reversed_left = left;
            reverse(reversed_left.begin(), reversed_left.end());
            palindrome += reversed_left;

            return palindrome > target;
        };

        for (int i = 0; i < n / 2; i++) {
            bool found = false;
            for (int j = 0; j < 26; j++) {
                if (cnt[j] == 0) {
                    continue;
                }

                cnt[j]--;
                if (check('a' + j)) {
                    
                    prefix.push_back('a' + j);
                    found = true;
                    break;
                } else {
                    cnt[j]++; 
                }
            }
            if (!found) {
                return "";  
            }

            if (prefix[i] >
                target[i]) { 
                string left = prefix;
                for (int j = 0; j < 26; j++) {
                    left.append(cnt[j], 'a' + j);
                }
                string palindrome = left + oddChar;
                string reversed_left = left;
                reverse(reversed_left.begin(), reversed_left.end());
                palindrome += reversed_left;
                return palindrome;
            }
        }

        string ans = prefix + oddChar;
        string reversed_prefix = prefix;
        reverse(reversed_prefix.begin(), reversed_prefix.end());
        ans += reversed_prefix;
        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------------------

# [2091. Removing Minimum and Maximum From Array](https://leetcode.com/problems/removing-minimum-and-maximum-from-array/)

Medium
 
You are given a 0-indexed array of distinct integers nums.

There is an element in nums that has the lowest value and an element that has the highest value. We call them the minimum and maximum respectively. Your goal is to remove both these elements from the array.

A deletion is defined as either removing an element from the front of the array or removing an element from the back of the array.

Return the minimum number of deletions it would take to remove both the minimum and maximum element from the array.

 

Example 1:

Input: nums = [2,10,7,5,4,1,8,6]

Output: 5

Explanation: 
The minimum element in the array is nums[5], which is 1.
The maximum element in the array is nums[1], which is 10.
We can remove both the minimum and maximum by removing 2 elements from the front and 3 elements from the back.
This results in 2 + 3 = 5 deletions, which is the minimum number possible.




Example 2:

Input: nums = [0,-4,19,1,8,-2,-3,5]

Output: 3

Explanation: 
The minimum element in the array is nums[1], which is -4.
The maximum element in the array is nums[2], which is 19.
We can remove both the minimum and maximum by removing 3 elements from the front.
This results in only 3 deletions, which is the minimum number possible.



Example 3:

Input: nums = [101]

Output: 1

Explanation:  
There is only one element in the array, which makes it both the minimum and maximum element.
We can remove it with 1 deletion.
 


Constraints:

1 <= nums.length <= 105
-105 <= nums[i] <= 105
The integers in nums are distinct





