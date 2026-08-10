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

# 1510. Stone Game IV

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







