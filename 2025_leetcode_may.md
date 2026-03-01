----
# LeetCode May - 2025
-----


# I121. Best Time to Buy and Sell Stock

Easy

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

### Example 1:

Input: prices = [7,1,5,3,6,4] <br/>
Output: 5 <br/>
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.


### Example 2:

Input: prices = [7,6,4,3,1] <br/>
Output: 0 <br/>
Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:

1 <= prices.length <= 105
0 <= prices[i] <= 104

# Code
```cpp []
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxP = 0;
        int mini = prices[0];

        for(int i=1 ; i<prices.size() ; ++i){
            int curP = prices[i] - mini;
            maxP = max(maxP,curP);
            mini = min(mini,prices[i]);
        }

        return maxP;
    }
};
```
------

# 217. Contains Duplicate

Easy

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

 

#### Example 1:

Input: nums = [1,2,3,1] <br/>

Output: true <br/>

Explanation: <br/>

The element 1 occurs at the indices 0 and 3.


#### Example 2:

Input: nums = [1,2,3,4] <br/>

Output: false <br/>

Explanation: <br/>

All elements are distinct.

#### Example 3:

Input: nums = [1,1,1,3,3,4,3,2,4,2] <br/>

Output: true <br/>
 
Constraints:

1 <= nums.length <= 105
-109 <= nums[i] <= 109

# Code
```cpp []
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> num;

        for(int i=0 ; i<nums.size() ; ++i){
            num.insert(nums[i]);
        }

        if( num.size() != nums.size()) return true;

        return false;
    }
};
```
-----
# 238. Product of Array Except Self

Medium

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

 

#### Example 1:

Input: nums = [1,2,3,4] <br/>
Output: [24,12,8,6] <br/>

#### Example 2:

Input: nums = [-1,1,0,-3,3] <br/>
Output: [0,0,9,0,0] <br/>
 

Constraints:

2 <= nums.length <= 105
-30 <= nums[i] <= 30
The input is generated such that answer[i] is guaranteed to fit in a 32-bit integer.


# Code
```cpp []
class Solution {
 public:
  vector<int> productExceptSelf(vector<int>& nums) {
    const int n = nums.size();
    vector<int> ans(n);        
    vector<int> prefix(n, 1);  
    vector<int> suffix(n, 1);  

    for(int i=1 ; i<n ; ++i){
        prefix[i] = prefix[i-1] * nums[i-1];
    }

    for(int i = n-2 ; i>=0 ; --i){
        suffix[i] = suffix[i+1] * nums[i+1];
    }

    for(int i=0 ; i<n ; ++i){
        ans[i] = prefix[i] * suffix[i];
    }


    return ans;
  }
};
```
----
# 53. Maximum Subarray

Medium

Given an integer array nums, find the subarray with the largest sum, and return its sum.

 

#### Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4] <br/>
Output: 6 <br/>
Explanation: The subarray [4,-1,2,1] has the largest sum 6. <br/>

#### Example 2:

Input: nums = [1] <br/>
Output: 1 <br/>
Explanation: The subarray [1] has the largest sum 1. <br/>

#### Example 3:

Input: nums = [5,4,-1,7,8] <br/>
Output: 23 <br/>
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23. <br/>
 

Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104

# Code
```cpp []
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ans = INT_MIN;
        int sum = 0;

        for(int i=0 ; i<nums.size() ; ++i){
            
            sum+=nums[i];

            ans = max(sum,ans);

            if(sum<0){
                sum = 0;
            }
        }

        return ans;
    }
};
```
----
# 152. Maximum Product Subarray

Medium

Given an integer array nums, find a subarray that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

 

#### Example 1:

Input: nums = [2,3,-2,4] <br/>
Output: 6 <br/>
Explanation: [2,3] has the largest product 6. <br/>

#### Example 2:

Input: nums = [-2,0,-1] <br/>
Output: 0 <br/>
Explanation: The result cannot be 2, because [-2,-1] is not a subarray. <br/>
 

Constraints:

1 <= nums.length <= 2 * 104
-10 <= nums[i] <= 10
The product of any subarray of nums is guaranteed to fit in a 32-bit integer.

# Code
```cpp []
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int l = nums.size();

        int ans = INT_MIN;
        
        int pre = 1;
        int suf = 1;

        for(int i=0 ; i<l ; ++i){

            if(pre == 0) pre = 1;
            if(suf == 0 ) suf = 1;

            pre *= nums[i];
            suf *= nums[l-i-1];

            ans = max(ans,max(pre,suf));
        }
        

        return ans;
    }
};
```
---
# 153. Find Minimum in Rotated Sorted Array

Medium

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

 
#### Example 1:

Input: nums = [3,4,5,1,2] <br/>
Output: 1 <br/>
Explanation: The original array was [1,2,3,4,5] rotated 3 times. <br/>

#### Example 2:

Input: nums = [4,5,6,7,0,1,2] <br/>
Output: 0 <br/>
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times. <br/>

#### Example 3:

Input: nums = [11,13,15,17] <br/>
Output: 11 <br/>
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.  <br/>
 

Constraints:

n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
All the integers of nums are unique.
nums is sorted and rotated between 1 and n times.

# Code
```cpp []
class Solution {
public:
    int findMin(vector<int>& nums) {

        int ans = INT_MAX;

        int l = 0;
        int h = nums.size() - 1;

        while(l <= h){
            int m = (l + h )/2;

            if(nums[l] <= nums[m] ){
                ans = min(ans,nums[l]);
                l = m + 1;
            }
            else{
                ans = min(ans,nums[m]);
                h = m - 1;
            }
        }
      

        return ans;
    }
};
```
----
# 33. Search in Rotated Sorted Array

Medium

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

#### Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0 <br/>
Output: 4 <br/>

#### Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3 <br/>
Output: -1 <br/>

#### Example 3:

Input: nums = [1], target = 0 <br/>
Output: -1 <br/>
 

Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104

# Code
```cpp []
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int ans = -1;
        int n = nums.size();
        int l = 0 , h = n - 1;

        while(l <= h){
            int m = (l+h) / 2;

            if(nums[m] == target){
                ans = m;
                break;
            }

            if(nums[l] <= nums[m]){
                if(nums[l] <= target && target <= nums[m]){
                    h = m ;
                }
                else{
                    l = m + 1;
                }
            }
            else{
                if(nums[m] <= target && target <= nums[h]){
                    l = m;
                }else{
                    h = m - 1;
                }
            }
           
        }

        return ans;
    }
};
```
-----
# 15. 3Sum

Medium

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

#### Example 1:

Input: nums = [-1,0,1,2,-1,-4] <br/>
Output: [[-1,-1,2],[-1,0,1]] <br/>
Explanation:  <br/>
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0. <br/>
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0. <br/>
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0. <br/>
The distinct triplets are [-1,0,1] and [-1,-1,2]. <br/>
Notice that the order of the output and the order of the triplets does not matter.

#### Example 2:

Input: nums = [0,1,1] <br/>
Output: [] <br/>
Explanation: The only possible triplet does not sum up to 0. <br/>

#### Example 3:

Input: nums = [0,0,0] <br/>
Output: [[0,0,0]] <br/>
Explanation: The only possible triplet sums up to 0. <br/>
 

Constraints:

3 <= nums.length <= 3000
-105 <= nums[i] <= 105

# Code
```cpp []
    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) { 

            vector<vector<int>> ans;

            sort(nums.begin(),nums.end());

            int n = nums.size();

            for(int i = 0 ; i < n ; ++i){
                if(i>0 && nums[i] == nums[i-1]) continue;

                int j = i + 1;
                int k = n - 1;

                while( j < k){
                    int sum = nums[i] + nums[j] + nums[k];
                    if(sum < 0) j++;
                    else if(sum > 0) k--;
                    else{
                        vector<int> temp = {nums[i],nums[j],nums[k]};
                        ans.push_back(temp);
                        j++;
                        k--;
                        while( j<k && nums[j] == nums[j-1]) j++;
                        while( j<k && nums[k] == nums[k+1]) k--;
                    }
                }
            }
            
            return ans; 
        }
    };

```
----
# 11. Container With Most Water

