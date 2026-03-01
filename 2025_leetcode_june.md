
-------------------------
# *LEETCODE june_2025*
-------------------------


# Largest Element in Array -> [Link](https://www.geeksforgeeks.org/problems/largest-element-in-array4009/1) (GFG)

Difficulty: Basic

Given an array arr[]. The task is to find the largest element and return it.

Examples:

Input: arr[] = [1, 8, 7, 56, 90]
Output: 90
Explanation: The largest element of the given array is 90.

Input: arr[] = [5, 5, 5, 5]
Output: 5
Explanation: The largest element of the given array is 5.

Input: arr[] = [10]
Output: 10
Explanation: There is only one element which is the largest.

Constraints:
1 <= arr.size()<= 106
0 <= arr[i] <= 106

Expected Complexities
Time Complexity: O(n)
Auxiliary Space: O(1)

Company Tags
Infosys Oracle Wipro Morgan Stanley


```cpp []
#include<bits/stdc++.h>
class Solution {
  public:
    int largest(vector<int> &arr) {
        int m = arr[0];
        for(int i=1 ; i<arr.size() ; ++i){
            m = max(m,arr[i]);
        }
        return m;
    }
};
```

----

# 215. Kth Largest Element in an Array -> [Link](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

Medium

Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

Can you solve it without sorting?

 

Example 1:

Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
Example 2:

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
 

Constraints:

1 <= k <= nums.length <= 105
-104 <= nums[i] <= 104


# Code
```cpp []
class Solution {
public:
    int findKthLargest(vector<int>& arr, int k) {
        priority_queue<int , vector<int> , greater<>> minheap;

        for(const int n:arr){
            minheap.push(n);
            if(minheap.size() > k) minheap.pop();
        }

        return minheap.top();
    }
};

```
----
# 1752. Check if Array Is Sorted and Rotated -> [LINK](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/description)

Easy

Given an array nums, return true if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return false.

There may be duplicates in the original array.

Note: An array A rotated by x positions results in an array B of the same length such that B[i] == A[(i+x) % A.length] for every valid index i.
 

Example 1:

Input: nums = [3,4,5,1,2]
Output: true
Explanation: [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 3 positions to begin on the element of value 3: [3,4,5,1,2].
Example 2:

Input: nums = [2,1,3,4]
Output: false
Explanation: There is no sorted array once rotated that can make nums.
Example 3:

Input: nums = [1,2,3]
Output: true
Explanation: [1,2,3] is the original sorted array.
You can rotate the array by x = 0 positions (i.e. no rotation) to make nums. 

Constraints:

1 <= nums.length <= 100
1 <= nums[i] <= 100

# Code
```cpp []
class Solution {
public:
    bool check(vector<int>& nums) {
        int n = nums.size();
        int r = 0;
        for(int i=0 ; i<n ; ++i){
            if(nums[i] > nums[(i+1) % n] && ++r > 1) return false;
        }
        return true;
    }
};
```
-----


# Rotate Array by One -> [LINK](https://www.geeksforgeeks.org/problems/cyclically-rotate-an-array-by-one2614/1) (GFG)
Difficulty: Basic

Given an array arr, rotate the array by one position in clockwise direction.

Examples:

Input: arr[] = [1, 2, 3, 4, 5]
Output: [5, 1, 2, 3, 4]
Explanation: If we rotate arr by one position in clockwise 5 come to the front and remaining those are shifted to the end.

Input: arr[] = [9, 8, 7, 6, 4, 2, 1, 3]
Output: [3, 9, 8, 7, 6, 4, 2, 1]
Explanation: After rotating clock-wise 3 comes in first position.

Constraints:
1<=arr.size()<=105
0<=arr[i]<=105

Expected Complexities
Time Complexity: O(n)
Auxiliary Space: O(1)

# Code
```cpp []
class Solution {
  public:
    void rotate(vector<int> &arr) {
        int n = arr.size();
        
       int temp = arr[n-1];
       
       for(int i = n - 2 ; i>=0 ; --i){
           arr[i + 1] = arr[i];
       }
       
       arr[0] = temp;
       
    }
};
```
----
# 189. Rotate Array -> [LINK](https://leetcode.com/problems/rotate-array/description/)

Medium

Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

 

Example 1:

Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]

Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

