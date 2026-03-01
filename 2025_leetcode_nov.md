----------------------------------------------------------------------------------------
# LeetCode problems - NOV 2025
----------------------------------------------------------------------------------------


# 3217. Delete Nodes From Linked List Present in Array -> [LeetCode](https://leetcode.com/problems/delete-nodes-from-linked-list-present-in-array/description/)
 
Medium
 
You are given an array of integers nums and the head of a linked list. Return the head of the modified linked list after removing all nodes from the linked list that have a value that exists in nums.

 

Example 1:

Input: nums = [1,2,3], head = [1,2,3,4,5]

Output: [4,5]

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample0.png)


Remove the nodes with values 1, 2, and 3.

Example 2:

Input: nums = [1], head = [1,2,1,2,1,2]

Output: [2,2,2]

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample1.png)


Remove the nodes with value 1.

Example 3:

Input: nums = [5], head = [1,2,3,4]

Output: [1,2,3,4]

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/11/linkedlistexample2.png)


No node has value 5.

 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 105
All elements in nums are unique.
The number of nodes in the given list is in the range [1, 105].
1 <= Node.val <= 105
The input is generated such that there is at least one node in the linked list that has a value not present in nums.



# Code
```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* modifiedList(vector<int>& nums, ListNode* head) {
        unordered_set<int> mpp(nums.begin(), nums.end());

        while (head && mpp.count(head->val))
            head = head->next;

        ListNode* curr = head;
        while (curr && curr->next) {
            while (curr->next && mpp.count(curr->next->val)) {
                curr->next = curr->next->next;
            }
            curr = curr->next;
        }
        return head;
    }
};
```

-----------------------------------------------------------------------------------------


# 2257. Count Unguarded Cells in the Grid -> [LeetCode](https://leetcode.com/problems/count-unguarded-cells-in-the-grid/description)
 
Medium
 
You are given two integers m and n representing a 0-indexed m x n grid. You are also given two 2D integer arrays guards and walls where guards[i] = [rowi, coli] and walls[j] = [rowj, colj] represent the positions of the ith guard and jth wall respectively.

A guard can see every cell in the four cardinal directions (north, east, south, or west) starting from their position unless obstructed by a wall or another guard. A cell is guarded if there is at least one guard that can see it.

Return the number of unoccupied cells that are not guarded.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2022/03/10/example1drawio2.png)

Input: m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]

Output: 7

Explanation: The guarded and unguarded cells are shown in red and green respectively in the above diagram.
There are a total of 7 unguarded cells, so we return 7.


Example 2:

![img](https://assets.leetcode.com/uploads/2022/03/10/example2drawio.png)

Input: m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]

Output: 4

Explanation: The unguarded cells are shown in green in the above diagram.
There are a total of 4 unguarded cells, so we return 4.
 

Constraints:

1 <= m, n <= 105
2 <= m * n <= 105
1 <= guards.length, walls.length <= 5 * 104
2 <= guards.length + walls.length <= m * n
guards[i].length == walls[j].length == 2
0 <= rowi, rowj < m
0 <= coli, colj < n
All the positions in guards and walls are unique.



# Code
```cpp []
class Solution {
public:
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) {
        // Initialize grid with zeros
        int g[m][n];
        memset(g, 0, sizeof(g));
        
        // Mark guards and walls as 2
        for (auto& e : guards) {
            g[e[0]][e[1]] = 2;
        }
        for (auto& e : walls) {
            g[e[0]][e[1]] = 2;
        }
        
        // Directions: up, right, down, left
        int dirs[5] = {-1, 0, 1, 0, -1};
        
        // Process each guard's line of sight
        for (auto& e : guards) {
            for (int k = 0; k < 4; ++k) {
                int x = e[0], y = e[1];
                int dx = dirs[k], dy = dirs[k + 1];
                
                // Check cells in current direction until hitting boundary or obstacle
                while (x + dx >= 0 && x + dx < m && y + dy >= 0 && y + dy < n && g[x + dx][y + dy] < 2) {
                    x += dx;
                    y += dy;
                    g[x][y] = 1;
                }
            }
        }
        
        // Count unguarded cells (cells with value 0)
        int unguardedCount = 0;
        for (int i = 0; i < m; i++) {
            unguardedCount += count(g[i], g[i] + n, 0);
        }
        
        return unguardedCount;
    }
};
```


----------------------------------------------------------------------------------------

# 1578. Minimum Time to Make Rope Colorful -> [LeetCode](https://leetcode.com/problems/minimum-time-to-make-rope-colorful/description)
 
Medium
 
Alice has n balloons arranged on a rope. You are given a 0-indexed string colors where colors[i] is the color of the ith balloon.

Alice wants the rope to be colorful. She does not want two consecutive balloons to be of the same color, so she asks Bob for help. Bob can remove some balloons from the rope to make it colorful. You are given a 0-indexed integer array neededTime where neededTime[i] is the time (in seconds) that Bob needs to remove the ith balloon from the rope.

Return the minimum time Bob needs to make the rope colorful.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/12/13/ballon1.jpg)

Input: colors = "abaac", neededTime = [1,2,3,4,5]

Output: 3

Explanation: In the above image, 'a' is blue, 'b' is red, and 'c' is green.
Bob can remove the blue balloon at index 2. This takes 3 seconds.
There are no longer two consecutive balloons of the same color. Total time = 3.


Example 2:

![img](https://assets.leetcode.com/uploads/2021/12/13/balloon2.jpg)

Input: colors = "abc", neededTime = [1,2,3]

Output: 0

Explanation: The rope is already colorful. Bob does not need to remove any balloons from the rope.


Example 3:

![img](https://assets.leetcode.com/uploads/2021/12/13/balloon3.jpg)

Input: colors = "aabaa", neededTime = [1,2,3,4,1]

Output: 2

Explanation: Bob will remove the balloons at indices 0 and 4. Each balloons takes 1 second to remove.
There are no longer two consecutive balloons of the same color. Total time = 1 + 1 = 2.
 

Constraints:

n == colors.length == neededTime.length
1 <= n <= 105
1 <= neededTime[i] <= 104
colors contains only lowercase English letters.


# Code
```cpp []

class Solution {
public:
    int minCost(string colors, vector<int>& neededTime) {
        int totalTime = 0; 
        int i = 0, j = 0; 

        while (i < neededTime.size() && j < neededTime.size()) {
            int currTotal = 0, currMax = 0; 

            while (j < neededTime.size() && colors[i] == colors[j]) {
                currTotal += neededTime[j];
        
                currMax = max(currMax, neededTime[j]); 
                                                       
                j++;
            }

            totalTime += currTotal - currMax; 
            i = j; 
        }
        return totalTime; 
    }
};

```

------------------------------------------------------------------------------------------


# 3318. Find X-Sum of All K-Long Subarrays I -> [LeetCode](https://leetcode.com/problems/find-x-sum-of-all-k-long-subarrays-i)
 
Easy
 
You are given an array nums of n integers and two integers k and x.

The x-sum of an array is calculated by the following procedure:

Count the occurrences of all elements in the array.
Keep only the occurrences of the top x most frequent elements. If two elements have the same number of occurrences, the element with the bigger value is considered more frequent.
Calculate the sum of the resulting array.
Note that if an array has less than x distinct elements, its x-sum is the sum of the array.

Return an integer array answer of length n - k + 1 where answer[i] is the x-sum of the subarray nums[i..i + k - 1].

 

Example 1:

Input: nums = [1,1,2,2,3,4,2,3], k = 6, x = 2

Output: [6,10,12]

Explanation:

For subarray [1, 1, 2, 2, 3, 4], only elements 1 and 2 will be kept in the resulting array. Hence, answer[0] = 1 + 1 + 2 + 2.
For subarray [1, 2, 2, 3, 4, 2], only elements 2 and 4 will be kept in the resulting array. Hence, answer[1] = 2 + 2 + 2 + 4. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
For subarray [2, 2, 3, 4, 2, 3], only elements 2 and 3 are kept in the resulting array. Hence, answer[2] = 2 + 2 + 2 + 3 + 3.



Example 2:

Input: nums = [3,8,7,8,7,5], k = 2, x = 2

Output: [11,15,15,15,12]

Explanation:

Since k == x, answer[i] is equal to the sum of the subarray nums[i..i + k - 1].

 

Constraints:

1 <= n == nums.length <= 50
1 <= nums[i] <= 50
1 <= x <= k <= nums.length



# Code
```cpp []
class Solution {
public:
    using int2=pair<int, int>;
    static int x_sum( const auto& freq, int k, int x){
        auto freq2=freq;
        sort(freq2.begin(), freq2.end(), greater<int2>());
        int sum=0;
        for (int i=0; i<x; i++){
            auto [f, num]=freq2[i];
            if (f==0) break;
            sum+=num*f;
        }
        return sum;
    }
    static vector<int> findXSum(vector<int>& nums, int k, int x) {
        const int n=nums.size(), sz=n-k+1;
        vector<int> ans(sz);
        array<int2, 51> freq;
        freq.fill({0, 0});
        for(int r=0; r<k; r++){
            int z=nums[r];
            freq[z].second=z;
            freq[z].first++;
        }
        ans[0]=x_sum(freq, k, x);
        for(int l=1, r=k; l<sz; l++, r++){
            int L=nums[l-1], R=nums[r];
            freq[L].first--;
            freq[R].first++;
            freq[R].second = R;
            ans[l]=x_sum(freq, k, x);
        }
        return ans;
    }
};


auto init = []() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    return 'c';
}();
```

-----------------------------------------------------------------------------------------


# 3321. Find X-Sum of All K-Long Subarrays II -> [LeetCode](https://leetcode.com/problems/find-x-sum-of-all-k-long-subarrays-ii/description)

Hard
 
You are given an array nums of n integers and two integers k and x.

The x-sum of an array is calculated by the following procedure:

Count the occurrences of all elements in the array.
Keep only the occurrences of the top x most frequent elements. If two elements have the same number of occurrences, the element with the bigger value is considered more frequent.
Calculate the sum of the resulting array.
Note that if an array has less than x distinct elements, its x-sum is the sum of the array.

Return an integer array answer of length n - k + 1 where answer[i] is the x-sum of the subarray nums[i..i + k - 1].

 

Example 1:

Input: nums = [1,1,2,2,3,4,2,3], k = 6, x = 2

Output: [6,10,12]

Explanation:

For subarray [1, 1, 2, 2, 3, 4], only elements 1 and 2 will be kept in the resulting array. Hence, answer[0] = 1 + 1 + 2 + 2.
For subarray [1, 2, 2, 3, 4, 2], only elements 2 and 4 will be kept in the resulting array. Hence, answer[1] = 2 + 2 + 2 + 4. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
For subarray [2, 2, 3, 4, 2, 3], only elements 2 and 3 are kept in the resulting array. Hence, answer[2] = 2 + 2 + 2 + 3 + 3.
Example 2:

Input: nums = [3,8,7,8,7,5], k = 2, x = 2

Output: [11,15,15,15,12]

Explanation:

Since k == x, answer[i] is equal to the sum of the subarray nums[i..i + k - 1].

 

Constraints:

nums.length == n
1 <= n <= 105
1 <= nums[i] <= 109
1 <= x <= k <= nums.length


# Code
```cpp []
#define ll long long

class comp1{
public:
    //{least frequent, smaller number}
    bool operator()(auto &a,auto &b) const{
        if(a.first==b.first) return a.second<b.second;

        return a.first<b.first;
    }
};

class comp2{
public:
    // {most frequent,bigger number}
    bool operator()(auto &a,auto &b) const{
        if(a.first==b.first) return a.second>b.second;

        return a.first>b.first;
    }
};

class Solution {
public:
    vector<long long> findXSum(vector<int>& nums, int k, int x) {
        int n=nums.size();
        //map to keep frequency of each element
        unordered_map<int ,ll>mp;
        //sum to keep final sum after each window
        ll sum=0;
        for(int i=0;i<k;i++)
        {
            mp[nums[i]]++;
        }
        int i=0;
        vector<ll>ans;
        set<pair<ll,int>,comp1>s1;//up->most likely to get out from here
        set<pair<ll,int>,comp2>s2,s;//up->most likely to come in s1

        for(auto &val: mp)
        {
            s.insert({val.second,val.first});
        }
        int cnt=0;
        //tranfer element belonging to appropriate set s1 or s2
        while(!s.empty())
        {
            auto [c,v]=*s.begin();
            s.erase(s.begin());
            if(cnt<x)
            {
                s1.insert({c,v});
                sum+=c*v;
            }
            else
            {
                s2.insert({c,v});
            }
            cnt++;
        }
        ans.push_back(sum);
        //window from i to j
        //NOTE: whenever s1 is update sum will also be updated
        for(int j=k;j<n;j++)
        {
            //now remove all cases because of number nums[i]
            if(s1.find({mp[nums[i]],nums[i]})==s1.end())
            {
                //found in s2
                auto it=s2.find({mp[nums[i]],nums[i]});
                // it->first--;
                if(it!=s2.end())
                {
                    s2.erase(it);
                }
                s2.insert({mp[nums[i]]-1,nums[i]});
                mp[nums[i]]--;
            }
            else
            {
                //found in s1
                auto it=s1.find({mp[nums[i]],nums[i]});
                // *it->first--;
                if(it!=s1.end())s1.erase(it);
                s1.insert({mp[nums[i]]-1,nums[i]});
                mp[nums[i]]--;
                sum-=nums[i];
            }
            i++;
            //now remove all cases because of nums[j]
            if(s1.find({mp[nums[j]],nums[j]})==s1.end())
            {
                //found in s2
                auto it=s2.find({mp[nums[j]],nums[j]});
                // *it->first++;
                if(it!=s2.end())s2.erase(it);
                s2.insert({mp[nums[j]]+1,nums[j]});
                mp[nums[j]]++;
            }
            else
            {
                //found in s1
                auto it=s1.find({mp[nums[j]],nums[j]});
                // *it->first++;
                if(it!=s1.end())s1.erase(it);
                s1.insert({mp[nums[j]]+1,nums[j]});
                mp[nums[j]]++;
                sum+=nums[j];
            }
            //make size of set s1 to x if possible
            while(s1.size()<x && s2.size()>0)
            {
                auto v2=*s2.begin();
                s2.erase(s2.begin());
                s1.insert(v2);
                sum+=v2.first*v2.second;
            }
            //now swap the positions of top of s1 and top of s2 if:
            // 1-> top frequency count of s1 < top frequency count of s2
            // 2-> if frequency equal and s1 element value < s2 element value
            while(s2.size()>0)
            {
                auto v1=*s1.begin();
                auto v2=*s2.begin();
                if(v1.first<v2.first || (v1.first==v2.first && v1.second<v2.second))
                {
                    sum-=v1.first*v1.second;
                    sum+=v2.first*v2.second;
                    s1.erase(s1.begin());
                    s2.erase(s2.begin());
                    s1.insert({v2.first,v2.second});
                    s2.insert({v1.first,v1.second});
                }
                else break;
            }
            ans.push_back(sum);
        }
        return ans;
    }
};
```

--------------------------------------------------------------------------------------------

# 3607. Power Grid Maintenance -> [LeetCode](https://leetcode.com/problems/power-grid-maintenance/description/)
 
Medium
 
You are given an integer c representing c power stations, each with a unique identifier id from 1 to c (1‑based indexing).

These stations are interconnected via n bidirectional cables, represented by a 2D array connections, where each element connections[i] = [ui, vi] indicates a connection between station ui and station vi. Stations that are directly or indirectly connected form a power grid.

Initially, all stations are online (operational).

You are also given a 2D array queries, where each query is one of the following two types:

[1, x]: A maintenance check is requested for station x. If station x is online, it resolves the check by itself. If station x is offline, the check is resolved by the operational station with the smallest id in the same power grid as x. If no operational station exists in that grid, return -1.

[2, x]: Station x goes offline (i.e., it becomes non-operational).

Return an array of integers representing the results of each query of type [1, x] in the order they appear.

Note: The power grid preserves its structure; an offline (non‑operational) node remains part of its grid and taking it offline does not alter connectivity.

 

Example 1:

Input: c = 5, connections = [[1,2],[2,3],[3,4],[4,5]], queries = [[1,3],[2,1],[1,1],[2,2],[1,2]]

Output: [3,2,3]

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/15/powergrid.jpg)

Initially, all stations {1, 2, 3, 4, 5} are online and form a single power grid.
Query [1,3]: Station 3 is online, so the maintenance check is resolved by station 3.
Query [2,1]: Station 1 goes offline. The remaining online stations are {2, 3, 4, 5}.
Query [1,1]: Station 1 is offline, so the check is resolved by the operational station with the smallest id among {2, 3, 4, 5}, which is station 2.
Query [2,2]: Station 2 goes offline. The remaining online stations are {3, 4, 5}.
Query [1,2]: Station 2 is offline, so the check is resolved by the operational station with the smallest id among {3, 4, 5}, which is station 3.



Example 2:

Input: c = 3, connections = [], queries = [[1,1],[2,1],[1,1]]

Output: [1,-1]

Explanation:

There are no connections, so each station is its own isolated grid.
Query [1,1]: Station 1 is online in its isolated grid, so the maintenance check is resolved by station 1.
Query [2,1]: Station 1 goes offline.
Query [1,1]: Station 1 is offline and there are no other stations in its grid, so the result is -1.
 

Constraints:

1 <= c <= 105
0 <= n == connections.length <= min(105, c * (c - 1) / 2)
connections[i].length == 2
1 <= ui, vi <= c
ui != vi
1 <= queries.length <= 2 * 105
queries[i].length == 2
queries[i][0] is either 1 or 2.
1 <= queries[i][1] <= c


# Code
```cpp []
class Solution {
public:
        unordered_map<int, set<int> > mp;
    void dfs(int node,vector<vector<int>> &adj, vector<int> &visited, int id,vector<int> &ids){
        visited[node] = 1;
        ids[node] = id;
        mp[id].insert(node);
        for(auto nodes : adj[node]){
            if(!visited[nodes]){
                dfs(nodes,adj,visited,id,ids);
            }
        }
    }

    vector<int> processQueries(int c, vector<vector<int>>& connections, vector<vector<int>>& queries) {
        vector<int> visited(c+1,0);

        vector<vector<int>> adj(c+1);

        for(int i=0;i<connections.size();i++){
            int u = connections[i][0];
            int v = connections[i][1];

            adj[u].push_back(v);
            adj[v].push_back(u);

        }

        vector<int> ids(c+1);


        for(int i=1;i<=c;i++){
            if(!visited[i]){
                dfs(i,adj,visited,i,ids);
            }
        }

        // for(int i=1;i<=c;i++){
        //     cout<<ids[i]<<" ";
        // }
        // cout<<endl;

        vector<int> ans;
        for(int i=0;i<queries.size();i++){
            if(queries[i][0] == 1){


                int node = queries[i][1];

                int check_id = ids[node];
                if(mp[check_id].count(node)){
                    ans.push_back(node);
                }
                else if(mp[check_id].size() != 0){
                    ans.push_back(*(mp[check_id].begin()));
                }
                else ans.push_back(-1);
            }
            else{

                int node=  queries[i][1];

                int check_id = ids[node];

                if(mp[check_id].count(node)){
                    mp[check_id].erase(node);
                }
            }
        }

        return ans;

    }
};
```


----------------------------------------------------------------------------------------------------------------


# 2528. Maximize the Minimum Powered City -> [LeetCode](https://leetcode.com/problems/maximize-the-minimum-powered-city/description)

Hard
 
You are given a 0-indexed integer array stations of length n, where stations[i] represents the number of power stations in the ith city.

Each power station can provide power to every city in a fixed range. In other words, if the range is denoted by r, then a power station at city i can provide power to all cities j such that |i - j| <= r and 0 <= i, j <= n - 1.

Note that |x| denotes absolute value. For example, |7 - 5| = 2 and |3 - 10| = 7.
The power of a city is the total number of power stations it is being provided power from.

The government has sanctioned building k more power stations, each of which can be built in any city, and have the same range as the pre-existing ones.

Given the two integers r and k, return the maximum possible minimum power of a city, if the additional power stations are built optimally.

Note that you can build the k power stations in multiple cities.

 

Example 1:

Input: stations = [1,2,4,5,0], r = 1, k = 2

Output: 5

Explanation: 

One of the optimal ways is to install both the power stations at city 1. 
So stations will become [1,4,4,5,0].
- City 0 is provided by 1 + 4 = 5 power stations.
- City 1 is provided by 1 + 4 + 4 = 9 power stations.
- City 2 is provided by 4 + 4 + 5 = 13 power stations.
- City 3 is provided by 5 + 4 = 9 power stations.
- City 4 is provided by 5 + 0 = 5 power stations.
So the minimum power of a city is 5.
Since it is not possible to obtain a larger power, we return 5.


Example 2:

Input: stations = [4,4,4,4], r = 0, k = 3

Output: 4

Explanation: 
It can be proved that we cannot make the minimum power of a city greater than 4.
 

Constraints:

n == stations.length
1 <= n <= 105
0 <= stations[i] <= 105
0 <= r <= n - 1
0 <= k <= 109


# Code
```cpp []
class Solution {
public:
    long long maxPower(vector<int>& stations, int r, int k) {
        long long left = 0;
        // The answer = `right`, when `r = n`, all value of stations are the same!
        long long right = accumulate(stations.begin(), stations.end(), 0LL) + k;
        long long ans = 0;
        while (left <= right) {
            long long mid = (left + right) / 2;
            if (isGood(stations, r, mid, k)) {
                ans = mid; // This is the maximum possible minimum power so far
                left = mid + 1; // Search for a larger value in the right side
            } else {
                right = mid - 1; // Decrease minPowerRequired to need fewer additional power stations
            }
        }
        return ans;
    }

    bool isGood(vector<int>& stations, int r, long long minPowerRequired, int additionalStations) {
        int n = stations.size();
        // init windowPower to store power of 0th city (minus stations[r])
        long long windowPower = accumulate(stations.begin(), stations.begin()+r, 0LL);
        vector<int> additions(n, 0);
        for (int i = 0; i < n; i++) {
            if (i + r < n) {
                // now, windowPower stores sum of power stations from [i-r..i+r],
                // it also means it's the power of city `ith`
                windowPower += stations[i + r];
            }
            if (windowPower < minPowerRequired) {
                long long needed = minPowerRequired - windowPower;
                if (needed > additionalStations) {
                    // Not enough additional stations to plant
                    return false;
                }
                // Plant the additional stations on the farthest city in the range
                // to cover as many cities as possible
                additions[min(n - 1, i + r)] += needed;
                windowPower = minPowerRequired;
                additionalStations -= needed;
            }
            if (i - r >= 0) {
                // out of window range
                windowPower -= stations[i - r] + additions[i - r];
            }
        }
        return true;
    }
};
```


-----------------------------------------------------------------------------------------------------------------------


# 1611. Minimum One Bit Operations to Make Integers Zero -> [LeetCode](https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero)

Hard
 
Given an integer n, you must transform it into 0 using the following operations any number of times:

Change the rightmost (0th) bit in the binary representation of n.
Change the ith bit in the binary representation of n if the (i-1)th bit is set to 1 and the (i-2)th through 0th bits are set to 0.
Return the minimum number of operations to transform n into 0.

 

Example 1:

Input: n = 3

Output: 2

Explanation: The binary representation of 3 is "11".
"11" -> "01" with the 2nd operation since the 0th bit is 1.
"01" -> "00" with the 1st operation.



Example 2:

Input: n = 6

Output: 4

Explanation: The binary representation of 6 is "110".
"110" -> "010" with the 2nd operation since the 1st bit is 1 and 0th through 0th bits are 0.
"010" -> "011" with the 1st operation.
"011" -> "001" with the 2nd operation since the 0th bit is 1.
"001" -> "000" with the 1st operation.
 

Constraints:

0 <= n <= 109


# Code
```cpp []
class Solution {
public:
    int minimumOneBitOperations(int n) {
        int res;
        for (res = 0; n > 0; n &= n - 1)
            res = -(res + (n ^ (n - 1)));
        return abs(res);
    }
};
```

-----------------------------------------------------------------------------------------------------------------------------


# 2169. Count Operations to Obtain Zero -> [LeetCode](https://leetcode.com/problems/count-operations-to-obtain-zero/description)
 
Easy
 
You are given two non-negative integers num1 and num2.

In one operation, if num1 >= num2, you must subtract num2 from num1, otherwise subtract num1 from num2.

For example, if num1 = 5 and num2 = 4, subtract num2 from num1, thus obtaining num1 = 1 and num2 = 4. However, if num1 = 4 and num2 = 5, after one operation, num1 = 4 and num2 = 1.
Return the number of operations required to make either num1 = 0 or num2 = 0.

 

Example 1:

Input: num1 = 2, num2 = 3

Output: 3

Explanation: 

- Operation 1: num1 = 2, num2 = 3. Since num1 < num2, we subtract num1 from num2 and get num1 = 2, num2 = 3 - 2 = 1.
- Operation 2: num1 = 2, num2 = 1. Since num1 > num2, we subtract num2 from num1.
- Operation 3: num1 = 1, num2 = 1. Since num1 == num2, we subtract num2 from num1.
Now num1 = 0 and num2 = 1. Since num1 == 0, we do not need to perform any further operations.
So the total number of operations required is 3.


Example 2:

Input: num1 = 10, num2 = 10

Output: 1

Explanation: 

- Operation 1: num1 = 10, num2 = 10. Since num1 == num2, we subtract num2 from num1 and get num1 = 10 - 10 = 0.
Now num1 = 0 and num2 = 10. Since num1 == 0, we are done.
So the total number of operations required is 1.
 

Constraints:

0 <= num1, num2 <= 105


# Code
```cpp []
class Solution {
public:
    int countOperations(int num1, int num2, int cnt=0) {
        return (num1==0 || num2==0)?cnt:countOperations(num2, num1%num2, cnt+num1/num2);
    }
};
```

----------------------------------------------------------------------------------------------------------------------------

# 3542. Minimum Operations to Convert All Elements to Zero -> [Leetcode](https://leetcode.com/problems/minimum-operations-to-convert-all-elements-to-zero)

Medium
 
You are given an array nums of size n, consisting of non-negative integers. Your task is to apply some (possibly zero) operations on the array so that all elements become 0.

In one operation, you can select a subarray [i, j] (where 0 <= i <= j < n) and set all occurrences of the minimum non-negative integer in that subarray to 0.

Return the minimum number of operations required to make all elements in the array 0.

 

Example 1:

Input: nums = [0,2]

Output: 1

Explanation:

Select the subarray [1,1] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0].
Thus, the minimum number of operations required is 1.