Medium

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

 

#### Example 1:


Input: height = [1,8,6,2,5,4,8,3,7] <br/>
Output: 49 <br/>
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49. <br/>

#### Example 2:

Input: height = [1,1] <br/>
Output: 1 <br/>
 

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

        while( l < r){
            int amt  = min(arr[l],arr[r]) * (r - l);
            ans = max(ans,amt);

            if(arr[l] < arr[r]){
                l++;
            }else{
                r--;
            }
        }

        return ans;
    }
};
```
----

# 3068. Find the Maximum Sum of Node Values

Hard

There exists an undirected tree with n nodes numbered 0 to n - 1. You are given a 0-indexed 2D integer array edges of length n - 1, where edges[i] = [ui, vi] indicates that there is an edge between nodes ui and vi in the tree. You are also given a positive integer k, and a 0-indexed array of non-negative integers nums of length n, where nums[i] represents the value of the node numbered i.

Alice wants the sum of values of tree nodes to be maximum, for which Alice can perform the following operation any number of times (including zero) on the tree:

Choose any edge [u, v] connecting the nodes u and v, and update their values as follows:
nums[u] = nums[u] XOR k
nums[v] = nums[v] XOR k
Return the maximum possible sum of the values Alice can achieve by performing the operation any number of times.

 

#### Example 1:


Input: nums = [1,2,1], k = 3, edges = [[0,1],[0,2]] <br/>
Output: 6 <br/>
Explanation: Alice can achieve the maximum sum of 6 using a single operation: <br/>
- Choose the edge [0,2]. nums[0] and nums[2] become: 1 XOR 3 = 2, and the array nums becomes: [1,2,1] -> [2,2,2]. <br/>
The total sum of values is 2 + 2 + 2 = 6. <br/>
It can be shown that 6 is the maximum achievable sum of values.

#### Example 2:


Input: nums = [2,3], k = 7, edges = [[0,1]] <br/>
Output: 9 <br/>
Explanation: Alice can achieve the maximum sum of 9 using a single operation: <br/>
- Choose the edge [0,1]. nums[0] becomes: 2 XOR 7 = 5 and nums[1] become: 3 XOR 7 = 4, and the array nums becomes: [2,3] -> [5,4]. <br/>
The total sum of values is 5 + 4 = 9. <br/>
It can be shown that 9 is the maximum achievable sum of values. <br/>

#### Example 3:


Input: nums = [7,7,7,7,7,7], k = 3, edges = [[0,1],[0,2],[0,3],[0,4],[0,5]] <br/>
Output: 42 <br/>
Explanation: The maximum achievable sum is 42 which can be achieved by Alice performing no operations. <br/>
 

Constraints:

2 <= n == nums.length <= 2 * 104
1 <= k <= 109
0 <= nums[i] <= 109
edges.length == n - 1
edges[i].length == 2
0 <= edges[i][0], edges[i][1] <= n - 1
The input is generated such that edges represent a valid tree.

# Code
```cpp []
class Solution {
 public:
  long long maximumValueSum(vector<int>& nums, int k,
                            vector<vector<int>>& edges) {
    long maxSum = 0;
    int changedCount = 0;
    int minChangeDiff = INT_MAX;

    for (const int num : nums) {
      maxSum += max(num, num ^ k);
      changedCount += ((num ^ k) > num) ? 1 : 0;
      minChangeDiff = min(minChangeDiff, abs(num - (num ^ k)));
    }

    if (changedCount % 2 == 0)
      return maxSum;
    return maxSum - minChangeDiff;
  }
};
```
-----
# 371. Sum of Two Integers [Link🚀](https://leetcode.com/problems/sum-of-two-integers/description/)
Medium

Given two integers a and b, return the sum of the two integers without using the operators + and -.

 

#### Example 1:

Input: a = 1, b = 2 <br/>
Output: 3 <br/>

#### Example 2:

Input: a = 2, b = 3 <br/>
Output: 5 <br/>
 

Constraints:

-1000 <= a, b <= 1000

# Code
```cpp []
class Solution {
 public:
  int getSum(unsigned a, unsigned b) {
    while (b != 0) {                 
      const unsigned carry = a & b;  
      a ^= b;  
      b = carry << 1;
    }
    return a;
  }
};
```
----

# 191. Number of 1 Bits [Link🚀](https://leetcode.com/problems/number-of-1-bits/description/)
Easy

Given a positive integer n, write a function that returns the number of set bits in its binary representation (also known as the Hamming weight).

 

Example 1:

Input: n = 11

Output: 3

Explanation:

The input binary string 1011 has a total of three set bits.

Example 2:

Input: n = 128

Output: 1

Explanation:

The input binary string 10000000 has a total of one set bit.

Example 3:

Input: n = 2147483645

Output: 30

Explanation:

The input binary string 1111111111111111111111111111101 has a total of thirty set bits.

 

Constraints:

1 <= n <= 231 - 1

# Code
```cpp []
class Solution {
public:
    int hammingWeight(int n) {
        int ans = 0;

        while(n){
            n &= (n-1);
            ans++;
        }

        return ans;
    }
};
```
----
# 338. Counting Bits [Link🚀](https://leetcode.com/problems/counting-bits/description/)
Easy

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

 

Example 1:

Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
Example 2:

Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
 

Constraints:

0 <= n <= 105


# Code
```cpp []
class Solution {
 public:
  vector<int> countBits(int n) {

    vector<int> ans(n+1,0);

    for(int i = 1 ; i<=n ; ++i){
        ans[i] = ans[i/2] + ( i & 1);
    }

    return ans;
  }
};
```
----
# 268. Missing Number [Link🚀](https://leetcode.com/problems/missing-number/description/)
Easy

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

 

Example 1:

Input: nums = [3,0,1]

Output: 2

Explanation:

n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.

Example 2:

Input: nums = [0,1]

Output: 2

Explanation:

n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.

Example 3:

Input: nums = [9,6,4,2,3,5,7,0,1]

Output: 8

Explanation:

n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
 

Constraints:

n == nums.length
1 <= n <= 104
0 <= nums[i] <= n
All the numbers of nums are unique.

# Code
```cpp []
class Solution {
public:
    int missingNumber(vector<int>& nums) {

        int N = nums.size();

        int xor1 = 0 ,xor2 = 0;

        for(int i=0 ; i<N ;++i){
            xor1 ^= (i+1);
            xor2 ^= nums[i];
        }

        return (xor1 ^ xor2);

    }
};
```
----

# 190. Reverse Bits [Link🚀](https://leetcode.com/problems/reverse-bits/description/)
Easy

Reverse bits of a given 32 bits unsigned integer.

Note:
Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer -3 and the output represents the signed integer -1073741825.
 

Example 1:

Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
Example 2:

Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
 

Constraints:

The input must be a binary string of length 32

# Code
```cpp []
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans = 0;

        for(int i=0 ; i<32 ; ++i){
            if((n>>i) & 1){
                ans |= 1 << (31 - i);
            }
        }

        return ans;
    }
};
```
-------
# 70. Climbing Stairs [Link🚀](https://leetcode.com/problems/climbing-stairs/description/)
Easy

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45

# Code
```cpp []
class Solution {
public:
    int climbStairs(int n) {
        vector<int> nums(n+1,-1);
        nums[0] = 1;
        nums[1] = 1;

        for(int i=2 ; i<=n ;++i){
            nums[i] = nums[i-1] + nums[i-2];
        }

        return nums[n];

    }
};
```
----

# 322. Coin Change [Link](https://leetcode.com/problems/coin-change/description/)
Medium

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
 

Constraints:

1 <= coins.length <= 12
1 <= coins[i] <= 231 - 1
0 <= amount <= 104

# Code
```cpp []
class Solution {
public:
    int coinChange(vector<int>& coins, int sum) {
        vector<int> dp(sum + 1, 1e9);
        dp[0] = 0;

        for(const int coin:coins){
            for(int i=coin ; i<=sum ; ++i){
                dp[i] = min(dp[i],dp[i-coin] + 1);
            }
        }

        return (dp[sum] >= 1e9) ? -1 : dp[sum];
    }
};