Example 2:

Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]

Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
 

Constraints:

1 <= nums.length <= 105
-231 <= nums[i] <= 231 - 1
0 <= k <= 105
 

Follow up:

Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
Could you do it in-place with O(1) extra space?

# Code
```cpp []
class Solution {
 public:
  void rotate(vector<int>& arr, int k) {
    int n = arr.size();
    k %= n;
    reverse(arr,0,n-1);
    reverse(arr,0,k-1);
    reverse(arr,k,n-1);
  }

  private:
  void  reverse(vector<int>& nums,int l,int h){
    while(l<h) swap(nums[l++],nums[h--]);
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

# 485. Max Consecutive Ones -> [LeetCode](https://leetcode.com/problems/max-consecutive-ones/description/)

Easy

Given a binary array nums, return the maximum number of consecutive 1's in the array.

 

Example 1:

Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

Example 2:

Input: nums = [1,0,1,1,0,1]
Output: 2
 

Constraints:

1 <= nums.length <= 105
nums[i] is either 0 or 1.

# Code
```cpp []
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        int count = 0;

        for(int i=0 ; i<n ; ++i){
            if(nums[i] == 1){
                count++;
            }else{
                count = 0;
            }
            ans = max(count,ans);
        }

        return ans;
    }
};
```
----

# 3443. Maximum Manhattan Distance After K Changes -> [LeetCode](https://leetcode.com/problems/maximum-manhattan-distance-after-k-changes/description/)

Medium

You are given a string s consisting of the characters 'N', 'S', 'E', and 'W', where s[i] indicates movements in an infinite grid:

'N' : Move north by 1 unit.
'S' : Move south by 1 unit.
'E' : Move east by 1 unit.
'W' : Move west by 1 unit.
Initially, you are at the origin (0, 0). You can change at most k characters to any of the four directions.

Find the maximum Manhattan distance from the origin that can be achieved at any time while performing the movements in order.

The Manhattan Distance between two cells (xi, yi) and (xj, yj) is |xi - xj| + |yi - yj|.
 

Example 1:

Input: s = "NWSE", k = 1

Output: 3

Explanation:

Change s[2] from 'S' to 'N'. The string s becomes "NWNE".

Movement	Position (x, y)	Manhattan Distance	Maximum
s[0] == 'N'	(0, 1)	0 + 1 = 1	1
s[1] == 'W'	(-1, 1)	1 + 1 = 2	2
s[2] == 'N'	(-1, 2)	1 + 2 = 3	3
s[3] == 'E'	(0, 2)	0 + 2 = 2	3
The maximum Manhattan distance from the origin that can be achieved is 3. Hence, 3 is the output.

Example 2:

Input: s = "NSWWEW", k = 3

Output: 6

Explanation:

Change s[1] from 'S' to 'N', and s[4] from 'E' to 'W'. The string s becomes "NNWWWW".

The maximum Manhattan distance from the origin that can be achieved is 6. Hence, 6 is the output.

 

Constraints:

1 <= s.length <= 105
0 <= k <= s.length
s consists of only 'N', 'S', 'E', and 'W'.

# Code
```cpp []
class Solution {
 public:
  int maxDistance(string s, int k) {
    return max({flip(s, k, "NE"), flip(s, k, "NW"),  //
                flip(s, k, "SE"), flip(s, k, "SW")});
  }

