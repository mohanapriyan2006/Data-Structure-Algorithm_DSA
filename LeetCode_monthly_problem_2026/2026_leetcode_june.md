# --------------------------------------- 
# LeetCode 2026 - June Month Problem
# --------------------------------------- 


# [2144. Minimum Cost of Buying Candies With Discount](https://leetcode.com/problems/minimum-cost-of-buying-candies-with-discount)

Easy
 
A shop is selling candies at a discount. For every two candies sold, the shop gives a third candy for free.

The customer can choose any candy to take away for free as long as the cost of the chosen candy is less than or equal to the minimum cost of the two candies bought.

For example, if there are 4 candies with costs 1, 2, 3, and 4, and the customer buys candies with costs 2 and 3, they can take the candy with cost 1 for free, but not the candy with cost 4.
Given a 0-indexed integer array cost, where cost[i] denotes the cost of the ith candy, return the minimum cost of buying all the candies.

 

Example 1:

Input: cost = [1,2,3]

Output: 5

Explanation: We buy the candies with costs 2 and 3, and take the candy with cost 1 for free.
The total cost of buying all candies is 2 + 3 = 5. This is the only way we can buy the candies.
Note that we cannot buy candies with costs 1 and 3, and then take the candy with cost 2 for free.
The cost of the free candy has to be less than or equal to the minimum cost of the purchased candies.




Example 2:

Input: cost = [6,5,7,9,2,2]

Output: 23

Explanation: The way in which we can get the minimum cost is described below:
- Buy candies with costs 9 and 7
- Take the candy with cost 6 for free
- We buy candies with costs 5 and 2
- Take the last remaining candy with cost 2 for free
Hence, the minimum cost to buy all candies is 9 + 7 + 5 + 2 = 23.




Example 3:

Input: cost = [5,5]

Output: 10

Explanation: Since there are only 2 candies, we buy both of them. There is not a third candy we can take for free.
Hence, the minimum cost to buy all candies is 5 + 5 = 10.
 


Constraints:

1 <= cost.length <= 100
1 <= cost[i] <= 100


# Code
```cpp []
class Solution {
public:
    int minimumCost(vector<int>& cost) {
        sort(cost.begin(),cost.end());
        int total =0;

        for(int i=cost.size()-1;i>=0;i-=3){
            total += cost[i];
            if(i-1>=0) total+=cost[i-1];
        }

        return total;
    }
};
```


---------------------------------------------------------------------------------------------------------------

# [3633. Earliest Finish Time for Land and Water Rides I](https://leetcode.com/problems/earliest-finish-time-for-land-and-water-rides-i/)

Easy
 
You are given two categories of theme park attractions: land rides and water rides.

Land rides
landStartTime[i] – the earliest time the ith land ride can be boarded.
landDuration[i] – how long the ith land ride lasts.
Water rides
waterStartTime[j] – the earliest time the jth water ride can be boarded.
waterDuration[j] – how long the jth water ride lasts.
A tourist must experience exactly one ride from each category, in either order.

A ride may be started at its opening time or any later moment.
If a ride is started at time t, it finishes at time t + duration.
Immediately after finishing one ride the tourist may board the other (if it is already open) or wait until it opens.
Return the earliest possible time at which the tourist can finish both rides.

 


Example 1:

Input: landStartTime = [2,8], landDuration = [4,1], waterStartTime = [6], waterDuration = [3]


Output: 9


Explanation:​​​​​​​

Plan A (land ride 0 → water ride 0):
Start land ride 0 at time landStartTime[0] = 2. Finish at 2 + landDuration[0] = 6.
Water ride 0 opens at time waterStartTime[0] = 6. Start immediately at 6, finish at 6 + waterDuration[0] = 9.
Plan B (water ride 0 → land ride 1):
Start water ride 0 at time waterStartTime[0] = 6. Finish at 6 + waterDuration[0] = 9.
Land ride 1 opens at landStartTime[1] = 8. Start at time 9, finish at 9 + landDuration[1] = 10.
Plan C (land ride 1 → water ride 0):
Start land ride 1 at time landStartTime[1] = 8. Finish at 8 + landDuration[1] = 9.
Water ride 0 opened at waterStartTime[0] = 6. Start at time 9, finish at 9 + waterDuration[0] = 12.
Plan D (water ride 0 → land ride 0):
Start water ride 0 at time waterStartTime[0] = 6. Finish at 6 + waterDuration[0] = 9.
Land ride 0 opened at landStartTime[0] = 2. Start at time 9, finish at 9 + landDuration[0] = 13.
Plan A gives the earliest finish time of 9.