Example 2:

Input: nums = [3,1,2,1]

Output: 3

Explanation:

Select subarray [1,3] (which is [1,2,1]), where the minimum non-negative integer is 1. Setting all occurrences of 1 to 0 results in [3,0,2,0].
Select subarray [2,2] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [3,0,0,0].
Select subarray [0,0] (which is [3]), where the minimum non-negative integer is 3. Setting all occurrences of 3 to 0 results in [0,0,0,0].
Thus, the minimum number of operations required is 3.


Example 3:

Input: nums = [1,2,1,2,1,2]

Output: 4

Explanation:

Select subarray [0,5] (which is [1,2,1,2,1,2]), where the minimum non-negative integer is 1. Setting all occurrences of 1 to 0 results in [0,2,0,2,0,2].
Select subarray [1,1] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,2,0,2].
Select subarray [3,3] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,0,0,2].
Select subarray [5,5] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,0,0,0].
Thus, the minimum number of operations required is 4.
 

Constraints:

1 <= n == nums.length <= 105
0 <= nums[i] <= 105


# Code
```cpp []
class Solution {
public:
    int minOperations(vector<int>& nums) {
        stack<int> st;
        int ans = 0;

        for (int num : nums) {
            while (!st.empty() && num < st.top()) {
                st.pop();  // Invalidate larger numbers for current context
            }
            if (st.empty() || num > st.top()) {
                if (num > 0) ans++;
                st.push(num);
            }
        }

        return ans;
    }
};
```