 private:
  int flip(const string& s, int k, const string& direction) {
    int res = 0;
    int pos = 0;
    int opposite = 0;

    for (const char c : s) {
      if (direction.find(c) != string::npos) {
        ++pos;
      } else {
        --pos;
        ++opposite;
      }
      res = max(res, pos + 2 * min(k, opposite));
    }

    return res;
  }
};
```
-----

# 3085. Minimum Deletions to Make String K-Special -> [LeetCode](https://leetcode.com/problems/minimum-deletions-to-make-string-k-special/description/)

Medium

You are given a string word and an integer k.

We consider word to be k-special if |freq(word[i]) - freq(word[j])| <= k for all indices i and j in the string.

Here, freq(x) denotes the frequency of the character x in word, and |y| denotes the absolute value of y.

Return the minimum number of characters you need to delete to make word k-special.

 

Example 1:

Input: word = "aabcaba", k = 0

Output: 3

Explanation: We can make word 0-special by deleting 2 occurrences of "a" and 1 occurrence of "c". Therefore, word becomes equal to "baba" where freq('a') == freq('b') == 2.

Example 2:

Input: word = "dabdcbdcdcd", k = 2

Output: 2

Explanation: We can make word 2-special by deleting 1 occurrence of "a" and 1 occurrence of "d". Therefore, word becomes equal to "bdcbdcdcd" where freq('b') == 2, freq('c') == 3, and freq('d') == 4.

Example 3:

Input: word = "aaabaaa", k = 2

Output: 1

Explanation: We can make word 2-special by deleting 1 occurrence of "b". Therefore, word becomes equal to "aaaaaa" where each letter's frequency is now uniformly 6.

 

Constraints:

1 <= word.length <= 105
0 <= k <= 105
word consists only of lowercase English letters.

# Code
```cpp []
class Solution {
 public:
  int minimumDeletions(string word, int k) {
    int ans = INT_MAX;
    vector<int> count(26);

    for (const char c : word)
      ++count[c - 'a'];

    for (const int minFreq : count) {
      int deletions = 0;
      for (const int freq : count)
        if (freq < minFreq)  // Delete all the letters with smaller frequency.
          deletions += freq;
        else  // Delete letters with exceeding frequency.
          deletions += max(0, freq - (minFreq + k));
      ans = min(ans, deletions);
    }

    return ans;
  }
};
```
----

# 219. Contains Duplicate II -> [LeetCode](https://leetcode.com/problems/contains-duplicate-ii/description/)

Easy

Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.

 

Example 1:

Input: nums = [1,2,3,1], k = 3
Output: true
Example 2:

Input: nums = [1,0,1,1], k = 1
Output: true
Example 3:

Input: nums = [1,2,3,1,2,3], k = 2
Output: false
 

Constraints:

1 <= nums.length <= 105
-109 <= nums[i] <= 109
0 <= k <= 105

# Code
```cpp []
class Solution {
 public:
  bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_set<int> seen;

    for (int i = 0; i < nums.size(); ++i) {
      if (!seen.insert(nums[i]).second)
        return true;
      if (i >= k)
        seen.erase(nums[i - k]);
    }