Example 2:

Input: landStartTime = [5], landDuration = [3], waterStartTime = [1], waterDuration = [10]

Output: 14

Explanation:​​​​​​​

Plan A (water ride 0 → land ride 0):
Start water ride 0 at time waterStartTime[0] = 1. Finish at 1 + waterDuration[0] = 11.
Land ride 0 opened at landStartTime[0] = 5. Start immediately at 11 and finish at 11 + landDuration[0] = 14.
Plan B (land ride 0 → water ride 0):
Start land ride 0 at time landStartTime[0] = 5. Finish at 5 + landDuration[0] = 8.
Water ride 0 opened at waterStartTime[0] = 1. Start immediately at 8 and finish at 8 + waterDuration[0] = 18.
Plan A provides the earliest finish time of 14.​​​​​​​



 

Constraints:

1 <= n, m <= 100
landStartTime.length == landDuration.length == n
waterStartTime.length == waterDuration.length == m
1 <= landStartTime[i], landDuration[i], waterStartTime[j], waterDuration[j] <= 1000



# Code
```cpp []
class Solution {
public:
    int earliestFinishTime(vector<int>& startL, vector<int>& durL, vector<int>& startW, vector<int>& durW) {
        int minL = 3000, minW = minL, res = minW;
        int n = startL.size(), m = startW.size();

        for (int i = 0; i < n; i++)
            minL = min(minL, startL[i] + durL[i]);

        for (int i = 0; i < m; i++) {
            minW = min(minW, startW[i] + durW[i]);
            res = min(res, max(minL, startW[i]) + durW[i]);
        }

        for (int i = 0; i < n; i++)
            res = min(res, max(minW, startL[i]) + durL[i]);

        return res;
    }
};
```


---------------------------------------------------------------------------------------------------------

# [3753. Total Waviness of Numbers in Range II](https://leetcode.com/problems/total-waviness-of-numbers-in-range-ii/)

Hard
 
You are given two integers num1 and num2 representing an inclusive range [num1, num2].

The waviness of a number is defined as the total count of its peaks and valleys:

A digit is a peak if it is strictly greater than both of its immediate neighbors.
A digit is a valley if it is strictly less than both of its immediate neighbors.
The first and last digits of a number cannot be peaks or valleys.
Any number with fewer than 3 digits has a waviness of 0.
Return the total sum of waviness for all numbers in the range [num1, num2].
 



Example 1:

Input: num1 = 120, num2 = 130

Output: 3

Explanation:

In the range [120, 130]:

120: middle digit 2 is a peak, waviness = 1.
121: middle digit 2 is a peak, waviness = 1.
130: middle digit 3 is a peak, waviness = 1.
All other numbers in the range have a waviness of 0.
Thus, total waviness is 1 + 1 + 1 = 3.




Example 2:

Input: num1 = 198, num2 = 202

Output: 3

Explanation:

In the range [198, 202]:

198: middle digit 9 is a peak, waviness = 1.
201: middle digit 0 is a valley, waviness = 1.
202: middle digit 0 is a valley, waviness = 1.
All other numbers in the range have a waviness of 0.
Thus, total waviness is 1 + 1 + 1 = 3.



Example 3:

Input: num1 = 4848, num2 = 4848

Output: 2

Explanation:

Number 4848: the second digit 8 is a peak, and the third digit 4 is a valley, giving a waviness of 2.

 

Constraints:

1 <= num1 <= num2 <= 1015​​​​​​​



