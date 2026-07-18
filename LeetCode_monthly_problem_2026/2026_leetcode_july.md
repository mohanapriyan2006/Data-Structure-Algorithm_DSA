# ----------------------------------------------------
# LeetCode problems - July 2026
# ----------------------------------------------------

# [3286. Find a Safe Walk Through a Grid](https://leetcode.com/problems/find-a-safe-walk-through-a-grid/)

Medium
 
You are given an m x n binary matrix grid and an integer health.

You start on the upper-left corner (0, 0) and would like to get to the lower-right corner (m - 1, n - 1).

You can move up, down, left, or right from one cell to another adjacent cell as long as your health remains positive.

Cells (i, j) with grid[i][j] = 1 are considered unsafe and reduce your health by 1.

Return true if you can reach the final cell with a health value of 1 or more, and false otherwise.

 



Example 1:

Input: grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]], health = 1

Output: true

Explanation:

![img](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_1drawio.png)

The final cell can be reached safely by walking along the gray cells below.




Example 2:

Input: grid = [[0,1,1,0,0,0],[1,0,1,0,0,0],[0,1,1,1,0,1],[0,0,1,0,1,0]], health = 3

Output: false

Explanation:

![img](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_2drawio.png)

A minimum of 4 health points is needed to reach the final cell safely.




Example 3:

Input: grid = [[1,1,1],[1,0,1],[1,1,1]], health = 5

Output: true

Explanation:

![img](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_3drawio.png)

The final cell can be reached safely by walking along the gray cells below.



Any path that does not go through the cell (1, 1) is unsafe since your health will drop to 0 when reaching the final cell.

 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 50
2 <= m * n
1 <= health <= m + n
grid[i][j] is either 0 or 1.


# Code
```cpp []
const int N=5000;
int q[N], front, back;
int maxH[N];
int d[5]={0, 1, 0, -1, 0};
class Solution {
public:
    static inline int idx(int i, int j, int c){ return i*c+j; }
    static bool outSide(int i, int j, int r, int c){
        return i<0||i>=r||j<0||j>=c;
    }
    static bool findSafeWalk(vector<vector<int>>& grid, int health) {
        const int r=grid.size(), c=grid[0].size();
        memset(maxH, -1, r*c*sizeof(int));
        front=back=N/2;
        q[back++]=0; 
        maxH[0]=health-grid[0][0];
        while(front<back){
            int ij=q[front++];
            int curH=maxH[ij];
            if (ij==r*c-1) return curH>0;
            auto [i, j]=div(ij, c);
            for(int a=0; a<4; a++){
                const int s=i+d[a], t=j+d[a+1];
                if (outSide(s, t, r, c)) continue;
                const int st=idx(s,t, c);
                int H2=curH-grid[s][t];
                if(H2>maxH[st]){
                    maxH[st]=H2;
                    if (grid[s][t]==0) q[--front]=st;
                    else q[back++]=st;
                }
            }
        }
        return 0;
    }
};
```

----------------------------------------------------------------------------------------------------------

# [3620. Network Recovery Pathways](https://leetcode.com/problems/network-recovery-pathways)
 
Hard
 
You are given a directed acyclic graph of n nodes numbered from 0 to n − 1. This is represented by a 2D array edges of length m, where edges[i] = [ui, vi, costi] indicates a one‑way communication from node ui to node vi with a recovery cost of costi.

Some nodes may be offline. You are given a boolean array online where online[i] = true means node i is online. Nodes 0 and n − 1 are always online.

A path from 0 to n − 1 is valid if:

All intermediate nodes on the path are online.
The total recovery cost of all edges on the path does not exceed k.
For each valid path, define its score as the minimum edge‑cost along that path.

Return the maximum path score (i.e., the largest minimum-edge cost) among all valid paths. If no valid path exists, return -1.

 

Example 1:

Input: edges = [[0,1,5],[1,3,10],[0,2,3],[2,3,4]], online = [true,true,true,true], k = 10

Output: 3

Explanation:

![img](https://assets.leetcode.com/uploads/2025/06/06/graph-10.png)

The graph has two possible routes from node 0 to node 3:

Path 0 → 1 → 3

Total cost = 5 + 10 = 15, which exceeds k (15 > 10), so this path is invalid.

Path 0 → 2 → 3

Total cost = 3 + 4 = 7 <= k, so this path is valid.

The minimum edge‐cost along this path is min(3, 4) = 3.

There are no other valid paths. Hence, the maximum among all valid path‐scores is 3.






Example 2:

Input: edges = [[0,1,7],[1,4,5],[0,2,6],[2,3,6],[3,4,2],[2,4,6]], online = [true,true,true,false,true], k = 12

Output: 6

Explanation:

![img](https://assets.leetcode.com/uploads/2025/06/06/graph-11.png)


Node 3 is offline, so any path passing through 3 is invalid.

Consider the remaining routes from 0 to 4:

Path 0 → 1 → 4

Total cost = 7 + 5 = 12 <= k, so this path is valid.

The minimum edge‐cost along this path is min(7, 5) = 5.

Path 0 → 2 → 3 → 4

Node 3 is offline, so this path is invalid regardless of cost.

Path 0 → 2 → 4

Total cost = 6 + 6 = 12 <= k, so this path is valid.

The minimum edge‐cost along this path is min(6, 6) = 6.

Among the two valid paths, their scores are 5 and 6. Therefore, the answer is 6.


 

Constraints:

n == online.length
2 <= n <= 5 * 104
0 <= m == edges.length <= min(105, n * (n - 1) / 2)
edges[i] = [ui, vi, costi]
0 <= ui, vi < n
ui != vi
0 <= costi <= 109
0 <= k <= 5 * 1013
online[i] is either true or false, and both online[0] and online[n − 1] are true.
The given graph is a directed acyclic graph.



# Code
```cpp []
class Solution {
public:
    int findMaxPathScore(vector<vector<int>>& edges, vector<bool>& online, long long k) {
        int n = online.size();

        vector<vector<pair<int,int>>> graph(n);
        vector<int> indegree(n, 0);

        for (auto &e : edges) {
            graph[e[0]].push_back({e[1], e[2]});
            indegree[e[1]]++;
        }

        queue<int> q;
        for (int i = 0; i < n; i++)
            if (indegree[i] == 0)
                q.push(i);

        vector<int> topo;
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            topo.push_back(u);

            for (auto &[v, w] : graph[u]) {
                if (--indegree[v] == 0)
                    q.push(v);
            }
        }

        auto check = [&](int limit) {
            const long long INF = (1LL << 60);

            vector<long long> dp(n, INF);
            dp[0] = 0;

            for (int u : topo) {

                if (dp[u] == INF)
                    continue;

                if (u != 0 && u != n - 1 && !online[u])
                    continue;

                for (auto &[v, w] : graph[u]) {

                    if (w < limit)
                        continue;

                    if (v != n - 1 && !online[v])
                        continue;

                    if (dp[u] + w < dp[v])
                        dp[v] = dp[u] + w;
                }
            }

            return dp[n - 1] <= k;
        };

        int left = 0, right = 1000000000;
        int ans = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (check(mid)) {
                ans = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------------------

# [2492. Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/)

Medium
 
You are given a positive integer n representing n cities numbered from 1 to n. You are also given a 2D array roads where roads[i] = [ai, bi, distancei] indicates that there is a bidirectional road between cities ai and bi with a distance equal to distancei. The cities graph is not necessarily connected.

The score of a path between two cities is defined as the minimum distance of a road in this path.

Return the minimum possible score of a path between cities 1 and n.

Note:

A path is a sequence of roads between two cities.
It is allowed for a path to contain the same road multiple times, and you can visit cities 1 and n multiple times along the path.
The test cases are generated such that there is at least one path between 1 and n.
 


Example 1:

![img](https://assets.leetcode.com/uploads/2022/10/12/graph11.png)

Input: n = 4, roads = [[1,2,9],[2,3,6],[2,4,5],[1,4,7]]

Output: 5

Explanation: The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 4. The score of this path is min(9,5) = 5.
It can be shown that no other path has less score.



Example 2:

![img](https://assets.leetcode.com/uploads/2022/10/12/graph22.png)

Input: n = 4, roads = [[1,2,2],[1,3,4],[3,4,7]]

Output: 2

Explanation: The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 1 -> 3 -> 4. The score of this path is min(2,2,4,7) = 2.
 



Constraints:

2 <= n <= 105
1 <= roads.length <= 105
roads[i].length == 3
1 <= ai, bi <= n
ai != bi
1 <= distancei <= 104
There are no repeated edges.
There is at least one path between 1 and n.


# Code
```cpp []
class Solution {
public:
    int minScore(int n, vector<vector<int>>& roads) {
        vector<int> root(n + 1);
        iota(root.begin(), root.end(), 0);

        auto find = [&](this auto& self, int i) -> int {
            return root[i] == i ? i : root[i] = self(root[i]);
        };

        for (auto& r : roads)
            root[find(r[0])] = find(r[1]);

        int res = 10001;
        for (auto& r : roads)
            if (find(r[0]) == find(1))
                res = min(res, r[2]);

        return res;
    }
};
```


------------------------------------------------------------------------------------------------

# [1301. Number of Paths with Max Score](https://leetcode.com/problems/number-of-paths-with-max-score)

Hard
 
You are given a square board of characters. You can move on the board starting at the bottom right square marked with the character 'S'.

You need to reach the top left square marked with the character 'E'. The rest of the squares are labeled either with a numeric character 1, 2, ..., 9 or with an obstacle 'X'. In one move you can go up, left or up-left (diagonally) only if there is no obstacle there.

Return a list of two integers: the first integer is the maximum sum of numeric characters you can collect, and the second is the number of such paths that you can take to get that maximum sum, taken modulo 10^9 + 7.

In case there is no path, return [0, 0].

 


Example 1:


Input: board = ["E23","2X2","12S"]

Output: [7,1]




Example 2:


Input: board = ["E12","1X1","21S"]

Output: [4,2]



Example 3:

Input: board = ["E11","XXX","11S"]

Output: [0,0]

 

Constraints:

2 <= board.length == board[i].length <= 100



# Code
```cpp []
class Solution {
public:
    vector<int> pathsWithMaxScore(vector<string>& board) {
        const int MOD = 1000000007;
        int n = board.size();

        vector<int> nextScore(n + 1, -1);
        vector<int> nextWays(n + 1, 0);

        for (int i = n - 1; i >= 0; --i) {
            vector<int> currScore(n + 1, -1);
            vector<int> currWays(n + 1, 0);

            for (int j = n - 1; j >= 0; --j) {
                char cell = board[i][j];

                if (cell == 'X') {
                    continue;
                }

                if (cell == 'S') {
                    currScore[j] = 0;
                    currWays[j] = 1;
                    continue;
                }

                int best = max({
                    nextScore[j],
                    currScore[j + 1],
                    nextScore[j + 1]
                });

                if (best == -1) {
                    continue;
                }

                long long ways = 0;

                if (nextScore[j] == best) {
                    ways += nextWays[j];
                }
                if (currScore[j + 1] == best) {
                    ways += currWays[j + 1];
                }
                if (nextScore[j + 1] == best) {
                    ways += nextWays[j + 1];
                }

                int value = (cell == 'E') ? 0 : cell - '0';

                currScore[j] = best + value;
                currWays[j] = ways % MOD;
            }

            nextScore = move(currScore);
            nextWays = move(currWays);
        }

        if (nextScore[0] == -1) {
            return {0, 0};
        }

        return {nextScore[0], nextWays[0]};
    }
};
```

-----------------------------------------------------------------------------------------------

# [1288. Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/)

Medium
 
Given an array intervals where intervals[i] = [li, ri] represent the interval [li, ri), remove all intervals that are covered by another interval in the list.

The interval [a, b) is covered by the interval [c, d) if and only if c <= a and b <= d.

Return the number of remaining intervals.


 

Example 1:


Input: intervals = [[1,4],[3,6],[2,8]]

Output: 2

Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.




Example 2:


Input: intervals = [[1,4],[2,3]]

Output: 1
 


Constraints:


1 <= intervals.length <= 1000
intervals[i].length == 2
0 <= li < ri <= 105
All the given intervals are unique.


# Code
```cpp []
class Solution {
public:
    int removeCoveredIntervals(vector<vector<int>>& A) {
        ranges::sort(A, {}, [](auto& x) {
            return pair{x[0], -x[1]};
        });

        int res = 0, r = 0;

        for (auto& x : A) {
            res += x[1] > r;
            r = max(r, x[1]);
        }

        return res;
    }
};
```


-------------------------------------------------------------------------------------------------------

# [3754. Concatenate Non-Zero Digits and Multiply by Sum I](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-i/)

Easy
 
You are given an integer n.

Form a new integer x by concatenating all the non-zero digits of n in their original order. If there are no non-zero digits, x = 0.

Let sum be the sum of digits in x.

Return an integer representing the value of x * sum.

 


Example 1:

Input: n = 10203004

Output: 12340

Explanation:

The non-zero digits are 1, 2, 3, and 4. Thus, x = 1234.
The sum of digits is sum = 1 + 2 + 3 + 4 = 10.
Therefore, the answer is x * sum = 1234 * 10 = 12340.




Example 2:


Input: n = 1000

Output: 1

Explanation:

The non-zero digit is 1, so x = 1 and sum = 1.
Therefore, the answer is x * sum = 1 * 1 = 1.
 


Constraints:

0 <= n <= 109


# Code
```cpp []
class Solution {
public:
    long long sumAndMultiply(int n) {
        long long x = 0, s = 0;
        for (char c : to_string(n))
            if (c != '0')
                x = x * 10 + c - '0', s += c - '0';
        return x * s;
    }
};
```

-------------------------------------------------------------------------------------------------------

# [3756. Concatenate Non-Zero Digits and Multiply by Sum II](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-ii/)

Medium
 
You are given a string s of length m consisting of digits. You are also given a 2D integer array queries, where queries[i] = [li, ri].

For each queries[i], extract the substring s[li..ri]. Then, perform the following:

Form a new integer x by concatenating all the non-zero digits from the substring in their original order. If there are no non-zero digits, x = 0.
Let sum be the sum of digits in x. The answer is x * sum.
Return an array of integers answer where answer[i] is the answer to the ith query.

Since the answers may be very large, return them modulo 109 + 7.


 

Example 1:

Input: s = "10203004", queries = [[0,7],[1,3],[4,6]]

Output: [12340, 4, 9]


Explanation:

s[0..7] = "10203004"
x = 1234
sum = 1 + 2 + 3 + 4 = 10
Therefore, answer is 1234 * 10 = 12340.
s[1..3] = "020"
x = 2
sum = 2
Therefore, the answer is 2 * 2 = 4.
s[4..6] = "300"
x = 3
sum = 3
Therefore, the answer is 3 * 3 = 9.




Example 2:


Input: s = "1000", queries = [[0,3],[1,1]]

Output: [1, 0]


Explanation:

s[0..3] = "1000"
x = 1
sum = 1
Therefore, the answer is 1 * 1 = 1.
s[1..1] = "0"
x = 0
sum = 0
Therefore, the answer is 0 * 0 = 0.




Example 3:

Input: s = "9876543210", queries = [[0,9]]

Output: [444444137]

Explanation:

s[0..9] = "9876543210"
x = 987654321
sum = 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 = 45
Therefore, the answer is 987654321 * 45 = 44444444445.
We return 44444444445 modulo (109 + 7) = 444444137.

 

Constraints:

1 <= m == s.length <= 105
s consists of digits only.
1 <= queries.length <= 105
queries[i] = [li, ri]
0 <= li <= ri < m



# Code
```cpp []
class Solution {
private:
    static constexpr int MOD = 1000000007;
    static constexpr int MAX = 100001;
    inline static int pow[MAX];

    inline static int init = []() {
        pow[0] = 1;
        for (int i = 1; i < MAX; i++)
            pow[i] = pow[i - 1] * 10LL % MOD;
        return 0;
    }();

public:
    vector<int> sumAndMultiply(string s, vector<vector<int>>& queries) {
        int n = s.length();
        
        vector<int> A(n + 1, 0);
        vector<int> B(n + 1, 0);
        vector<int> len(n + 1, 0);

        for (int i = 0; i < n; i++) {
            int d = s[i] - '0';            
            A[i + 1] = A[i] + d;
            
            if (d) {
                B[i + 1] = (B[i] * 10LL + d) % MOD;
                len[i + 1] = len[i] + 1;
            } else {
                B[i + 1] = B[i];
                len[i + 1] = len[i];
            }
        }

        vector<int> res;
        res.reserve(queries.size());

        for (auto& q : queries) {
            int l = q[0], r = q[1] + 1;

            long long sub = B[l] * 1LL * pow[len[r] - len[l]] % MOD;
            long long x = (B[r] - sub + MOD) % MOD;

            res.push_back(x * (A[r] - A[l]) % MOD);
        }

        return res;
    }
};
```

-------------------------------------------------------------------------------------------

# [3532. Path Existence Queries in a Graph I](https://leetcode.com/problems/path-existence-queries-in-a-graph-i)

Medium

You are given an integer n representing the number of nodes in a graph, labeled from 0 to n - 1.

You are also given an integer array nums of length n sorted in non-decreasing order, and an integer maxDiff.

An undirected edge exists between nodes i and j if the absolute difference between nums[i] and nums[j] is at most maxDiff (i.e., |nums[i] - nums[j]| <= maxDiff).

You are also given a 2D integer array queries. For each queries[i] = [ui, vi], determine whether there exists a path between nodes ui and vi.

Return a boolean array answer, where answer[i] is true if there exists a path between ui and vi in the ith query and false otherwise.

 


Example 1:


Input: n = 2, nums = [1,3], maxDiff = 1, queries = [[0,0],[0,1]]

Output: [true,false]

Explanation:

Query [0,0]: Node 0 has a trivial path to itself.
Query [0,1]: There is no edge between Node 0 and Node 1 because |nums[0] - nums[1]| = |1 - 3| = 2, which is greater than maxDiff.
Thus, the final answer after processing all the queries is [true, false].




Example 2:


Input: n = 4, nums = [2,5,6,8], maxDiff = 2, queries = [[0,1],[0,2],[1,3],[2,3]]

Output: [false,false,true,true]

Explanation:

The resulting graph is:

![img](https://assets.leetcode.com/uploads/2025/03/25/screenshot-2025-03-26-at-122249.png)

Query [0,1]: There is no edge between Node 0 and Node 1 because |nums[0] - nums[1]| = |2 - 5| = 3, which is greater than maxDiff.
Query [0,2]: There is no edge between Node 0 and Node 2 because |nums[0] - nums[2]| = |2 - 6| = 4, which is greater than maxDiff.
Query [1,3]: There is a path between Node 1 and Node 3 through Node 2 since |nums[1] - nums[2]| = |5 - 6| = 1 and |nums[2] - nums[3]| = |6 - 8| = 2, both of which are within maxDiff.
Query [2,3]: There is an edge between Node 2 and Node 3 because |nums[2] - nums[3]| = |6 - 8| = 2, which is equal to maxDiff.
Thus, the final answer after processing all the queries is [false, false, true, true].
 




Constraints:


1 <= n == nums.length <= 105
0 <= nums[i] <= 105
nums is sorted in non-decreasing order.
0 <= maxDiff <= 105
1 <= queries.length <= 105
queries[i] == [ui, vi]
0 <= ui, vi < n

# code
```cpp []
class Solution {
public:
    vector<bool> pathExistenceQueries(int n, vector<int>& nums, int maxDiff, vector<vector<int>>& queries) {
        vector<int> component(n, 0);
        int compNo = 0;
        for(int i = 1; i < n; i++){
            if(nums[i] - nums[i - 1] > maxDiff){
                compNo++;
            }
            component[i] = compNo;
        }
        vector<bool> sol;

        for(auto &it : queries){
            sol.push_back(component[it[0]] == component[it[1]]);
        }

        return sol;
    }
};
```


----------------------------------------------------------------------------------

# [3534. Path Existence Queries in a Graph II](https://leetcode.com/problems/path-existence-queries-in-a-graph-ii)

Hard
 
You are given an integer n representing the number of nodes in a graph, labeled from 0 to n - 1.

You are also given an integer array nums of length n and an integer maxDiff.

An undirected edge exists between nodes i and j if the absolute difference between nums[i] and nums[j] is at most maxDiff (i.e., |nums[i] - nums[j]| <= maxDiff).

You are also given a 2D integer array queries. For each queries[i] = [ui, vi], find the minimum distance between nodes ui and vi. If no path exists between the two nodes, return -1 for that query.

Return an array answer, where answer[i] is the result of the ith query.

Note: The edges between the nodes are unweighted.


 

Example 1:


Input: n = 5, nums = [1,8,3,4,2], maxDiff = 3, queries = [[0,3],[2,4]]

Output: [1,1]

Explanation:

The resulting graph is:

![img](https://assets.leetcode.com/uploads/2025/03/25/4149example1drawio.png)

Query	Shortest Path	Minimum Distance
[0, 3]	0 → 3	1
[2, 4]	2 → 4	1
Thus, the output is [1, 1].




Example 2:


Input: n = 5, nums = [5,3,1,9,10], maxDiff = 2, queries = [[0,1],[0,2],[2,3],[4,3]]

Output: [1,2,-1,1]

Explanation:

The resulting graph is:

![img](https://assets.leetcode.com/uploads/2025/03/25/4149example2drawio.png)

Query	Shortest Path	Minimum Distance
[0, 1]	0 → 1	1
[0, 2]	0 → 1 → 2	2
[2, 3]	None	-1
[4, 3]	3 → 4	1
Thus, the output is [1, 2, -1, 1].




Example 3:

Input: n = 3, nums = [3,6,1], maxDiff = 1, queries = [[0,0],[0,1],[1,2]]

Output: [0,-1,-1]

Explanation:

There are no edges between any two nodes because:

Nodes 0 and 1: |nums[0] - nums[1]| = |3 - 6| = 3 > 1
Nodes 0 and 2: |nums[0] - nums[2]| = |3 - 1| = 2 > 1
Nodes 1 and 2: |nums[1] - nums[2]| = |6 - 1| = 5 > 1
Thus, no node can reach any other node, and the output is [0, -1, -1].




 

Constraints:

1 <= n == nums.length <= 105
0 <= nums[i] <= 105
0 <= maxDiff <= 105
1 <= queries.length <= 105
queries[i] == [ui, vi]
0 <= ui, vi < n


# Code
```cpp []
constexpr int L=18, N=1e5+1;
using int2=pair<int, int>;
int up[L][N], pos[N]; 
int2 xId[N];

class Solution {
public:
    static int cnt(int u, int v, int L) {
        if (u==v) return 0;
        if (up[0][u]>=v) return 1;
        // cannot reach v
        if (up[L-1][u]<v) return -1; 

        int step=0;
        for (int j=L-1; j>=0; j--) { 
            if (up[j][u]<v) { // Only jump when satisfied
                step+=(1<<j);
                u=up[j][u];
            }
        }
        return step+1;
    }

    vector<int> pathExistenceQueries(int n, vector<int>& nums, int maxDiff, vector<vector<int>>& queries) {
        int maxL=bit_width((unsigned)n)+1;
        for(int i=0; i<n; i++) 
            xId[i]={nums[i], i};
        
        sort(xId, xId+n);
        for (int i=0; i<n; i++)// pos of index in sorted xId
            pos[xId[i].second]=i;
        
        //sliding window 
        for (int l=0, r=0; l<n; l++) {
            while (r+1<n && xId[r+1].first-xId[l].first<=maxDiff) 
                r++;
            up[0][l]=r;
        }

        // Compute binary lifting tables
        for (int j=1; j<maxL; j++) {
            for (int i=0; i<n; i++) {
                up[j][i]=up[j-1][up[j-1][i]];
            }
        }

        const int qz=queries.size();
        vector<int> ans(qz);
        int i=0;
        for (auto& q : queries) {
            auto [u, v]=minmax(pos[q[0]], pos[q[1]]);
            ans[i++]=cnt(u, v, maxL);
        }
        return ans;
    }
};
```

-------------------------------------------------------------------------------------

# [2685. Count the Number of Complete Components](https://leetcode.com/problems/count-the-number-of-complete-components/)

Medium
 
You are given an integer n. There is an undirected graph with n vertices, numbered from 0 to n - 1. You are given a 2D integer array edges where edges[i] = [ai, bi] denotes that there exists an undirected edge connecting vertices ai and bi.

Return the number of complete connected components of the graph.

A connected component is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be complete if there exists an edge between every pair of its vertices.

 


Example 1:

![img](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)

Input: n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]

Output: 3

Explanation: From the picture above, one can see that all of the components of this graph are complete.




Example 2:

![img](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)

Input: n = 6, edges = [[0,1],[0,2],[1,2],[3,4],[3,5]]

Output: 1

Explanation: The component containing vertices 0, 1, and 2 is complete since there is an edge between every pair of two vertices. On the other hand, the component containing vertices 3, 4, and 5 is not complete since there is no edge between vertices 4 and 5. Thus, the number of complete components in this graph is 1.
 



Constraints:

1 <= n <= 50
0 <= edges.length <= n * (n - 1) / 2
edges[i].length == 2
0 <= ai, bi <= n - 1
ai != bi
There are no repeated edges.



# Code
```cpp []
class Solution {
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> A(n);

        for (auto& e : edges) {
            int u = e[0], v = e[1];
            A[u].push_back(v);
            A[v].push_back(u);
        }

        bitset<51> vis;
        int res = 0;

        for (int i = 0; i < n; i++) {
            bool state = vis.test(i);

            if (!state) {
                int V = 0, E = 0;

                auto dfs = [&](auto& self, int x) -> void {
                    V++;
                    E += A[x].size();
                    vis.set(x);

                    for (auto& state : A[x])
                        if (!vis.test(state))
                            self(self, state);
                };

                dfs(dfs, i);

                res += E == V * (V - 1);
            }
        }

        return res;
    }
};
```

---------------------------------------------------------------------------------------------------------


# [1331. Rank Transform of an Array](https://leetcode.com/problems/rank-transform-of-an-array)

Easy
 
Given an array of integers arr, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

Rank is an integer starting from 1.
The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
Rank should be as small as possible.
 


Example 1:


Input: arr = [40,10,20,30]

Output: [4,1,2,3]

Explanation: 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.



Example 2:


Input: arr = [100,100,100]

Output: [1,1,1]

Explanation: Same elements share the same rank.




Example 3:


Input: arr = [37,12,28,9,100,56,80,5,12]

Output: [5,3,4,2,8,6,7,1,3]

 

Constraints:


0 <= arr.length <= 105
-109 <= arr[i] <= 109


# Code
```cpp []
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<int> s = arr;
        sort(s.begin(), s.end());
        s.erase(unique(s.begin(), s.end()), s.end());
        for (int i = 0; i < arr.size(); i++) {
            arr[i] = lower_bound(s.begin(), s.end(), arr[i]) - s.begin() + 1;
        }
        return arr;
    }
};
```

-------------------------------------------------------------------------------------------------------------------

# [1291. Sequential Digits](https://leetcode.com/problems/sequential-digits/)

Medium
 
An integer has sequential digits if and only if each digit in the number is one more than the previous digit.

Return a sorted list of all the integers in the range [low, high] inclusive that have sequential digits.


 


Example 1:


Input: low = 100, high = 300

Output: [123,234]




Example 2:


Input: low = 1000, high = 13000

Output: [1234,2345,3456,4567,5678,6789,12345]

 


Constraints:

10 <= low <= high <= 10^9



# Code
```cpp []
class Solution {
public:
    inline static int q[45];
    inline static bool init = []() {
        int n = 0;
        for (int i = 1; i < 10; i++)
            q[n++] = i;

        for (int i = 0; i < n; i++) {
            int d = q[i] % 10;
            if (d < 9) q[n++] = q[i] * 10 + d + 1;
        }

        return 0;
    }();

