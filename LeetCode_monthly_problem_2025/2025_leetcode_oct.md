----------------------------------------------------------------------------------------
# LeetCode problems - OCT 2025
----------------------------------------------------------------------------------------


# 407. Trapping Rain Water II -> [LeetCode](https://leetcode.com/problems/trapping-rain-water-ii/description)
 
Hard

Given an m x n integer matrix heightMap representing the height of each unit cell in a 2D elevation map, return the volume of water it can trap after raining.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]

Output: 4

Explanation: After the rain, water is trapped between the blocks.
We have two small ponds 1 and 3 units trapped.
The total volume of water trapped is 4.


Example 2:

![img](https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg)

Input: heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]

Output: 10
 

Constraints:

m == heightMap.length
n == heightMap[i].length
1 <= m, n <= 200
0 <= heightMap[i][j] <= 2 * 104


# Code
```cpp []
class Solution {
public:

    int trapRainWater(vector<vector<int>>& heights) {
        int vol = 0;
        const int M = heights.size(), N = heights[0].size();
        vector<vector<bool>> visited(M, vector<bool>(N, false));
        vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        
        auto comp = [&](const array<int, 3>& a, const array<int, 3>& b) { return a[0] >= b[0]; };
        
        // <height, row, col>
        priority_queue<array<int, 3>, vector<array<int, 3>>, decltype(comp)> min_heap(comp);
        
        // add all boundary cells
        // top and bottom cells
        for(int i = 0; i < N; i++) {
            min_heap.push({heights[0][i], 0, i}),
            min_heap.push({heights[M-1][i], M-1, i});
            visited[0][i] = true, visited[M-1][i] = true;
        }
        
        // right and left cells
        for(int i = 0; i < M; i++) {
            min_heap.push({heights[i][0], i, 0}),
            min_heap.push({heights[i][N-1], i, N-1});
            visited[i][0] = true, visited[i][N-1] = true;
        }
            
        while(!min_heap.empty()) {
            auto [height, row, col] = min_heap.top();
            min_heap.pop();
            
            // explore the neighbouring cells
            for(auto dir: directions) {
                int r = row + dir[0], c = col + dir[1];
                // process the inner cell
                if(r >= 0 && r < M && c >= 0 && c < N && !visited[r][c]) {
                    // mark current cell as visited
                    visited[r][c] = true;
                    // if current is smaller, it can store water
                    if(heights[r][c] < height)
                        vol += height - heights[r][c];
                    
                    min_heap.push({max(height, heights[r][c]), r, c});
                }
            }
        }
        return vol;
    }
};
```

--------------------------------------------------------------------------------

# 11. Container With Most Water
 
Medium
 
You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

 

Example 1:

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

Input: height = [1,8,6,2,5,4,8,3,7]

Output: 49

Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.


Example 2:

Input: height = [1,1]

Output: 1
 

Constraints:

n == height.length
2 <= n <= 105
0 <= height[i] <= 104


# Code
```cpp []
class Solution {
public:
    int maxArea(vector<int>& arr) {
        int ans = 0;
        int n = arr.size();
        int l = 0 , r = n - 1;

        while(l<r){
            ans = max(ans , min(arr[l] , arr[r]) * (r-l));
            if(arr[l] <= arr[r]) l++;
            else r--;
        }
       
        return ans;
    }
};
```

----------------------------------------------------------------------------------

# 417. Pacific Atlantic Water Flow -> [LeetCode](https://leetcode.com/problems/pacific-atlantic-water-flow/description)
 
Medium
 
There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)


Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]

Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]

Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.


Example 2:

Input: heights = [[1]]

Output: [[0,0]]

Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
 

Constraints:

m == heights.length
n == heights[r].length
1 <= m, n <= 200
0 <= heights[r][c] <= 105


# Code
```cpp []
class Solution {
public:
    int m, n;
    vector<vector<int>> directions = {{1,0}, {-1,0}, {0,1}, {0,-1}};

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        m = heights.size();
        n = heights[0].size();

        vector<vector<bool>> pacific(m, vector<bool>(n, false));
        vector<vector<bool>> atlantic(m, vector<bool>(n, false));

        for (int j = 0; j < n; j++) dfs(0, j, heights, pacific);
        for (int i = 0; i < m; i++) dfs(i, 0, heights, pacific);

        for (int j = 0; j < n; j++) dfs(m-1, j, heights, atlantic);
        for (int i = 0; i < m; i++) dfs(i, n-1, heights, atlantic);

        vector<vector<int>> result;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    result.push_back({i, j});
                }
            }
        }

        return result;
    }

    void dfs(int i, int j, vector<vector<int>>& heights, vector<vector<bool>>& visited) {
        visited[i][j] = true;
        
        for (auto& d : directions) {
            int x = i + d[0], y = j + d[1];
            
            if (x < 0 || x >= m || y < 0 || y >= n) continue;
            if (visited[x][y]) continue;
            if (heights[x][y] < heights[i][j]) continue;
            
            dfs(x, y, heights, visited);
        }
    }
};
```

----------------------------------------------------------------

# 778. Swim in Rising Water -> [LeetCode](https://leetcode.com/problems/swim-in-rising-water/description/)
 
Hard
 
You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point (i, j).

It starts raining, and water gradually rises over time. At time t, the water level is t, meaning any cell with elevation less than equal to t is submerged or reachable.

You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the minimum time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

Input: grid = [[0,2],[1,3]]

Output: 3

Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.


Example 2:

![img](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)

Input: grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]

Output: 16

Explanation: The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.
 

Constraints:

n == grid.length
n == grid[i].length
1 <= n <= 50
0 <= grid[i][j] < n2
Each value grid[i][j] is unique.


# Code
```cpp []
class Disjointset
{
private:
    vector<int> parent, size; // parent will store the parents '
                              // size will be taken to rank or compare the parent order while doing union.
public:
    Disjointset(int n)
    {
        parent.resize(n + 1);
        size.resize(n, 1);
        for (int i = 0; i < n + 1; i++)
        {
            parent[i] = i; // element will be it's parent only
        }
    }
    int find(int X)
    {
        if (parent[X] == X)
            return X;

        return parent[X] = find(parent[X]); // path compression
    }
    void UNION(int X, int Y)
    {
        int parent1 = find(X);
        int parent2 = find(Y);
        if (parent1 == parent2)
            return; // CYCLE DETECTED.
        if (size[parent1] < size[parent2])
        {
            parent[parent1] = parent2;
            size[parent2] += size[parent1];
        }
        else
        {
            parent[parent2] = parent1;
            size[parent1] += size[parent2];
        }
    }
    void printArrays()
    {
        cout << "PARENT" << endl;
        for (auto it : parent)
        {
            cout << it << " ";
        }
        cout << endl;
        cout << "SIZE" << endl;
        for (auto it : size)
        {
            cout << it << " ";
        }
    }
    int SIZE(int vertex) // returns the size of the whole disjoint set of which the current vertex is a part of.
    {
        return size[find(vertex)]; // returns the size of disjoint set that it is a part of.
    }

    int noOfdisjointSets(int n) // pass n as indexing. , gives all disjoint sets and not particular
    {
        // find the parent of all nodes.

        for (int i = 0; i < n; i++)
        {
            parent[i] = find(i);
        }

        int cnt = 0;
        for (int i = 0; i < n; i++)
        {
            if (parent[i] == i)
            {
                cnt++;
            }
        }
        return cnt;

        /*
      //   METHOD 2
        set<int> s;
        for(int i=0;i<n;i++)
        {
            s.insert(parent[i]);
        }
        return s.size();
        */
    }
};

class Solution
{
public:
    int swimInWater(vector<vector<int>> &grid)
    {
        return UnionAndFind_swimInWater(grid);
    }
    int UnionAndFind_swimInWater(vector<vector<int>> &grid)
    {
        int n = grid.size();
        Disjointset ds(n * n);
        pair<int, int> locations[n * n];                // store the location of the element reachable exactly at time t.
        vector<vector<int>> vis(n, vector<int>(n, -1)); // can use grid for this purpose also.
        for (int i = 0; i < n; i++)                     // storing the locations of elements
        {
            for (int j = 0; j < n; j++)
            {
                locations[grid[i][j]] = {i, j};
            }
        }

        int delRow[] = {0, 0, 1, -1};
        int delCol[] = {1, -1, 0, 0};
        // now the time will be in range from 0 to n*n -1
        for (int time = 0; time < n * n; time++)
        {

            // find the loc of element reachable exactly at  this time {x,y}
            int x = locations[time].first;
            int y = locations[time].second;
            // mark the current as visited
            vis[x][y] = 1;
            // check if all it's neighbours, if they have already been reached then merge with it.
            for (int k = 0; k < 4; k++)
            {
                int nrow = x + delRow[k];
                int ncol = y + delCol[k];
                if (underBoundaries(nrow, ncol, n, n) && vis[nrow][ncol] == 1)
                {
                    ds.UNION(x*n+y,nrow*n+ncol);//join the locations here and not the heights.
                }
            }
            if (ds.find(0) == ds.find(n*n-1)) // if last element location in the connected component
            {
                return time;
            }
        }

        return n * n - 1; // since in max time it will be possible to reach the desired
        // the code will itself cover this situation and does not require it here only.
        /*
         some intutive improvements in the problem:
          since time is increasing linearly,
          so any element which has lesser value than current time is already visited
          so we can say element<time then already visited.
          we can save ourselves of vis extra n square space.
        */
    }
    bool underBoundaries(int nrow, int ncol, int n, int m)
    {
        if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m)
        {
            return true;
        }
        return false;
    }
};
```