# Code
```cpp []
using ll = long long;
class Solution {
    static inline int waves[570];
    static inline bool init = []() {
        int j = 0;
        for (int i = 0; i < 1000; i++) {
            int r = i % 10;
            int m = (i / 10) % 10;
            int l = (i / 100) % 10;
            if ((m > max(l, r)) | (m < min(l, r)))
                waves[j++] = i;
        }
        return 0;
    }();

public:
    ll totalWaviness(ll A, ll B) { return waveCount(B) - waveCount(A - 1); }

private:
    ll waveCount(ll num) {
        if (num < 100) return 0;            
        return accumulate(begin(waves), end(waves), 0LL, [&](ll a, int p) {
            return a + countWays(num, p);
        });
    }

    ll countWays(ll num, int pattern) {
        ll t = pattern < 100;
        ll count = 0, mult = 1;

        for (int i = 0; mult * 100 <= num; i++) {
            ll pre = num / (mult * 1000);
            ll cur = (num / mult) % 1000;
            ll suf = num % mult;
            ll ways = 0;

            if (cur > pattern)
                ways = pre - t + 1;
            else if (cur == pattern) {
                ways = max(0LL, pre - t);
                count += suf + 1;
            } else
                ways = max(0LL, pre - t);
            count += ways * mult;
            mult *= 10;
        }

        return count;
    }
};
```

--------------------------------------------------------------------------------------------------

# [2574. Left and Right Sum Differences](https://leetcode.com/problems/left-and-right-sum-differences)

Easy
 
You are given a 0-indexed integer array nums of size n.

Define two arrays leftSum and rightSum where:

leftSum[i] is the sum of elements to the left of the index i in the array nums. If there is no such element, leftSum[i] = 0.
rightSum[i] is the sum of elements to the right of the index i in the array nums. If there is no such element, rightSum[i] = 0.
Return an integer array answer of size n where answer[i] = |leftSum[i] - rightSum[i]|.

 


Example 1:


Input: nums = [10,4,8,3]

Output: [15,1,11,22]

Explanation: The array leftSum is [0,10,14,22] and the array rightSum is [15,11,3,0].
The array answer is [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22].




Example 2:

Input: nums = [1]

Output: [0]

Explanation: The array leftSum is [0] and the array rightSum is [0].
The array answer is [|0 - 0|] = [0].
 


Constraints:

1 <= nums.length <= 1000
1 <= nums[i] <= 105


# Code
```cpp []
class Solution {
public:
    vector<int> leftRightDifference(vector<int>& nums) {
        const int n=nums.size();
        vector<int> ans(n);
        int lsum=0, rsum=accumulate(nums.begin(), nums.end(), 0);
        for(int i=0; i<n; i++){
            const int x=nums[i];
            rsum-=x;
            ans[i]=(rsum>=lsum)?rsum-lsum:lsum-rsum;
            lsum+=x;
        }
        return ans;
    }
};
```

-----------------------------------------------------------------------------------------------

# [3689. Maximum Total Subarray Value I](https://leetcode.com/problems/maximum-total-subarray-value-i/)

Medium
 
You are given an integer array nums of length n and an integer k.

You need to choose exactly k non-empty subarrays nums[l..r] of nums. Subarrays may overlap, and the exact same subarray (same l and r) can be chosen more than once.

The value of a subarray nums[l..r] is defined as: max(nums[l..r]) - min(nums[l..r]).

The total value is the sum of the values of all chosen subarrays.

Return the maximum possible total value you can achieve.


 

Example 1:

Input: nums = [1,3,2], k = 2

Output: 4

Explanation:

One optimal approach is:

Choose nums[0..1] = [1, 3]. The maximum is 3 and the minimum is 1, giving a value of 3 - 1 = 2.
Choose nums[0..2] = [1, 3, 2]. The maximum is still 3 and the minimum is still 1, so the value is also 3 - 1 = 2.
Adding these gives 2 + 2 = 4.




Example 2:

Input: nums = [4,2,5,1], k = 3

Output: 12

Explanation:

One optimal approach is:

Choose nums[0..3] = [4, 2, 5, 1]. The maximum is 5 and the minimum is 1, giving a value of 5 - 1 = 4.
Choose nums[0..3] = [4, 2, 5, 1]. The maximum is 5 and the minimum is 1, so the value is also 4.
Choose nums[2..3] = [5, 1]. The maximum is 5 and the minimum is 1, so the value is again 4.
Adding these gives 4 + 4 + 4 = 12.


 

Constraints:

1 <= n == nums.length <= 5 * 10​​​​​​​4
0 <= nums[i] <= 109
1 <= k <= 105




# Code
```cpp []
class Solution {
public:
    long long maxTotalValue(vector<int>& A, int k) {
        int gMin = A.front(), gMax = A.front();

        for (auto& n : A) {
            gMin = min(gMin, n);
            gMax = max(gMax, n);
        }
        
        return (long long)(gMax - gMin) * k;
    }
};
```