    vector<int> sequentialDigits(int low, int high) {
        vector<int> res;

        for (auto& x : q)
            if (x >= low && x <= high)
                res.push_back(x);

        return res;
    }
};
```

------------------------------------------------------------------------------------------


# [3336. Find the Number of Subsequences With Equal GCD](https://leetcode.com/problems/find-the-number-of-subsequences-with-equal-gcd)

Hard
 
You are given an integer array nums.

Your task is to find the number of pairs of non-empty subsequences (seq1, seq2) of nums that satisfy the following conditions:

The subsequences seq1 and seq2 are disjoint, meaning no index of nums is common between them.
The GCD of the elements of seq1 is equal to the GCD of the elements of seq2.
Return the total number of such pairs.

Since the answer may be very large, return it modulo 109 + 7.

 


Example 1:

Input: nums = [1,2,3,4]

Output: 10

Explanation:

The subsequence pairs which have the GCD of their elements equal to 1 are:

([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])
([1, 2, 3, 4], [1, 2, 3, 4])



Example 2:

Input: nums = [10,20,30]

Output: 2

Explanation:

The subsequence pairs which have the GCD of their elements equal to 10 are:

([10, 20, 30], [10, 20, 30])
([10, 20, 30], [10, 20, 30])





Example 3:

Input: nums = [1,1,1,1]

Output: 50

 

Constraints:

1 <= nums.length <= 200
1 <= nums[i] <= 200


# Code

```cpp []
class Solution {
    static constexpr int MOD = 1000000007;
    static constexpr int LIM = 201;