-------------------------------------------------------


# 1488. Avoid Flood in The City -> [LeetCode](https://leetcode.com/problems/avoid-flood-in-the-city/description)
 
Medium
 
Your country has an infinite number of lakes. Initially, all the lakes are empty, but when it rains over the nth lake, the nth lake becomes full of water. If it rains over a lake that is full of water, there will be a flood. Your goal is to avoid floods in any lake.

Given an integer array rains where:

rains[i] > 0 means there will be rains over the rains[i] lake.
rains[i] == 0 means there are no rains this day and you can choose one lake this day and dry it.
Return an array ans where:

ans.length == rains.length
ans[i] == -1 if rains[i] > 0.
ans[i] is the lake you choose to dry in the ith day if rains[i] == 0.
If there are multiple valid answers return any of them. If it is impossible to avoid flood return an empty array.

Notice that if you chose to dry a full lake, it becomes empty, but if you chose to dry an empty lake, nothing changes.

 

Example 1:

Input: rains = [1,2,3,4]

Output: [-1,-1,-1,-1]

Explanation: After the first day full lakes are [1]
After the second day full lakes are [1,2]
After the third day full lakes are [1,2,3]
After the fourth day full lakes are [1,2,3,4]
There's no day to dry any lake and there is no flood in any lake.


Example 2:

Input: rains = [1,2,0,0,2,1]

Output: [-1,-1,2,1,-1,-1]

Explanation: After the first day full lakes are [1]
After the second day full lakes are [1,2]
After the third day, we dry lake 2. Full lakes are [1]
After the fourth day, we dry lake 1. There is no full lakes.
After the fifth day, full lakes are [2].
After the sixth day, full lakes are [1,2].
It is easy that this scenario is flood-free. [-1,-1,1,2,-1,-1] is another acceptable scenario.


Example 3:

Input: rains = [1,2,0,1,2]

Output: []

Explanation: After the second day, full lakes are  [1,2]. We have to dry one lake in the third day.
After that, it will rain over lakes [1,2]. It's easy to prove that no matter which lake you choose to dry in the 3rd day, the other one will flood.
 

Constraints:

1 <= rains.length <= 105
0 <= rains[i] <= 109


# Code
```cpp []
class UnionFind {
public:
    vector<int> parent;

    UnionFind(int size) : parent(size + 1) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int i) {
        if (i == parent[i])
            return i;
        parent[i] = find(parent[i]);
        return parent[i];
    }

    void unite(int i) { parent[i] = find(i + 1); }
};

class Solution {
public:
    vector<int> avoidFlood(vector<int>& rain) {
        int n = rain.size();
        UnionFind uf(n + 1);
        unordered_map<int, int> map;
        vector<int> res(n, 1);

        for (int i = 0; i < n; i++) {
            int lake = rain[i];
            if (lake == 0) continue;

            res[i] = -1;
            uf.unite(i);

            if (map.find(lake) != map.end()) {
                int prev = map[lake];
                int dry = uf.find(prev + 1);

                if (dry >= i) return {};

                res[dry] = lake;
                uf.unite(dry);
                map[lake] = i;
            } else {
                map[lake] = i;
            }
        }

        return res;
    }
};
```

---------------------------------------------------------------

# 2300. Successful Pairs of Spells and Potions -> [LeetCode](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/description)
 
Medium
 
You are given two positive integer arrays spells and potions, of length n and m respectively, where spells[i] represents the strength of the ith spell and potions[j] represents the strength of the jth potion.

You are also given an integer success. A spell and potion pair is considered successful if the product of their strengths is at least success.

Return an integer array pairs of length n where pairs[i] is the number of potions that will form a successful pair with the ith spell.

 

Example 1:

Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7

Output: [4,0,3]

Explanation:
- 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
- 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
- 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.
Thus, [4,0,3] is returned.


Example 2:

Input: spells = [3,1,2], potions = [8,5,8], success = 16

Output: [2,0,2]

Explanation:
- 0th spell: 3 * [8,5,8] = [24,15,24]. 2 pairs are successful.
- 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful. 
- 2nd spell: 2 * [8,5,8] = [16,10,16]. 2 pairs are successful. 
Thus, [2,0,2] is returned.
 

Constraints:

n == spells.length
m == potions.length
1 <= n, m <= 105
1 <= spells[i], potions[i] <= 105
1 <= success <= 1010

# Code
```cpp []
class Solution {
public:
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) {
        int n = spells.size();
        int m = potions.size();
        vector<int> pairs(n, 0);
        sort(potions.begin(), potions.end());
        for (int i = 0; i < n; i++) {
            int spell = spells[i];
            int left = 0;
            int right = m - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                long long product = (long long)spell * (long long)potions[mid];
                if (product >= success) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            pairs[i] = m - left;
        }
        return pairs;
    }
};
```
------------------------------------------------


# 3494. Find the Minimum Amount of Time to Brew Potions -> [LeetCode](https://leetcode.com/problems/find-the-minimum-amount-of-time-to-brew-potions/description)
 
Medium
 
You are given two integer arrays, skill and mana, of length n and m, respectively.

In a laboratory, n wizards must brew m potions in order. Each potion has a mana capacity mana[j] and must pass through all the wizards sequentially to be brewed properly. The time taken by the ith wizard on the jth potion is timeij = skill[i] * mana[j].

Since the brewing process is delicate, a potion must be passed to the next wizard immediately after the current wizard completes their work. This means the timing must be synchronized so that each wizard begins working on a potion exactly when it arrives. ​

Return the minimum amount of time required for the potions to be brewed properly.

 

Example 1:

Input: skill = [1,5,2,4], mana = [5,1,4,2]

Output: 110

Explanation:

Potion Number	Start time	Wizard 0 done by	Wizard 1 done by	Wizard 2 done by	Wizard 3 done by
0	0	5	30	40	60
1	52	53	58	60	64
2	54	58	78	86	102
3	86	88	98	102	110
As an example for why wizard 0 cannot start working on the 1st potion before time t = 52, consider the case where the wizards started preparing the 1st potion at time t = 50. At time t = 58, wizard 2 is done with the 1st potion, but wizard 3 will still be working on the 0th potion till time t = 60.


