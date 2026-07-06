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



