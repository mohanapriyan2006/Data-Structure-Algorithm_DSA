------------------------------------------------------------------------------
# **Leetcode** DEC - 2025 problems
------------------------------------------------------------------------------

# 2141. Maximum Running Time of N Computers -> [LeetCode](https://leetcode.com/problems/maximum-running-time-of-n-computers/description)
 
Hard
 
You have n computers. You are given the integer n and a 0-indexed integer array batteries where the ith battery can run a computer for batteries[i] minutes. You are interested in running all n computers simultaneously using the given batteries.

Initially, you can insert at most one battery into each computer. After that and at any integer time moment, you can remove a battery from a computer and insert another battery any number of times. The inserted battery can be a totally new battery or a battery from another computer. You may assume that the removing and inserting processes take no time.

Note that the batteries cannot be recharged.

Return the maximum number of minutes you can run all the n computers simultaneously.

 

Example 1:


Input: n = 2, batteries = [3,3,3]

Output: 4

![img](https://assets.leetcode.com/uploads/2022/01/06/example1-fit.png)

Explanation: 
Initially, insert battery 0 into the first computer and battery 1 into the second computer.
After two minutes, remove battery 1 from the second computer and insert battery 2 instead. Note that battery 1 can still run for one minute.
At the end of the third minute, battery 0 is drained, and you need to remove it from the first computer and insert battery 1 instead.
By the end of the fourth minute, battery 1 is also drained, and the first computer is no longer running.
We can run the two computers simultaneously for at most 4 minutes, so we return 4.



Example 2:


Input: n = 2, batteries = [1,1,1,1]

Output: 2

![img](https://assets.leetcode.com/uploads/2022/01/06/example2.png)

Explanation: 
Initially, insert battery 0 into the first computer and battery 2 into the second computer. 
After one minute, battery 0 and battery 2 are drained so you need to remove them and insert battery 1 into the first computer and battery 3 into the second computer. 
After another minute, battery 1 and battery 3 are also drained so the first and second computers are no longer running.
We can run the two computers simultaneously for at most 2 minutes, so we return 2.
 

Constraints:

1 <= n <= batteries.length <= 105
1 <= batteries[i] <= 109

# Code
```cpp []
class Solution {
public:
    long long maxRunTime(int n, vector<int>& arr) {
        sort(arr.begin(), arr.end());
        long long total = accumulate(arr.begin(), arr.end(), 0LL);

        for (int i = arr.size() - 1; i >= 0; i--) {
            if (arr[i] <= total / n) break;
            total -= arr[i];
            n--;
        }

        return total / n;
    }
};

```


----------------------------------------------------------------------

# 3623. Count Number of Trapezoids I -> [LeetCode](https://leetcode.com/problems/count-number-of-trapezoids-i/description)

Medium
 
You are given a 2D integer array points, where points[i] = [xi, yi] represents the coordinates of the ith point on the Cartesian plane.

A horizontal trapezoid is a convex quadrilateral with at least one pair of horizontal sides (i.e. parallel to the x-axis). Two lines are parallel if and only if they have the same slope.

Return the number of unique horizontal trapezoids that can be formed by choosing any four distinct points from points.

Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: points = [[1,0],[2,0],[3,0],[2,2],[3,2]]

Output: 3

Explanation:

![img](https://assets.leetcode.com/uploads/2025/05/01/desmos-graph-7.png)


There are three distinct ways to pick four points that form a horizontal trapezoid:

Using points [1,0], [2,0], [3,2], and [2,2].
Using points [2,0], [3,0], [3,2], and [2,2].
Using points [1,0], [3,0], [3,2], and [2,2].



Example 2:

Input: points = [[0,0],[1,0],[0,1],[2,1]]

Output: 1

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-5.png)

There is only one horizontal trapezoid that can be formed.

 
Constraints:

4 <= points.length <= 105
–108 <= xi, yi <= 108
All points are pairwise distinct.



# Code
```cpp []
class Solution {
public:
    int countTrapezoids(vector<vector<int>>& points) {
        int MOD = 1000000007;
        unordered_map<int, long long> groups;
        for (auto& point : points)
            groups[point[1]]++;
        long long res = 0, total = 0;
        for (auto& group : groups){
            long long lines = group.second * (group.second - 1) / 2;
            res = (res + total * lines) % MOD;
            total = (total + lines) % MOD;
        }
        return (int)res;
    }
};
```

--------------------------------------------------------------------------------------------

# 3625. Count Number of Trapezoids II -> [LeetCode](https://leetcode.com/problems/count-number-of-trapezoids-ii/description)

Hard

You are given a 2D integer array points where points[i] = [xi, yi] represents the coordinates of the ith point on the Cartesian plane.

Return the number of unique trapezoids that can be formed by choosing any four distinct points from points.

A trapezoid is a convex quadrilateral with at least one pair of parallel sides. Two lines are parallel if and only if they have the same slope.

 

Example 1:

Input: points = [[-3,2],[3,0],[2,3],[3,2],[2,-3]]

Output: 2

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-3.png)

There are two distinct ways to pick four points that form a trapezoid:

The points [-3,2], [2,3], [3,2], [2,-3] form one trapezoid.
The points [2,3], [3,2], [3,0], [2,-3] form another trapezoid.



Example 2:

Input: points = [[0,0],[1,0],[0,1],[2,1]]

Output: 1

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-5.png)

There is only one trapezoid which can be formed.

 

Constraints:

4 <= points.length <= 500
–1000 <= xi, yi <= 1000
All points are pairwise distinct.




# Code
```cpp []
constexpr int bias=1<<11;
class Solution {
public:
    using ll=long long;

    // Pack 2 integers into one  key
    static int pack2(int a, int b) {
        return ((ll)(a+bias)<<16) | (b+bias);
    }

    // Pack 3 integers into one 64-bit key, note |c|<=2e6
    static ll pack3(int a, int b, int c) {
        return ((ll)(a+bias)<<50)|((ll)(b+bias)<<30)|(c+bias*bias);
    }

    // Pack 4 integers into one 64-bit key
    static ll pack4(int a, int b, int c, int d) {
        return ((ll)(a+bias)<<48)|((ll)(b+bias)<<32)|((ll)(c+bias)<<16)|(d+ bias);
    }

    static int countTrapezoids(vector<vector<int>>& points) {
        const int n=points.size();
        const int nn=n*(n-1);

        unordered_map<ll,int> coeff, midPointWslope;
        unordered_map<ll,int> slope, midPoint;

        coeff.reserve(nn);
        slope.reserve(nn);
        midPointWslope.reserve(nn);
        midPoint.reserve(nn);

        int cnt=0;

        for(int i=0; i<n-1; i++) {
            const int x0=points[i][0], y0=points[i][1];
            for(int j=i+1; j< n; j++) {
                const int x1=points[j][0], y1=points[j][1];
                
                int a=y1-y0;
                int b=x0-x1;
                int c=y0*x1-y1*x0;

                if(a==0 && b<0) { b=-b; c=-c; }
                else if(a<0) { a=-a; b=-b; c=-c; }

                int gm=gcd(a, b), gc=gcd(gm, c);

                int ab=pack2(a/gm, b/gm);           
                ll abc=pack3(a/gc, b/gc, c/gc);    

                ll midP=pack2(x0+x1, y0+y1);       
                ll midab=pack4(x0+x1, y0+y1, a/gm, b/gm); 

                cnt+=(slope[ab]++)
                    -(coeff[abc]++)
                    -(midPoint[midP]++)
                    +(midPointWslope[midab]++);
            }
        }
        return cnt;
    }
};
```

--------------------------------------------------------------------------------------------------

# 2211. Count Collisions on a Road -> [LeetCode](https://leetcode.com/problems/count-collisions-on-a-road/description)

Medium
 
There are n cars on an infinitely long road. The cars are numbered from 0 to n - 1 from left to right and each car is present at a unique point.

You are given a 0-indexed string directions of length n. directions[i] can be either 'L', 'R', or 'S' denoting whether the ith car is moving towards the left, towards the right, or staying at its current point respectively. Each moving car has the same speed.

The number of collisions can be calculated as follows:

When two cars moving in opposite directions collide with each other, the number of collisions increases by 2.
When a moving car collides with a stationary car, the number of collisions increases by 1.
After a collision, the cars involved can no longer move and will stay at the point where they collided. Other than that, cars cannot change their state or direction of motion.

Return the total number of collisions that will happen on the road.

 

Example 1:

Input: directions = "RLRSLL"

Output: 5

Explanation:

The collisions that will happen on the road are:
- Cars 0 and 1 will collide with each other. Since they are moving in opposite directions, the number of collisions becomes 0 + 2 = 2.
- Cars 2 and 3 will collide with each other. Since car 3 is stationary, the number of collisions becomes 2 + 1 = 3.
- Cars 3 and 4 will collide with each other. Since car 3 is stationary, the number of collisions becomes 3 + 1 = 4.
- Cars 4 and 5 will collide with each other. After car 4 collides with car 3, it will stay at the point of collision and get hit by car 5. The number of collisions becomes 4 + 1 = 5.
Thus, the total number of collisions that will happen on the road is 5. 



Example 2:

Input: directions = "LLRR"

Output: 0

Explanation:

No cars will collide with each other. Thus, the total number of collisions that will happen on the road is 0.
 

Constraints:

1 <= directions.length <= 105
directions[i] is either 'L', 'R', or 'S'.



