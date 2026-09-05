# 
# LeetCode Monthly Problems - September 2026

# [3876. Construct Uniform Parity Array II](https://leetcode.com/problems/construct-uniform-parity-array-ii)

Medium

You are given an array nums1 of n distinct integers.

You want to construct another array nums2 of length n such that the elements in nums2 are either all odd or all even.

For each index i, you must choose exactly one of the following (in any order):

nums2[i] = nums1[i]​​​​​​​
nums2[i] = nums1[i] - nums1[j], for an index j != i, such that nums1[i] - nums1[j] >= 1
Return true if it is possible to construct such an array, otherwise return false.

 

Example 1:

Input: nums1 = [1,4,7]

Output: true

Explanation:​​​​​​​​​​​​​​

Set nums2[0] = nums1[0] = 1.
Set nums2[1] = nums1[1] - nums1[0] = 4 - 1 = 3.
Set nums2[2] = nums1[2] = 7.
nums2 = [1, 3, 7], and all elements are odd. Thus, the answer is true.




Example 2:

Input: nums1 = [2,3]

Output: false

Explanation:

It is not possible to construct nums2 such that all elements have the same parity. Thus, the answer is false.




Example 3:

Input: nums1 = [4,6]

Output: true

Explanation:

Set nums2[0] = nums1[0] = 4.
Set nums2[1] = nums1[1] = 6.
nums2 = [4, 6], and all elements are even. Thus, the answer is true.


 

Constraints:

1 <= n == nums1.length <= 105
1 <= nums1[i] <= 109
nums1 consists of distinct integers.



# Code
```cpp []
class Solution {
public:
    bool uniformArray(vector<int>& A) {
        int xmin = A[0];
        bool odd = 0;

        for (auto& x : A) {
            xmin = min(xmin, x);
            odd |= x & 1;
        }

        return (xmin & 1) == odd;
    }
};
```

--------------------------------------------------------------------------------------------


# [3903. Smallest Stable Index I](https://leetcode.com/problems/smallest-stable-index-i)

Easy
 
You are given an integer array nums of length n and an integer k.

For each index i, define its instability score as max(nums[0..i]) - min(nums[i..n - 1]).

In other words:

max(nums[0..i]) is the largest value among the elements from index 0 to index i.
min(nums[i..n - 1]) is the smallest value among the elements from index i to index n - 1.
An index i is called stable if its instability score is less than or equal to k.

Return the smallest stable index. If no such index exists, return -1.

 


Example 1:

Input: nums = [5,0,1,4], k = 3

Output: 3

Explanation:

At index 0: The maximum in [5] is 5, and the minimum in [5, 0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 1: The maximum in [5, 0] is 5, and the minimum in [0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 2: The maximum in [5, 0, 1] is 5, and the minimum in [1, 4] is 1, so the instability score is 5 - 1 = 4.
At index 3: The maximum in [5, 0, 1, 4] is 5, and the minimum in [4] is 4, so the instability score is 5 - 4 = 1.
This is the first index with an instability score less than or equal to k = 3. Thus, the answer is 3.



Example 2:

Input: nums = [3,2,1], k = 1

Output: -1

Explanation:

At index 0, the instability score is 3 - 1 = 2.
At index 1, the instability score is 3 - 1 = 2.
At index 2, the instability score is 3 - 1 = 2.
None of these values is less than or equal to k = 1, so the answer is -1.



Example 3:

Input: nums = [0], k = 0

Output: 0

Explanation:

At index 0, the instability score is 0 - 0 = 0, which is less than or equal to k = 0. Therefore, the answer is 0.

 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 109
0 <= k <= 109


# Code
```cpp []
class Solution {
public:
    inline static int suf[100];
    int firstStableIndex(vector<int>& nums, int k) {
        int n = nums.size();
        suf[n - 1] = nums.back();

        for (int i = n - 2; i >= 0; i--)
            suf[i] = min(suf[i + 1], nums[i]);        

        int maxSoFar = 0;
        for (int i = 0; i < n; i++) {
            maxSoFar = max(maxSoFar, nums[i]);
            if (maxSoFar - suf[i] <= k)
                return i;
        }

        return -1;
    }
};
```

---------------------------------------------------------------------------------------------------------------------


# [3904. Smallest Stable Index II](https://leetcode.com/problems/smallest-stable-index-ii/)

Medium

You are given an integer array nums of length n and an integer k.

For each index i, define its instability score as max(nums[0..i]) - min(nums[i..n - 1]).

In other words:

max(nums[0..i]) is the largest value among the elements from index 0 to index i.
min(nums[i..n - 1]) is the smallest value among the elements from index i to index n - 1.
An index i is called stable if its instability score is less than or equal to k.

Return the smallest stable index. If no such index exists, return -1.

 

Example 1:

Input: nums = [5,0,1,4], k = 3

Output: 3

Explanation:

At index 0: The maximum in [5] is 5, and the minimum in [5, 0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 1: The maximum in [5, 0] is 5, and the minimum in [0, 1, 4] is 0, so the instability score is 5 - 0 = 5.
At index 2: The maximum in [5, 0, 1] is 5, and the minimum in [1, 4] is 1, so the instability score is 5 - 1 = 4.
At index 3: The maximum in [5, 0, 1, 4] is 5, and the minimum in [4] is 4, so the instability score is 5 - 4 = 1.
This is the first index with an instability score less than or equal to k = 3. Thus, the answer is 3.





Example 2:

Input: nums = [3,2,1], k = 1

Output: -1

Explanation:

At index 0, the instability score is 3 - 1 = 2.
At index 1, the instability score is 3 - 1 = 2.
At index 2, the instability score is 3 - 1 = 2.
None of these values is less than or equal to k = 1, so the answer is -1.




Example 3:

Input: nums = [0], k = 0

Output: 0

Explanation:

At index 0, the instability score is 0 - 0 = 0, which is less than or equal to k = 0. Therefore, the answer is 0.


 

Constraints:

1 <= nums.length <= 105
0 <= nums[i] <= 109
0 <= k <= 109







