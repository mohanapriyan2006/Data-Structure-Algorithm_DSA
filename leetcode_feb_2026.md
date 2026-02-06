# ------------------------------------------------------------
# LeetCode problems - FEB 2026
# ------------------------------------------------------------


# [3637. Trionic Array I](https://leetcode.com/problems/trionic-array-i/description/)

Easy
 
You are given an integer array nums of length n.

An array is trionic if there exist indices 0 < p < q < n − 1 such that:

nums[0...p] is strictly increasing,
nums[p...q] is strictly decreasing,
nums[q...n − 1] is strictly increasing.
Return true if nums is trionic, otherwise return false.

 

Example 1:

Input: nums = [1,3,5,4,2,6]

Output: true

Explanation:

Pick p = 2, q = 4:

nums[0...2] = [1, 3, 5] is strictly increasing (1 < 3 < 5).
nums[2...4] = [5, 4, 2] is strictly decreasing (5 > 4 > 2).
nums[4...5] = [2, 6] is strictly increasing (2 < 6).



Example 2:

Input: nums = [2,1,3]

Output: false

Explanation:

There is no way to pick p and q to form the required three segments.

 

Constraints:

3 <= n <= 100
-1000 <= nums[i] <= 1000



# Code
```cpp []
class Solution {
public:
    bool isTrionic(vector<int>& nums) {
        int n = nums.size();
        int i=0;
        
        while(i+1 < n && nums[i] < nums[i+1]) i++;
        if(i == 0 || i == n-1) return false;

        int k=i;
        while(i+1 < n && nums[i] > nums[i+1]) i++;
        if(i == k || i == n-1) return false;

        while(i+1 < n && nums[i] < nums[i+1]) i++;

        return i == n-1;

    }
};
```

------------------------------------------------------------------------------------------------------------



# [3640. Trionic Array II](https://leetcode.com/problems/trionic-array-ii)

Hard
 
You are given an integer array nums of length n.

A trionic subarray is a contiguous subarray nums[l...r] (with 0 <= l < r < n) for which there exist indices l < p < q < r such that:

nums[l...p] is strictly increasing,
nums[p...q] is strictly decreasing,
nums[q...r] is strictly increasing.
Return the maximum sum of any trionic subarray in nums.

 


Example 1:

Input: nums = [0,-2,-1,-3,0,2,-1]

Output: -4

Explanation:

Pick l = 1, p = 2, q = 3, r = 5:

nums[l...p] = nums[1...2] = [-2, -1] is strictly increasing (-2 < -1).
nums[p...q] = nums[2...3] = [-1, -3] is strictly decreasing (-1 > -3)
nums[q...r] = nums[3...5] = [-3, 0, 2] is strictly increasing (-3 < 0 < 2).
Sum = (-2) + (-1) + (-3) + 0 + 2 = -4.



Example 2:

Input: nums = [1,4,2,7]

Output: 14

Explanation:

Pick l = 0, p = 1, q = 2, r = 3:

nums[l...p] = nums[0...1] = [1, 4] is strictly increasing (1 < 4).
nums[p...q] = nums[1...2] = [4, 2] is strictly decreasing (4 > 2).
nums[q...r] = nums[2...3] = [2, 7] is strictly increasing (2 < 7).
Sum = 1 + 4 + 2 + 7 = 14.
 

Constraints:

4 <= n = nums.length <= 105
-109 <= nums[i] <= 109
It is guaranteed that at least one trionic subarray exists.


# Code
```cpp []

class Solution {
public:
    vector<tuple<int, int, long long> > decompose(vector<int>& nums){
        int n = (int)nums.size();
        vector<tuple<int, int, long long> > subarrays;

        int l = 0;
        long long sum = nums[0];
        
        for(int i = 1; i < n; i ++){
            if(nums[i - 1] <= nums[i]){
                subarrays.push_back({l, i - 1, sum});
                l = i;
                sum = 0;
            }
            sum += nums[i];
        }
        subarrays.push_back({l, n - 1, sum});
        return subarrays;
    }
    long long maxSumTrionic(vector<int>& nums){
        int n = (int)nums.size();
        long long maxEndingAt[n];
        for(int i = 0; i < n; i ++){
            maxEndingAt[i] = nums[i];
            if(i > 0 && nums[i - 1] < nums[i]){
                if(maxEndingAt[i - 1] > 0){
                    maxEndingAt[i] += maxEndingAt[i - 1];
                }
            }
        }
        long long maxStartingAt[n];
        for(int i = n - 1; i >= 0; i --){
            maxStartingAt[i] = nums[i];
            if(i < n - 1 && nums[i] < nums[i + 1]){
                if(maxStartingAt[i + 1] > 0){
                    maxStartingAt[i] += maxStartingAt[i + 1];
                }
            }
        }
        vector<tuple<int, int, long long> > PQS = decompose(nums);
        long long ans = LLONG_MIN;
        for(auto [p, q, sum] : PQS){
            
            if(p > 0 && nums[p-1] < nums[p] &&
               q < n - 1 && nums[q] < nums[q + 1] &&
               p < q){
                ans = max(ans, maxEndingAt[p-1] + sum + maxStartingAt[q+1]);
            }
        }
        return ans;
    }
};
```