Example 2:

Input: skill = [1,1,1], mana = [1,1,1]

Output: 5

Explanation:

Preparation of the 0th potion begins at time t = 0, and is completed by time t = 3.
Preparation of the 1st potion begins at time t = 1, and is completed by time t = 4.
Preparation of the 2nd potion begins at time t = 2, and is completed by time t = 5.


Example 3:

Input: skill = [1,2,3,4], mana = [1,2]

Output: 21

 

Constraints:

n == skill.length
m == mana.length
1 <= n, m <= 5000
1 <= mana[i], skill[i] <= 5000

# Code
```cpp []
class Solution {
public:
    long long minTime(vector<int>& skill, vector<int>& mana) {
    int n = skill.size(), m = mana.size();
    vector<long long> done(n + 1);
    for (int j = 0; j < m; ++j) {
        for (int i = 0; i < n; ++i)
            done[i + 1] = max(done[i + 1], done[i]) + mana[j] * skill[i];
        for (int i = n - 1; i > 0; --i)
            done[i] = done[i + 1] - mana[j] * skill[i];     
    }
    return done.back();
}
};
```

-----------------------------------------------------

# 3147. Taking Maximum Energy From the Mystic Dungeon -> [LeetCode](https://leetcode.com/problems/taking-maximum-energy-from-the-mystic-dungeon/description)
 
Medium
 
In a mystic dungeon, n magicians are standing in a line. Each magician has an attribute that gives you energy. Some magicians can give you negative energy, which means taking energy from you.

You have been cursed in such a way that after absorbing energy from magician i, you will be instantly transported to magician (i + k). This process will be repeated until you reach the magician where (i + k) does not exist.

In other words, you will choose a starting point and then teleport with k jumps until you reach the end of the magicians' sequence, absorbing all the energy during the journey.

You are given an array energy and an integer k. Return the maximum possible energy you can gain.

Note that when you are reach a magician, you must take energy from them, whether it is negative or positive energy.

 

Example 1:

Input: energy = [5,2,-10,-5,1], k = 3

Output: 3

Explanation: We can gain a total energy of 3 by starting from magician 1 absorbing 2 + 1 = 3.

Example 2:

Input: energy = [-2,-3,-1], k = 2

Output: -1

Explanation: We can gain a total energy of -1 by starting from magician 2.

 

Constraints:

1 <= energy.length <= 105
-1000 <= energy[i] <= 1000
1 <= k <= energy.length - 1


# Code
```cpp []
class Solution {
public:
    int maximumEnergy(vector<int>& energy, int k){
        int n = energy.size(), ans = INT_MIN;

        for(int j = n - k - 1; j >= 0; j--){
            energy[j] += energy[j + k];
        }

        for(int i = 0; i < n; i++){
            ans = max(ans, energy[i]);
        }
        return ans;
    }
};
```
---------------------------------------------------------------------

# 3186. Maximum Total Damage With Spell Casting -> [LeetCode](https://leetcode.com/problems/maximum-total-damage-with-spell-casting/description)
 
Medium
 
A magician has various spells.

You are given an array power, where each element represents the damage of a spell. Multiple spells can have the same damage value.

It is a known fact that if a magician decides to cast a spell with a damage of power[i], they cannot cast any spell with a damage of power[i] - 2, power[i] - 1, power[i] + 1, or power[i] + 2.

Each spell can be cast only once.

Return the maximum possible total damage that a magician can cast.

 

Example 1:

Input: power = [1,1,3,4]

Output: 6

Explanation:

The maximum possible damage of 6 is produced by casting spells 0, 1, 3 with damage 1, 1, 4.

Example 2:

Input: power = [7,1,6,6]

Output: 13

Explanation:

The maximum possible damage of 13 is produced by casting spells 1, 2, 3 with damage 1, 6, 6.

 

Constraints:

1 <= power.length <= 105
1 <= power[i] <= 109


# Code
```cpp []
#include <vector>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    long long maximumTotalDamage(std::vector<int>& power) {
        std::unordered_map<int, long long> damageFrequency;
        for (int damage : power) {
            damageFrequency[damage]++;
        }

        std::vector<int> uniqueDamages;
        for (const auto& pair : damageFrequency) {
            uniqueDamages.push_back(pair.first);
        }
        std::sort(uniqueDamages.begin(), uniqueDamages.end());

        int totalUniqueDamages = uniqueDamages.size();
        std::vector<long long> maxDamageDP(totalUniqueDamages, 0);

        maxDamageDP[0] = uniqueDamages[0] * damageFrequency[uniqueDamages[0]];

        for (int i = 1; i < totalUniqueDamages; i++) {
            int currentDamageValue = uniqueDamages[i];
            long long currentDamageTotal = currentDamageValue * damageFrequency[currentDamageValue];

            maxDamageDP[i] = maxDamageDP[i - 1];

            int previousIndex = i - 1;
            while (previousIndex >= 0 && 
                   (uniqueDamages[previousIndex] == currentDamageValue - 1 || 
                    uniqueDamages[previousIndex] == currentDamageValue - 2 || 
                    uniqueDamages[previousIndex] == currentDamageValue + 1 || 
                    uniqueDamages[previousIndex] == currentDamageValue + 2)) {
                previousIndex--;
            }

            if (previousIndex >= 0) {
                maxDamageDP[i] = std::max(maxDamageDP[i], maxDamageDP[previousIndex] + currentDamageTotal);
            } else {
                maxDamageDP[i] = std::max(maxDamageDP[i], currentDamageTotal);
            }
        }

        return maxDamageDP[totalUniqueDamages - 1];
    }
};
```

--------------------------------------------------------------------------

# 3539. Find Sum of Array Product of Magical Sequences -> [LeetCode](https://leetcode.com/problems/find-sum-of-array-product-of-magical-sequences/description)
 
Hard
 
You are given two integers, m and k, and an integer array nums.

A sequence of integers seq is called magical if:
seq has a size of m.
0 <= seq[i] < nums.length
The binary representation of 2seq[0] + 2seq[1] + ... + 2seq[m - 1] has k set bits.
The array product of this sequence is defined as prod(seq) = (nums[seq[0]] * nums[seq[1]] * ... * nums[seq[m - 1]]).

Return the sum of the array products for all valid magical sequences.

Since the answer may be large, return it modulo 109 + 7.

A set bit refers to a bit in the binary representation of a number that has a value of 1.

 

Example 1:

Input: m = 5, k = 5, nums = [1,10,100,10000,1000000]

Output: 991600007

Explanation:

All permutations of [0, 1, 2, 3, 4] are magical sequences, each with an array product of 1013.

Example 2:

Input: m = 2, k = 2, nums = [5,4,3,2,1]

Output: 170

Explanation:

The magical sequences are [0, 1], [0, 2], [0, 3], [0, 4], [1, 0], [1, 2], [1, 3], [1, 4], [2, 0], [2, 1], [2, 3], [2, 4], [3, 0], [3, 1], [3, 2], [3, 4], [4, 0], [4, 1], [4, 2], and [4, 3].

Example 3:

Input: m = 1, k = 1, nums = [28]

Output: 28

Explanation:

The only magical sequence is [0].

 

Constraints:

1 <= k <= m <= 30
1 <= nums.length <= 50
1 <= nums[i] <= 108