    static inline int μ[LIM];
    static inline int lcms[LIM][LIM];
    static inline int pow2[LIM], pow3[LIM];

    static inline int init = [] {
        for (int i = 1; i < LIM; i++)
            for (int j = 1; j < LIM; j++)
                lcms[i][j] = ::lcm(i, j);

        pow2[0] = pow3[0] = 1;

        for (int i = 1; i < LIM; i++) {
            pow2[i] = (pow2[i - 1] * 2) % MOD;
            pow3[i] = ((long long)pow3[i - 1] * 3) % MOD;
        }

        μ[1] = 1;
        for (int i = 1; i < LIM; i++)
            for (int j = i * 2; j < LIM; j += i)
                μ[j] -= μ[i];

        return false;
    }();

public:
    int subsequencePairCount(vector<int>& nums) {
        int mx = *std::max_element(nums.begin(), nums.end());

        vector<int> count(mx + 1);

        for (int n : nums)
            count[n]++;

        for (int i = 1; i <= mx; i++)
            for (int j = i * 2; j <= mx; j += i)
                count[i] += count[j];

        vector<vector<int>> dp(mx + 1, vector<int>(mx + 1));
        for (int i = 1; i <= mx; i++) {
            for (int j = 1; j <= mx; j++) {
                int l = lcms[i][j];
                int c = 0;
                if (l <= mx) c = count[l];                
                
                int ci = count[i];
                int cj = count[j];
                dp[i][j] = ((long long)pow3[c] * pow2[ci + cj - c * 2] - pow2[ci] - pow2[cj] + 1) % MOD;
            }
        }

        long long res = 0;

        for (int i = 1; i <= mx; i++)
            for (int j = 1; j <= mx / i; j++)
                for (int k = 1; k <= mx / i; k++)
                    res += (long long)μ[j] * μ[k] * dp[j * i][k * i];

        return (res % MOD + MOD) % MOD;
    }
};
```

------------------------------------------------------------------------------------------------------------------------

# [3658. GCD of Odd and Even Sums](https://leetcode.com/problems/gcd-of-odd-and-even-sums/)

Easy
 
You are given an integer n. Your task is to compute the GCD (greatest common divisor) of two values:

sumOdd: the sum of the smallest n positive odd numbers.

sumEven: the sum of the smallest n positive even numbers.

Return the GCD of sumOdd and sumEven.


 

Example 1:

Input: n = 4

Output: 4

Explanation:

Sum of the first 4 odd numbers sumOdd = 1 + 3 + 5 + 7 = 16
Sum of the first 4 even numbers sumEven = 2 + 4 + 6 + 8 = 20
Hence, GCD(sumOdd, sumEven) = GCD(16, 20) = 4.

![img](https://assets.leetcode.com/users/images/612e5247-b648-43cb-bef9-fdc587003463_1784062951.1061575.gif)



Example 2:

Input: n = 5

Output: 5

Explanation:

Sum of the first 5 odd numbers sumOdd = 1 + 3 + 5 + 7 + 9 = 25
Sum of the first 5 even numbers sumEven = 2 + 4 + 6 + 8 + 10 = 30
Hence, GCD(sumOdd, sumEven) = GCD(25, 30) = 5.

 

Constraints:

1 <= n <= 10​​​​​​​00



# Code
```cpp []
class Solution {
public:
    int gcdOfOddEvenSums(int n) { return n; }
};
```

---------------------------------------------------------------------------------------------------

# [3867. Sum of GCD of Formed Pairs](https://leetcode.com/problems/sum-of-gcd-of-formed-pairs)

Medium
 
You are given an integer array nums of length n.

Construct an array prefixGcd where for each index i:

Let mxi = max(nums[0], nums[1], ..., nums[i]).
prefixGcd[i] = gcd(nums[i], mxi).
After constructing prefixGcd:

Sort prefixGcd in non-decreasing order.
Form pairs by taking the smallest unpaired element and the largest unpaired element.
Repeat this process until no more pairs can be formed.
For each formed pair, compute the gcd of the two elements.
If n is odd, the middle element in the prefixGcd array remains unpaired and should be ignored.
Return an integer denoting the sum of the GCD values of all formed pairs.

The term gcd(a, b) denotes the greatest common divisor of a and b.
 


Example 1:

Input: nums = [2,6,4]

Output: 2

Explanation:

Construct prefixGcd:

i	nums[i]	mxi	prefixGcd[i]
0	2	2	2
1	6	6	6
2	4	6	2
prefixGcd = [2, 6, 2]. After sorting, it forms [2, 2, 6].

Pair the smallest and largest elements: gcd(2, 6) = 2. The remaining middle element 2 is ignored. Thus, the sum is 2.




Example 2:

Input: nums = [3,6,2,8]

Output: 5

Explanation:

Construct prefixGcd:

i	nums[i]	mxi	prefixGcd[i]
0	3	3	3
1	6	6	6
2	2	6	2
3	8	8	8
prefixGcd = [3, 6, 2, 8]. After sorting, it forms [2, 3, 6, 8].

Form pairs: gcd(2, 8) = 2 and gcd(3, 6) = 3. Thus, the sum is 2 + 3 = 5.

 

Constraints:

1 <= n == nums.length <= 105
1 <= nums[i] <= 10​​​​​​​9


# Code
```cpp []
class Solution {
public:
    long long gcdSum(vector<int>& A) {
        int max = 0;

        for (auto& n : A) {
            max = ::max(max, n);
            n = gcd(n, max);
        }

        ranges::sort(A);

        long long res = 0;
        for (int i = 0, j = A.size() - 1; i < j; i++, j--)
            res += gcd(A[i], A[j]);

        return res;
    }
};
```

--------------------------------------------------------------------------------------------------

# [3312. Sorted GCD Pair Queries](https://leetcode.com/problems/sorted-gcd-pair-queries/)

Hard
 
You are given an integer array nums of length n and an integer array queries.

Let gcdPairs denote an array obtained by calculating the GCD of all possible pairs (nums[i], nums[j]), where 0 <= i < j < n, and then sorting these values in ascending order.

For each query queries[i], you need to find the element at index queries[i] in gcdPairs.

Return an integer array answer, where answer[i] is the value at gcdPairs[queries[i]] for each query.

The term gcd(a, b) denotes the greatest common divisor of a and b.

 

Example 1:

Input: nums = [2,3,4], queries = [0,2,2]

Output: [1,2,2]

Explanation:

gcdPairs = [gcd(nums[0], nums[1]), gcd(nums[0], nums[2]), gcd(nums[1], nums[2])] = [1, 2, 1].

After sorting in ascending order, gcdPairs = [1, 1, 2].

So, the answer is [gcdPairs[queries[0]], gcdPairs[queries[1]], gcdPairs[queries[2]]] = [1, 2, 2].

Example 2:

Input: nums = [4,4,2,1], queries = [5,3,1,0]

Output: [4,2,1,1]

Explanation:

gcdPairs sorted in ascending order is [1, 1, 1, 2, 2, 4].

Example 3:

Input: nums = [2,2], queries = [0,0]

Output: [2,2]

Explanation:

gcdPairs = [2].

 

Constraints:

2 <= n == nums.length <= 105
1 <= nums[i] <= 5 * 104
1 <= queries.length <= 105
0 <= queries[i] < n * (n - 1) / 2


# Code
```cpp []
class Solution {
    using ll = long long;
    static const int N = 50001;
public:
    vector<int> gcdValues(vector<int>& A, vector<long long>& queries) {
        int max = 0;
        int freq[N] = {0};

        for (auto& n : A) {
            freq[n]++;
            max = ::max(max, n);
        }

        ll mFreq[N] = {0};
        ll pairs[N] = {0};
        ll overlaps[N] = {0};
        ll GCD[N] = {0};

        for (int i = max; i > 0; i--) {
            ll sum = 0, extra = 0;

            for (int j = i; j <= max; j += i) {
                sum += freq[j];
                extra += GCD[j];
            }

            mFreq[i] = sum;
            pairs[i] = sum * (sum - 1) / 2;
            overlaps[i] = extra;
            GCD[i] = pairs[i] - overlaps[i];
        }

        for (int i = 1; i <= max; i++)
            GCD[i] += GCD[i - 1];

        vector<int> res(queries.size());
        for (int i = 0; i < queries.size(); i++)
            res[i] = upper_bound(GCD, GCD + max + 1, queries[i]) - GCD;

        return res;
    }
};
```

--------------------------------------------------------------------------------

# 1979. Find Greatest Common Divisor of Array

Easy
 
Given an integer array nums, return the greatest common divisor of the smallest number and largest number in nums.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

 

Example 1:

Input: nums = [2,5,6,9,10]
Output: 2
Explanation:
The smallest number in nums is 2.
The largest number in nums is 10.
The greatest common divisor of 2 and 10 is 2.
Example 2:

Input: nums = [7,5,6,8,3]
Output: 1
Explanation:
The smallest number in nums is 3.
The largest number in nums is 8.
The greatest common divisor of 3 and 8 is 1.
Example 3:

Input: nums = [3,3]
Output: 3
Explanation:
The smallest number in nums is 3.
The largest number in nums is 3.
The greatest common divisor of 3 and 3 is 3.
 

Constraints:

2 <= nums.length <= 1000
1 <= nums[i] <= 1000