------------------------------------------------------------------------------------------------------------------------------


# 46. Permutations -> [LeetCode](https://leetcode.com/problems/permutations/description/)
 
Medium
 
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

 

Example 1:

Input: nums = [1,2,3]

Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]


Example 2:

Input: nums = [0,1]

Output: [[0,1],[1,0]]


Example 3:

Input: nums = [1]
Output: [[1]]
 

Constraints:

1 <= nums.length <= 6
-10 <= nums[i] <= 10
All the integers of nums are unique.


# Code
```cpp []
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
           vector<vector<int>> res;
           backtrack(nums , 0 , res);
           return res;
    }

    void backtrack(vector<int>& nums , int st , vector<vector<int>> &res){
        if(st == nums.size()){
            res.push_back(nums);
            return;
        }
        for(int i=st ; i<nums.size() ; ++i){
            swap(nums[i] , nums[st]);
            backtrack(nums , st + 1 , res);
            swap(nums[i] , nums[st]);
        }
    }

    void swap(int &a , int &b){
        int temp = a;
        a = b;
        b = temp;
    }
};
```


------------------------------------------------------------------------------------------------------

# 474. Ones and Zeroes -> [LeetCode](https://leetcode.com/problems/ones-and-zeroes/)
 
Medium
 
You are given an array of binary strings strs and two integers m and n.

Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

A set x is a subset of a set y if all elements of x are also elements of y.

 

Example 1:

Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3

Output: 4

Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.


Example 2:

Input: strs = ["10","0","1"], m = 1, n = 1

Output: 2

Explanation: The largest subset is {"0", "1"}, so the answer is 2.
 

Constraints:

1 <= strs.length <= 600
1 <= strs[i].length <= 100
strs[i] consists only of digits '0' and '1'.
1 <= m, n <= 100


# Code
```cpp []
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (string str : strs) {
            int one = 0;
            int zero = 0;
            for (char c : str) {
                if (c == '1')
                    one++;
                else
                    zero++;
            }
            for (int i = m; i >= zero; i--) {
                for (int j = n; j >= one; j--) {
                    if (one <= j && zero <= i)
                        dp[i][j] = max(dp[i][j], dp[i - zero][j - one] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```


-------------------------------------------------------------------------------------------------


# 2654. Minimum Number of Operations to Make All Array Elements Equal to 1 -> [LeetCode](https://leetcode.com/problems/minimum-number-of-operations-to-make-all-array-elements-equal-to-1/description)
 
Medium
 
You are given a 0-indexed array nums consisiting of positive integers. You can do the following operation on the array any number of times:

Select an index i such that 0 <= i < n - 1 and replace either of nums[i] or nums[i+1] with their gcd value.
Return the minimum number of operations to make all elements of nums equal to 1. If it is impossible, return -1.

The gcd of two integers is the greatest common divisor of the two integers.

 