# Code
```cpp []
static constexpr int MOD=1e9+7;
static int C[31][31]={{0}}; 
static int dp[31][31][50][31];
class Solution {
    int m, k, n;
    void Pascal(){ 
        if (C[0][0]==1) return;// compute only once 
        for(int i=1; i<=30; i++){ 
            C[i][0]=C[i][i]=1; 
            for(int j=1; j<=i/2; j++){ 
                const int Cij=C[i-1][j-1]+C[i-1][j]; 
                // if (Cij>=mod) Cij-=mod; 
                C[i][j]=C[i][i-j]=Cij; 
                // cout<<Cij<<","; } // cout<<endl; 
            } 
        }
    }
   
    int dfs(int m, int k, int i, unsigned flag, vector<int>& nums) {
        const int bz=popcount(flag);
        if (m<0 || k<0 || m+bz<k)
            return 0;
        if (m==0)
            return (k==bz)?1:0;
        if (i>=n) return 0;

        if (dp[m][k][i][flag]!=-1) return dp[m][k][i][flag];

        long long ans=0, powX=1;
        const int x=nums[i];
        for (int f=0; f<=m; f++) {
            long long perm=C[m][f]*powX%MOD;

            unsigned newFlag=flag+f;
            unsigned nextFlag=newFlag>>1;
            bool bitSet=newFlag&1;

            ans=(ans+perm*dfs(m-f, k-bitSet, i+1, nextFlag, nums))%MOD;
            powX=(powX*x)%MOD;
        }

        return dp[m][k][i][flag]=ans;
    }

public:
    int magicalSum(int m, int k, vector<int>& nums) {
        Pascal();
        this->m=m;
        this->k=k; 
        n=nums.size();
        for(int i=0; i<=m; i++)
            for(int j=0; j<=m; j++)
                for(int s=0; s<n; s++)
                    memset(dp[i][j][s], -1, sizeof(int)*(m+1));

        return dfs(m, k, 0, 0, nums);
    }
};


```

-----------------------------------------------------------------------------------------------

# 2273. Find Resultant Array After Removing Anagrams -> [LeetCode](https://leetcode.com/problems/find-resultant-array-after-removing-anagrams/description)
 
Easy
 
You are given a 0-indexed string array words, where words[i] consists of lowercase English letters.

In one operation, select any index i such that 0 < i < words.length and words[i - 1] and words[i] are anagrams, and delete words[i] from words. Keep performing this operation as long as you can select an index that satisfies the conditions.

Return words after performing all operations. It can be shown that selecting the indices for each operation in any arbitrary order will lead to the same result.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase using all the original letters exactly once. For example, "dacb" is an anagram of "abdc".

 

Example 1:

Input: words = ["abba","baba","bbaa","cd","cd"]

Output: ["abba","cd"]

Explanation:
One of the ways we can obtain the resultant array is by using the following operations:
- Since words[2] = "bbaa" and words[1] = "baba" are anagrams, we choose index 2 and delete words[2].
  Now words = ["abba","baba","cd","cd"].
- Since words[1] = "baba" and words[0] = "abba" are anagrams, we choose index 1 and delete words[1].
  Now words = ["abba","cd","cd"].
- Since words[2] = "cd" and words[1] = "cd" are anagrams, we choose index 2 and delete words[2].
  Now words = ["abba","cd"].
We can no longer perform any operations, so ["abba","cd"] is the final answer.


Example 2:

Input: words = ["a","b","c","d","e"]

Output: ["a","b","c","d","e"]

Explanation:
No two adjacent strings in words are anagrams of each other, so no operations are performed.
 

Constraints:

1 <= words.length <= 100
1 <= words[i].length <= 10
words[i] consists of lowercase English letters.


# Code
```cpp []
class Solution {
public:
    vector<string> removeAnagrams(vector<string>& words) {
        vector<string> res;
        res.push_back(words[0]);

        for(int i=1 ; i<words.size() ; ++i){
            string a = words[i] , b = res.back();
            sort(a.begin() , a.end()); sort(b.begin() , b.end());
            if(a != b) res.push_back(words[i]);
        }

        return res;
    }
};
```

----------------------------------------------------------------------------------------------


# 3349. Adjacent Increasing Subarrays Detection I -> [LeetCode](https://leetcode.com/problems/adjacent-increasing-subarrays-detection-i/description/)
 
Easy
 
Given an array nums of n integers and an integer k, determine whether there exist two adjacent subarrays of length k such that both subarrays are strictly increasing. Specifically, check if there are two subarrays starting at indices a and b (a < b), where:

Both subarrays nums[a..a + k - 1] and nums[b..b + k - 1] are strictly increasing.
The subarrays must be adjacent, meaning b = a + k.
Return true if it is possible to find two such subarrays, and false otherwise.

 

Example 1:

Input: nums = [2,5,7,8,9,2,3,4,3,1], k = 3

Output: true

Explanation:

The subarray starting at index 2 is [7, 8, 9], which is strictly increasing.
The subarray starting at index 5 is [2, 3, 4], which is also strictly increasing.
These two subarrays are adjacent, so the result is true.


Example 2:

Input: nums = [1,2,3,4,4,4,4,5,6,7], k = 5

Output: false

 

Constraints:

2 <= nums.length <= 100
1 < 2 * k <= nums.length
-1000 <= nums[i] <= 1000

# Code
```cpp []
class Solution {
public:
    bool hasIncreasingSubarrays(vector<int>& nums, int k) {
        int n = nums.size(), inc = 1, prevInc = 0, maxLen = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) inc++;
            else {
                prevInc = inc;
                inc = 1;
            }
            maxLen = max(maxLen, max(inc >> 1, min(prevInc, inc)));
            if (maxLen >= k) return true;
        }
        return false;
    }
};
```

-----------------------------------------------------------------------------------------------------

# 3350. Adjacent Increasing Subarrays Detection II -> [LeetCode](https://leetcode.com/problems/adjacent-increasing-subarrays-detection-ii/description)
 
Medium
 
Given an array nums of n integers, your task is to find the maximum value of k for which there exist two adjacent subarrays of length k each, such that both subarrays are strictly increasing. Specifically, check if there are two subarrays of length k starting at indices a and b (a < b), where:

Both subarrays nums[a..a + k - 1] and nums[b..b + k - 1] are strictly increasing.
The subarrays must be adjacent, meaning b = a + k.
Return the maximum possible value of k.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [2,5,7,8,9,2,3,4,3,1]

Output: 3

Explanation:

The subarray starting at index 2 is [7, 8, 9], which is strictly increasing.
The subarray starting at index 5 is [2, 3, 4], which is also strictly increasing.
These two subarrays are adjacent, and 3 is the maximum possible value of k for which two such adjacent strictly increasing subarrays exist.



Example 2:

Input: nums = [1,2,3,4,4,4,4,5,6,7]

Output: 2

Explanation:

The subarray starting at index 0 is [1, 2], which is strictly increasing.
The subarray starting at index 2 is [3, 4], which is also strictly increasing.
These two subarrays are adjacent, and 2 is the maximum possible value of k for which two such adjacent strictly increasing subarrays exist.
 

Constraints:

2 <= nums.length <= 2 * 105
-109 <= nums[i] <= 109


# Code
```cpp []
class Solution {
public:
    int maxIncreasingSubarrays(vector<int>& nums) {
        int n = nums.size();
        int up = 1, preUp = 0, res = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) up++;
            else {
                preUp = up;
                up = 1;
            }
            int half = up >> 1;
            int m = min(preUp, up);
            int candidate = max(half, m);
            if (candidate > res) res = candidate;
        }
        return res;
    }
};
```
-------------------------------------------------------------------------------------------------

# 2598. Smallest Missing Non-negative Integer After Operations -> [LeetCode](https://leetcode.com/problems/smallest-missing-non-negative-integer-after-operations/description)
 
Medium
 
You are given a 0-indexed integer array nums and an integer value.

In one operation, you can add or subtract value from any element of nums.

For example, if nums = [1,2,3] and value = 2, you can choose to subtract value from nums[0] to make nums = [-1,2,3].
The MEX (minimum excluded) of an array is the smallest missing non-negative integer in it.

For example, the MEX of [-1,2,3] is 0 while the MEX of [1,0,3] is 2.
Return the maximum MEX of nums after applying the mentioned operation any number of times.

 

Example 1:

Input: nums = [1,-10,7,13,6,8], value = 5