    return false;
  }
};
```
-----


# 136. Single Number -> [LeetCode](https://leetcode.com/problems/single-number/description/)

Easy

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

 

Example 1:

Input: nums = [2,2,1]

Output: 1

Example 2:

Input: nums = [4,1,2,1,2]

Output: 4

Example 3:

Input: nums = [1]

Output: 1

 

Constraints:

1 <= nums.length <= 3 * 104
-3 * 104 <= nums[i] <= 3 * 104
Each element in the array appears twice except for one element which appears only once.


# Code
```cpp []
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;

        for(const int n:nums){
            ans ^= n;
        }

        return ans;
    }
};
```

----

# 2138. Divide a String Into Groups of Size k -> [LeetCode](https://leetcode.com/problems/divide-a-string-into-groups-of-size-k/description/)

Easy

A string s can be partitioned into groups of size k using the following procedure:

The first group consists of the first k characters of the string, the second group consists of the next k characters of the string, and so on. Each element can be a part of exactly one group.
For the last group, if the string does not have k characters remaining, a character fill is used to complete the group.
Note that the partition is done so that after removing the fill character from the last group (if it exists) and concatenating all the groups in order, the resultant string should be s.

Given the string s, the size of each group k and the character fill, return a string array denoting the composition of every group s has been divided into, using the above procedure.

 

Example 1:

Input: s = "abcdefghi", k = 3, fill = "x"
Output: ["abc","def","ghi"]
Explanation:
The first 3 characters "abc" form the first group.
The next 3 characters "def" form the second group.
The last 3 characters "ghi" form the third group.
Since all groups can be completely filled by characters from the string, we do not need to use fill.
Thus, the groups formed are "abc", "def", and "ghi".
Example 2:

Input: s = "abcdefghij", k = 3, fill = "x"
Output: ["abc","def","ghi","jxx"]
Explanation:
Similar to the previous example, we are forming the first three groups "abc", "def", and "ghi".
For the last group, we can only use the character 'j' from the string. To complete this group, we add 'x' twice.
Thus, the 4 groups formed are "abc", "def", "ghi", and "jxx".
 

Constraints:

1 <= s.length <= 100
s consists of lowercase English letters only.
1 <= k <= 100
fill is a lowercase English letter.



# Code
```cpp []
class Solution {
 public:
  vector<string> divideString(string s, int k, char fill) {

    vector<string> ans;

    for (int i = 0; i < s.length(); i += k)
      ans.push_back(i + k > s.length()
                        ? s.substr(i) + string(i + k - s.length(), fill)
                        : s.substr(i, k));

    return ans;
    
  }
};
```
------


# 66. Plus One -> [LeetCode](https://leetcode.com/problems/plus-one/description/)

Easy

You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

 

Example 1:

Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
Example 2:

Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
Example 3:

Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
 

Constraints:

1 <= digits.length <= 100
0 <= digits[i] <= 9
digits does not contain any leading 0's.


# Code
```cpp []
class Solution {
 public:
  vector<int> plusOne(vector<int>& nums) {

    int n = nums.size();

    for(int i=n-1 ; i>=0 ; --i){
        if(nums[i] < 9){
            ++nums[i];
            return nums;
        }
        nums[i] = 0;
    }

    nums.insert(nums.begin() , 1);
    return nums;

  }
};
```

--------

# 2200. Find All K-Distant Indices in an Array -> [LeetCode](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array/description)

Easy

You are given a 0-indexed integer array nums and two integers key and k. A k-distant index is an index i of nums for which there exists at least one index j such that |i - j| <= k and nums[j] == key.

Return a list of all k-distant indices sorted in increasing order.

 

Example 1:

Input: nums = [3,4,9,1,3,9,5], key = 9, k = 1
Output: [1,2,3,4,5,6]
Explanation: Here, nums[2] == key and nums[5] == key.
- For index 0, |0 - 2| > k and |0 - 5| > k, so there is no j where |0 - j| <= k and nums[j] == key. Thus, 0 is not a k-distant index.
- For index 1, |1 - 2| <= k and nums[2] == key, so 1 is a k-distant index.
- For index 2, |2 - 2| <= k and nums[2] == key, so 2 is a k-distant index.
- For index 3, |3 - 2| <= k and nums[2] == key, so 3 is a k-distant index.
- For index 4, |4 - 5| <= k and nums[5] == key, so 4 is a k-distant index.
- For index 5, |5 - 5| <= k and nums[5] == key, so 5 is a k-distant index.
- For index 6, |6 - 5| <= k and nums[5] == key, so 6 is a k-distant index.
Thus, we return [1,2,3,4,5,6] which is sorted in increasing order. 
Example 2:

Input: nums = [2,2,2,2,2], key = 2, k = 2
Output: [0,1,2,3,4]
Explanation: For all indices i in nums, there exists some index j such that |i - j| <= k and nums[j] == key, so every index is a k-distant index. 
Hence, we return [0,1,2,3,4].
 

Constraints:

1 <= nums.length <= 1000
1 <= nums[i] <= 1000
key is an integer from the array nums.
1 <= k <= nums.length

# Code
```cpp []
class Solution {
public:
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        vector<int> res;
        int n = nums.size();

        int end = 0;

        for(int i=0 ; i<n ; ++i){
            if(nums[i] == key){ 
                int rt = max(end , i-k);
                end = min(n-1 , i+k) + 1;

                for (int j = rt; j < end; ++j){   
                    res.push_back(j);
                }
            }
        }