Example 1:

Input: nums = [2,6,3,4]

Output: 4

Explanation: We can do the following operations:
- Choose index i = 2 and replace nums[2] with gcd(3,4) = 1. Now we have nums = [2,6,1,4].
- Choose index i = 1 and replace nums[1] with gcd(6,1) = 1. Now we have nums = [2,1,1,4].
- Choose index i = 0 and replace nums[0] with gcd(2,1) = 1. Now we have nums = [1,1,1,4].
- Choose index i = 2 and replace nums[3] with gcd(1,4) = 1. Now we have nums = [1,1,1,1].


Example 2:

Input: nums = [2,10,6,14]

Output: -1

Explanation: It can be shown that it is impossible to make all the elements equal to 1.
 

Constraints:

2 <= nums.length <= 50
1 <= nums[i] <= 106



# Code
```cpp []
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int n = nums.size();
        int c = count(nums.begin(), nums.end(), 1);
        if (c != 0)
            return n - c;
        int res = 1e7;
        for (int i = 0; i < n; i++) {
            int g = nums[i];
            for (int j = i + 1; j < n; j++) {
                g = __gcd(g, nums[j]);
                if (g == 1) {
                    res = min(res, j - i + (n - 1));
                    break;
                }
            }
        }
        return res == 1e7 ? -1 : res;
    }
};
```

-------------------------------------------------------------------------------------------------

# 3228. Maximum Number of Operations to Move Ones to the End -> [LeetCode](https://leetcode.com/problems/maximum-number-of-operations-to-move-ones-to-the-end)

Medium
 
You are given a binary string s.

You can perform the following operation on the string any number of times:

Choose any index i from the string where i + 1 < s.length such that s[i] == '1' and s[i + 1] == '0'.
Move the character s[i] to the right until it reaches the end of the string or another '1'. For example, for s = "010010", if we choose i = 1, the resulting string will be s = "000110".
Return the maximum number of operations that you can perform.

 

Example 1:

Input: s = "1001101"

Output: 4

Explanation:

We can perform the following operations:

Choose index i = 0. The resulting string is s = "0011101".
Choose index i = 4. The resulting string is s = "0011011".
Choose index i = 3. The resulting string is s = "0010111".
Choose index i = 2. The resulting string is s = "0001111".
Example 2:

Input: s = "00111"

Output: 0

 

Constraints:

1 <= s.length <= 105
s[i] is either '0' or '1'.


# Code
```cpp []
class Solution {
public:
    int maxOperations(string s) {
        int ones = 0, res = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '1')
                ones++;
            else if ((i > 0) && s[i - 1] == '1')
                res += ones;
        }

        return res;
    }
};
```

---------------------------------------------------------------------------------------------------

# 2536. Increment Submatrices by One -> [LeetCode](https://leetcode.com/problems/increment-submatrices-by-one)
 
Topics
 
You are given a positive integer n, indicating that we initially have an n x n 0-indexed integer matrix mat filled with zeroes.

You are also given a 2D integer array query. For each query[i] = [row1i, col1i, row2i, col2i], you should do the following operation:

Add 1 to every element in the submatrix with the top left corner (row1i, col1i) and the bottom right corner (row2i, col2i). That is, add 1 to mat[x][y] for all row1i <= x <= row2i and col1i <= y <= col2i.
Return the matrix mat after performing every query.

 

Example 1:


Input: n = 3, queries = [[1,1,2,2],[0,0,1,1]]
Output: [[1,1,0],[1,2,1],[0,1,1]]
Explanation: The diagram above shows the initial matrix, the matrix after the first query, and the matrix after the second query.
- In the first query, we add 1 to every element in the submatrix with the top left corner (1, 1) and bottom right corner (2, 2).
- In the second query, we add 1 to every element in the submatrix with the top left corner (0, 0) and bottom right corner (1, 1).
Example 2:


Input: n = 2, queries = [[0,0,1,1]]
Output: [[1,1],[1,1]]
Explanation: The diagram above shows the initial matrix and the matrix after the first query.
- In the first query we add 1 to every element in the matrix.
 

Constraints:

1 <= n <= 500
1 <= queries.length <= 104
0 <= row1i <= row2i < n
0 <= col1i <= col2i < n



# Code
```cpp []
class Solution {
public:
    vector<vector<int>> rangeAddQueries(int n, vector<vector<int>>& queries) {
        vector<vector<int>> mat(n, vector<int>(n, 0));
        
        for (auto q : queries) {
            int r1 = q[0], c1 = q[1], r2 = q[2], c2 = q[3];
            
            mat[r1][c1] += 1;
            if(r2 + 1 < n && c2 + 1 < n) mat[r2 + 1][c2 + 1] += 1;
            if(r2 + 1< n) mat[r2 + 1][c1] -= 1;
            if(c2 + 1 < n) mat[r1][c2 + 1] -= 1;
        }
        
        for(int i=0; i<n; i++) {
            for (int j=1; j<n; j++) {
                mat[i][j] += mat[i][j - 1];
            }
        }
        for(int i=1; i<n; i++) {
            for (int j=0; j<n; j++) {
                mat[i][j] += mat[i - 1][j];
            }
        }
        return mat;
    }
};
```


------------------------------------------------------------------------------------------------------------

# 3234. Count the Number of Substrings With Dominant Ones -> [LeetCode](https://leetcode.com/problems/count-the-number-of-substrings-with-dominant-ones)
 
Medium
 
You are given a binary string s.

Return the number of substrings with dominant ones.

A string has dominant ones if the number of ones in the string is greater than or equal to the square of the number of zeros in the string.

 

Example 1:

Input: s = "00011"

Output: 5

Explanation:

The substrings with dominant ones are shown in the table below.

i	j	s[i..j]	Number of Zeros	Number of Ones
3	3	1	0	1
4	4	1	0	1
2	3	01	1	1
3	4	11	0	2
2	4	011	1	2
Example 2:

Input: s = "101101"

Output: 16

Explanation:

The substrings with non-dominant ones are shown in the table below.

Since there are 21 substrings total and 5 of them have non-dominant ones, it follows that there are 16 substrings with dominant ones.

i	j	s[i..j]	Number of Zeros	Number of Ones
1	1	0	1	0
4	4	0	1	0
1	4	0110	2	2
0	4	10110	2	3
1	5	01101	2	3
 

Constraints:

1 <= s.length <= 4 * 104
s consists only of characters '0' and '1'.


# Code
```cpp []
class Solution {
public:
    int numberOfSubstrings(string s) {
        int n = s.size();
        vector<int> zero;
        for (int i = 0; i < n; i++) {
            if (s[i] == '0') zero.push_back(i);
        }
        int ones = n - zero.size(); 
        zero.push_back(n); 

        int res = 0, sid = 0; 

        for (int left = 0; left < n; left++) {
           
            for (int id = sid; id < zero.size() - 1; id++) {
                int cnt0 = id - sid + 1; 
                if (cnt0 * cnt0 > ones) break;
                int p = zero[id], q = zero[id + 1];
                int cnt1 = zero[id] - left - (id - sid);
                if (cnt1 >= cnt0 * cnt0) {
                    res += q - p;
                } else {
                    res += max(q - p - (cnt0 * cnt0 - cnt1), 0);
                }
            }
            if (s[left] == '0') {
                sid++;
            } else {
               
                res += zero[sid] - left;
                ones--;
            }
        }
        return res;
    }
};
```

----------------------------------------------------------------------------------------------------------------------------

# 1513. Number of Substrings With Only 1s -> [LeetCode](https://leetcode.com/problems/number-of-substrings-with-only-1s/description)
 
Medium
 
Given a binary string s, return the number of substrings with all characters 1's. Since the answer may be too large, return it modulo 109 + 7.

 

Example 1:

Input: s = "0110111"

Output: 9