# Code
```cpp []
class Solution {
public:
    static int countCollisions(string& D) {
        int n=D.size();
        if (n==1) return 0;
        int l=0, r=n-1;
        while (D[l]=='L') l++;
        while (l<r && D[r]=='R') r--;
        if (l>=r) return 0;
        int col=0;
        for( ; l<=r; l++){
           col+=D[l]!='S';
        }
        return col;      
    }
};
```

--------------------------------------------------------------------------------------------

# 3432. Count Partitions with Even Sum Difference -> [LeetCode](https://leetcode.com/problems/count-partitions-with-even-sum-difference)
 
Easy

You are given an integer array nums of length n.

A partition is defined as an index i where 0 <= i < n - 1, splitting the array into two non-empty subarrays such that:

Left subarray contains indices [0, i].
Right subarray contains indices [i + 1, n - 1].
Return the number of partitions where the difference between the sum of the left and right subarrays is even.

 

Example 1:

Input: nums = [10,10,3,7,6]

Output: 4

Explanation:

The 4 partitions are:

[10], [10, 3, 7, 6] with a sum difference of 10 - 26 = -16, which is even.
[10, 10], [3, 7, 6] with a sum difference of 20 - 16 = 4, which is even.
[10, 10, 3], [7, 6] with a sum difference of 23 - 13 = 10, which is even.
[10, 10, 3, 7], [6] with a sum difference of 30 - 6 = 24, which is even.


Example 2:

Input: nums = [1,2,2]

Output: 0

Explanation:

No partition results in an even sum difference.


Example 3:

Input: nums = [2,4,6,8]

Output: 3

Explanation:

All partitions result in an even sum difference.

 

Constraints:

2 <= n == nums.length <= 100
1 <= nums[i] <= 100


# Code
```cpp []
class Solution {
public:
    int countPartitions(vector<int>& nums) {
        int sum = accumulate(nums.begin() , nums.end() , 0);
        int res = -1;

        for(const int num:nums){
            int rem = sum - num;
            if(abs(num - rem) % 2 == 0) res++;
        }

        return (res == -1) ? 0 : res;
    }
};
```

--------------------------------------------------------------------------------------


# 3578. Count Partitions With Max-Min Difference at Most K -> [LeetCode](https://leetcode.com/problems/count-partitions-with-max-min-difference-at-most-k/)

Medium
 
You are given an integer array nums and an integer k. Your task is to partition nums into one or more non-empty contiguous segments such that in each segment, the difference between its maximum and minimum elements is at most k.

Return the total number of ways to partition nums under this condition.

Since the answer may be too large, return it modulo 109 + 7.

 

Example 1:

Input: nums = [9,4,1,3,7], k = 4

Output: 6

Explanation:

There are 6 valid partitions where the difference between the maximum and minimum elements in each segment is at most k = 4:

[[9], [4], [1], [3], [7]]
[[9], [4], [1], [3, 7]]
[[9], [4], [1, 3], [7]]
[[9], [4, 1], [3], [7]]
[[9], [4, 1], [3, 7]]
[[9], [4, 1, 3], [7]]



Example 2:

Input: nums = [3,3,4], k = 0

Output: 2

Explanation:

There are 2 valid partitions that satisfy the given conditions:

[[3], [3], [4]]
[[3, 3], [4]]
 

Constraints:

2 <= nums.length <= 5 * 104
1 <= nums[i] <= 109
0 <= k <= 109


# Code
```cpp []
class Solution {
public:
    int countPartitions(vector<int>& A, int k) {
        int n = A.size(), mod = 1e9 + 7, acc = 1;
        vector<int> dp(n + 1, 0);
        dp[0] = 1;

        deque<int> minq, maxq;
        for (int i = 0, j = 0; j < n; ++j) {
            while (!maxq.empty() && A[j] > A[maxq.back()])
                maxq.pop_back();
            maxq.push_back(j);
            while (!minq.empty() && A[j] < A[minq.back()])
                minq.pop_back();
            minq.push_back(j);
            while (A[maxq.front()] - A[minq.front()] > k) {
                acc = (acc - dp[i++] + mod) % mod;
                if (minq.front() < i)
                    minq.pop_front();
                if (maxq.front() < i)
                    maxq.pop_front();
            }

            dp[j + 1] = acc;
            acc = (acc + dp[j + 1]) % mod;
        }
        return dp[n];
    }
};
```

------------------------------------------------------------------------------------------------------------

# 1523. Count Odd Numbers in an Interval Range -> [LeetCode](https://leetcode.com/problems/count-odd-numbers-in-an-interval-range/description)

Easy

Given two non-negative integers low and high. Return the count of odd numbers between low and high (inclusive).

 

Example 1:

Input: low = 3, high = 7

Output: 3

Explanation: The odd numbers between 3 and 7 are [3,5,7].



Example 2:

Input: low = 8, high = 10

Output: 1

Explanation: The odd numbers between 8 and 10 are [9].
 

Constraints:

0 <= low <= high <= 10^9


# Code
```cpp []
class Solution {
public:
    int countOdds(int low, int high) {
        int res = 0;
        for(int i=low ; i<=high ; ++i){
            if(i % 2 == 1) res++;
        }
        return res;
    }
};
```


------------------------------------------------------------------------------------------------------------------------------------------------------

# 1925. Count Square Sum Triples -> [LeetCode](https://leetcode.com/problems/count-square-sum-triples)

Easy
 
A square triple (a,b,c) is a triple where a, b, and c are integers and a2 + b2 = c2.

Given an integer n, return the number of square triples such that 1 <= a, b, c <= n.

 

Example 1:

Input: n = 5

Output: 2

Explanation: The square triples are (3,4,5) and (4,3,5).


Example 2:

Input: n = 10

Output: 4

Explanation: The square triples are (3,4,5), (4,3,5), (6,8,10), and (8,6,10).

 

Constraints:

1 <= n <= 250



# Code
```cpp []
class Solution {
public:
    int countTriples(int n) {
        int res = 0;
        for (int u = 2; u <= sqrt(n); u++) {
            for (int v = 1; v < u; v++) {
                if (~(u - v) & 1 || gcd(u, v) != 1) continue;
                int c = u * u + v * v;
                if (c > n) continue;
                res += (n / c) << 1;
            }
        }
        return res;
    }
};

```

-----------------------------------------------------------------------------------------------------------------------


# 3583. Count Special Triplets -> [LeetCode](https://leetcode.com/problems/count-special-triplets/)

Medium
 
You are given an integer array nums.

A special triplet is defined as a triplet of indices (i, j, k) such that:

0 <= i < j < k < n, where n = nums.length
nums[i] == nums[j] * 2
nums[k] == nums[j] * 2
Return the total number of special triplets in the array.

Since the answer may be large, return it modulo 109 + 7.

 

Example 1:

Input: nums = [6,3,6]

Output: 1

Explanation:

The only special triplet is (i, j, k) = (0, 1, 2), where:

nums[0] = 6, nums[1] = 3, nums[2] = 6
nums[0] = nums[1] * 2 = 3 * 2 = 6
nums[2] = nums[1] * 2 = 3 * 2 = 6


Example 2:

Input: nums = [0,1,0,0]

Output: 1

Explanation:

The only special triplet is (i, j, k) = (0, 2, 3), where:

nums[0] = 0, nums[2] = 0, nums[3] = 0
nums[0] = nums[2] * 2 = 0 * 2 = 0
nums[3] = nums[2] * 2 = 0 * 2 = 0


Example 3:

Input: nums = [8,4,2,8,4]

Output: 2

Explanation:

There are exactly two special triplets:

(i, j, k) = (0, 1, 3)
nums[0] = 8, nums[1] = 4, nums[3] = 8
nums[0] = nums[1] * 2 = 4 * 2 = 8
nums[3] = nums[1] * 2 = 4 * 2 = 8
(i, j, k) = (1, 2, 4)
nums[1] = 4, nums[2] = 2, nums[4] = 4
nums[1] = nums[2] * 2 = 2 * 2 = 4
nums[4] = nums[2] * 2 = 2 * 2 = 4
 

Constraints:

3 <= n == nums.length <= 105
0 <= nums[i] <= 105



# Code
```cpp []
const int MOD = 1e9 + 7;
class Solution {
public:
    int specialTriplets(vector<int>& nums) {
        int n = nums.size();
        long long result = 0;
        unordered_map<int, int> r, l;
        for (int val : nums) {
            r[val]++;
        }

        for (int j = 0; j < n; ++j) {
            int mid = nums[j];
            r[mid]--; 
            int left = l[2 * mid];
            int right = r[2 * mid];
            result = (result + 1LL * left * right) % MOD;
            l[mid]++;
        }

        return result;
    }
};
```

---------------------------------------------------------------------------------------------------------------------



# 3577. Count the Number of Computer Unlocking Permutations -> [LeetCode](https://leetcode.com/problems/count-the-number-of-computer-unlocking-permutations/)

Medium

You are given an array complexity of length n.

There are n locked computers in a room with labels from 0 to n - 1, each with its own unique password. The password of the computer i has a complexity complexity[i].

The password for the computer labeled 0 is already decrypted and serves as the root. All other computers must be unlocked using it or another previously unlocked computer, following this information:

You can decrypt the password for the computer i using the password for computer j, where j is any integer less than i with a lower complexity. (i.e. j < i and complexity[j] < complexity[i])
To decrypt the password for computer i, you must have already unlocked a computer j such that j < i and complexity[j] < complexity[i].
Find the number of permutations of [0, 1, 2, ..., (n - 1)] that represent a valid order in which the computers can be unlocked, starting from computer 0 as the only initially unlocked one.