----------------------------------------------------------------------------------------------------

# [3691. Maximum Total Subarray Value II](https://leetcode.com/problems/maximum-total-subarray-value-ii/)

Hard
 
You are given an integer array nums of length n and an integer k.

You must select exactly k distinct non-empty subarrays nums[l..r] of nums. Subarrays may overlap, but the exact same subarray (same l and r) cannot be chosen more than once.

The value of a subarray nums[l..r] is defined as: max(nums[l..r]) - min(nums[l..r]).

The total value is the sum of the values of all chosen subarrays.

Return the maximum possible total value you can achieve.

 

Example 1:

Input: nums = [1,3,2], k = 2

Output: 4

Explanation:

One optimal approach is:

Choose nums[0..1] = [1, 3]. The maximum is 3 and the minimum is 1, giving a value of 3 - 1 = 2.
Choose nums[0..2] = [1, 3, 2]. The maximum is still 3 and the minimum is still 1, so the value is also 3 - 1 = 2.
Adding these gives 2 + 2 = 4.




Example 2:

Input: nums = [4,2,5,1], k = 3

Output: 12

Explanation:

One optimal approach is:

Choose nums[0..3] = [4, 2, 5, 1]. The maximum is 5 and the minimum is 1, giving a value of 5 - 1 = 4.
Choose nums[1..3] = [2, 5, 1]. The maximum is 5 and the minimum is 1, so the value is also 4.
Choose nums[2..3] = [5, 1]. The maximum is 5 and the minimum is 1, so the value is again 4.
Adding these gives 4 + 4 + 4 = 12.

 

Constraints:

1 <= n == nums.length <= 5 * 10​​​​​​​4
0 <= nums[i] <= 109
1 <= k <= min(105, n * (n + 1) / 2)


# Code
```cpp []
class SparseTable {
    vector<vector<int>> Min, Max;

public:
    SparseTable(const vector<int>& num) {
        size_t n = num.size();
        int w = bit_width(n);
        Min.resize(w, vector<int>(n));
        Max.resize(w, vector<int>(n));

        for (int i = 0; i < n; i++)
            Min[0][i] = Max[0][i] = num[i];

        for (int i = 1; i < w; i++)
            for (int j = 0; j + (1 << i) <= n; j++) {
                Min[i][j] = min(Min[i - 1][j], Min[i - 1][j + (1 << (i - 1))]);
                Max[i][j] = max(Max[i - 1][j], Max[i - 1][j + (1 << (i - 1))]);
            }
    }

    int query(int left, int right) {
        int k = bit_width((uint32_t)right - left) - 1;
        return max(Max[k][left], Max[k][right - (1 << k)]) -
               min(Min[k][left], Min[k][right - (1 << k)]);
    }
};

class Solution {
public:
    long long maxTotalValue(vector<int>& nums, int k) {
        int n = nums.size();
        long long res = 0;
        SparseTable LUT(nums);

        priority_queue<tuple<int, int, int>> pq;
        for (int i = 0; i < n; i++)
            pq.emplace(LUT.query(i, n), i, n);

        while (get<0>(pq.top()) && k--) {
            auto [val, l, r] = pq.top();
            pq.pop();
            res += val;
            pq.emplace(LUT.query(l, r - 1), l, r - 1);
        }

        return res;
    }
};
```

---------------------------------------------------------------------------------------------------------------------


# [3838. Weighted Word Mapping](https://leetcode.com/problems/weighted-word-mapping/)

Easy
 
You are given an array of strings words, where each string represents a word containing lowercase English letters.

You are also given an integer array weights of length 26, where weights[i] represents the weight of the ith lowercase English letter.

The weight of a word is defined as the sum of the weights of its characters.

For each word, take its weight modulo 26 and map the result to a lowercase English letter using reverse alphabetical order (0 -> 'z', 1 -> 'y', ..., 25 -> 'a').

Return a string formed by concatenating the mapped characters for all words in order.

 


Example 1:

Input: words = ["abcd","def","xyz"], weights = [5,3,12,14,1,2,3,2,10,6,6,9,7,8,7,10,8,9,6,9,9,8,3,7,7,2]

Output: "rij"

Explanation:

The weight of "abcd" is 5 + 3 + 12 + 14 = 34. The result modulo 26 is 34 % 26 = 8, which maps to 'r'.
The weight of "def" is 14 + 1 + 2 = 17. The result modulo 26 is 17 % 26 = 17, which maps to 'i'.
The weight of "xyz" is 7 + 7 + 2 = 16. The result modulo 26 is 16 % 26 = 16, which maps to 'j'.
Thus, the string formed by concatenating the mapped characters is "rij".




Example 2:

Input: words = ["a","b","c"], weights = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]

Output: "yyy"

Explanation:

Each word has weight 1. The result modulo 26 is 1 % 26 = 1, which maps to 'y'.

Thus, the string formed by concatenating the mapped characters is "yyy".




Example 3:

Input: words = ["abcd"], weights = [7,5,3,4,3,5,4,9,4,2,2,7,10,2,5,10,6,1,2,2,4,1,3,4,4,5]

Output: "g"

Explanation:​​​​​​​

The weight of "abcd" is 7 + 5 + 3 + 4 = 19. The result modulo 26 is 19 % 26 = 19, which maps to 'g'.

Thus, the string formed by concatenating the mapped characters is "g".

 


Constraints:

1 <= words.length <= 100
1 <= words[i].length <= 10
weights.length == 26
1 <= weights[i] <= 100
words[i] consists of lowercase English letters.