```
----
# 300. Longest Increasing Subsequence [Link](https://leetcode.com/problems/longest-increasing-subsequence/description/)
Medium

Given an integer array nums, return the length of the longest strictly increasing subsequence.

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
 

Constraints:

1 <= nums.length <= 2500
-104 <= nums[i] <= 104

# Code
```cpp []
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int N = nums.size();
        vector<int> dp(N+1 , 1);
        int ans = 1;

        for(int i=1 ; i<N ; ++i){
            for(int j=0 ; j<i ; ++j){
                if(nums[j] < nums[i]){
                    dp[i] = max(dp[i] , dp[j] + 1);
                }
            }
            ans = max(ans,dp[i]);
        }

        return ans;
    }
};
```
----
# 1143. Longest Common Subsequence [Link](https://leetcode.com/problems/longest-common-subsequence/description/)
Medium

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

 

Example 1:

Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
Example 2:

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
Example 3:

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
 

Constraints:

1 <= text1.length, text2.length <= 1000
text1 and text2 consist of only lowercase English characters.

# Code
```cpp []
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size();
        int m = text2.size();

        vector<vector<int>> dp(n+1 , vector<int>(m+1 , 0));


        for(int i=1 ; i<=n ; ++i){
            for(int j=1; j<=m ; ++j){
                if(text1[i-1] == text2[j-1]){
                    dp[i][j] = 1 + dp[i-1][j-1];
                }
                else{
                    dp[i][j] = max(dp[i-1][j] , dp[i][j-1]);
                }
            }
        }

        return dp[n][m];

    }
};
```
----
# 231. Power of Two [Link](https://leetcode.com/problems/power-of-two/description/)
Easy

Given an integer n, return true if it is a power of two. Otherwise, return false.

An integer n is a power of two, if there exists an integer x such that n == 2x.

 

Example 1:

Input: n = 1
Output: true
Explanation: 20 = 1
Example 2:

Input: n = 16
Output: true
Explanation: 24 = 16
Example 3:

Input: n = 3
Output: false
 

Constraints:

-231 <= n <= 231 - 1

# Code
```cpp []
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return (n > 0 ) && ( (n & (n-1)) == 0 );
    }
};
```
-----
# 242. Valid Anagram [Link](https://leetcode.com/problems/valid-anagram/description/)
Easy

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

 

Example 1:

Input: s = "anagram", t = "nagaram"

Output: true

Example 2:

Input: s = "rat", t = "car"

Output: false

 

Constraints:

1 <= s.length, t.length <= 5 * 104
s and t consist of lowercase English letters.


# Code
```cpp []
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()) return false;

        vector<int> dp(26);

        for(char c:s){
            ++dp[c - 'a'];
        }
        for(int c:t){
            if(dp[c - 'a'] == 0) return false;
            else --dp[c - 'a'];
        }

        return true;
    }
};
```
----

# 283. Move Zeroes -> [LeetCode](https://leetcode.com/problems/move-zeroes/)

Easy

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

 

Example 1:

Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
Example 2:

Input: nums = [0]
Output: [0]
 

Constraints:

1 <= nums.length <= 104
-231 <= nums[i] <= 231 - 1
 

Follow up: Could you minimize the total number of operations done?

# Code
```cpp []
class Solution {
public:
    void moveZeroes(vector<int>& a) {

        int n = a.size();
        int i =0 ;

        for(const int n : a){
            if(n != 0){
            a[i++] = n;
            }
        }

        while(i<n){
            a[i++] = 0;
        }

    }
};
```

----