Output: 4

Explanation: One can achieve this result by applying the following operations:
- Add value to nums[1] twice to make nums = [1,0,7,13,6,8]
- Subtract value from nums[2] once to make nums = [1,0,2,13,6,8]
- Subtract value from nums[3] twice to make nums = [1,0,2,3,6,8]
The MEX of nums is 4. It can be shown that 4 is the maximum MEX we can achieve.


Example 2:

Input: nums = [1,-10,7,13,6,8], value = 7

Output: 2

Explanation: One can achieve this result by applying the following operation:
- subtract value from nums[2] once to make nums = [1,-10,0,13,6,8]
The MEX of nums is 2. It can be shown that 2 is the maximum MEX we can achieve.
 

Constraints:

1 <= nums.length, value <= 105
-109 <= nums[i] <= 109

# Code
```cpp []
class Solution {
public:
	int findSmallestInteger(vector<int>& nums, int v) {
		long long n = nums.size(), x, res = 0;
		vector<int> rem(v, 0);
		for (int i = 0; i < n; i++) {
			x = ((nums[i] % v) + v) % v;
			rem[x]++;
		}

		while (rem[res % v]--) res++;
		return res;
	}
};
```

-------------------------------------------------------------------------------------------------

# 3003. Maximize the Number of Partitions After Operations -> [LeetCode](https://leetcode.com/problems/maximize-the-number-of-partitions-after-operations/description/)
 
Hard
 
You are given a string s and an integer k.

First, you are allowed to change at most one index in s to another lowercase English letter.

After that, do the following partitioning operation until s is empty:

Choose the longest prefix of s containing at most k distinct characters.
Delete the prefix from s and increase the number of partitions by one. The remaining characters (if any) in s maintain their initial order.
Return an integer denoting the maximum number of resulting partitions after the operations by optimally choosing at most one index to change.

 

Example 1:

Input: s = "accca", k = 2

Output: 3

Explanation:

The optimal way is to change s[2] to something other than a and c, for example, b. then it becomes "acbca".

Then we perform the operations:

The longest prefix containing at most 2 distinct characters is "ac", we remove it and s becomes "bca".
Now The longest prefix containing at most 2 distinct characters is "bc", so we remove it and s becomes "a".
Finally, we remove "a" and s becomes empty, so the procedure ends.
Doing the operations, the string is divided into 3 partitions, so the answer is 3.


Example 2:

Input: s = "aabaab", k = 3

Output: 1

Explanation:

Initially s contains 2 distinct characters, so whichever character we change, it will contain at most 3 distinct characters, so the longest prefix with at most 3 distinct characters would always be all of it, therefore the answer is 1.

Example 3:

Input: s = "xxyz", k = 1

Output: 4

Explanation:

The optimal way is to change s[0] or s[1] to something other than characters in s, for example, to change s[0] to w.

Then s becomes "wxyz", which consists of 4 distinct characters, so as k is 1, it will divide into 4 partitions.

 

Constraints:

1 <= s.length <= 104
s consists only of lowercase English letters.
1 <= k <= 26

# Code
```cpp []
class Solution {
public:
    unordered_map<long long, int> cache;
    string s;
    int k;

    int maxPartitionsAfterOperations(string s, int k) {
        this->s = s;
        this->k = k;
        return dp(0, 0, true) + 1;
    }

private:
    int dp(long long index, long long currentSet, bool canChange) {
        long long key = (index << 27) | (currentSet << 1) | canChange;

        if (cache.count(key)) {
            return cache[key];
        }

        if (index == s.size()) {
            return 0;
        }

        int characterIndex = s[index] - 'a';
        int currentSetUpdated = currentSet | (1 << characterIndex);
        int distinctCount = __builtin_popcount(currentSetUpdated);

        int res;
        if (distinctCount > k) {
            res = 1 + dp(index + 1, 1 << characterIndex, canChange);
        } else {
            res = dp(index + 1, currentSetUpdated, canChange);
        }

        if (canChange) {
            for (int newCharIndex = 0; newCharIndex < 26; ++newCharIndex) {
                int newSet = currentSet | (1 << newCharIndex);
                int newDistinctCount = __builtin_popcount(newSet);

                if (newDistinctCount > k) {
                    res = max(res, 1 + dp(index + 1, 1 << newCharIndex, false));
                } else {
                    res = max(res, dp(index + 1, newSet, false));
                }
            }
        }

        return cache[key] = res;
    }
};
```

------------------------------------------------------------------------------------------------------------

# 3397. Maximum Number of Distinct Elements After Operations -> [LeetCode](https://leetcode.com/problems/maximum-number-of-distinct-elements-after-operations/description)
 
Medium
 
You are given an integer array nums and an integer k.

You are allowed to perform the following operation on each element of the array at most once:

Add an integer in the range [-k, k] to the element.
Return the maximum possible number of distinct elements in nums after performing the operations.

 

Example 1:

Input: nums = [1,2,2,3,3,4], k = 2

Output: 6

Explanation:

nums changes to [-1, 0, 1, 2, 3, 4] after performing operations on the first four elements.

Example 2:

Input: nums = [4,4,4,4], k = 1

Output: 3

Explanation:

By adding -1 to nums[0] and 1 to nums[1], nums changes to [3, 5, 4, 4].

 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
0 <= k <= 109

# Code
```cpp []
class Solution {
public:
    int maxDistinctElements(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = 0, prev = -1e9;

        for (int i = 0; i < nums.size(); i++) {
            int l = max(nums[i] - k, prev + 1);
            if (l <= nums[i] + k) {
                prev = l, ans++;
            }
        }
        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------------------

# 1625. Lexicographically Smallest String After Applying Operations -> [LeetCode](https://leetcode.com/problems/lexicographically-smallest-string-after-applying-operations/description)
 
Medium
 
You are given a string s of even length consisting of digits from 0 to 9, and two integers a and b.

You can apply either of the following two operations any number of times and in any order on s:

Add a to all odd indices of s (0-indexed). Digits post 9 are cycled back to 0. For example, if s = "3456" and a = 5, s becomes "3951".
Rotate s to the right by b positions. For example, if s = "3456" and b = 1, s becomes "6345".
Return the lexicographically smallest string you can obtain by applying the above operations any number of times on s.

A string a is lexicographically smaller than a string b (of the same length) if in the first position where a and b differ, string a has a letter that appears earlier in the alphabet than the corresponding letter in b. For example, "0158" is lexicographically smaller than "0190" because the first position they differ is at the third letter, and '5' comes before '9'.

 

Example 1:

Input: s = "5525", a = 9, b = 2

Output: "2050"

Explanation: We can apply the following operations:
Start:  "5525"
Rotate: "2555"
Add:    "2454"
Add:    "2353"
Rotate: "5323"
Add:    "5222"
Add:    "5121"
Rotate: "2151"
Add:    "2050"​​​​​
There is no way to obtain a string that is lexicographically smaller than "2050".


Example 2:

Input: s = "74", a = 5, b = 1

Output: "24"

Explanation: We can apply the following operations:
Start:  "74"
Rotate: "47"
​​​​​​​Add:    "42"
​​​​​​​Rotate: "24"​​​​​​​​​​​​
There is no way to obtain a string that is lexicographically smaller than "24".


Example 3:

Input: s = "0011", a = 4, b = 2

Output: "0011"

Explanation: There are no sequence of operations that will give us a lexicographically smaller string than "0011".
 

Constraints:

2 <= s.length <= 100
s.length is even.
s consists of digits from 0 to 9 only.
1 <= a <= 9
1 <= b <= s.length - 1

# Code
```cpp []
class Solution {
public:
    string findLexSmallestString(string s, int a, int b) {
        unordered_set<string> vis;
        string smallest = s;
        queue<string> q;
        q.push(s);
        vis.insert(s);

        while (!q.empty()) {
            string cur = q.front(); q.pop();
            if (cur < smallest) smallest = cur;

            string added = cur;
            for (int i = 1; i < added.size(); i += 2)
                added[i] = ((added[i] - '0' + a) % 10) + '0';
            if (!vis.count(added)) {
                vis.insert(added);
                q.push(added);
            }

            string rotated = cur.substr(cur.size() - b) + cur.substr(0, cur.size() - b);
            if (!vis.count(rotated)) {
                vis.insert(rotated);
                q.push(rotated);
            }
        }
        return smallest;
    }
};
```

-----------------------------------------------------------------------------------------------------------------

# 2011. Final Value of Variable After Performing Operations -> [LeetCode](https://leetcode.com/problems/final-value-of-variable-after-performing-operations/description)
 
Easy
 
There is a programming language with only four operations and one variable X:

++X and X++ increments the value of the variable X by 1.
--X and X-- decrements the value of the variable X by 1.
Initially, the value of X is 0.

Given an array of strings operations containing a list of operations, return the final value of X after performing all the operations.

 

Example 1:

Input: operations = ["--X","X++","X++"]

Output: 1

Explanation: The operations are performed as follows:
Initially, X = 0.
--X: X is decremented by 1, X =  0 - 1 = -1.
X++: X is incremented by 1, X = -1 + 1 =  0.
X++: X is incremented by 1, X =  0 + 1 =  1.


Example 2:

Input: operations = ["++X","++X","X++"]

Output: 3

Explanation: The operations are performed as follows:
Initially, X = 0.
++X: X is incremented by 1, X = 0 + 1 = 1.
++X: X is incremented by 1, X = 1 + 1 = 2.
X++: X is incremented by 1, X = 2 + 1 = 3.


Example 3:

Input: operations = ["X++","++X","--X","X--"]

Output: 0

Explanation: The operations are performed as follows:
Initially, X = 0.
X++: X is incremented by 1, X = 0 + 1 = 1.
++X: X is incremented by 1, X = 1 + 1 = 2.
--X: X is decremented by 1, X = 2 - 1 = 1.
X--: X is decremented by 1, X = 1 - 1 = 0.
 

Constraints:

1 <= operations.length <= 100
operations[i] will be either "++X", "X++", "--X", or "X--".

# Code
```cpp []
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int x=0;
        for(auto& op: operations)
            x+=(op[1]=='+')?1:-1;
        return x;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------