        return res;
    }
};
```

-----


# 2040. Kth Smallest Product of Two Sorted Arrays -> [LeetCode](https://leetcode.com/problems/kth-smallest-product-of-two-sorted-arrays/description/)

Hard

Given two sorted 0-indexed integer arrays nums1 and nums2 as well as an integer k, return the kth (1-based) smallest product of nums1[i] * nums2[j] where 0 <= i < nums1.length and 0 <= j < nums2.length.
 

Example 1:

Input: nums1 = [2,5], nums2 = [3,4], k = 2
Output: 8
Explanation: The 2 smallest products are:
- nums1[0] * nums2[0] = 2 * 3 = 6
- nums1[0] * nums2[1] = 2 * 4 = 8
The 2nd smallest product is 8.
Example 2:

Input: nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6
Output: 0
Explanation: The 6 smallest products are:
- nums1[0] * nums2[1] = (-4) * 4 = -16
- nums1[0] * nums2[0] = (-4) * 2 = -8
- nums1[1] * nums2[1] = (-2) * 4 = -8
- nums1[1] * nums2[0] = (-2) * 2 = -4
- nums1[2] * nums2[0] = 0 * 2 = 0
- nums1[2] * nums2[1] = 0 * 4 = 0
The 6th smallest product is 0.
Example 3:

Input: nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3
Output: -6
Explanation: The 3 smallest products are:
- nums1[0] * nums2[4] = (-2) * 5 = -10
- nums1[0] * nums2[3] = (-2) * 4 = -8
- nums1[4] * nums2[0] = 2 * (-3) = -6
The 3rd smallest product is -6.
 

Constraints:

1 <= nums1.length, nums2.length <= 5 * 104
-105 <= nums1[i], nums2[j] <= 105
1 <= k <= nums1.length * nums2.length
nums1 and nums2 are sorted.


# Code
```cpp []
class Solution {
 public:
  long long kthSmallestProduct(vector<int>& nums1, vector<int>& nums2,
                               long long k) {
    vector<int> A1;
    vector<int> A2;
    vector<int> B1;
    vector<int> B2;

    seperate(nums1, A1, A2);
    seperate(nums2, B1, B2);

    const long negCount = A1.size() * B2.size() + A2.size() * B1.size();
    int sign = 1;

    if (k > negCount) {
      k -= negCount; 
    } else {
      k = negCount - k + 1; 
      sign = -1;
      swap(B1, B2);
    }

    long l = 0;
    long r = 1e10;

    while (l < r) {
      const long m = (l + r) / 2;
      if (numProductNoGreaterThan(A1, B1, m) +
              numProductNoGreaterThan(A2, B2, m) >=
          k)
        r = m;
      else
        l = m + 1;
    }

    return sign * l;
  }

 private:
  void seperate(const vector<int>& arr, vector<int>& A1, vector<int>& A2) {
    for (const int a : arr)
      if (a < 0)
        A1.push_back(-a);
      else
        A2.push_back(a);
    ranges::reverse(A1); 
  }

  long numProductNoGreaterThan(const vector<int>& A, const vector<int>& B,
                               long m) {
    long count = 0;
    int j = B.size() - 1;
    for (const long a : A) {
      while (j >= 0 && a * B[j] > m)
        --j;
      count += j + 1;
    }
    return count;
  }
};
```
-----

# 2311. Longest Binary Subsequence Less Than or Equal to K -> [LeetCode](https://leetcode.com/problems/longest-binary-subsequence-less-than-or-equal-to-k/description/)

Medium

You are given a binary string s and a positive integer k.

Return the length of the longest subsequence of s that makes up a binary number less than or equal to k.

Note:

The subsequence can contain leading zeroes.
The empty string is considered to be equal to 0.
A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.
 

Example 1:

Input: s = "1001010", k = 5
Output: 5
Explanation: The longest subsequence of s that makes up a binary number less than or equal to 5 is "00010", as this number is equal to 2 in decimal.
Note that "00100" and "00101" are also possible, which are equal to 4 and 5 in decimal, respectively.
The length of this subsequence is 5, so 5 is returned.
Example 2:

Input: s = "00101001", k = 1
Output: 6
Explanation: "000001" is the longest subsequence of s that makes up a binary number less than or equal to 1, as this number is equal to 1 in decimal.
The length of this subsequence is 6, so 6 is returned.
 

Constraints:

1 <= s.length <= 1000
s[i] is either '0' or '1'.
1 <= k <= 109

# Code
```cpp []
class Solution {
 public:
  int longestSubsequence(string s, int k) {
    int oneCount = 0;
    int num = 0;
    int pow = 1;

    for (int i = s.length() - 1; i >= 0 && num + pow <= k; --i) {
      if (s[i] == '1') {
        ++oneCount;
        num += pow;
      }
      pow *= 2;
    }

    return ranges::count(s, '0') + oneCount;
  }
};
```

------


# 2014. Longest Subsequence Repeated k Times -> [LeetCode](https://leetcode.com/problems/longest-subsequence-repeated-k-times/description/)

Hard

You are given a string s of length n, and an integer k. You are tasked to find the longest subsequence repeated k times in string s.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

A subsequence seq is repeated k times in the string s if seq * k is a subsequence of s, where seq * k represents a string constructed by concatenating seq k times.

For example, "bba" is repeated 2 times in the string "bababcba", because the string "bbabba", constructed by concatenating "bba" 2 times, is a subsequence of the string "bababcba".
Return the longest subsequence repeated k times in string s. If multiple such subsequences are found, return the lexicographically largest one. If there is no such subsequence, return an empty string.

 

Example 1:

example 1

Input: s = "letsleetcode", k = 2
Output: "let"
Explanation: There are two longest subsequences repeated 2 times: "let" and "ete".
"let" is the lexicographically largest one.

Example 2:

Input: s = "bb", k = 2
Output: "b"
Explanation: The longest subsequence repeated 2 times is "b".

Example 3:

Input: s = "ab", k = 2
Output: ""
Explanation: There is no subsequence repeated 2 times. Empty string is returned.
 

Constraints:

n == s.length
2 <= n, k <= 2000
2 <= n < k * 8
s consists of lowercase English letters.

# Code
```cpp []
#include <iostream>
#include <queue>
#include <string>
#include <vector>