Since the answer may be large, return it modulo 109 + 7.

Note that the password for the computer with label 0 is decrypted, and not the computer with the first position in the permutation.

 

Example 1:

Input: complexity = [1,2,3]

Output: 2

Explanation:

The valid permutations are:

[0, 1, 2]
Unlock computer 0 first with root password.
Unlock computer 1 with password of computer 0 since complexity[0] < complexity[1].
Unlock computer 2 with password of computer 1 since complexity[1] < complexity[2].
[0, 2, 1]
Unlock computer 0 first with root password.
Unlock computer 2 with password of computer 0 since complexity[0] < complexity[2].
Unlock computer 1 with password of computer 0 since complexity[0] < complexity[1].




Example 2:

Input: complexity = [3,3,3,4,4,4]

Output: 0

Explanation:

There are no possible permutations which can unlock all computers.

 

Constraints:

2 <= complexity.length <= 105
1 <= complexity[i] <= 109



# code 
```cpp []
class Solution {
public:
    static const int MOD = 1000000007;

    int countPermutations(vector<int>& complexity) {
        int n = complexity.size();
        int first = complexity[0];

        for (int i = 1; i < n; i++) {
            if (complexity[i] <= first) return 0;
        }

        long long fact = 1;
        for (int i = 2; i < n; i++) {
            fact = (fact * i) % MOD;
        }

        return (int)fact;
    }
};
```


------------------------------------------------------------------------



# 3531. Count Covered Buildings -> [LeetCode](https://leetcode.com/problems/count-covered-buildings/)

Medium

You are given a positive integer n, representing an n x n city. You are also given a 2D grid buildings, where buildings[i] = [x, y] denotes a unique building located at coordinates [x, y].

A building is covered if there is at least one building in all four directions: left, right, above, and below.

Return the number of covered buildings.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2025/03/04/telegram-cloud-photo-size-5-6212982906394101085-m.jpg)

Input: n = 3, buildings = [[1,2],[2,2],[3,2],[2,1],[2,3]]

Output: 1

Explanation:

Only building [2,2] is covered as it has at least one building:
above ([1,2])
below ([3,2])
left ([2,1])
right ([2,3])
Thus, the count of covered buildings is 1.



Example 2:

![img](https://assets.leetcode.com/uploads/2025/03/04/telegram-cloud-photo-size-5-6212982906394101086-m.jpg)

Input: n = 3, buildings = [[1,1],[1,2],[2,1],[2,2]]

Output: 0

Explanation:

No building has at least one building in all four directions.



Example 3:

![img](https://assets.leetcode.com/uploads/2025/03/16/telegram-cloud-photo-size-5-6248862251436067566-x.jpg)

Input: n = 5, buildings = [[1,3],[3,2],[3,3],[3,5],[5,3]]

Output: 1

Explanation:

Only building [3,3] is covered as it has at least one building:
above ([1,3])
below ([5,3])
left ([3,2])
right ([3,5])
Thus, the count of covered buildings is 1.
 

Constraints:

2 <= n <= 105
1 <= buildings.length <= 105 
buildings[i] = [x, y]
1 <= x, y <= n
All coordinates of buildings are unique.


# Code
```cpp []
class Solution {
public:
    int countCoveredBuildings(int n, vector<vector<int>>& grid, int count = 0) {
        unordered_map<int,set<int>> st1, st2;
        for (auto& p : grid) {
            st1[p[0]].insert(p[1]);
            st2[p[1]].insert(p[0]);
        }
        
        for (auto& p : grid) {
            auto& it1 = st1[p[0]];
            auto& it2 = st2[p[1]];
            auto[downy, uph] = it1.equal_range(p[1]);
            auto[downx, upx] = it2.equal_range(p[0]);
            bool up = downy != it1.begin();
            bool down = uph != it1.end();
            bool left = downx != it2.begin();
            bool right = upx != it2.end();
            if (up && down && left && right) ++count;
        }
        return count;
    }
};
```


----------------------------------------------------------------------------------------------------------------------------------------------


# 3433. Count Mentions Per User -> [LeetCode](https://leetcode.com/problems/count-mentions-per-user)

Medium

You are given an integer numberOfUsers representing the total number of users and an array events of size n x 3.

Each events[i] can be either of the following two types:

Message Event: ["MESSAGE", "timestampi", "mentions_stringi"]
This event indicates that a set of users was mentioned in a message at timestampi.
The mentions_stringi string can contain one of the following tokens:
id<number>: where <number> is an integer in range [0,numberOfUsers - 1]. There can be multiple ids separated by a single whitespace and may contain duplicates. This can mention even the offline users.
ALL: mentions all users.
HERE: mentions all online users.
Offline Event: ["OFFLINE", "timestampi", "idi"]
This event indicates that the user idi had become offline at timestampi for 60 time units. The user will automatically be online again at time timestampi + 60.
Return an array mentions where mentions[i] represents the number of mentions the user with id i has across all MESSAGE events.

All users are initially online, and if a user goes offline or comes back online, their status change is processed before handling any message event that occurs at the same timestamp.

Note that a user can be mentioned multiple times in a single message event, and each mention should be counted separately.


 

Example 1:

Input: numberOfUsers = 2, events = [["MESSAGE","10","id1 id0"],["OFFLINE","11","0"],["MESSAGE","71","HERE"]]

Output: [2,2]

Explanation:

Initially, all users are online.

At timestamp 10, id1 and id0 are mentioned. mentions = [1,1]

At timestamp 11, id0 goes offline.

At timestamp 71, id0 comes back online and "HERE" is mentioned. mentions = [2,2]



Example 2:

Input: numberOfUsers = 2, events = [["MESSAGE","10","id1 id0"],["OFFLINE","11","0"],["MESSAGE","12","ALL"]]

Output: [2,2]

Explanation:

Initially, all users are online.

At timestamp 10, id1 and id0 are mentioned. mentions = [1,1]

At timestamp 11, id0 goes offline.

At timestamp 12, "ALL" is mentioned. This includes offline users, so both id0 and id1 are mentioned. mentions = [2,2]



Example 3:

Input: numberOfUsers = 2, events = [["OFFLINE","10","0"],["MESSAGE","12","HERE"]]

Output: [0,1]

Explanation:

Initially, all users are online.

At timestamp 10, id0 goes offline.

At timestamp 12, "HERE" is mentioned. Because id0 is still offline, they will not be mentioned. mentions = [0,1]

 

Constraints:

1 <= numberOfUsers <= 100
1 <= events.length <= 100
events[i].length == 3
events[i][0] will be one of MESSAGE or OFFLINE.
1 <= int(events[i][1]) <= 105
The number of id<number> mentions in any "MESSAGE" event is between 1 and 100.
0 <= <number> <= numberOfUsers - 1
It is guaranteed that the user id referenced in the OFFLINE event is online at the time the event occurs.



# Code
```cpp []
#include<ranges>
class Solution {
public:
    vector<int> countMentions(int numberOfUsers, vector<vector<string>>& events) {
        
        ranges::sort(events, {}, [](auto& e) {
            return pair(stoi(e[1]), e[0][2]);
        });

        vector<int> ans(numberOfUsers);
        vector<int> online_t(numberOfUsers);
        for (auto& e : events) {
            int cur_t = stoi(e[1]);
            string& mention = e[2];
            if (e[0][0] == 'O') {
                online_t[stoi(mention)] = cur_t + 60;
            } else if (mention[0] == 'A') {
                for (int& v : ans) {
                    v++;
                }
            } else if (mention[0] == 'H') {
                for (int i = 0; i < numberOfUsers; i++) {
                    if (online_t[i] <= cur_t) { 
                        ans[i]++;
                    }
                }
            } else {
                for (const auto& part : mention | ranges::views::split(' ')) {
                    string s(part.begin() + 2, part.end());
                    ans[stoi(s)]++;
                }
            }
        }
        return ans;
    }
};
```


------------------------------------------------------------------------------------------------------------------------------------------------------



# 3606. Coupon Code Validator -> [LeetCode](https://leetcode.com/problems/coupon-code-validator)

Easy

You are given three arrays of length n that describe the properties of n coupons: code, businessLine, and isActive. The ith coupon has:

code[i]: a string representing the coupon identifier.
businessLine[i]: a string denoting the business category of the coupon.
isActive[i]: a boolean indicating whether the coupon is currently active.
A coupon is considered valid if all of the following conditions hold:

code[i] is non-empty and consists only of alphanumeric characters (a-z, A-Z, 0-9) and underscores (_).
businessLine[i] is one of the following four categories: "electronics", "grocery", "pharmacy", "restaurant".
isActive[i] is true.
Return an array of the codes of all valid coupons, sorted first by their businessLine in the order: "electronics", "grocery", "pharmacy", "restaurant", and then by code in lexicographical (ascending) order within each category.

 

Example 1:

Input: code = ["SAVE20","","PHARMA5","SAVE@20"], businessLine = ["restaurant","grocery","pharmacy","restaurant"], isActive = [true,true,true,true]

Output: ["PHARMA5","SAVE20"]

Explanation:

First coupon is valid.
Second coupon has empty code (invalid).
Third coupon is valid.
Fourth coupon has special character @ (invalid).



Example 2:

Input: code = ["GROCERY15","ELECTRONICS_50","DISCOUNT10"], businessLine = ["grocery","electronics","invalid"], isActive = [false,true,true]

Output: ["ELECTRONICS_50"]

Explanation:

First coupon is inactive (invalid).
Second coupon is valid.
Third coupon has invalid business line (invalid).
 

Constraints:

n == code.length == businessLine.length == isActive.length
1 <= n <= 100
0 <= code[i].length, businessLine[i].length <= 100
code[i] and businessLine[i] consist of printable ASCII characters.
isActive[i] is either true or false.



# Code
```cpp []
class Solution {
public:
    vector<string> validateCoupons(vector<string>& code, vector<string>& businessLine, vector<bool>& isActive) {
        int n = code.size();

        unordered_map<string, int> businessLineSortOrder = {
            {"electronics", 0},
            {"grocery", 1},
            {"pharmacy", 2},
            {"restaurant", 3}
        };

        vector<pair<pair<int, string>, string>> sortableCoupons;

        for (int i = 0; i < n; ++i) {
            if (!isActive[i]) continue;

            if (businessLineSortOrder.find(businessLine[i]) == businessLineSortOrder.end()) continue;

            if (code[i].empty()) continue;
            bool isCodeValid = true;
            for (char c : code[i]) {
                if (!(isalnum(c) || c == '_')) {
                    isCodeValid = false;
                    break;
                }
            }
            if (!isCodeValid) continue;

            int sortIndex = businessLineSortOrder[businessLine[i]];
            sortableCoupons.push_back({{sortIndex, code[i]}, code[i]});
        }

        sort(sortableCoupons.begin(), sortableCoupons.end());

        vector<string> sortedValidCodes;
        for (auto& entry : sortableCoupons) {
            sortedValidCodes.push_back(entry.second);
        }

        return sortedValidCodes;
    }
};
```


-----------------------------------------------------------------------------------------------------------------------------------------


# 2110. Number of Smooth Descent Periods of a Stock -> [LeetCode](https://leetcode.com/problems/number-of-smooth-descent-periods-of-a-stock)

Medium
 
You are given an integer array prices representing the daily price history of a stock, where prices[i] is the stock price on the ith day.

A smooth descent period of a stock consists of one or more contiguous days such that the price on each day is lower than the price on the preceding day by exactly 1. The first day of the period is exempted from this rule.

Return the number of smooth descent periods.

 

Example 1:

Input: prices = [3,2,1,4]
Output: 7
Explanation: There are 7 smooth descent periods:
[3], [2], [1], [4], [3,2], [2,1], and [3,2,1]
Note that a period with one day is a smooth descent period by the definition.


Example 2:

Input: prices = [8,6,7,7]
Output: 4
Explanation: There are 4 smooth descent periods: [8], [6], [7], and [7]
Note that [8,6] is not a smooth descent period as 8 - 6 ≠ 1.


Example 3:

Input: prices = [1]
Output: 1
Explanation: There is 1 smooth descent period: [1]
 

Constraints:

1 <= prices.length <= 105
1 <= prices[i] <= 105


# Code
```cpp []
class Solution {
public:
    long long getDescentPeriods(vector<int>& prices) {
        const int n=prices.size();
        long long sum=1, des=1;
        for(int i=1; i<n; i++){
            des=(prices[i]+1==prices[i-1])*des+1;
            sum+=des;
        }
        return sum;
    }
};
```

----------------------------------------------------------------------------------------------------------

# 2147. Number of Ways to Divide a Long Corridor -> [LeetCode](https://leetcode.com/problems/number-of-ways-to-divide-a-long-corridor)

Hard

Along a long library corridor, there is a line of seats and decorative plants. You are given a 0-indexed string corridor of length n consisting of letters 'S' and 'P' where each 'S' represents a seat and each 'P' represents a plant.

One room divider has already been installed to the left of index 0, and another to the right of index n - 1. Additional room dividers can be installed. For each position between indices i - 1 and i (1 <= i <= n - 1), at most one divider can be installed.

Divide the corridor into non-overlapping sections, where each section has exactly two seats with any number of plants. There may be multiple ways to perform the division. Two ways are different if there is a position with a room divider installed in the first way but not in the second way.

Return the number of ways to divide the corridor. Since the answer may be very large, return it modulo 109 + 7. If there is no way, return 0.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/12/04/1.png)

Input: corridor = "SSPPSPS"
Output: 3
Explanation: There are 3 different ways to divide the corridor.
The black bars in the above image indicate the two room dividers already installed.
Note that in each of the ways, each section has exactly two seats.



Example 2:

![img](https://assets.leetcode.com/uploads/2021/12/04/1.png)

Input: corridor = "PPSPSP"
Output: 1
Explanation: There is only 1 way to divide the corridor, by not installing any additional dividers.
Installing any would create some section that does not have exactly two seats.




Example 3:

![img](https://assets.leetcode.com/uploads/2021/12/12/3.png)

Input: corridor = "S"
Output: 0
Explanation: There is no way to divide the corridor because there will always be a section that does not have exactly two seats.
 

Constraints:

n == corridor.length
1 <= n <= 105
corridor[i] is either 'S' or 'P'.



# Code
```cpp []
class Solution {
public:
    const int mod = 1e9 + 7;
    int numberOfWays(string corridor) {
        vector<int> pos;
        for (int i = 0; i < corridor.size(); i++) {
            if (corridor[i] == 'S') {
                pos.push_back(i);
            }
        }
        
        if (pos.size() % 2 or pos.size() == 0) {
            return 0;
        }
        
        long res = 1;
        for (int i = 2; i < pos.size(); i += 2) {
            int len_of_gap = pos[i] - pos[i - 1];
            res = (res * len_of_gap) % mod;
        }

        return res;
    }
};
```

---------------------------------------------------------------------------------------------------------------------------------


# 3562. Maximum Profit from Trading Stocks with Discounts -> [LeetCode](https://leetcode.com/problems/maximum-profit-from-trading-stocks-with-discounts/)

Hard

You are given an integer n, representing the number of employees in a company. Each employee is assigned a unique ID from 1 to n, and employee 1 is the CEO. You are given two 1-based integer arrays, present and future, each of length n, where:

present[i] represents the current price at which the ith employee can buy a stock today.
future[i] represents the expected price at which the ith employee can sell the stock tomorrow.
The company's hierarchy is represented by a 2D integer array hierarchy, where hierarchy[i] = [ui, vi] means that employee ui is the direct boss of employee vi.

Additionally, you have an integer budget representing the total funds available for investment.

However, the company has a discount policy: if an employee's direct boss purchases their own stock, then the employee can buy their stock at half the original price (floor(present[v] / 2)).

Return the maximum profit that can be achieved without exceeding the given budget.

Note:

You may buy each stock at most once.
You cannot use any profit earned from future stock prices to fund additional investments and must buy only from budget.
 

Example 1:

Input: n = 2, present = [1,2], future = [4,3], hierarchy = [[1,2]], budget = 3

Output: 5

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

Employee 1 buys the stock at price 1 and earns a profit of 4 - 1 = 3.
Since Employee 1 is the direct boss of Employee 2, Employee 2 gets a discounted price of floor(2 / 2) = 1.
Employee 2 buys the stock at price 1 and earns a profit of 3 - 1 = 2.
The total buying cost is 1 + 1 = 2 <= budget. Thus, the maximum total profit achieved is 3 + 2 = 5.



Example 2:

Input: n = 2, present = [3,4], future = [5,8], hierarchy = [[1,2]], budget = 4

Output: 4

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

Employee 2 buys the stock at price 4 and earns a profit of 8 - 4 = 4.
Since both employees cannot buy together, the maximum profit is 4.



Example 3:

Input: n = 3, present = [4,6,8], future = [7,9,11], hierarchy = [[1,2],[1,3]], budget = 10

Output: 10

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/09/image.png)

Employee 1 buys the stock at price 4 and earns a profit of 7 - 4 = 3.
Employee 3 would get a discounted price of floor(8 / 2) = 4 and earns a profit of 11 - 4 = 7.
Employee 1 and Employee 3 buy their stocks at a total cost of 4 + 4 = 8 <= budget. Thus, the maximum total profit achieved is 3 + 7 = 10.



Example 4:

Input: n = 3, present = [5,2,3], future = [8,5,6], hierarchy = [[1,2],[2,3]], budget = 7

Output: 12

Explanation:

![img](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-054114.png)

Employee 1 buys the stock at price 5 and earns a profit of 8 - 5 = 3.
Employee 2 would get a discounted price of floor(2 / 2) = 1 and earns a profit of 5 - 1 = 4.
Employee 3 would get a discounted price of floor(3 / 2) = 1 and earns a profit of 6 - 1 = 5.
The total cost becomes 5 + 1 + 1 = 7 <= budget. Thus, the maximum total profit achieved is 3 + 4 + 5 = 12.
 

Constraints:

1 <= n <= 160
present.length, future.length == n
1 <= present[i], future[i] <= 50
hierarchy.length == n - 1
hierarchy[i] == [ui, vi]
1 <= ui, vi <= n
ui != vi
1 <= budget <= 160
There are no duplicate edges.
Employee 1 is the direct or indirect boss of every employee.
The input graph hierarchy is guaranteed to have no cycles.



# Code
```cpp []
const int N=161 ,N4=161*4;
int profit[N][2];
vector<int> children[N];
int dp[N][2][2][N];
bitset<N4> vis=0;

class Solution {
public:
    int n;

    void build_tree(const vector<vector<int>>& hierarchy) {
        for (int i=0; i<n; i++) children[i].clear();
        for (auto& e : hierarchy)
            children[e[0]-1].push_back(e[1]-1);
    }

    void dfs(int node, bool bossBuy, bool buy, int budget,
             const vector<int>& present) {

        if (vis[(node<<2)|(bossBuy<<1)|buy]) return;
        vis[(node<<2)|(bossBuy<<1)|buy]=1;

        int* cache=dp[node][bossBuy][buy];
        fill(cache, cache+budget+1, INT_MIN);

        int cost=buy?(bossBuy?present[node]/2:present[node]):0;
        if (cost<=budget)
            cache[cost]=buy?profit[node][bossBuy]:0;

        int* cur=(int*)alloca(sizeof(int)*(budget+1));
        int* merged=(int*)alloca(sizeof(int)*(budget+1));
        memcpy(cur, cache, sizeof(int)*(budget+1));

        for (int child : children[node]) {
            dfs(child, buy, 1, budget, present);
            dfs(child, 0, 0, budget, present);

            int* take=dp[child][buy][1];
            int* skip=dp[child][0][0];

            fill(merged, merged+budget+1, INT_MIN);

            for (int b=0; b<=budget; b++) if (cur[b]!=INT_MIN) {
                for (int x=0; b+x <= budget; x++) {
                    int best=max(take[x], skip[x]);
                    if (best!=INT_MIN)
                        merged[b+x]=max(merged[b+x], cur[b]+best);
                }
            }
            memcpy(cur, merged, sizeof(int)*(budget+1));
        }
        memcpy(cache, cur, sizeof(int)*(budget+1));
    }

    int maxProfit(int n, vector<int>& present, vector<int>& future,
                  vector<vector<int>>& hierarchy, int budget) {
        this->n=n;
        vis.reset();

        for (int i=0; i<n; i++) {
            profit[i][0]=future[i]-present[i];
            profit[i][1]=future[i]-present[i]/2;
        }

        build_tree(hierarchy);

        dfs(0, 0, 0, budget, present);
        dfs(0, 0, 1, budget, present);

        int ans=0;
        for (int b=0; b<=budget; b++) {
            ans=max(ans, dp[0][0][0][b]);
            ans=max(ans, dp[0][0][1][b]);
        }
        return ans;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------------------------


# 3573. Best Time to Buy and Sell Stock V -> [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-v)

Medium

You are given an integer array prices where prices[i] is the price of a stock in dollars on the ith day, and an integer k.

You are allowed to make at most k transactions, where each transaction can be either of the following:

Normal transaction: Buy on day i, then sell on a later day j where i < j. You profit prices[j] - prices[i].

Short selling transaction: Sell on day i, then buy back on a later day j where i < j. You profit prices[i] - prices[j].

Note that you must complete each transaction before starting another. Additionally, you can't buy or sell on the same day you are selling or buying back as part of a previous transaction.

Return the maximum total profit you can earn by making at most k transactions.

 

Example 1:

Input: prices = [1,7,9,8,2], k = 2

Output: 14

Explanation:

We can make $14 of profit through 2 transactions:
A normal transaction: buy the stock on day 0 for $1 then sell it on day 2 for $9.
A short selling transaction: sell the stock on day 3 for $8 then buy back on day 4 for $2.



Example 2:

Input: prices = [12,16,19,19,8,1,19,13,9], k = 3

Output: 36

Explanation:

We can make $36 of profit through 3 transactions:
A normal transaction: buy the stock on day 0 for $12 then sell it on day 2 for $19.
A short selling transaction: sell the stock on day 3 for $19 then buy back on day 4 for $8.
A normal transaction: buy the stock on day 5 for $1 then sell it on day 6 for $19.
 

Constraints:

2 <= prices.length <= 103
1 <= prices[i] <= 109
1 <= k <= prices.length / 2


# Code
```cpp []
class Solution {
public:
    vector<int> prices;
    long long mn = -1e14;
    vector<vector<vector<long long>>> dp;
    long long f(int i, int k, int state){
        if(i == prices.size()){
            return state == 0 ? 0 : mn;
        }
        if(dp[i][k][state] != mn) return dp[i][k][state];

        long long p = prices[i];
        long long profit = mn;
        profit = max(profit, f(i + 1, k, state));

        if(state == 0){
            profit = max(profit, f(i+1, k, 1) - p);
            profit = max(profit, f(i+1, k, 2) + p);
        }
        else if(k > 0){
            if(state == 1){
                profit = max(profit, f(i+1, k-1, 0) + p);
                
            }
            else{
                 profit = max(profit, f(i+1, k-1, 0) - p);
            }
        }
        return dp[i][k][state] = profit;
    }
    
    long long maximumProfit(vector<int>& prices, int k) {
        this->prices = prices;
        dp.assign(prices.size() + 1, vector<vector<long long>> (k + 1, vector<long long> (3, mn)));
        return f(0, k, 0);  
    }
};
```

-----------------------------------------------------------------------------------------------------------------------------------------------


# 3652. Best Time to Buy and Sell Stock using Strategy -> [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-using-strategy)

Medium

You are given two integer arrays prices and strategy, where:

prices[i] is the price of a given stock on the ith day.
strategy[i] represents a trading action on the ith day, where:
-1 indicates buying one unit of the stock.
0 indicates holding the stock.
1 indicates selling one unit of the stock.
You are also given an even integer k, and may perform at most one modification to strategy. A modification consists of:

Selecting exactly k consecutive elements in strategy.
Set the first k / 2 elements to 0 (hold).
Set the last k / 2 elements to 1 (sell).
The profit is defined as the sum of strategy[i] * prices[i] across all days.

Return the maximum possible profit you can achieve.

Note: There are no constraints on budget or stock ownership, so all buy and sell operations are feasible regardless of past actions.

 

Example 1:

Input: prices = [4,2,8], strategy = [-1,0,1], k = 2

Output: 10

Explanation:

Modification	Strategy	Profit Calculation	Profit
Original	[-1, 0, 1]	(-1 × 4) + (0 × 2) + (1 × 8) = -4 + 0 + 8	4
Modify [0, 1]	[0, 1, 1]	(0 × 4) + (1 × 2) + (1 × 8) = 0 + 2 + 8	10
Modify [1, 2]	[-1, 0, 1]	(-1 × 4) + (0 × 2) + (1 × 8) = -4 + 0 + 8	4
Thus, the maximum possible profit is 10, which is achieved by modifying the subarray [0, 1]​​​​​​​.


Example 2:

Input: prices = [5,4,3], strategy = [1,1,0], k = 2

Output: 9

Explanation:

Modification	Strategy	Profit Calculation	Profit
Original	[1, 1, 0]	(1 × 5) + (1 × 4) + (0 × 3) = 5 + 4 + 0	9
Modify [0, 1]	[0, 1, 0]	(0 × 5) + (1 × 4) + (0 × 3) = 0 + 4 + 0	4
Modify [1, 2]	[1, 0, 1]	(1 × 5) + (0 × 4) + (1 × 3) = 5 + 0 + 3	8
Thus, the maximum possible profit is 9, which is achieved without any modification.

 

Constraints:

2 <= prices.length == strategy.length <= 105
1 <= prices[i] <= 105
-1 <= strategy[i] <= 1
2 <= k <= prices.length
k is even
 


# Code
```cpp []
const int N=1e5+1;
long long sum[N];
class Solution {
public:
    static long long maxProfit(vector<int>& prices, vector<int>& strategy, int k) {
        const int n=prices.size(), k2=k/2;
        memset(sum, 0, sizeof(long long)*(n+1));
        for(int i=0; i<n; i++){
            sum[i+1]=sum[i]+1LL*strategy[i]*prices[i];
        }

        long long modify=reduce(prices.begin()+k2, prices.begin()+k, 0LL);
        long long profit=max(sum[n], modify+sum[n]-sum[k]);

        for(int i=1; i+k<=n; i++){ 
            modify+=prices[i+k-1]-prices[i+k2-1];
            profit=max(profit, modify+sum[n]-sum[i+k]+sum[i]);
        }
        return profit;
    }
};

```

---------------------------------------------------------------------------------------------------------------------------------------

# 2092. Find All People With Secret -> [LeetCode](https://leetcode.com/problems/find-all-people-with-secret)

Hard

You are given an integer n indicating there are n people numbered from 0 to n - 1. You are also given a 0-indexed 2D integer array meetings where meetings[i] = [xi, yi, timei] indicates that person xi and person yi have a meeting at timei. A person may attend multiple meetings at the same time. Finally, you are given an integer firstPerson.

Person 0 has a secret and initially shares the secret with a person firstPerson at time 0. This secret is then shared every time a meeting takes place with a person that has the secret. More formally, for every meeting, if a person xi has the secret at timei, then they will share the secret with person yi, and vice versa.

The secrets are shared instantaneously. That is, a person may receive the secret and share it with people in other meetings within the same time frame.

Return a list of all the people that have the secret after all the meetings have taken place. You may return the answer in any order.

 

Example 1:

Input: n = 6, meetings = [[1,2,5],[2,3,8],[1,5,10]], firstPerson = 1
Output: [0,1,2,3,5]
Explanation:
At time 0, person 0 shares the secret with person 1.
At time 5, person 1 shares the secret with person 2.
At time 8, person 2 shares the secret with person 3.
At time 10, person 1 shares the secret with person 5.​​​​
Thus, people 0, 1, 2, 3, and 5 know the secret after all the meetings.


Example 2:

Input: n = 4, meetings = [[3,1,3],[1,2,2],[0,3,3]], firstPerson = 3
Output: [0,1,3]
Explanation:
At time 0, person 0 shares the secret with person 3.
At time 2, neither person 1 nor person 2 know the secret.
At time 3, person 3 shares the secret with person 0 and person 1.
Thus, people 0, 1, and 3 know the secret after all the meetings.


Example 3:

Input: n = 5, meetings = [[3,4,2],[1,2,1],[2,3,1]], firstPerson = 1
Output: [0,1,2,3,4]
Explanation:
At time 0, person 0 shares the secret with person 1.
At time 1, person 1 shares the secret with person 2, and person 2 shares the secret with person 3.
Note that person 2 can share the secret at the same time as receiving it.
At time 2, person 3 shares the secret with person 4.
Thus, people 0, 1, 2, 3, and 4 know the secret after all the meetings.
 

Constraints:

2 <= n <= 105
1 <= meetings.length <= 105
meetings[i].length == 3
0 <= xi, yi <= n - 1
xi != yi
1 <= timei <= 105
1 <= firstPerson <= n - 1



# Code
```cpp []
class Solution {
public:
    #define ppr pair<int, int>
    vector<int> findAllPeople(int n, vector<vector<int>>& meetings, int firstPerson) {
        vector<pair<int,int>> adj[n];
        for (auto it : meetings) {
            adj[it[0]].push_back({it[1],it[2]});
            adj[it[1]].push_back({it[0],it[2]});
        }

        priority_queue<ppr, vector<ppr>, greater<ppr> > pq;
        pq.push({0, 0});
        pq.push({0, firstPerson});

        vector<int> vis(n, 0);

        while (!pq.empty()) {
            auto it = pq.top();
            int time=it.first; 
            int person=it.second;
            pq.pop();
            if (vis[person]) {
                continue;
            }
            vis[person] = true;
            for (auto it : adj[person]) {
                
    
                if (!vis[it.first] && it.second >= time) {
                    pq.push({it.second, it.first});
                }
            }
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                ans.push_back(i);
            }
        }

        return ans;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------------------------------


# 944. Delete Columns to Make Sorted -> [LeetCode](https://leetcode.com/problems/delete-columns-to-make-sorted)

Easy
 
You are given an array of n strings strs, all of the same length.

The strings can be arranged such that there is one on each line, making a grid.

For example, strs = ["abc", "bce", "cae"] can be arranged as follows:
abc
bce
cae
You want to delete the columns that are not sorted lexicographically. In the above example (0-indexed), columns 0 ('a', 'b', 'c') and 2 ('c', 'e', 'e') are sorted, while column 1 ('b', 'c', 'a') is not, so you would delete column 1.

Return the number of columns that you will delete.

 

Example 1:

Input: strs = ["cba","daf","ghi"]

Output: 1

Explanation: The grid looks as follows:

  cba
  daf
  ghi
Columns 0 and 2 are sorted, but column 1 is not, so you only need to delete 1 column.


Example 2:

Input: strs = ["a","b"]

Output: 0

Explanation: The grid looks as follows:

  a
  b
Column 0 is the only column and is sorted, so you will not delete any columns.



Example 3:

Input: strs = ["zyx","wvu","tsr"]

Output: 3

Explanation: The grid looks as follows:

  zyx
  wvu
  tsr
All 3 columns are not sorted, so you will delete all 3.
 

Constraints:

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 1000
strs[i] consists of lowercase English letters.


# Code
```cpp []
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int delete_count=0;
        int row = strs.size();
        int col = strs[0].size();
        
        for(int j=0; j<col; j++)
        {
            for(int i=0; i<row-1; i++)
            {
                if(strs[i][j]>strs[i+1][j])
                {
                    delete_count++;
                    break;
                }
            }
        }
        return delete_count;
    }
};
```


----------------------------------------------------------------------------------------------------------------------------------------------


# 955. Delete Columns to Make Sorted II -> [LeetCode](https://leetcode.com/problems/delete-columns-to-make-sorted-ii/)

Medium

You are given an array of n strings strs, all of the same length.

We may choose any deletion indices, and we delete all the characters in those indices for each string.

For example, if we have strs = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"].

Suppose we chose a set of deletion indices answer such that after deletions, the final array has its elements in lexicographic order (i.e., strs[0] <= strs[1] <= strs[2] <= ... <= strs[n - 1]). Return the minimum possible value of answer.length.

 

Example 1:

Input: strs = ["ca","bb","ac"]

Output: 1

Explanation: 

After deleting the first column, strs = ["a", "b", "c"].
Now strs is in lexicographic order (ie. strs[0] <= strs[1] <= strs[2]).
We require at least 1 deletion since initially strs was not in lexicographic order, so the answer is 1.



Example 2:

Input: strs = ["xc","yb","za"]

Output: 0

Explanation: 

strs is already in lexicographic order, so we do not need to delete anything.
Note that the rows of strs are not necessarily in lexicographic order:
i.e., it is NOT necessarily true that (strs[0][0] <= strs[0][1] <= ...)



Example 3:

Input: strs = ["zyx","wvu","tsr"]

Output: 3

Explanation: We have to delete every column.
 

Constraints:

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.



# Code
```cpp []
class Solution {
public:
    int minDeletionSize(vector<string>& A) {
        int res = 0, n = A.size(), m = A[0].length(), i, j;
        vector<bool> sorted(n - 1, false);
        for (j = 0; j < m; ++j) {
            for (i = 0; i < n - 1; ++i) {
                if (!sorted[i] && A[i][j] > A[i + 1][j]) {
                    res++;
                    break;
                }
            }
            if (i < n - 1)
                continue;
            for (i = 0; i < n - 1; ++i) {
                sorted[i] = sorted[i] || A[i][j] < A[i + 1][j];
            }
        }
        return res;
    }
};
```


------------------------------------------------------------------------------------------------------------------


# 960. Delete Columns to Make Sorted III -> [LeetCode](https://leetcode.com/problems/delete-columns-to-make-sorted-iii)

Hard

You are given an array of n strings strs, all of the same length.

We may choose any deletion indices, and we delete all the characters in those indices for each string.

For example, if we have strs = ["abcdef","uvwxyz"] and deletion indices {0, 2, 3}, then the final array after deletions is ["bef", "vyz"].

Suppose we chose a set of deletion indices answer such that after deletions, the final array has every string (row) in lexicographic order. (i.e., (strs[0][0] <= strs[0][1] <= ... <= strs[0][strs[0].length - 1]), and (strs[1][0] <= strs[1][1] <= ... <= strs[1][strs[1].length - 1]), and so on). Return the minimum possible value of answer.length.

 

Example 1:

Input: strs = ["babca","bbazb"]

Output: 3

Explanation: After deleting columns 0, 1, and 4, the final array is strs = ["bc", "az"].
Both these rows are individually in lexicographic order (ie. strs[0][0] <= strs[0][1] and strs[1][0] <= strs[1][1]).
Note that strs[0] > strs[1] - the array strs is not necessarily in lexicographic order.


Example 2:

Input: strs = ["edcba"]

Output: 4

Explanation: If we delete less than 4 columns, the only row will not be lexicographically sorted.


Example 3:

Input: strs = ["ghi","def","abc"]

Output: 0

Explanation: All rows are already lexicographically sorted.
 

Constraints:

n == strs.length
1 <= n <= 100
1 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.



# Code
```cpp []
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int n = (int)strs[0].size();
        int m = (int)strs.size();
        vector<int> dp(n, 1);

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                bool ok = true;
                for (int r = 0; r < m; r++) {
                    if (strs[r][j] > strs[r][i]) { ok = false; break; }
                }
                if (ok) dp[i] = max(dp[i], dp[j] + 1);
            }
        }

        int mx = 0;
        for (int v : dp) mx = max(mx, v);
        return n - mx;
    }
};
```

---------------------------------------------------------------------------------------------------------------------------------------------------------------


# 2054. Two Best Non-Overlapping Events -> [LeetCode](https://leetcode.com/problems/two-best-non-overlapping-events)

Medium

You are given a 0-indexed 2D integer array of events where events[i] = [startTimei, endTimei, valuei]. The ith event starts at startTimei and ends at endTimei, and if you attend this event, you will receive a value of valuei. You can choose at most two non-overlapping events to attend such that the sum of their values is maximized.

Return this maximum sum.

Note that the start time and end time is inclusive: that is, you cannot attend two events where one of them starts and the other ends at the same time. More specifically, if you attend an event with end time t, the next event must start at or after t + 1.

 

Example 1:


Input: events = [[1,3,2],[4,5,2],[2,4,3]]

Output: 4

Explanation: Choose the green events, 0 and 1 for a sum of 2 + 2 = 4.


Example 2:

Example 1 Diagram

Input: events = [[1,3,2],[4,5,2],[1,5,5]]

Output: 5

Explanation: Choose event 2 for a sum of 5.


Example 3:


Input: events = [[1,5,3],[1,5,1],[6,6,5]]

Output: 8

Explanation: Choose events 0 and 2 for a sum of 3 + 5 = 8.
 

Constraints:

2 <= events.length <= 105
events[i].length == 3
1 <= startTimei <= endTimei <= 109
1 <= valuei <= 106




# Code
```cpp []
class Solution {
public:
    int maxTwoEvents(vector<vector<int>>& events) {
        int n = events.size();
        
        sort(events.begin(), events.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        });
        
        vector<int> suffixMax(n);
        suffixMax[n - 1] = events[n - 1][2];
        
        for (int i = n - 2; i >= 0; --i) {
            suffixMax[i] = max(events[i][2], suffixMax[i + 1]);
        }
        
        int maxSum = 0;
        
        for (int i = 0; i < n; ++i) {
            int left = i + 1, right = n - 1;
            int nextEventIndex = -1;
            
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (events[mid][0] > events[i][1]) {
                    nextEventIndex = mid;
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            
            if (nextEventIndex != -1) {
                maxSum = max(maxSum, events[i][2] + suffixMax[nextEventIndex]);
            }
            
            maxSum = max(maxSum, events[i][2]);
        }
        
        return maxSum;
    }
};
```

---------------------------------------------------------------------------------------------------------------------------------


# 3074. Apple Redistribution into Boxes -> [LeetCode](https://leetcode.com/problems/apple-redistribution-into-boxes)

Easy

You are given an array apple of size n and an array capacity of size m.

There are n packs where the ith pack contains apple[i] apples. There are m boxes as well, and the ith box has a capacity of capacity[i] apples.

Return the minimum number of boxes you need to select to redistribute these n packs of apples into boxes.

Note that, apples from the same pack can be distributed into different boxes.

 

Example 1:

Input: apple = [1,3,2], capacity = [4,3,1,5,2]

Output: 2

Explanation: We will use boxes with capacities 4 and 5.
It is possible to distribute the apples as the total capacity is greater than or equal to the total number of apples.



Example 2:

Input: apple = [5,5,5], capacity = [2,4,2,7]

Output: 4

Explanation: We will need to use all the boxes.
 

Constraints:

1 <= n == apple.length <= 50
1 <= m == capacity.length <= 50
1 <= apple[i], capacity[i] <= 50
The input is generated such that it's possible to redistribute packs of apples into boxes



 
# Code
```cpp []
class Solution {
public:
    int minimumBoxes(vector<int>& apple, vector<int>& capacity) {
        int sum = accumulate(apple.begin() , apple.end() , 0);
        sort(capacity.begin() , capacity.end() , greater<int>());

        int res = 0;
        while(sum > 0)
            sum -= capacity[res++];

        return res;
    }
};
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------------


# 3075. Maximize Happiness of Selected Children -> [LeetCode](https://leetcode.com/problems/maximize-happiness-of-selected-children)

Medium

You are given an array happiness of length n, and a positive integer k.

There are n children standing in a queue, where the ith child has happiness value happiness[i]. You want to select k children from these n children in k turns.

In each turn, when you select a child, the happiness value of all the children that have not been selected till now decreases by 1. Note that the happiness value cannot become negative and gets decremented only if it is positive.

Return the maximum sum of the happiness values of the selected children you can achieve by selecting k children.

 

Example 1:

Input: happiness = [1,2,3], k = 2

Output: 4

Explanation: 

We can pick 2 children in the following way:
- Pick the child with the happiness value == 3. The happiness value of the remaining children becomes [0,1].
- Pick the child with the happiness value == 1. The happiness value of the remaining child becomes [0]. Note that the happiness value cannot become less than 0.
The sum of the happiness values of the selected children is 3 + 1 = 4.


Example 2:

Input: happiness = [1,1,1,1], k = 2


Output: 1

Explanation: We can pick 2 children in the following way:
- Pick any child with the happiness value == 1. The happiness value of the remaining children becomes [0,0,0].
- Pick the child with the happiness value == 0. The happiness value of the remaining child becomes [0,0].
The sum of the happiness values of the selected children is 1 + 0 = 1.


Example 3:

Input: happiness = [2,3,4,5], k = 1

Output: 5

Explanation: We can pick 1 child in the following way:
- Pick the child with the happiness value == 5. The happiness value of the remaining children becomes [1,2,3].
The sum of the happiness values of the selected children is 5.
 

Constraints:

1 <= n == happiness.length <= 2 * 105
1 <= happiness[i] <= 108
1 <= k <= n



# Code
```cpp []
class Solution {
public:
    long long maximumHappinessSum(vector<int>& happiness, int k) {
        ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
        sort(begin(happiness), end(happiness), greater<int>());
        int i = 0;
        long long res = 0;

        while(k--) {
            happiness[i] = max(happiness[i] - i, 0);
            res += happiness[i++];
        }

        return res;
    }
};
```

------------------------------------------------------------------------------------------------------------------------------


# 2483. Minimum Penalty for a Shop -> [LeetCode](https://leetcode.com/problems/minimum-penalty-for-a-shop)

Medium
 
You are given the customer visit log of a shop represented by a 0-indexed string customers consisting only of characters 'N' and 'Y':

if the ith character is 'Y', it means that customers come at the ith hour
whereas 'N' indicates that no customers come at the ith hour.
If the shop closes at the jth hour (0 <= j <= n), the penalty is calculated as follows:

For every hour when the shop is open and no customers come, the penalty increases by 1.
For every hour when the shop is closed and customers come, the penalty increases by 1.
Return the earliest hour at which the shop must be closed to incur a minimum penalty.

Note that if a shop closes at the jth hour, it means the shop is closed at the hour j.

 

Example 1:

Input: customers = "YYNY"

Output: 2

Explanation: 

- Closing the shop at the 0th hour incurs in 1+1+0+1 = 3 penalty.
- Closing the shop at the 1st hour incurs in 0+1+0+1 = 2 penalty.
- Closing the shop at the 2nd hour incurs in 0+0+0+1 = 1 penalty.
- Closing the shop at the 3rd hour incurs in 0+0+1+1 = 2 penalty.
- Closing the shop at the 4th hour incurs in 0+0+1+0 = 1 penalty.
Closing the shop at 2nd or 4th hour gives a minimum penalty. Since 2 is earlier, the optimal closing time is 2.


Example 2:

Input: customers = "NNNNN"

Output: 0

Explanation: It is best to close the shop at the 0th hour as no customers arrive.



Example 3:

Input: customers = "YYYY"

Output: 4

Explanation: It is best to close the shop at the 4th hour as customers arrive at each hour.
 


Constraints:

1 <= customers.length <= 105
customers consists only of characters 'Y' and 'N'.



# Code
```cpp []
class Solution {
public:
    int bestClosingTime(string customers) {
        int max_score = 0, score = 0, best_hour = -1;
        for(int i = 0; i < customers.size(); ++i) {
            score += (customers[i] == 'Y') ? 1 : -1;
            if(score > max_score) {
                max_score = score;
                best_hour = i;
            }
        }
        return best_hour + 1;
    }
};
```

------------------------------------------------------------------------------------------------------------------------------------------------------------


# 2402. Meeting Rooms III -> [LeetCode](https://leetcode.com/problems/meeting-rooms-iii/)

Hard

You are given an integer n. There are n rooms numbered from 0 to n - 1.

You are given a 2D integer array meetings where meetings[i] = [starti, endi] means that a meeting will be held during the half-closed time interval [starti, endi). All the values of starti are unique.

Meetings are allocated to rooms in the following manner:

Each meeting will take place in the unused room with the lowest number.
If there are no available rooms, the meeting will be delayed until a room becomes free. The delayed meeting should have the same duration as the original meeting.
When a room becomes unused, meetings that have an earlier original start time should be given the room.
Return the number of the room that held the most meetings. If there are multiple rooms, return the room with the lowest number.

A half-closed interval [a, b) is the interval between a and b including a and not including b.

 

Example 1:

Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]

Output: 0

Explanation:
- At time 0, both rooms are not being used. The first meeting starts in room 0.
- At time 1, only room 1 is not being used. The second meeting starts in room 1.
- At time 2, both rooms are being used. The third meeting is delayed.
- At time 3, both rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 1 finishes. The third meeting starts in room 1 for the time period [5,10).
- At time 10, the meetings in both rooms finish. The fourth meeting starts in room 0 for the time period [10,11).
Both rooms 0 and 1 held 2 meetings, so we return 0. 



Example 2:

Input: n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]

Output: 1

Explanation:
- At time 1, all three rooms are not being used. The first meeting starts in room 0.
- At time 2, rooms 1 and 2 are not being used. The second meeting starts in room 1.
- At time 3, only room 2 is not being used. The third meeting starts in room 2.
- At time 4, all three rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 2 finishes. The fourth meeting starts in room 2 for the time period [5,10).
- At time 6, all three rooms are being used. The fifth meeting is delayed.
- At time 10, the meetings in rooms 1 and 2 finish. The fifth meeting starts in room 1 for the time period [10,12).
Room 0 held 1 meeting while rooms 1 and 2 each held 2 meetings, so we return 1. 
 

Constraints:

1 <= n <= 100
1 <= meetings.length <= 105
meetings[i].length == 2
0 <= starti < endi <= 5 * 105
All the values of starti are unique.



# Code
```cpp []
struct T {
    long endTime;
    int roomId;
};

class Solution {
public:
    int mostBooked(int n, vector<vector<int>>& meetings) {
        vector<int> count(n);

        ranges::sort(meetings);

        auto compare = [](const T& a, const T& b) {
            return a.endTime == b.endTime ? a.roomId > b.roomId
                                          : a.endTime > b.endTime;
        };
        priority_queue<T, vector<T>, decltype(compare)> occupied(compare);
        priority_queue<int, vector<int>, greater<>> availableRoomIds;

        for (int i = 0; i < n; ++i)
            availableRoomIds.push(i);

        for (const vector<int>& meeting : meetings) {
            const int start = meeting[0];
            const int end = meeting[1];
            while (!occupied.empty() && occupied.top().endTime <= start)
                availableRoomIds.push(occupied.top().roomId), occupied.pop();
            if (availableRoomIds.empty()) {
                const auto [newStart, roomId] = occupied.top();
                occupied.pop();
                ++count[roomId];
                occupied.push({newStart + (end - start), roomId});
            } else {
                const int roomId = availableRoomIds.top();
                availableRoomIds.pop();
                ++count[roomId];
                occupied.push({end, roomId});
            }
        }

        return ranges::max_element(count) - count.begin();
    }
};
```

-----------------------------------------------------------------------------------------------------------------------------------------------------


# 1351. Count Negative Numbers in a Sorted Matrix -> [LeetCode](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix)

Easy
 
Given a m x n matrix grid which is sorted in non-increasing order both row-wise and column-wise, return the number of negative numbers in grid.

 

Example 1:

Input: grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]

Output: 8

Explanation: There are 8 negatives number in the matrix.



Example 2:

Input: grid = [[3,2],[1,0]]

Output: 0
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 100
-100 <= grid[i][j] <= 100
 

Follow up: Could you find an O(n + m) solution?


# Code
```cpp []
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size() , n = grid[0].size();
        int i = m - 1;
        int j = 0;

        int res = 0;

        while( i>=0 && j<n ){
            if(grid[i][j] < 0){
                res += n - j;
                i--;
            }else j++;
        }

        return res;
    }
};
```

--------------------------------------------------------------------------------------------------------------------------------


# 756. Pyramid Transition Matrix -> [LeetCode](https://leetcode.com/problems/pyramid-transition-matrix)

Medium

You are stacking blocks to form a pyramid. Each block has a color, which is represented by a single letter. Each row of blocks contains one less block than the row beneath it and is centered on top.

To make the pyramid aesthetically pleasing, there are only specific triangular patterns that are allowed. A triangular pattern consists of a single block stacked on top of two blocks. The patterns are given as a list of three-letter strings allowed, where the first two characters of a pattern represent the left and right bottom blocks respectively, and the third character is the top block.

For example, "ABC" represents a triangular pattern with a 'C' block stacked on top of an 'A' (left) and 'B' (right) block. Note that this is different from "BAC" where 'B' is on the left bottom and 'A' is on the right bottom.
You start with a bottom row of blocks bottom, given as a single string, that you must use as the base of the pyramid.

Given bottom and allowed, return true if you can build the pyramid all the way to the top such that every triangular pattern in the pyramid is in allowed, or false otherwise.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/08/26/pyramid1-grid.jpg)

Input: bottom = "BCD", allowed = ["BCC","CDE","CEA","FFF"]

Output: true

Explanation: The allowed triangular patterns are shown on the right.
Starting from the bottom (level 3), we can build "CE" on level 2 and then build "A" on level 1.
There are three triangular patterns in the pyramid, which are "BCC", "CDE", and "CEA". All are allowed.



Example 2:

![img](https://assets.leetcode.com/uploads/2021/08/26/pyramid2-grid.jpg)

Input: bottom = "AAAA", allowed = ["AAB","AAC","BCD","BBE","DEF"]

Output: false

Explanation: The allowed triangular patterns are shown on the right.
Starting from the bottom (level 4), there are multiple ways to build level 3, but trying all the possibilites, you will get always stuck before building level 1.
 

Constraints:

2 <= bottom.length <= 6
0 <= allowed.length <= 216
allowed[i].length == 3
The letters in all input strings are from the set {'A', 'B', 'C', 'D', 'E', 'F'}.
All the values of allowed are unique.



# Code
```cpp []
class Solution {
    unordered_map<string,vector<char> > m;
public:
    bool dfs(string bot,int i,string tem){
        if(bot.size()==1) return true;
        if(i==bot.size()-1) {
            string st;
            return dfs(tem,0,st);
        }
        for(auto v:m[bot.substr(i,2)]){
            tem.push_back(v);
            if(dfs(bot,i+1,tem)){
                return true;
            }
            tem.pop_back();
        }
        return false;
    }
    bool pyramidTransition(string bottom, vector<string>& allowed) {
        for(auto a:allowed){
            m[a.substr(0,2)].push_back(a[2]);
        }
        string te;
        return dfs(bottom,0,te);
    }
};
```

-------------------------------------------------------------------------------------------------------------

# 840. Magic Squares In Grid -> [LeetCode](https://leetcode.com/problems/magic-squares-in-grid)

Medium

A 3 x 3 magic square is a 3 x 3 grid filled with distinct numbers from 1 to 9 such that each row, column, and both diagonals all have the same sum.

Given a row x col grid of integers, how many 3 x 3 magic square subgrids are there?

Note: while a magic square can only contain numbers from 1 to 9, grid may contain numbers up to 15.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/09/11/magic_main.jpg)

Input: grid = [[4,3,8,4],[9,5,1,9],[2,7,6,2]]

Output: 1

Explanation: 

The following subgrid is a 3 x 3 magic square:

while this one is not:

In total, there is only one magic square inside the given grid.



Example 2:

![img](https://assets.leetcode.com/uploads/2020/09/11/magic_valid.jpg)

Input: grid = [[8]]

Output: 0
 

Constraints:

row == grid.length
col == grid[i].length
1 <= row, col <= 10
0 <= grid[i][j] <= 15



# Code
```cpp []
class Solution {
    bool isValid(int i, int j, vector<vector<int>>& grid) {
        vector<int> count(10, 0);
        for (int x = 0; x < 3; ++x) {
            for (int y = 0; y < 3; ++y) {
                int num = grid[i + x][j + y];
                if (num < 1 || num > 9 || count[num] > 0) return false;
                count[num]++;
            }
        }

        int sum = grid[i][j] + grid[i][j+1] + grid[i][j+2];
        
        for(int x = 0; x < 3; ++x) {
            if(sum != (grid[i + x][j] + grid[i + x][j + 1] + grid[i + x][j + 2])) return false;
        }
        
        for(int y = 0; y < 3; ++y) {
            if(sum != (grid[i][j + y] + grid[i + 1][j + y] + grid[i + 2][j + y])) return false;
        }
        
        if(sum != (grid[i][j] + grid[i+1][j+1] + grid[i+2][j+2])) return false;
        if(sum != (grid[i+2][j] + grid[i+1][j+1] + grid[i][j+2])) return false;

        return true;
    }
    
public:
    int numMagicSquaresInside(vector<vector<int>>& grid) {
        int cnt = 0;
        int row = grid.size();
        int col = grid[0].size();
        
        if(row < 3 || col < 3) return 0;

        for(int i = 0; i <= row - 3; ++i) {
            for(int j = 0; j <= col - 3; ++j) {
                if(isValid(i, j, grid)) cnt++;
            }
        }
        return cnt;
    }
};
```

--------------------------------------------------------------------------------------------------------------------------------------------------


# 1970. Last Day Where You Can Still Cross -> [LeetCode](https://leetcode.com/problems/last-day-where-you-can-still-cross)

Hard

There is a 1-based binary matrix where 0 represents land and 1 represents water. You are given integers row and col representing the number of rows and columns in the matrix, respectively.

Initially on day 0, the entire matrix is land. However, each day a new cell becomes flooded with water. You are given a 1-based 2D array cells, where cells[i] = [ri, ci] represents that on the ith day, the cell on the rith row and cith column (1-based coordinates) will be covered with water (i.e., changed to 1).

You want to find the last day that it is possible to walk from the top to the bottom by only walking on land cells. You can start from any cell in the top row and end at any cell in the bottom row. You can only travel in the four cardinal directions (left, right, up, and down).

Return the last day where it is possible to walk from the top to the bottom by only walking on land cells.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/07/27/1.png)

Input: row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]

Output: 2

Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 2.


Example 2:

![img](https://assets.leetcode.com/uploads/2021/07/27/2.png)

Input: row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]

Output: 1

Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 1.



Example 3:

![img](https://assets.leetcode.com/uploads/2021/07/27/3.png)

Input: row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]

Output: 3

Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 3.
 

Constraints:

2 <= row, col <= 2 * 104
4 <= row * col <= 2 * 104
cells.length == row * col
1 <= ri <= row
1 <= ci <= col
All the values of cells are unique.


# Code
```cpp []
class Solution {
public:
    bool isPossible(int m, int n, int t, vector<vector<int>>& cells) {
        vector<vector<int>> grid(m + 1, vector<int>(n + 1, 0)); 
        vector<pair<int, int>> directions {{1, 0}, {-1, 0}, {0, 1}, {0, -1}}; 

        for (int i = 0; i < t; i++) {
            grid[cells[i][0]][cells[i][1]] = 1;
        }

        queue<pair<int, int>> q;
        
        for (int i = 1; i <= n; i++) {
            if (grid[1][i] == 0) {
                q.push({1, i}); 
                grid[1][i] = 1;
            }
        }
        while (!q.empty()) {
            pair<int, int> p = q.front();
            q.pop();
            int r = p.first, c = p.second;
            for (auto d : directions) {
                int nr = r + d.first, nc = c + d.second;
                if (nr > 0 && nc > 0 && nr <= m && nc <= n && grid[nr][nc] == 0) {
                    grid[nr][nc] = 1;
                    if (nr == m) {
                        return true;
                    }
                    q.push({nr, nc});
                }
            }
        }
        return false;
    }

    int latestDayToCross(int row, int col, vector<vector<int>>& cells) {
        int left = 0, right = row * col, latestPossibleDay = 0;
        while (left < right - 1) {
            int mid = left + (right - left) / 2;
            if (isPossible(row, col, mid, cells)) {
                left = mid; 
                latestPossibleDay = mid;
            } else {
                right = mid;
            }
        }
        return latestPossibleDay;
    }
};
```

---------------------------------------------------------------------------------------------------------------------------------------------------------------





----------------------------------------------------------------------------------------------------------------------------

![badge](https://assets.leetcode.com/static_assets/marketing/202512.gif)

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------