------------------------------------------------------------------------------------------------------------------------------------------------


# [3379. Transformed Array](https://leetcode.com/problems/transformed-array/)

Easy

You are given an integer array nums that represents a circular array. Your task is to create a new array result of the same size, following these rules:

For each index i (where 0 <= i < nums.length), perform the following independent actions:
If nums[i] > 0: Start at index i and move nums[i] steps to the right in the circular array. Set result[i] to the value of the index where you land.
If nums[i] < 0: Start at index i and move abs(nums[i]) steps to the left in the circular array. Set result[i] to the value of the index where you land.
If nums[i] == 0: Set result[i] to nums[i].
Return the new array result.

Note: Since nums is circular, moving past the last element wraps around to the beginning, and moving before the first element wraps back to the end.

 

Example 1:

Input: nums = [3,-2,1,1]

Output: [1,1,1,3]

Explanation:

For nums[0] that is equal to 3, If we move 3 steps to right, we reach nums[3]. So result[0] should be 1.
For nums[1] that is equal to -2, If we move 2 steps to left, we reach nums[3]. So result[1] should be 1.
For nums[2] that is equal to 1, If we move 1 step to right, we reach nums[3]. So result[2] should be 1.
For nums[3] that is equal to 1, If we move 1 step to right, we reach nums[0]. So result[3] should be 3.



Example 2:

Input: nums = [-1,4,-1]

Output: [-1,-1,4]

Explanation:

For nums[0] that is equal to -1, If we move 1 step to left, we reach nums[2]. So result[0] should be -1.
For nums[1] that is equal to 4, If we move 4 steps to right, we reach nums[2]. So result[1] should be -1.
For nums[2] that is equal to -1, If we move 1 step to left, we reach nums[1]. So result[2] should be 4.
 

Constraints:

1 <= nums.length <= 100
-100 <= nums[i] <= 100


# Code
```cpp []
class Solution {
public:
    vector<int> constructTransformedArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> res;
        
        for(int i=0 ; i<n ; ++i){
            int ind = ( ( (i + nums[i]) % n ) + n ) % n ;
            res.push_back(nums[ind]);
        }

        return res;

    }
};
```


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

# [3634. Minimum Removals to Balance Array](https://leetcode.com/problems/minimum-removals-to-balance-array)

Medium
 
You are given an integer array nums and an integer k.

An array is considered balanced if the value of its maximum element is at most k times the minimum element.

You may remove any number of elements from nums​​​​​​​ without making it empty.

Return the minimum number of elements to remove so that the remaining array is balanced.

Note: An array of size 1 is considered balanced as its maximum and minimum are equal, and the condition always holds true.

 

Example 1:

Input: nums = [2,1,5], k = 2

Output: 1

Explanation:

Remove nums[2] = 5 to get nums = [2, 1].
Now max = 2, min = 1 and max <= min * k as 2 <= 1 * 2. Thus, the answer is 1.



Example 2:

Input: nums = [1,6,2,9], k = 3

Output: 2

Explanation:

Remove nums[0] = 1 and nums[3] = 9 to get nums = [6, 2].
Now max = 6, min = 2 and max <= min * k as 6 <= 2 * 3. Thus, the answer is 2.



Example 3:

Input: nums = [4,6], k = 2

Output: 0

Explanation:

Since nums is already balanced as 6 <= 4 * 2, no elements need to be removed.
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
1 <= k <= 105


# Code
```cpp []
class Solution {
public:
    int minRemoval(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), left = 0, maxSize = 0;

        for (int right = 0; right < n; ++right) {
            while ((long long)nums[right] > (long long)k * nums[left]) left++;
            maxSize = max(maxSize, right - left + 1);
        }
        return n - maxSize;
    }
};
```

---------------------------------------------------------------------------------------------------------------------------------------