class Solution {
 public:
  std::string longestSubsequenceRepeatedK(const std::string& s, int k) {
    std::string ans;
    std::vector<int> count(26);
    std::vector<char> possibleChars;
    std::queue<std::string> q{{""}};

    for (const char c : s)
      ++count[c - 'a'];

    for (char c = 'a'; c <= 'z'; ++c)
      if (count[c - 'a'] >= k)
        possibleChars.push_back(c);

    while (!q.empty()) {
      const std::string currSubseq = q.front();
      q.pop();
      if (currSubseq.length() * k > s.length())
        return ans;
      for (const char c : possibleChars) {
        const std::string& newSubseq = currSubseq + c;
        if (isSubsequence(newSubseq, s, k)) {
          q.push(newSubseq);
          ans = newSubseq;
        }
      }
    }

    return ans;
  }

 private:
  bool isSubsequence(const std::string& subseq, const std::string& s, int k) {
    int i = 0;
    for (const char c : s)
      if (c == subseq[i])
        if (++i == subseq.length()) {
          if (--k == 0)
            return true;
          i = 0;
        }
    return false;
  }
};
```

-----


# 35. Search Insert Position -> [LeetCode](https://leetcode.com/problems/search-insert-position/description/)

Easy

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [1,3,5,6], target = 5
Output: 2


Example 2:

Input: nums = [1,3,5,6], target = 2
Output: 1


Example 3:

Input: nums = [1,3,5,6], target = 7
Output: 4
 

Constraints:

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums contains distinct values sorted in ascending order.
-104 <= target <= 104

# Code
```cpp []
class Solution {
public:
    int searchInsert(vector<int>& arr, int target) {
        int n = arr.size();
        int l = 0 , h = n;

        while( l < h){
            int mid = ( l + h ) / 2;

            if(arr[mid] == target) return mid;

            if(arr[mid] < target) l = mid + 1;
            else h = mid;
        }

        return l;
    }
};
```

---------



# Q1. Check if Any Element Has Prime Frequency -> [LeetCode](https://leetcode.com/contest/weekly-contest-455/problems/check-if-any-element-has-prime-frequency/description/)
Easy

You are given an integer array nums.

Return true if the frequency of any element of the array is prime, otherwise, return false.

The frequency of an element x is the number of times it occurs in the array.

A prime number is a natural number greater than 1 with only two factors, 1 and itself.

 

Example 1:

Input: nums = [1,2,3,4,5,4]

Output: true

Explanation:

4 has a frequency of two, which is a prime number.

Example 2:

Input: nums = [1,2,3,4,5]

Output: false

Explanation:

All elements have a frequency of one.

Example 3:

Input: nums = [2,2,2,4,4]

Output: true

Explanation:

Both 2 and 4 have a prime frequency.

 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 100

# Code
```cpp []
class Solution {
public:
    
