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