# 3346. Maximum Frequency of an Element After Performing Operations I -> [LeetCode](https://leetcode.com/problems/maximum-frequency-of-an-element-after-performing-operations-i/description)
 
Medium
 
You are given an integer array nums and two integers k and numOperations.

You must perform an operation numOperations times on nums, where in each operation you:

Select an index i that was not selected in any previous operations.
Add an integer in the range [-k, k] to nums[i].
Return the maximum possible frequency of any element in nums after performing the operations.

 

Example 1:

Input: nums = [1,4,5], k = 1, numOperations = 2

Output: 2

Explanation:

We can achieve a maximum frequency of two by:

Adding 0 to nums[1]. nums becomes [1, 4, 5].
Adding -1 to nums[2]. nums becomes [1, 4, 4].
Example 2:

Input: nums = [5,11,20,20], k = 5, numOperations = 1

Output: 2

Explanation:

We can achieve a maximum frequency of two by:

Adding 0 to nums[1].
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 105
0 <= k <= 105
0 <= numOperations <= nums.length


# Code
```cpp []
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k, int numOperations) {
        int maxVal = *max_element(nums.begin(), nums.end()) + k + 2;
        int* count = new int[maxVal]();

        for (int v : nums)
            count[v]++;

        for (int i = 1; i < maxVal; i++)
            count[i] += count[i - 1];

        int res = 0;
        for (int i = 0; i < maxVal; i++) {
            int left = max(0, i - k);
            int right = min(maxVal - 1, i + k);
            int total = count[right] - (left ? count[left - 1] : 0);
            int freq = count[i] - (i ? count[i - 1] : 0);
            res = max(res, freq + min(numOperations, total - freq));
        }

        return res;
    }
};

```

-------------------------------------------------------------------------------------------------------------------

# 3347. Maximum Frequency of an Element After Performing Operations II -> [LeetCode](https://leetcode.com/problems/maximum-frequency-of-an-element-after-performing-operations-ii/description)
 
Hard
 
You are given an integer array nums and two integers k and numOperations.

You must perform an operation numOperations times on nums, where in each operation you:

Select an index i that was not selected in any previous operations.
Add an integer in the range [-k, k] to nums[i].
Return the maximum possible frequency of any element in nums after performing the operations.

 

Example 1:

Input: nums = [1,4,5], k = 1, numOperations = 2

Output: 2

Explanation:

We can achieve a maximum frequency of two by:

Adding 0 to nums[1], after which nums becomes [1, 4, 5].
Adding -1 to nums[2], after which nums becomes [1, 4, 4].


Example 2:

Input: nums = [5,11,20,20], k = 5, numOperations = 1

Output: 2

Explanation:

We can achieve a maximum frequency of two by:

Adding 0 to nums[1].
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
0 <= k <= 109
0 <= numOperations <= nums.length

# Code
```cpp []
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k, int numOperations) {
        map<int,int>mp;
        unordered_map<int,int>hash;
        set<int>points;
        for(auto &it:nums){
            hash[it]++;
            mp[it-k]++;
            mp[it+k+1]--;
            points.insert(it);
            
            points.insert(it-k);
            points.insert(it+k+1);
        }
        int ans=1;
        int sum=0;
        for(auto &it:points){
            sum+=mp[it];
            ans=max(ans,hash[it]+min(sum-hash[it],numOperations));
        }
        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------------

# 3461. Check If Digits Are Equal in String After Operations I -> [LeetCode](https://leetcode.com/problems/check-if-digits-are-equal-in-string-after-operations-i/description/)
 
Easy
 
You are given a string s consisting of digits. Perform the following operation repeatedly until the string has exactly two digits:

For each pair of consecutive digits in s, starting from the first digit, calculate a new digit as the sum of the two digits modulo 10.
Replace s with the sequence of newly calculated digits, maintaining the order in which they are computed.
Return true if the final two digits in s are the same; otherwise, return false.

 

Example 1:

Input: s = "3902"

Output: true

Explanation:

Initially, s = "3902"
First operation:
(s[0] + s[1]) % 10 = (3 + 9) % 10 = 2
(s[1] + s[2]) % 10 = (9 + 0) % 10 = 9
(s[2] + s[3]) % 10 = (0 + 2) % 10 = 2
s becomes "292"
Second operation:
(s[0] + s[1]) % 10 = (2 + 9) % 10 = 1
(s[1] + s[2]) % 10 = (9 + 2) % 10 = 1
s becomes "11"
Since the digits in "11" are the same, the output is true.


Example 2:

Input: s = "34789"

Output: false

Explanation:

Initially, s = "34789".
After the first operation, s = "7157".
After the second operation, s = "862".
After the third operation, s = "48".
Since '4' != '8', the output is false.
 

Constraints:

3 <= s.length <= 100
s consists of only digits.


# Code
```cpp []
class Solution {
public:
    bool hasSameDigits(string s) {
        while (s.size() > 2) {
            string t;
            for (size_t i = 0; i + 1 < s.size(); i++) {
                t += ((s[i] - '0' + s[i + 1] - '0') % 10) + '0';
            }
            s = t;
        }
        return s[0] == s[1];
    }
};
```

----------------------------------------------------------------------------------------------------------

# 2048. Next Greater Numerically Balanced Number -> [LeetCode](https://leetcode.com/problems/next-greater-numerically-balanced-number/description/)
 
Medium
 
An integer x is numerically balanced if for every digit d in the number x, there are exactly d occurrences of that digit in x.

Given an integer n, return the smallest numerically balanced number strictly greater than n.

 

Example 1:

Input: n = 1

Output: 22

Explanation: 
22 is numerically balanced since:
- The digit 2 occurs 2 times. 
It is also the smallest numerically balanced number strictly greater than 1.


Example 2:

Input: n = 1000

Output: 1333

Explanation: 
1333 is numerically balanced since:
- The digit 1 occurs 1 time.
- The digit 3 occurs 3 times. 
It is also the smallest numerically balanced number strictly greater than 1000.
Note that 1022 cannot be the answer because 0 appeared more than 0 times.


Example 3:

Input: n = 3000

Output: 3133

Explanation: 
3133 is numerically balanced since:
- The digit 1 occurs 1 time.
- The digit 3 occurs 3 times.
It is also the smallest numerically balanced number strictly greater than 3000.
 

Constraints:

0 <= n <= 106



# Code
```cpp []
class Solution {
public:
    int nextBeautifulNumber(int n) {
        vector<int> list;
        vector<int> count(10, 0);
        generate(0, count, list);
        sort(list.begin(), list.end());
        for (int num : list) {
            if (num > n) return num;
        }
        return -1;
    }

private:
    void generate(long num, vector<int>& count, vector<int>& list) {
        if (num > 0 && isBeautiful(count)) {
            list.push_back((int)num);
        }
        if (num > 1224444) return;

        for (int d = 1; d <= 7; ++d) {
            if (count[d] < d) {
                count[d]++;
                generate(num * 10 + d, count, list);
                count[d]--;
            }
        }
    }