    bool checkPrimeFrequency(vector<int>& nums) {
        
         unordered_map<int,int> freq;

        for(int n:nums){
            freq[n]++;
        }

        for(auto [n,fre]:freq){
            int count = 0;
            for(int i=1 ; i<=fre ; ++i){
                if(fre % i == 0 ) count++;
            }
            if(count == 2) return true;
        }

        return false;
    }
};
```

---------


# 2099. Find Subsequence of Length K With the Largest Sum -> [LeetCode](https://leetcode.com/problems/find-subsequence-of-length-k-with-the-largest-sum/description/)

Easy

You are given an integer array nums and an integer k. You want to find a subsequence of nums of length k that has the largest sum.

Return any such subsequence as an integer array of length k.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

 

Example 1:

Input: nums = [2,1,3,3], k = 2
Output: [3,3]
Explanation:
The subsequence has the largest sum of 3 + 3 = 6.


Example 2:

Input: nums = [-1,-2,3,4], k = 3
Output: [-1,3,4]
Explanation: 
The subsequence has the largest sum of -1 + 3 + 4 = 6.


Example 3:

Input: nums = [3,4,3,3], k = 2
Output: [3,4]
Explanation:
The subsequence has the largest sum of 3 + 4 = 7. 
Another possible subsequence is [4, 3].
 

Constraints:

1 <= nums.length <= 1000
-105 <= nums[i] <= 105
1 <= k <= nums.length

# Code
```cpp []
class Solution {
 public:
  vector<int> maxSubsequence(vector<int>& nums, int k) {
    vector<int> ans;
    vector<int> arr(nums);
    nth_element(arr.begin(), arr.end() - k, arr.end());
    const int threshold = arr[arr.size() - k];
    const int larger =
        ranges::count_if(nums, [&](int num) { return num > threshold; });
    int equal = k - larger;

    for (const int num : nums)
      if (num > threshold) {
        ans.push_back(num);
      } else if (num == threshold && equal) {
        ans.push_back(num);
        --equal;
      }

    return ans;
  }
};
```

------

# 1498. Number of Subsequences That Satisfy the Given Sum Condition -> [LeetCode](https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/description/)

Medium

You are given an array of integers nums and an integer target.

Return the number of non-empty subsequences of nums such that the sum of the minimum and maximum element on it is less or equal to target. Since the answer may be too large, return it modulo 109 + 7.

 

Example 1:

Input: nums = [3,5,6,7], target = 9
Output: 4

Explanation: There are 4 subsequences that satisfy the condition.
[3] -> Min value + max value <= target (3 + 3 <= 9)
[3,5] -> (3 + 5 <= 9)
[3,5,6] -> (3 + 6 <= 9)
[3,6] -> (3 + 6 <= 9)


Example 2:

Input: nums = [3,3,6,8], target = 10
Output: 6

Explanation: There are 6 subsequences that satisfy the condition. (nums can have repeated numbers).
[3] , [3] , [3,3], [3,6] , [3,6] , [3,3,6]


Example 3:

Input: nums = [2,3,3,4,6,7], target = 12
Output: 61

Explanation: There are 63 non-empty subsequences, two of them do not satisfy the condition ([6,7], [7]).
Number of valid subsequences (63 - 2 = 61).
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 106
1 <= target <= 106

# Code
```cpp []
class Solution {
 public:
  int numSubseq(vector<int>& nums, int target) {
    constexpr int kMod = 1'000'000'007;
    const int n = nums.size();
    int ans = 0;
    vector<int> pows(n, 1);  // pows[i] = 2^i % kMod

    for (int i = 1; i < n; ++i)
      pows[i] = pows[i - 1] * 2 % kMod;

    ranges::sort(nums);

    for (int l = 0, r = n - 1; l <= r;)
      if (nums[l] + nums[r] <= target) {
        ans += pows[r - l];
        ans %= kMod;
        ++l;
      } else {
        --r;
      }

    return ans;
  }
};