# Code
```cpp []
class Solution {
public:
    string mapWordWeights(vector<string>& words, vector<int>& weights) {
        string res(words.size(), 0);
        int i = 0;

        for (auto& word : words) {
            int s = 0;
            for (auto& c : word)
                s += weights[(c & (1 << 5) - 1) - 1];
            res[i++] = 'z' - (s - ((s * 2521) >> (1 << 4)) * 26);
        }

        return res;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------

# [2130. Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)

Medium
 
In a linked list of size n, where n is even, the ith node (0-indexed) of the linked list is known as the twin of the (n-1-i)th node, if 0 <= i <= (n / 2) - 1.

For example, if n = 4, then node 0 is the twin of node 3, and node 1 is the twin of node 2. These are the only nodes with twins for n = 4.
The twin sum is defined as the sum of a node and its twin.

Given the head of a linked list with even length, return the maximum twin sum of the linked list.


 

Example 1:


Input: head = [5,4,2,1]

Output: 6

Explanation:

Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
There are no other nodes with twins in the linked list.
Thus, the maximum twin sum of the linked list is 6. 



Example 2:

Input: head = [4,2,2,3]

Output: 7

Explanation:

The nodes with twins present in this linked list are:
- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
Thus, the maximum twin sum of the linked list is max(7, 4) = 7. 




Example 3:

Input: head = [1,100000]

Output: 100001

Explanation:

There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.



Constraints:

The number of nodes in the list is an even integer in the range [2, 105].
1 <= Node.val <= 105




# Code
```cpp []
class Solution {
public:
    int pairSum(ListNode* head) {
        int res = 0;
        ListNode* left = head;

        auto dfs = [&](this auto&& dfs, ListNode* right) -> void {
            if (right->next)
                dfs(right->next);

            res = max(res, left->val + right->val);
            left = left->next;
        };

        dfs(head);
        return res;
    }
};
```

------------------------------------------------------------------------------------------------------------

# [3612. Process String with Special Operations I](https://leetcode.com/problems/process-string-with-special-operations-i/)
 
Medium
 
You are given a string s consisting of lowercase English letters and the special characters: *, #, and %.

Build a new string result by processing s according to the following rules from left to right:

If the letter is a lowercase English letter append it to result.
A '*' removes the last character from result, if it exists.
A '#' duplicates the current result and appends it to itself.
A '%' reverses the current result.
Return the final string result after processing all characters in s.


 

Example 1:

Input: s = "a#b%*"

Output: "ba"

Explanation:

i	s[i]	Operation	Current result
0	'a'	Append 'a'	"a"
1	'#'	Duplicate result	"aa"
2	'b'	Append 'b'	"aab"
3	'%'	Reverse result	"baa"
4	'*'	Remove the last character	"ba"
Thus, the final result is "ba".




Example 2:

Input: s = "z*#"

Output: ""

Explanation:

i	s[i]	Operation	Current result
0	'z'	Append 'z'	"z"
1	'*'	Remove the last character	""
2	'#'	Duplicate the string	""
Thus, the final result is "".

 

Constraints:

1 <= s.length <= 20
s consists of only lowercase English letters and special characters *, #, and %.


# Code
```cpp []
//translated using AI
class Solution {
public:
    string processStr(string s) {
        string res;
        int n = s.length();

        for (int i = 0; i < n; i++) {
            char ch = s[i];

            if (ch == '*') {
                if (res.length() != 0) {
                    res.pop_back();
                }
            } 
            else if (ch == '#') {
                res += res;
            } 
            else if (ch == '%') {
                reverse(res.begin(), res.end());
            } 
            else if (ch >= 'a' && ch <= 'z') {
                res.push_back(ch);
            }
        }

        return res;
    }
};
```

----------------------------------------------------------------------------


# [1732. Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude/)

Easy
 
There is a biker going on a road trip. The road trip consists of n + 1 points at different altitudes. The biker starts his trip on point 0 with altitude equal 0.

You are given an integer array gain of length n where gain[i] is the net gain in altitude between points i​​​​​​ and i + 1 for all (0 <= i < n). Return the highest altitude of a point.

 

Example 1:

Input: gain = [-5,1,5,0,-7]

Output: 1

Explanation: The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.



Example 2:

Input: gain = [-4,-3,-2,-1,4,3,2]

Output: 0

Explanation: The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0.
 


Constraints:

n == gain.length
1 <= n <= 100
-100 <= gain[i] <= 100


# Code
```cpp []
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int ans = 0, sum = 0;

        for (auto& it : gain) {
            sum += it;
            int d = sum - ans;
            ans += d & ~(d >> 0x1F);
        }

        return ans;
    }
};
```

------------------------------------------------------------------------------------------------------------


# [1840. Maximum Building Height](https://leetcode.com/problems/maximum-building-height/)

Hard
 
You want to build n new buildings in a city. The new buildings will be built in a line and are labeled from 1 to n.

However, there are city restrictions on the heights of the new buildings:

The height of each building must be a non-negative integer.
The height of the first building must be 0.
The height difference between any two adjacent buildings cannot exceed 1.
Additionally, there are city restrictions on the maximum height of specific buildings. These restrictions are given as a 2D integer array restrictions where restrictions[i] = [idi, maxHeighti] indicates that building idi must have a height less than or equal to maxHeighti.

It is guaranteed that each building will appear at most once in restrictions, and building 1 will not be in restrictions.

Return the maximum possible height of the tallest building.

 


Example 1:


Input: n = 5, restrictions = [[2,1],[4,1]]

Output: 2

![img](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex1-1.png)

Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,1,2], and the tallest building has a height of 2.



Example 2:

Input: n = 6, restrictions = []

Output: 5

![img](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex2.png)

Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,3,4,5], and the tallest building has a height of 5.



Example 3:

Input: n = 10, restrictions = [[5,3],[2,5],[7,4],[10,3]]

Output: 5

![img](https://assets.leetcode.com/uploads/2021/04/08/ic236-q4-ex3.png)

Explanation: The green area in the image indicates the maximum allowed height for each building.
We can build the buildings with heights [0,1,2,3,3,4,4,5,4,3], and the tallest building has a height of 5.
 


Constraints:

2 <= n <= 109
0 <= restrictions.length <= min(n - 1, 105)
2 <= idi <= n
idi is unique.
0 <= maxHeighti <= 109


# Code
```cpp []
class Solution {
public:
    int maxBuilding(int num, vector<vector<int>>& r) {
        r.push_back({1, 0});
        sort(r.begin(), r.end());
        int n = r.size();

        for (int i = 1; i < n; i++)
            r[i][1] = yCap(r[i - 1], r[i]);

        for (int i = n - 2; i >= 0; i--)
            r[i][1] = yCap(r[i + 1], r[i]);

        int res = 0;
        for (int i = 1; i < n; i++)
            res = max(res, yPeak(r[i - 1], r[i]));

        return max(res, r[n - 1][1] + num - r[n - 1][0]);
    }

    int yCap(vector<int>& l, vector<int>& r) {
        int x1 = l[0], y1 = l[1], x2 = r[0], y2 = r[1];
        return min(y2, y1 + abs(x2 - x1));
    }

    int yPeak(vector<int>& l, vector<int>& r) {
        int x1 = l[0], y1 = l[1], x2 = r[0], y2 = r[1];
        return (y1 + y2 + x2 - x1) >> 1;
    }
};
```

------------------------------------------------------------------------------------------------