    bool isBeautiful(const vector<int>& count) {
        for (int d = 1; d <= 7; ++d) {
            if (count[d] != 0 && count[d] != d)
                return false;
        }
        return true;
    }
};
```

---------------------------------------------------------------------------------------------------


# 1716. Calculate Money in Leetcode Bank -> [LeetCode](https://leetcode.com/problems/calculate-money-in-leetcode-bank/description/)
 
Easy
 
Hercy wants to save money for his first car. He puts money in the Leetcode bank every day.

He starts by putting in $1 on Monday, the first day. Every day from Tuesday to Sunday, he will put in $1 more than the day before. On every subsequent Monday, he will put in $1 more than the previous Monday.

Given n, return the total amount of money he will have in the Leetcode bank at the end of the nth day.

 

Example 1:

Input: n = 4

Output: 10

Explanation: After the 4th day, the total is 1 + 2 + 3 + 4 = 10.

Example 2:

Input: n = 10

Output: 37

Explanation: After the 10th day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37. Notice that on the 2nd Monday, Hercy only puts in $2.


Example 3:

Input: n = 20

Output: 96

Explanation: After the 20th day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96.
 

Constraints:

1 <= n <= 1000


# Code
```cpp []
class Solution {
public:
    int totalMoney(int n) {
        int week_cnt = n / 7;
        int rem_days = n % 7;

        int total = ( (week_cnt * (week_cnt - 1)) / 2 ) * 7;
        total += week_cnt * 28;
        total += ( (rem_days * (rem_days + 1)) / 2 ) + (rem_days * week_cnt);

        return total;
    }
};
```


---------------------------------------------------------------------------------------------------

# 2043. Simple Bank System -> [LeetCode](https://leetcode.com/problems/simple-bank-system/description/)
 
Medium
 
You have been tasked with writing a program for a popular bank that will automate all its incoming transactions (transfer, deposit, and withdraw). The bank has n accounts numbered from 1 to n. The initial balance of each account is stored in a 0-indexed integer array balance, with the (i + 1)th account having an initial balance of balance[i].

Execute all the valid transactions. A transaction is valid if:

The given account number(s) are between 1 and n, and
The amount of money withdrawn or transferred from is less than or equal to the balance of the account.
Implement the Bank class:

Bank(long[] balance) Initializes the object with the 0-indexed integer array balance.
boolean transfer(int account1, int account2, long money) Transfers money dollars from the account numbered account1 to the account numbered account2. Return true if the transaction was successful, false otherwise.
boolean deposit(int account, long money) Deposit money dollars into the account numbered account. Return true if the transaction was successful, false otherwise.
boolean withdraw(int account, long money) Withdraw money dollars from the account numbered account. Return true if the transaction was successful, false otherwise.
 

Example 1:

Input
["Bank", "withdraw", "transfer", "deposit", "transfer", "withdraw"]
[[[10, 100, 20, 50, 30]], [3, 10], [5, 1, 20], [5, 20], [3, 4, 15], [10, 50]]

Output
[null, true, true, true, false, false]

Explanation
Bank bank = new Bank([10, 100, 20, 50, 30]);
bank.withdraw(3, 10);    // return true, account 3 has a balance of $20, so it is valid to withdraw $10.
                         // Account 3 has $20 - $10 = $10.
bank.transfer(5, 1, 20); // return true, account 5 has a balance of $30, so it is valid to transfer $20.
                         // Account 5 has $30 - $20 = $10, and account 1 has $10 + $20 = $30.
bank.deposit(5, 20);     // return true, it is valid to deposit $20 to account 5.
                         // Account 5 has $10 + $20 = $30.
bank.transfer(3, 4, 15); // return false, the current balance of account 3 is $10,
                         // so it is invalid to transfer $15 from it.
bank.withdraw(10, 50);   // return false, it is invalid because account 10 does not exist.
 

Constraints:

n == balance.length
1 <= n, account, account1, account2 <= 105
0 <= balance[i], money <= 1012
At most 104 calls will be made to each function transfer, deposit, withdraw.

# Code
```cpp []
class Bank {
private:
    vector<long long> bal;
    int n;

    bool valid(int acc) {
        return acc > 0 && acc <= n;
    }

public:
    Bank(vector<long long>& balance) {
        bal = balance;
        n = balance.size();
    }

    bool transfer(int from, int to, long long amt) {
        if (!valid(from) || !valid(to) || bal[from - 1] < amt) return false;
        bal[from - 1] -= amt;
        bal[to - 1] += amt;
        return true;
    }

    bool deposit(int acc, long long amt) {
        if (!valid(acc)) return false;
        bal[acc - 1] += amt;
        return true;
    }