Explanation: There are 9 substring in total with only 1's characters.
"1" -> 5 times.
"11" -> 3 times.
"111" -> 1 time.



Example 2:

Input: s = "101"

Output: 2

Explanation: Substring "1" is shown 2 times in s.


Example 3:

Input: s = "111111"

Output: 21

Explanation: Each substring contains only 1's characters.
 


Constraints:

1 <= s.length <= 105
s[i] is either '0' or '1'.



# Code
```cpp []
class Solution {
public:
    int numSub(string s) {
        int cnt = 0, total = 0, mod = 1e9 + 7;
        for (char a : s) {
            if (a == '1') {
                cnt++;
            } else {
                cnt = 0;
            }
            total = (total + cnt) % mod;
        }
        return total;
    }
};
```

----------------------------------------------------------------------------------------------------------


# 1437. Check If All 1's Are at Least Length K Places Away -> [LeetCode](https://leetcode.com/problems/check-if-all-1s-are-at-least-length-k-places-away/description)
 
Easy
 
Given an binary array nums and an integer k, return true if all 1's are at least k places away from each other, otherwise return false.

 

Example 1:

Input: nums = [1,0,0,0,1,0,0,1], k = 2

Output: true

![img](https://assets.leetcode.com/uploads/2020/04/15/sample_1_1791.png)

Explanation: Each of the 1s are at least 2 places away from each other.


Example 2:

Input: nums = [1,0,0,1,0,1], k = 2

Output: false

![img](https://assets.leetcode.com/uploads/2020/04/15/sample_2_1791.png)

Explanation: The second 1 and third 1 are only one apart from each other.
 

Constraints:

1 <= nums.length <= 105
0 <= k <= nums.length
nums[i] is 0 or 1

# 
# Code
```cpp []
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int n = nums.size();
        int prevIdx;
        bool notFound = true;
        for(int i=0 ; i<n  ; ++i){
            if(notFound && nums[i] == 1){
                notFound = false;
                prevIdx = i;
                continue;
            }
            if(nums[i] == 1){
                if(i - prevIdx - 1 >= k) prevIdx = i;
                else return false;
            }
        }
        return true;
    }
};
```


-----------------------------------------------------------------------------------------------------------------


# 717. 1-bit and 2-bit Characters -> [LeetCode](https://leetcode.com/problems/1-bit-and-2-bit-characters/description/)
 
Easy
 
We have two special characters:

The first character can be represented by one bit 0.
The second character can be represented by two bits (10 or 11).
Given a binary array bits that ends with 0, return true if the last character must be a one-bit character.

 

Example 1:

Input: bits = [1,0,0]

Output: true

Explanation: The only way to decode it is two-bit character and one-bit character.
So the last character is one-bit character.


Example 2:

Input: bits = [1,1,1,0]

Output: false

Explanation: The only way to decode it is two-bit character and two-bit character.
So the last character is not one-bit character.
 

Constraints:

1 <= bits.length <= 1000
bits[i] is either 0 or 1.

# Code
```cpp []
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int n = bits.size();
        int i = 0;        
        while (i < n - 1)
            i += bits[i] + 1;

        return i == n - 1;
    }
};

```

-----------------------------------------------------------------------------------------

# 2154. Keep Multiplying Found Values by Two -> [LeetCode](https://leetcode.com/problems/keep-multiplying-found-values-by-two/description)
 
Easy
 
You are given an array of integers nums. You are also given an integer original which is the first number that needs to be searched for in nums.

You then do the following steps:

If original is found in nums, multiply it by two (i.e., set original = 2 * original).
Otherwise, stop the process.
Repeat this process with the new number as long as you keep finding the number.
Return the final value of original.

 

Example 1:

Input: nums = [5,3,6,1,12], original = 3

Output: 24

Explanation: 
- 3 is found in nums. 3 is multiplied by 2 to obtain 6.
- 6 is found in nums. 6 is multiplied by 2 to obtain 12.
- 12 is found in nums. 12 is multiplied by 2 to obtain 24.
- 24 is not found in nums. Thus, 24 is returned.


Example 2:

Input: nums = [2,7,9], original = 4

Output: 4

Explanation:
- 4 is not found in nums. Thus, 4 is returned.
 

Constraints:

1 <= nums.length <= 1000
1 <= nums[i], original <= 1000


# Code
```cpp []
class Solution {
public:
    int findFinalValue(vector<int>& nums, int k) {
        int bits = 0;
        for (auto& n : nums) {
            if (n % k != 0) continue;
            n /= k;
            if ((n & (n - 1)) == 0)
                bits |= n;
        }
        bits++;
        return k * (bits & -bits);
    }
};

```

----------------------------------------------------------------------------------------------------------------------------


# 757. Set Intersection Size At Least Two -> [LeetCode](https://leetcode.com/problems/set-intersection-size-at-least-two/description)
 
Hard
 
You are given a 2D integer array intervals where intervals[i] = [starti, endi] represents all the integers from starti to endi inclusively.

A containing set is an array nums where each interval from intervals has at least two integers in nums.

For example, if intervals = [[1,3], [3,7], [8,9]], then [1,2,4,7,8,9] and [2,3,4,8,9] are containing sets.
Return the minimum possible size of a containing set.

 

Example 1:

Input: intervals = [[1,3],[3,7],[8,9]]

Output: 5

Explanation: let nums = [2, 3, 4, 8, 9].
It can be shown that there cannot be any containing array of size 4.


Example 2:

Input: intervals = [[1,3],[1,4],[2,5],[3,5]]

Output: 3

Explanation: let nums = [2, 3, 4].
It can be shown that there cannot be any containing array of size 2.


Example 3:

Input: intervals = [[1,2],[2,3],[2,4],[4,5]]

Output: 5

Explanation: let nums = [1, 2, 3, 4, 5].
It can be shown that there cannot be any containing array of size 4.
 

Constraints:

1 <= intervals.length <= 3000
intervals[i].length == 2
0 <= starti < endi <= 108 


# Code
```cpp []
class Solution {
public:
    static int intersectionSizeTwo(vector<vector<int>>& I) {
        sort(I.begin(), I.end(), [](vector<int>& X, vector<int>& Y){ 
            const int x0=X[0], x1=X[1], y0=Y[0], y1=Y[1]; 
            return (x1==y1)?x0>y0:x1<y1; 
        });

        int cnt=2, n=I.size();
        int b=I[0][1], a=b-1;
        for (int i=1; i<n; i++) {
            const int L=I[i][0], R=I[i][1];
            if (a>=L) continue;
            const bool NO=L>b;
            cnt+=1+NO;
            a=(NO)?R-1:b;
            b=R;
        }
        return cnt;
    }
};


```


--------------------------------------------------------------------------------------------------------------------------------


# 1930. Unique Length-3 Palindromic Subsequences -> [LeetCode](https://leetcode.com/problems/unique-length-3-palindromic-subsequences/description)
 
Medium
 
Given a string s, return the number of unique palindromes of length three that are a subsequence of s.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted once.

A palindrome is a string that reads the same forwards and backwards.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
 

Example 1:

Input: s = "aabca"

Output: 3

Explanation: The 3 palindromic subsequences of length 3 are:
- "aba" (subsequence of "aabca")
- "aaa" (subsequence of "aabca")
- "aca" (subsequence of "aabca")


Example 2:

Input: s = "adc"

Output: 0

Explanation: There are no palindromic subsequences of length 3 in "adc".



Example 3:

Input: s = "bbcbaba"

Output: 4

Explanation: The 4 palindromic subsequences of length 3 are:
- "bbb" (subsequence of "bbcbaba")
- "bcb" (subsequence of "bbcbaba")
- "bab" (subsequence of "bbcbaba")
- "aba" (subsequence of "bbcbaba")
 

Constraints:

3 <= s.length <= 105
s consists of only lowercase English letters.



# Code
```cpp []
class Solution {
public:
    int countPalindromicSubsequence(string inputString) {
        vector<int> minExist(26, INT_MAX);
        vector<int> maxExist(26, INT_MIN);

        for (int i = 0; i < inputString.size(); i++) {
            int charIndex = inputString[i] - 'a';
            minExist[charIndex] = min(minExist[charIndex], i);
            maxExist[charIndex] = max(maxExist[charIndex], i);
        }

        int uniqueCount = 0;

        for (int charIndex = 0; charIndex < 26; charIndex++) {
            if (minExist[charIndex] == INT_MAX ||
                maxExist[charIndex] == INT_MIN) {
                continue;
            }

            unordered_set<char> uniqueCharsBetween;

            for (int j = minExist[charIndex] + 1; j < maxExist[charIndex];
                 j++) {
                uniqueCharsBetween.insert(inputString[j]);
            }

            uniqueCount += uniqueCharsBetween.size();
        }
        return uniqueCount;
    }
};
```

-----------------------------------------------------------------------------


# 3190. Find Minimum Operations to Make All Elements Divisible by Three -> [LeetCode](https://leetcode.com/problems/find-minimum-operations-to-make-all-elements-divisible-by-three/description/)

Easy

You are given an integer array nums. In one operation, you can add or subtract 1 from any element of nums.

Return the minimum number of operations to make all elements of nums divisible by 3.

 

Example 1:

Input: nums = [1,2,3,4]

Output: 3

Explanation:

All array elements can be made divisible by 3 using 3 operations:

Subtract 1 from 1.
Add 1 to 2.
Subtract 1 from 4.


Example 2:

Input: nums = [3,6,9]

Output: 0

 

Constraints:

1 <= nums.length <= 50
1 <= nums[i] <= 50



# Code
```cpp []
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        int ans = 0;
        for(const int num:nums) if(num % 3 != 0) ans++;
        return ans;
    }
};
```


------------------------------------------------------------------------------------------------------------------------------


# 1262. Greatest Sum Divisible by Three -> [leetCode](https://leetcode.com/problems/greatest-sum-divisible-by-three)

Medium

Given an integer array nums, return the maximum possible sum of elements of the array such that it is divisible by three.

 

Example 1:

Input: nums = [3,6,5,1,8]

Output: 18

Explanation: Pick numbers 3, 6, 1 and 8 their sum is 18 (maximum sum divisible by 3).


Example 2:

Input: nums = [4]

Output: 0

Explanation: Since 4 is not divisible by 3, do not pick any number.


Example 3:

Input: nums = [1,2,3,4,4]

Output: 12

Explanation: Pick numbers 1, 3, 4 and 4 their sum is 12 (maximum sum divisible by 3).
 


Constraints:

1 <= nums.length <= 4 * 104
1 <= nums[i] <= 104


# Code
```cpp []
class Solution {
public:
    static int maxSumDivThree(vector<int>& nums) {
        const int n=nums.size(), minus=-1e9;
        int dp[2][3]={ {0, 0, 0}, {0, minus, minus}};
        for(int i=0; i<n; i++){
            const int x=nums[i];
            for(int mod=0; mod<3; mod++){
                int modPrev=mod-x%3; modPrev+=(-(modPrev<0)) & 3;
                const int take=x+dp[(i+1)&1][modPrev];
                const int skip=dp[(i+1)&1][mod];
                dp[i&1][mod]=max(take, skip);
            }
        }
        return max(0, dp[(n-1)&1][0]);
    }
};
```


-----------------------------------------------------------------------------------------------------------------------------------------------------------


# 1018. Binary Prefix Divisible By 5 -> [LeetCode](https://leetcode.com/problems/binary-prefix-divisible-by-5/)

Easy
 
You are given a binary array nums (0-indexed).

We define xi as the number whose binary representation is the subarray nums[0..i] (from most-significant-bit to least-significant-bit).

For example, if nums = [1,0,1], then x0 = 1, x1 = 2, and x2 = 5.
Return an array of booleans answer where answer[i] is true if xi is divisible by 5.

 

Example 1:

Input: nums = [0,1,1]

Output: [true,false,false]

Explanation: The input numbers in binary are 0, 01, 011; which are 0, 1, and 3 in base-10.
Only the first number is divisible by 5, so answer[0] is true.



Example 2:

Input: nums = [1,1,1]

Output: [false,false,false]
 

Constraints:

1 <= nums.length <= 105
nums[i] is either 0 or 1.



# Code
```cpp []
class Solution {
public:
    vector<bool> prefixesDivBy5(vector<int>& nums) {
        int val = 0;
        vector<bool> res;

        for (auto& n : nums) {
            val = ((val << 1) + n) % 5;
            res.push_back(val == 0);
        }

        return res;
    }
};

```


---------------------------------------------------------------------------------------------------------------------------------------

# 1015. Smallest Integer Divisible by K -> [LeetCode](https://leetcode.com/problems/smallest-integer-divisible-by-k/description/)

Medium
 
Given a positive integer k, you need to find the length of the smallest positive integer n such that n is divisible by k, and n only contains the digit 1.

Return the length of n. If there is no such n, return -1.

Note: n may not fit in a 64-bit signed integer.

 

Example 1:

Input: k = 1

Output: 1

Explanation: The smallest answer is n = 1, which has length 1.



Example 2:

Input: k = 2

Output: -1

Explanation: There is no such positive integer n divisible by 2.



Example 3:

Input: k = 3

Output: 3

Explanation: The smallest answer is n = 111, which has length 3.
 

Constraints:

1 <= k <= 105



# Code
```cpp []
class Solution {
public: 
    int smallestRepunitDivByK(int K) {
        for (int r = 0, N = 1; N <= K; ++N)
            if ((r = (r * 10 + 1) % K) == 0)
                return N;
        return -1;
    }
};
```


-----------------------------------------------------------------------------------------------------

# 2435. Paths in Matrix Whose Sum Is Divisible by K -> [LeetCode](https://leetcode.com/problems/paths-in-matrix-whose-sum-is-divisible-by-k)

Hard

You are given a 0-indexed m x n integer matrix grid and an integer k. You are currently at position (0, 0) and you want to reach position (m - 1, n - 1) moving only down or right.

Return the number of paths where the sum of the elements on the path is divisible by k. Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: grid = [[5,2,4],[3,0,5],[0,7,2]], k = 3

Output: 2

Explanation: There are two paths where the sum of the elements on the path is divisible by k.
The first path highlighted in red has a sum of 5 + 2 + 4 + 5 + 2 = 18 which is divisible by 3.
The second path highlighted in blue has a sum of 5 + 3 + 0 + 5 + 2 = 15 which is divisible by 3.

![img](https://assets.leetcode.com/uploads/2022/08/13/image-20220813183124-1.png)



Example 2:

Input: grid = [[0,0]], k = 5

Output: 1
Explanation: The path highlighted in red has a sum of 0 + 0 = 0 which is divisible by 5.

![img](https://assets.leetcode.com/uploads/2022/08/17/image-20220817112930-3.png)


Example 3:

Input: grid = [[7,3,4,9],[2,3,6,2],[2,3,7,0]], k = 1

Output: 10

Explanation: Every integer is divisible by 1 so the sum of the elements on every possible path is divisible by k.

![img](https://assets.leetcode.com/uploads/2022/08/12/image-20220812224605-3.png)

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 5 * 104
1 <= m * n <= 5 * 104
0 <= grid[i][j] <= 100
1 <= k <= 50


# code

```cpp []
class Solution {
public:
    int numberOfPaths(vector<vector<int>>& grid, int k) {
        const int MOD = 1000000007;
        int m = grid.size(), n = grid[0].size();

        vector<vector<int>> prev(n, vector<int>(k)), curr(n, vector<int>(k));
        int sum = 0;

        for (int j = 0; j < n; j++) {
            sum = (sum + grid[0][j]) % k;
            prev[j][sum] = 1;
        }

        sum = grid[0][0] % k;

        for (int i = 1; i < m; i++) {
            sum = (sum + grid[i][0]) % k;
            fill(curr[0].begin(), curr[0].end(), 0);
            curr[0][sum] = 1;

            for (int j = 1; j < n; j++) {
                fill(curr[j].begin(), curr[j].end(), 0);
                int val = grid[i][j];
                for (int r = 0; r < k; r++) {
                    int nr = (r + val) % k;
                    curr[j][nr] = (prev[j][r] + curr[j - 1][r]) % MOD;
                }
            }

            swap(prev, curr);
        }

        return prev[n - 1][0];
    }
};
```

-------------------------------------------------------------------------------------------------------------------------------------------------

# 3381. Maximum Subarray Sum With Length Divisible by K -> [LeetCode](https://leetcode.com/problems/maximum-subarray-sum-with-length-divisible-by-k/description/)
 
Medium
 
You are given an array of integers nums and an integer k.

Return the maximum sum of a subarray of nums, such that the size of the subarray is divisible by k.

 

Example 1:

Input: nums = [1,2], k = 1

Output: 3

Explanation:

The subarray [1, 2] with sum 3 has length equal to 2 which is divisible by 1.

Example 2:

Input: nums = [-1,-2,-3,-4,-5], k = 4

Output: -10

Explanation:

The maximum sum subarray is [-1, -2, -3, -4] which has length equal to 4 which is divisible by 4.

Example 3:

Input: nums = [-5,1,2,-3,4], k = 2

Output: 4

Explanation:

The maximum sum subarray is [1, 2, -3, 4] which has length equal to 4 which is divisible by 2.

 

Constraints:

1 <= k <= nums.length <= 2 * 105
-109 <= nums[i] <= 109


# Code
```cpp []
class Solution {
public:
    using ll = long long;
    static long long maxSubarraySum(vector<int>& nums, int k) {
        const int n = nums.size();
        vector<ll> minS(k, LLONG_MAX / 2);
        ll prefix = 0, ans = LLONG_MIN;
        minS[k - 1] = 0;
        for (int i = 0; i < n; i++) {
            prefix += nums[i];
            ll& ksum = minS[i % k];
            ans = max(ans, prefix - ksum);
            ksum = min(prefix, ksum);
        }
        return ans;
    }
};

```

----------------------------------------------------------------------------------------------------------------------------------------


# 2872. Maximum Number of K-Divisible Components -> [LeetCode](https://leetcode.com/problems/maximum-number-of-k-divisible-components/description)
 
Hard

There is an undirected tree with n nodes labeled from 0 to n - 1. You are given the integer n and a 2D integer array edges of length n - 1, where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

You are also given a 0-indexed integer array values of length n, where values[i] is the value associated with the ith node, and an integer k.

A valid split of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by k, where the value of a connected component is the sum of the values of its nodes.

Return the maximum number of components in any valid split.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2023/08/07/example12-cropped2svg.jpg)

Input: n = 5, edges = [[0,2],[1,2],[1,3],[2,4]], values = [1,8,1,4,4], k = 6

Output: 2

Explanation: We remove the edge connecting node 1 with 2. The resulting split is valid because:
- The value of the component containing nodes 1 and 3 is values[1] + values[3] = 12.
- The value of the component containing nodes 0, 2, and 4 is values[0] + values[2] + values[4] = 6.
It can be shown that no other valid split has more than 2 connected components.



Example 2:

![img](https://assets.leetcode.com/uploads/2023/08/07/example21svg-1.jpg)

Input: n = 7, edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [3,0,6,1,5,2,1], k = 3

Output: 3

Explanation: We remove the edge connecting node 0 with 2, and the edge connecting node 0 with 1. The resulting split is valid because:
- The value of the component containing node 0 is values[0] = 3.
- The value of the component containing nodes 2, 5, and 6 is values[2] + values[5] + values[6] = 9.
- The value of the component containing nodes 1, 3, and 4 is values[1] + values[3] + values[4] = 6.
It can be shown that no other valid split has more than 3 connected components.
 

Constraints:

1 <= n <= 3 * 104
edges.length == n - 1
edges[i].length == 2
0 <= ai, bi < n
values.length == n
0 <= values[i] <= 109
1 <= k <= 109
Sum of values is divisible by k.
The input is generated such that edges represents a valid tree.



# Code
```cpp []
class Solution {
private:
    unordered_map<int, vector<int>> adj;
    unordered_set<int> visited;
    int comp;

public:
    int maxKDivisibleComponents(int n, vector<vector<int>>& edges, vector<int>& values, int k) {
        comp = 0;

        int src = 0;

        for (const vector<int>& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        dfs(src, values, k);
        return comp;
    }

private:
    int dfs(int root, vector<int>& values, int k) {
        if (visited.count(root)) {
            return 0;
        }

        visited.insert(root);
        int ans = values[root];

        for (int neigh : adj[root]) {
            ans += dfs(neigh, values, k);
        }

        if (ans % k == 0) {
            comp++;
            return 0;
        }

        return ans % k;
    }
};
```

-----------------------------------------------------------------------------------------------------------------------------------------------

# 3512. Minimum Operations to Make Array Sum Divisible by K -> [LeetCode](https://leetcode.com/problems/minimum-operations-to-make-array-sum-divisible-by-k/description)
 
Easy
 
You are given an integer array nums and an integer k. You can perform the following operation any number of times:

Select an index i and replace nums[i] with nums[i] - 1.
Return the minimum number of operations required to make the sum of the array divisible by k.

 

Example 1:

Input: nums = [3,9,7], k = 5

Output: 4

Explanation:

Perform 4 operations on nums[1] = 9. Now, nums = [3, 5, 7].
The sum is 15, which is divisible by 5.
Example 2:

Input: nums = [4,1,3], k = 4

Output: 0

Explanation:

The sum is 8, which is already divisible by 4. Hence, no operations are needed.
Example 3:

Input: nums = [3,2], k = 6

Output: 5

Explanation:

Perform 3 operations on nums[0] = 3 and 2 operations on nums[1] = 2. Now, nums = [0, 0].
The sum is 0, which is divisible by 6.
 

Constraints:

1 <= nums.length <= 1000
1 <= nums[i] <= 1000
1 <= k <= 100


# Code
```cpp []
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        long long sum = 0;
        for(const int n:nums) sum += n;
        return sum % k;
    }
};
```


---------------------------------------------------------------------------------------------------------------------------------------


# 1590. Make Sum Divisible by P -> [LeetCode](https://leetcode.com/problems/make-sum-divisible-by-p/)

Medium
 
Given an array of positive integers nums, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by p. It is not allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

A subarray is defined as a contiguous block of elements in the array.

 

Example 1:

Input: nums = [3,1,4,2], p = 6

Output: 1

Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.


Example 2:

Input: nums = [6,3,5,2], p = 9

Output: 2

Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.


Example 3:

Input: nums = [1,2,3], p = 3

Output: 0

Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
 


Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
1 <= p <= 109



# Code
```cpp []
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        long totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }

        int rem = totalSum % p;
        if (rem == 0) return 0; 

        unordered_map<int, int> prefixMod;
        prefixMod[0] = -1;  
        long prefixSum = 0;
        int minLength = nums.size();

        for (int i = 0; i < nums.size(); ++i) {
            prefixSum += nums[i];
            int currentMod = prefixSum % p;
            int targetMod = (currentMod - rem + p) % p;

            if (prefixMod.find(targetMod) != prefixMod.end()) {
                minLength = min(minLength, i - prefixMod[targetMod]);
            }

            prefixMod[currentMod] = i;
        }

        return minLength == nums.size() ? -1 : minLength;
    }
};
```

-----------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------


![img](https://assets.leetcode.com/static_assets/marketing/202511.gif)


-----------------------------------------------------------------------------------------------------------------------------------------------