```

-------------

# 594. Longest Harmonious Subsequence -> [LeetCode](https://leetcode.com/problems/longest-harmonious-subsequence/description/)

Easy

We define a harmonious array as an array where the difference between its maximum value and its minimum value is exactly 1.

Given an integer array nums, return the length of its longest harmonious subsequence among all its possible subsequences.

 

Example 1:

Input: nums = [1,3,2,2,5,2,3,7]

Output: 5

Explanation:

The longest harmonious subsequence is [3,2,2,2,3].

Example 2:

Input: nums = [1,2,3,4]

Output: 2

Explanation:

The longest harmonious subsequences are [1,2], [2,3], and [3,4], all of which have a length of 2.

Example 3:

Input: nums = [1,1,1,1]

Output: 0

Explanation:

No harmonic subsequence exists.

 

Constraints:

1 <= nums.length <= 2 * 104
-109 <= nums[i] <= 109


# Code
```cpp []
class Solution {
 public:
  int findLHS(vector<int>& nums) {
    int ans = 0;
    unordered_map<int, int> count;

    for (const int num : nums)
      ++count[num];

    for (const auto& [num, freq] : count)
      if (const auto it = count.find(num + 1); it != count.cend())
        ans = max(ans, freq + it->second);

    return ans;
  }
};
```

-----


# 74. Search a 2D Matrix -> [Leetcode](https://leetcode.com/problems/search-a-2d-matrix/description/)

Medium

You are given an m x n integer matrix matrix with the following two properties:

Each row is sorted in non-decreasing order.
The first integer of each row is greater than the last integer of the previous row.
Given an integer target, return true if target is in matrix or false otherwise.

You must write a solution in O(log(m * n)) time complexity.

 

Example 1:

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3

Output: true


Example 2:

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13

Output: false
 

Constraints:

m == matrix.length
n == matrix[i].length
1 <= m, n <= 100
-104 <= matrix[i][j], target <= 104

# Code
```cpp []
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size();
        int m = matrix[0].size();

        int i = 0 , j = n*m - 1;

        while(i<=j){
            int mid = (i + j) / 2;
            int r = mid / m;
            int c = mid % m;

            int ele = matrix[r][c];

            if(ele == target) return true;

            if(ele < target) i = mid + 1;
            else j = mid - 1; 
        }


        return false;
    }
};
```


------

# 416. Partition Equal Subset Sum -> [LeetCode](https://leetcode.com/problems/partition-equal-subset-sum/description/)

Medium

Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.

 

Example 1:

Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].


Example 2:

Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
 

Constraints:

1 <= nums.length <= 200
1 <= nums[i] <= 100


# Code
```cpp []
#include<bits/stdc++.h>
using namespace std;

class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int total = 0;
        for(int num:nums){
            total += num;
        }

        if(total % 2 != 0) return false;

        int target = total / 2;

        vector<bool> dp(target+1 , false);

        dp[0] = true;

        for(int i=0 ; i<nums.size() ; ++i){
            for(int j = target ; j >= nums[i] ; --j){
                dp[j] = dp[j] || dp[ j - nums[i] ];
            }
        }

        if(dp[target]) return true;

        return false;
    }
};
```

------

# 4. Median of Two Sorted Arrays -> [LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)
Hard

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

 

Example 1:

Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.


Example 2:

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
 

Constraints:

nums1.length == m
nums2.length == n
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
-106 <= nums1[i], nums2[i] <= 106

# Code
```cpp []
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size();
        int n2 = nums2.size();

        int i=0 , j=0 , k=0;

        vector<double> arr(n1+n2);

        while(i < n1 && j < n2){
            if(nums1[i] < nums2[j]) arr[k++] = nums1[i++];
            else arr[k++] = nums2[j++];
        }

        while(i < n1) arr[k++] = nums1[i++];

        while(j < n2) arr[k++] = nums2[j++];

        int len = arr.size();

        double mean = 0;

        if(len % 2 != 0){
            mean = arr[len/2];
        }else{
            mean = (arr[len/2] + arr[len/2 - 1]) / 2.0;
        }

        return mean;
    }
};
```

---------------