    bool withdraw(int acc, long long amt) {
        if (!valid(acc) || bal[acc - 1] < amt) return false;
        bal[acc - 1] -= amt;
        return true;
    }
};
```

-------------------------------------------------------------------------------------------

# 2125. Number of Laser Beams in a Bank -> [LeetCode](https://leetcode.com/problems/number-of-laser-beams-in-a-bank)
 
Medium
 
Anti-theft security devices are activated inside a bank. You are given a 0-indexed binary string array bank representing the floor plan of the bank, which is an m x n 2D matrix. bank[i] represents the ith row, consisting of '0's and '1's. '0' means the cell is empty, while'1' means the cell has a security device.

There is one laser beam between any two security devices if both conditions are met:

The two devices are located on two different rows: r1 and r2, where r1 < r2.
For each row i where r1 < i < r2, there are no security devices in the ith row.
Laser beams are independent, i.e., one beam does not interfere nor join with another.

Return the total number of laser beams in the bank.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/12/24/laser1.jpg)

Input: bank = ["011001","000000","010100","001000"]

Output: 8

Explanation: Between each of the following device pairs, there is one beam. In total, there are 8 beams:
 * bank[0][1] -- bank[2][1]
 * bank[0][1] -- bank[2][3]
 * bank[0][2] -- bank[2][1]
 * bank[0][2] -- bank[2][3]
 * bank[0][5] -- bank[2][1]
 * bank[0][5] -- bank[2][3]
 * bank[2][1] -- bank[3][2]
 * bank[2][3] -- bank[3][2]
Note that there is no beam between any device on the 0th row with any on the 3rd row.
This is because the 2nd row contains security devices, which breaks the second condition.


Example 2:

![img](https://assets.leetcode.com/uploads/2021/12/24/laser2.jpg)

Input: bank = ["000","111","000"]

Output: 0

Explanation: There does not exist two devices located on two different rows.
 

Constraints:

m == bank.length
n == bank[i].length
1 <= m, n <= 500
bank[i][j] is either '0' or '1'.


# Code
```cpp []
class Solution {
public:
    int numberOfBeams(std::vector<std::string>& bank) {
        int prevRowCount = 0;
        int total = 0;

        for (const std::string& row : bank) {
            int curRowCount = calc(row);
            if (curRowCount == 0)
                continue;

            total += curRowCount * prevRowCount;
            prevRowCount = curRowCount;
        }
        return total;
    }

private:
    int calc(const std::string& s) {
        int count = 0;
        for (char c : s)
            count += c - '0';

        return count;
    }
};
```


---------------------------------------------------------------------------------------------------------------

# 3354. Make Array Elements Equal to Zero -> [LeetCode](https://leetcode.com/problems/make-array-elements-equal-to-zero/description)
 
Easy
 
You are given an integer array nums.

Start by selecting a starting position curr such that nums[curr] == 0, and choose a movement direction of either left or right.

After that, you repeat the following process:

If curr is out of the range [0, n - 1], this process ends.
If nums[curr] == 0, move in the current direction by incrementing curr if you are moving right, or decrementing curr if you are moving left.
Else if nums[curr] > 0:
Decrement nums[curr] by 1.
Reverse your movement direction (left becomes right and vice versa).
Take a step in your new direction.
A selection of the initial position curr and movement direction is considered valid if every element in nums becomes 0 by the end of the process.

Return the number of possible valid selections.

 

Example 1:

Input: nums = [1,0,2,0,3]

Output: 2

Explanation:

The only possible valid selections are the following:

Choose curr = 3, and a movement direction to the left.
[1,0,2,0,3] -> [1,0,2,0,3] -> [1,0,1,0,3] -> [1,0,1,0,3] -> [1,0,1,0,2] -> [1,0,1,0,2] -> [1,0,0,0,2] -> [1,0,0,0,2] -> [1,0,0,0,1] -> [1,0,0,0,1] -> [1,0,0,0,1] -> [1,0,0,0,1] -> [0,0,0,0,1] -> [0,0,0,0,1] -> [0,0,0,0,1] -> [0,0,0,0,1] -> [0,0,0,0,0].
Choose curr = 3, and a movement direction to the right.
[1,0,2,0,3] -> [1,0,2,0,3] -> [1,0,2,0,2] -> [1,0,2,0,2] -> [1,0,1,0,2] -> [1,0,1,0,2] -> [1,0,1,0,1] -> [1,0,1,0,1] -> [1,0,0,0,1] -> [1,0,0,0,1] -> [1,0,0,0,0] -> [1,0,0,0,0] -> [1,0,0,0,0] -> [1,0,0,0,0] -> [0,0,0,0,0].


Example 2:

Input: nums = [2,3,4,0,4,1,0]

Output: 0

Explanation:

There are no possible valid selections.

 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 100
There is at least one element i where nums[i] == 0.


# Code
```cpp []
class Solution {
public:
    int countValidSelections(vector<int>& nums) {
        int n = nums.size();
        int count = 0;
        vector<int> left(n, 0);
        vector<int> right(n, 0);

        for (int i = 1; i < n; i++) {
            left[i] = left[i - 1] + nums[i - 1];
            right[n - i - 1] = right[n - i] + nums[n - i];
        }

        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) continue;
            if (left[i] == right[i]) count += 2;
            else if (abs(left[i] - right[i]) == 1) count += 1;
        }

        return count;
    }
};

```

------------------------------------------------------------------------------------

# 3370. Smallest Number With All Set Bits -> [LeetCode](https://leetcode.com/problems/smallest-number-with-all-set-bits/)
 
Easy
 
You are given a positive number n.

Return the smallest number x greater than or equal to n, such that the binary representation of x contains only set bits

 

Example 1:

Input: n = 5

Output: 7

Explanation:

The binary representation of 7 is "111".

Example 2:

Input: n = 10

Output: 15

Explanation:

The binary representation of 15 is "1111".

Example 3:

Input: n = 3

Output: 3

Explanation:

The binary representation of 3 is "11".

 

Constraints:

1 <= n <= 1000



# Code
```cpp []
class Solution {
public:
    int smallestNumber(int n) {
        while (n & (n + 1)) {
            n |= n + 1;
        }
        return n;
    }
};
```


--------------------------------------------------------------------------------------------


# 1526. Minimum Number of Increments on Subarrays to Form a Target Array -> [LeetCode](https://leetcode.com/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array/)
 
Hard
 
You are given an integer array target. You have an integer array initial of the same size as target with all elements initially zeros.

In one operation you can choose any subarray from initial and increment each value by one.

Return the minimum number of operations to form a target array from initial.

The test cases are generated so that the answer fits in a 32-bit integer.

 

Example 1:

Input: target = [1,2,3,2,1]

Output: 3

Explanation: We need at least 3 operations to form the target array from the initial array.
[0,0,0,0,0] increment 1 from index 0 to 4 (inclusive).
[1,1,1,1,1] increment 1 from index 1 to 3 (inclusive).
[1,2,2,2,1] increment 1 at index 2.
[1,2,3,2,1] target array is formed.


Example 2:

Input: target = [3,1,1,2]

Output: 4

Explanation: [0,0,0,0] -> [1,1,1,1] -> [1,1,1,2] -> [2,1,1,2] -> [3,1,1,2]


Example 3:

Input: target = [3,1,5,4,2]

Output: 7

Explanation: [0,0,0,0,0] -> [1,1,1,1,1] -> [2,1,1,1,1] -> [3,1,1,1,1] -> [3,1,2,2,2] -> [3,1,3,3,2] -> [3,1,4,4,2] -> [3,1,5,4,2].
 

Constraints:

1 <= target.length <= 105
1 <= target[i] <= 105


# Code
```cpp []
class Solution {
public:
    int minNumberOperations(vector<int>& target) {
        int noOfOperations = target[0];
        for(int i = 1; i < target. size(); i++) {
            if(target[i] > target[i-1]) 
                noOfOperations += (target[i]-target[i-1]);
        }
        return noOfOperations;
    }
};
```


---------------------------------------------------------------------------------------------------

# 3289. The Two Sneaky Numbers of Digitville -> [LeetCode](https://leetcode.com/problems/the-two-sneaky-numbers-of-digitville/)
 
Easy
 
In the town of Digitville, there was a list of numbers called nums containing integers from 0 to n - 1. Each number was supposed to appear exactly once in the list, however, two mischievous numbers sneaked in an additional time, making the list longer than usual.

As the town detective, your task is to find these two sneaky numbers. Return an array of size two containing the two numbers (in any order), so peace can return to Digitville.

 

Example 1:

Input: nums = [0,1,1,0]

Output: [0,1]

Explanation:

The numbers 0 and 1 each appear twice in the array.

Example 2:

Input: nums = [0,3,2,1,3,2]

Output: [2,3]

Explanation:

The numbers 2 and 3 each appear twice in the array.

Example 3:

Input: nums = [7,1,5,4,3,4,6,0,9,5,8,2]

Output: [4,5]

Explanation:

The numbers 4 and 5 each appear twice in the array.

 

Constraints:

2 <= n <= 100
nums.length == n + 2
0 <= nums[i] < n
The input is generated such that nums contains exactly two repeated elements.



# Code
```cpp []
class Solution {
public:
    vector<int> getSneakyNumbers(vector<int>& nums) {
        vector<int> ans;
        map<int,int> mpp;
        for(const int num:nums) mpp[num]++;
        for(auto it:mpp){
            if(it.second == 2) ans.push_back(it.first);
        }
        return ans;
    }
};
```


--------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------------------


![img](https://assets.leetcode.com/static_assets/marketing/202510.gif)
