
# ***LeetCode_Problem - (feb_2025)***
-----


# 1790. Check if One String Swap Can Make Strings Equal

You are given two strings s1 and s2 of equal length. A string swap is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return true if it is possible to make both strings equal by performing at most one string swap on exactly one of the strings. Otherwise, return false.

 

Example 1:

Input: s1 = "bank", s2 = "kanb"
<br/>Output: true
<br/>Explanation: For example, swap the first character with the last character of s2 to make "bank".
Example 2:

Input: s1 = "attack", s2 = "defend"
<br/>Output: false
<br/>Explanation: It is impossible to make them equal with one string swap.
Example 3:

Input: s1 = "kelb", s2 = "kelb"
<br/>Output: true
<br/>Explanation: The two strings are already equal, so no string swap operation is required.

# Complexity
- Time complexity: o(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: o(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public boolean areAlmostEqual(String s1, String s2) {
        List<Integer> diff = new ArrayList<>();

        for(int i=0;i<s1.length();i++){
            if(s1.charAt(i) != s2.charAt(i)){
                diff.add(i);
            }
        }

        return diff.isEmpty() || (diff.size() == 2 && s1.charAt(diff.get(0)) == s2.charAt(diff.get(1)) && s1.charAt(diff.get(1)) == s2.charAt(diff.get(0))  );
    }
}
```

---

# 1. Two Sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
<br/>Output: [0,1]
<br/>Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
<br/>Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
<br/>Output: [0,1]

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int ans[] = new int[2];
        int n = nums.length;
        for(int i=0;i<n-1;i++){
            for(int j=i+1;j<n;j++){
                if(nums[i] + nums[j] == target){
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }

        return ans;
    }
}
```

---

# 70. Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
<br/>Output: 2
<br/>Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
<br/>Output: 3
<br/>Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
  public int climbStairs(int n) {
    int steps[] = new int[n + 1];
    steps[0]=1;
    steps[1]=1;

    for(int i=2;i<=n;i++){
        steps[i]= steps[i-1]+steps[i-2];
    }

    return steps[n];
  }
}
```

---

# 509. Fibonacci Number

The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.
Given n, calculate F(n).

 

Example 1:

Input: n = 2
<br/>Output: 1
<br/>Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
Example 2:

Input: n = 3
<br/>Output: 2
<br/>Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
Example 3:

Input: n = 4
<br/>Output: 3
<br/>Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.

# Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int fib(int n) {
     if( n == 0){
        return 0;
     }   
     if( n == 1){
        return 1;
     }
     return fib(n-1) + fib(n-2);
    }
}
```

---

# 1137. N-th Tribonacci Number

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given n, return the value of Tn.

 

Example 1:

Input: n = 4
<br/>Output: 4
<br/>Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
Example 2:

Input: n = 25
<br/>Output: 1389537

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
  public int tribonacci(int n) {
    if( n < 2 ) return n;

    int[] tribo = {0,1,1};

    for(int i=3;i<=n;i++){
        final int next = tribo[0] + tribo[1] + tribo[2];
        tribo[0] = tribo[1];
        tribo[1] = tribo[2];
        tribo[2] = next;
    }

    return tribo[2];
  }
}
```

---

# 746. Min Cost Climbing Stairs

You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

 

Example 1:

Input: cost = [10,15,20]
<br/>Output: 15
<br/>Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.
Example 2:

Input: cost = [1,100,1,1,1,100,1,1,100,1]
<br/>Output: 6
<br/>Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int L = cost.length;
        int[] minCost = new int[L + 1];
        for(int i=2 ; i<=L ; i++){
            minCost[i] = Math.min(cost[i - 1] + minCost[i - 1] , cost[i - 2] + minCost[i - 2]  );
        }

        return minCost[L];
    }
}
```

---

# 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
<br/>Output: 4
<br/>Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
<br/>Output: 12
<br/>Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int rob(int[] nums) {
        int L = nums.length;

        if(L == 0) return 0;
        if ( L == 1) return nums[0];
        
        int[] dp = new int[L+1];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[1],nums[0]);

        for(int i=2 ; i<L ;i++){
            dp[i] = Math.max(dp[i-1] , dp[i-2] + nums[i]);
        }

        return dp[L - 1];
    }
}
```

---

# 740. Delete and Earn

You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times:

Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1.
Return the maximum number of points you can earn by applying the above operation some number of times.

 

Example 1:

Input: nums = [3,4,2]
<br/>Output: 6
<br/>Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.
Example 2:

Input: nums = [2,2,3,3,3,4]
<br/>Output: 9
<br/>Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int deleteAndEarn(int[] nums) {
        int[] arr = new int[100001];
        for(final int num:nums){
            arr[num] += num;
        }

        int prev1 = 0;
        int prev2 = 0;

        for(int num:arr){
            final int dp = Math.max(prev1,prev2 + num);
            prev2 = prev1;
            prev1 = dp;
        }

        return prev1;
    }
}
```

---

# 62. Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

 

Example 1:


Input: m = 3, n = 7
<br/>Output: 28
Example 2:

Input: m = 3, n = 2
<br/>Output: 3
<br/>Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] arr = new int[m][n];
        Arrays.stream(arr).forEach( A -> Arrays.fill(A,1));

        for(int i=1 ; i<m ; i++){
            for(int j=1 ; j<n ; j++){
                arr[i][j] = arr[i-1][j] + arr[i][j-1];
            }
        }

        return arr[m-1][n-1];
    }
}
```

---

# 64. Minimum Path Sum
Solved
Medium
Topics
Companies
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

 

Example 1:


Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
<br/>Output: 7
<br/>Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
Example 2:

Input: grid = [[1,2,3],[4,5,6]]
<br/>Output: 12

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int minPathSum(int[][] grid) {
        final int m = grid.length;
        final int n = grid[0].length;
        
        for(int i=0 ; i< m ; i++){
            for(int j=0 ; j< n ; j++){
                if( i > 0 && j > 0){
                    grid[i][j] += Math.min(grid[i-1][j] , grid[i][j-1]);
                }
                else if( j > 0 ){
                    grid[0][j] += grid[0][j-1];
                }
                else if( i > 0 ){
                    grid[i][0] += grid[i-1][0];
                }
            }
        }

        return grid[m-1][n-1];

    }
}
```

---

# 63. Unique Paths II
Solved
Medium
Topics
Companies
Hint
You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.

 

Example 1:


Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
<br/>Output: 2
<br/>Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
Example 2:


Input: obstacleGrid = [[0,1],[0,0]]
<br/>Output: 1

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        final int m = obstacleGrid.length;
        final int n = obstacleGrid[0].length;

        int[][] arr = new int[m+1][n+1];
        arr[0][1] = 1;

        for(int i=1 ; i<=m ; i++){
            for(int j=1 ; j<=n ; j++){
                if(obstacleGrid[i -1][j - 1] == 0){
                    arr[i][j] = arr[i-1][j] + arr[i][j-1];
                }
            }
        }

        return arr[m][n];

    }
}
```

---

# 9. Palindrome Number

Given an integer x, return true if x is a 
palindrome
, and false otherwise.

 

Example 1:

Input: x = 121
<br/>Output: true
<br/>Explanation: 121 reads as 121 from left to right and from right to left.
Example 2:

Input: x = -121
<br/>Output: false
<br/>Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
Example 3:

Input: x = 10
<br/>Output: false
<br/>Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public boolean isPalindrome(int x) {
        
        int d = x;

        int sum = 0;
        while(d>0){
            int rem = d%10;
            sum = (sum * 10) + rem;
            d/=10;
        }

        if(sum == x)
            return true;
        
        return false;
    }
}
```

---

# 88. Merge Sorted Array

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

 

Example 1:

Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
<br/>Output: [1,2,2,3,5,6]
<br/>Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
Example 2:

Input: nums1 = [1], m = 1, nums2 = [], n = 0
<br/>Output: [1]
<br/>Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
Example 3:

Input: nums1 = [0], m = 0, nums2 = [1], n = 1
<br/>Output: [1]
<br/>Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1 ;
    int j = n - 1 ;
    int k = m + n - 1 ;

    while(j>=0){
        if( i >= 0 && nums1[i] > nums2[j])
            nums1[k--] = nums1[i--];
        else
            nums1[k--] = nums2[j--];
    }
  }
}
```

---

# 1726. Tuple with Same Product

Given an array nums of distinct positive integers, return the number of tuples (a, b, c, d) such that a * b = c * d where a, b, c, and d are elements of nums, and a != b != c != d.

 

Example 1:

Input: nums = [2,3,4,6]
<br/>Output: 8
<br/>Explanation: There are 8 valid tuples:
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)
Example 2:

Input: nums = [1,2,4,5,10]
<br/>Output: 16
<br/>Explanation: There are 16 valid tuples:
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,5,4)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
  public int tupleSameProduct(int[] nums) {
    int ans = 0;
    Map <Integer,Integer> count = new HashMap<>();

    for(int i=0 ; i<nums.length ; i++){
        for(int j=0 ; j < i ; j++){
            final int pod = nums[i] * nums[j];
            ans += count.getOrDefault(pod,0) * 8 ;
            count.merge(pod,1,Integer::sum);
        }
    }

    return ans;
  }
}
```

---

# 3160. Find the Number of Distinct Colors Among the Balls
Solved
Medium
Topics
Companies
Hint
You are given an integer limit and a 2D array queries of size n x 2.

There are limit + 1 balls with distinct labels in the range [0, limit]. Initially, all balls are uncolored. For every query in queries that is of the form [x, y], you mark ball x with the color y. After each query, you need to find the number of distinct colors among the balls.

Return an array result of length n, where result[i] denotes the number of distinct colors after ith query.

Note that when answering a query, lack of a color will not be considered as a color.

 

Example 1:

Input: limit = 4, queries = [[1,4],[2,5],[1,3],[3,4]]

<br/>Output: [1,2,2,3]

<br/>Explanation:



After query 0, ball 1 has color 4.
After query 1, ball 1 has color 4, and ball 2 has color 5.
After query 2, ball 1 has color 3, and ball 2 has color 5.
After query 3, ball 1 has color 3, ball 2 has color 5, and ball 3 has color 4.
Example 2:

Input: limit = 4, queries = [[0,1],[1,2],[2,2],[3,4],[4,5]]

<br/>Output: [1,2,2,3,4]

<br/>Explanation:



After query 0, ball 0 has color 1.
After query 1, ball 0 has color 1, and ball 1 has color 2.
After query 2, ball 0 has color 1, and balls 1 and 2 have color 2.
After query 3, ball 0 has color 1, balls 1 and 2 have color 2, and ball 3 has color 4.
After query 4, ball 0 has color 1, balls 1 and 2 have color 2, ball 3 has color 4, and ball 4 has color 5.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
  public int[] queryResults(int limit, int[][] queries) {
    int[] ans = new int[queries.length];
    Map<Integer, Integer> ballToColor = new HashMap<>();
    Map<Integer, Integer> colorCount = new HashMap<>();

    for (int i = 0; i < queries.length; ++i) {
      final int ball = queries[i][0];
      final int color = queries[i][1];
      if (ballToColor.containsKey(ball)) {
        final int prevColor = ballToColor.get(ball);
        if (colorCount.merge(prevColor, -1, Integer::sum) == 0)
          colorCount.remove(prevColor);
      }
      ballToColor.put(ball, color);
      colorCount.merge(color, 1, Integer::sum);
      ans[i] = colorCount.size();
    }

    return ans;
  }
}
```
---

# 27. Remove Element

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

Example 1:

Input: nums = [3,2,2,3], val = 3
<br/>Output: 2, nums = [2,2,_,_]
<br/>Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
<br/>Output: 5, nums = [0,1,4,0,3,_,_,_]
<br/>Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
 

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public int removeElement(int[] nums, int val) {
        int j=0;
        for(int i=0 ; i < nums.length ; i++){
            if(nums[i] == val){
                continue;
            }
            nums[j++] = nums[i];
        }
        return j;
    }
}
```
---

# 2349. Design a Number Container System

Design a number container system that can do the following:

Insert or Replace a number at the given index in the system.
Return the smallest index for the given number in the system.
Implement the NumberContainers class:

NumberContainers() Initializes the number container system.
void change(int index, int number) Fills the container at index with the number. If there is already a number at that index, replace it.
int find(int number) Returns the smallest index for the given number, or -1 if there is no index that is filled by number in the system.
 

Example 1:

Input
["NumberContainers", "find", "change", "change", "change", "change", "find", "change", "find"]
[[], [10], [2, 10], [1, 10], [3, 10], [5, 10], [10], [1, 20], [10]]
<br/>Output
[null, -1, null, null, null, null, 1, null, 2]

<br/>Explanation
NumberContainers nc = new NumberContainers();
nc.find(10); // There is no index that is filled with number 10. Therefore, we return -1.
nc.change(2, 10); // Your container at index 2 will be filled with number 10.
nc.change(1, 10); // Your container at index 1 will be filled with number 10.
nc.change(3, 10); // Your container at index 3 will be filled with number 10.
nc.change(5, 10); // Your container at index 5 will be filled with number 10.
nc.find(10); // Number 10 is at the indices 1, 2, 3, and 5. Since the smallest index that is filled with 10 is 1, we return 1.
nc.change(1, 20); // Your container at index 1 will be filled with number 20. Note that index 1 was filled with 10 and then replaced with 20. 
nc.find(10); // Number 10 is at the indices 2, 3, and 5. The smallest index that is filled with 10 is 2. Therefore, we return 2.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class NumberContainers {
  public void change(int index, int number) {
    if (indexToNumbers.containsKey(index)) {
      final int originalNumber = indexToNumbers.get(index);
      numberToIndices.get(originalNumber).remove(index);
      if (numberToIndices.get(originalNumber).isEmpty())
        numberToIndices.remove(originalNumber);
    }
    indexToNumbers.put(index, number);
    numberToIndices.putIfAbsent(number, new TreeSet<>());
    numberToIndices.get(number).add(index);
  }

  public int find(int number) {
    if (numberToIndices.containsKey(number))
      return numberToIndices.get(number).first();
    return -1;
  }

  private Map<Integer, TreeSet<Integer>> numberToIndices = new HashMap<>();
  private Map<Integer, Integer> indexToNumbers = new HashMap<>();
}
```

---

# 3151. Special Array I

An array is considered special if every pair of its adjacent elements contains two numbers with different parity.

You are given an array of integers nums. Return true if nums is a special array, otherwise, return false.

 

Example 1:

Input: nums = [1]

<br/>Output: true

<br/>Explanation:

There is only one element. So the answer is true.

Example 2:

Input: nums = [2,1,4]

<br/>Output: true

<br/>Explanation:

There is only two pairs: (2,1) and (1,4), and both of them contain numbers with different parity. So the answer is true.

Example 3:

Input: nums = [4,3,1,6]

<br/>Output: false

<br/>Explanation:

nums[1] and nums[2] are both odd. So the answer is false.

# Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public boolean isArraySpecial(int[] nums) {
        for(int i = 1 ; i<nums.length ; i++)
            if(nums[i]%2 == nums[i-1]%2)
                return false;
        return true;
    }
}
```

---

# 2364. Count Number of Bad Pairs

You are given a 0-indexed integer array nums. A pair of indices (i, j) is a bad pair if i < j and j - i != nums[j] - nums[i].

Return the total number of bad pairs in nums.

 

Example 1:

Input: nums = [4,1,3,3]
<br/>Output: 5
<br/>Explanation: The pair (0, 1) is a bad pair since 1 - 0 != 1 - 4.
The pair (0, 2) is a bad pair since 2 - 0 != 3 - 4, 2 != -1.
The pair (0, 3) is a bad pair since 3 - 0 != 3 - 4, 3 != -1.
The pair (1, 2) is a bad pair since 2 - 1 != 3 - 1, 1 != 2.
The pair (2, 3) is a bad pair since 3 - 2 != 3 - 3, 1 != 0.
There are a total of 5 bad pairs, so we return 5.
Example 2:

Input: nums = [1,2,3,4,5]
<br/>Output: 0
<br/>Explanation: There are no bad pairs.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```java []
class Solution {
    public long countBadPairs(int[] nums) {
        long ans = 0;

        Map<Integer,Long> count = new HashMap<>();

        for(int i=0 ; i<nums.length; i++){
            ans += i - count.getOrDefault(nums[i] - i , 0L);
            count.merge(nums[i] - i , 1L , Long::sum);
        }
        return ans;
    }
}
```
---

# 3174. Clear Digits
##### Easy
You are given a string s.

Your task is to remove all digits by doing this operation repeatedly:

Delete the first digit and the closest non-digit character to its left.
Return the resulting string after removing all digits.

 

Example 1:

Input: s = "abc"

<br/>Output: "abc"

<br/>Explanation:

There is no digit in the string.

Example 2:

Input: s = "cb34"

<br/>Output: ""

<br/>Explanation:

First, we apply the operation on s[2], and s becomes "c4".

Then we apply the operation on s[1], and s becomes "".

# Code
```java []
class Solution {
  public String clearDigits(String s) {
    StringBuilder sb = new StringBuilder();

    for(final char c:s.toCharArray()){
        if(Character.isDigit(c)){
            sb.setLength(sb.length()-1);
        }
        else{
            sb.append(c);
        }
    }

    return sb.toString();
  }
}
```

---

## leetcode
# 7. Reverse Integer

Medium

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

 

Example 1:

Input: x = 123
<br/>Output: 321
Example 2:

Input: x = -123
<br/>Output: -321
Example 3:

Input: x = 120
<br/>Output: 21
 

Constraints:

-231 <= x <= 231 - 1

# Code
```java []
class Solution {
    public int reverse(int x) {

        long ans = 0;
        while(x != 0){
            ans = (ans * 10) + (x % 10);
            x /= 10;
        }

        return (ans < Integer.MIN_VALUE || ans > Integer.MAX_VALUE) ? 0 : (int)ans;
    }
}
```

---

# 1910. Remove All Occurrences of a Substring
Solved
Medium

Given two strings s and part, perform the following operation on s until all occurrences of the substring part are removed:

Find the leftmost occurrence of the substring part and remove it from s.
Return s after removing all occurrences of part.

A substring is a contiguous sequence of characters in a string.

 

### Example 1:

Input: s = "daabcbaabcbc", part = "abc"
<br/>Output: "dab"
<br/>Explanation: The following operations are done:
- s = "daabcbaabcbc", remove "abc" starting at index 2, so s = "dabaabcbc".
- s = "dabaabcbc", remove "abc" starting at index 4, so s = "dababc".
- s = "dababc", remove "abc" starting at index 3, so s = "dab".
Now s has no occurrences of "abc".

### Example 2:

Input: s = "axxxxyyyyb", part = "xy"
<br/>Output: "ab"
<br/>Explanation: The following operations are done:
- s = "axxxxyyyyb", remove "xy" starting at index 4 so s = "axxxyyyb".
- s = "axxxyyyb", remove "xy" starting at index 3 so s = "axxyyb".
- s = "axxyyb", remove "xy" starting at index 2 so s = "axyb".
- s = "axyb", remove "xy" starting at index 1 so s = "ab".
Now s has no occurrences of "xy".
 

Constraints:

1 <= s.length <= 1000
1 <= part.length <= 1000
s​​​​​​ and part consists of lowercase English letters.

# Code
```java []
import java.util.regex.Pattern;
import java.util.regex.Matcher;
class Solution {
    public String removeOccurrences(String s, String part) {
        StringBuilder ss = new StringBuilder(s);
        while(ss.indexOf(part) != -1){
            int idx = ss.indexOf(part);
            ss.delete(idx,idx + part.length());
        }
        return ss.toString();
    }
}
```
---

# 125. Valid Palindrome
Solved
Easy

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

 

### Example 1:

Input: s = "A man, a plan, a canal: Panama"
<br/>Output: true
<br/>Explanation: "amanaplanacanalpanama" is a palindrome.

### Example 2:

Input: s = "race a car"
<br/>Output: false
<br/>Explanation: "raceacar" is not a palindrome.

### Example 3:

Input: s = " "
<br/>Output: true
<br/>Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
 

Constraints:

1 <= s.length <= 2 * 105
s consists only of printable ASCII characters.

# Code
```java []
class Solution {
    public boolean isPalindrome(String s) {
        int st = 0 ; int end = s.length() -1;
        while(st < end){
            while(st < end && !Character.isLetterOrDigit(s.charAt(st))){
                ++st;
            }
            while(st < end && !Character.isLetterOrDigit(s.charAt(end))){
                --end;
            }
            if( Character.toLowerCase(s.charAt(st)) != Character.toLowerCase(s.charAt(end)) ){
                return false;
            }
            ++st;
            --end;
        }

        return true;
    }
}
```
---
# 2342. Max Sum of a Pair With Equal Sum of Digits
Solved
Medium
Topics
Companies
Hint
You are given a 0-indexed array nums consisting of positive integers. You can choose two indices i and j, such that i != j, and the sum of digits of the number nums[i] is equal to that of nums[j].

Return the maximum value of nums[i] + nums[j] that you can obtain over all possible indices i and j that satisfy the conditions.

 

### Example 1:

Input: nums = [18,43,36,13,7]
<br/>Output: 54
<br/>Explanation: The pairs (i, j) that satisfy the conditions are:
- (0, 2), both numbers have a sum of digits equal to 9, and their sum is 18 + 36 = 54.
- (1, 4), both numbers have a sum of digits equal to 7, and their sum is 43 + 7 = 50.
So the maximum sum that we can obtain is 54.

### Example 2:

Input: nums = [10,12,19,14]
<br/>Output: -1
<br/>Explanation: There are no two numbers that satisfy the conditions, so we return -1.
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109

# Code
```java []
class Solution {
  public int maximumSum(int[] nums) {
    final int kMax = 9 * 9; // 999,999,999
    int ans = -1;
    List<Integer>[] count = new List[kMax + 1];

    for (int i = 0; i <= kMax; ++i)
      count[i] = new ArrayList<>();

    for (final int num : nums)
      count[getDigitSum(num)].add(num);

    for (List<Integer> groupNums : count) {
      if (groupNums.size() < 2)
        continue;
      Collections.sort(groupNums, Collections.reverseOrder());
      ans = Math.max(ans, groupNums.get(0) + groupNums.get(1));
    }

    return ans;
  }

  private int getDigitSum(int num) {
    int digitSum = 0;
    while (num > 0) {
      digitSum += num % 10;
      num /= 10;
    }
    return digitSum;
  }
}
```
---
# 169. Majority Element
Solved
Easy

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 

### Example 1:

Input: nums = [3,2,3]
<br/>Output: 3
Example 2:

Input: nums = [2,2,1,1,1,2,2]
<br/>Output: 2
 

Constraints:

n == nums.length
1 <= n <= 5 * 104
-109 <= nums[i] <= 109

# Code
```java []
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;

        Map<Integer,Integer> sumOfNo = new HashMap<>();

        for(int i=0 ; i<n ; ++i){
            sumOfNo.merge(nums[i],1,Integer::sum);
        }
        
        for(Map.Entry<Integer,Integer> aa : sumOfNo.entrySet()){
            if(aa.getValue() > (n/2)){
                return aa.getKey();
            }
        }

        return 0;

    }
}
```
---
# 26. Remove Duplicates from Sorted Array
Solved
Easy

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.
Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

### Example 1:

Input: nums = [1,1,2]
<br/>Output: 2, nums = [1,2,_]
<br/>Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

### Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]
<br/>Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
<br/>Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
 

Constraints:

1 <= nums.length <= 3 * 104
-100 <= nums[i] <= 100
nums is sorted in non-decreasing order.

# Code
```java []
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int j = 1 ;
        for(int i=1 ; i<n ; ++i){
            if(nums[i] == nums[i-1]) continue;
            nums[j++] = nums[i];
        }
    
        
        return j;
    }
}
```
---
# 80. Remove Duplicates from Sorted Array II
Solved
Medium

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

Custom Judge:

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.

 

### Example 1:

Input: nums = [1,1,1,2,2,3]
<br/>Output: 5, nums = [1,1,2,2,3,_]
<br/>Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

### Example 2:

Input: nums = [0,0,1,1,1,1,2,3,3]
<br/>Output: 7, nums = [0,0,1,1,2,3,3,_,_]
<br/>Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
 

Constraints:

1 <= nums.length <= 3 * 104
-104 <= nums[i] <= 104
nums is sorted in non-decreasing order.

# Code
```java []
class Solution {
    public int removeDuplicates(int[] nums) {
        int k = 2;
        for (int i = 2; i < nums.length; i++) {
            if( nums[i] != nums[k-2]){
                nums[k++] = nums[i];
            }
        }
        return k;
    }
}
```
---
# 3066. Minimum Operations to Exceed Threshold Value II
Solved
Medium

You are given a 0-indexed integer array nums, and an integer k.

In one operation, you will:

Take the two smallest integers x and y in nums.
Remove x and y from nums.
Add min(x, y) * 2 + max(x, y) anywhere in the array.
Note that you can only apply the described operation if nums contains at least two elements.

Return the minimum number of operations needed so that all elements of the array are greater than or equal to k.

 

### Example 1:

Input: nums = [2,11,10,1,3], k = 10
<br/>Output: 2
<br/>Explanation: In the first operation, we remove elements 1 and 2, then add 1 * 2 + 2 to nums. nums becomes equal to [4, 11, 10, 3].
In the second operation, we remove elements 3 and 4, then add 3 * 2 + 4 to nums. nums becomes equal to [10, 11, 10].
At this stage, all the elements of nums are greater than or equal to 10 so we can stop.
It can be shown that 2 is the minimum number of operations needed so that all elements of the array are greater than or equal to 10.

### Example 2:

Input: nums = [1,1,2,4,9], k = 20
<br/>Output: 4
<br/>Explanation: After one operation, nums becomes equal to [2, 4, 9, 3].
After two operations, nums becomes equal to [7, 4, 9].
After three operations, nums becomes equal to [15, 9].
After four operations, nums becomes equal to [33].
At this stage, all the elements of nums are greater than 20 so we can stop.
It can be shown that 4 is the minimum number of operations needed so that all elements of the array are greater than or equal to 20.
 

Constraints:

2 <= nums.length <= 2 * 105
1 <= nums[i] <= 109
1 <= k <= 109
The input is generated such that an answer always exists. That is, there exists some sequence of operations after which all elements of the array are greater than or equal to k.

# Code
```java []
class Solution {
  public int minOperations(int[] nums, int k) {
    int ans = 0;
    Queue<Long> minHeap = new PriorityQueue<>();

    for (final int num : nums)
      minHeap.add((long) num);

    while (minHeap.size() > 1 && minHeap.peek() < k) {
      final long x = minHeap.poll();
      final long y = minHeap.poll();
      minHeap.add(Math.min(x, y) * 2 + Math.max(x, y));
      ++ans;
    }

    return ans;
  }
}
```
---
# 1352. Product of the Last K Numbers
Solved
Medium

Design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream.

Implement the ProductOfNumbers class:

ProductOfNumbers() Initializes the object with an empty stream.
void add(int num) Appends the integer num to the stream.
int getProduct(int k) Returns the product of the last k numbers in the current list. You can assume that always the current list has at least k numbers.
The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

 

### Example:

Input
["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

<br/>Output
[null,null,null,null,null,null,20,40,0,null,32]

##### <br/>Explanation
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 
 

Constraints:

0 <= num <= 100
1 <= k <= 4 * 104
At most 4 * 104 calls will be made to add and getProduct.
The product of the stream at any point in time will fit in a 32-bit integer.

# Code
```java []
class ProductOfNumbers {
  public ProductOfNumbers() {
    prefix = new ArrayList<>(List.of(1));
  }

  public void add(int num) {
    if (num == 0)
      prefix = new ArrayList<>(List.of(1));
    else
      prefix.add(prefix.get(prefix.size() - 1) * num);
  }

  public int getProduct(int k) {
    return k >= prefix.size() ? 0
                              : prefix.get(prefix.size() - 1) / prefix.get(prefix.size() - k - 1);
  }

  private List<Integer> prefix = new ArrayList<>();
}
```
---
# 2698. Find the Punishment Number of an Integer
Solved
Medium

Given a positive integer n, return the punishment number of n.

The punishment number of n is defined as the sum of the squares of all integers i such that:

1 <= i <= n
The decimal representation of i * i can be partitioned into contiguous substrings such that the sum of the integer values of these substrings equals i.
 

#### Example 1:

Input: n = 10
<br/>Output: 182
<br/>Explanation: There are exactly 3 integers i in the range [1, 10] that satisfy the conditions in the statement:
- 1 since 1 * 1 = 1
- 9 since 9 * 9 = 81 and 81 can be partitioned into 8 and 1 with a sum equal to 8 + 1 == 9.
- 10 since 10 * 10 = 100 and 100 can be partitioned into 10 and 0 with a sum equal to 10 + 0 == 10.
Hence, the punishment number of 10 is 1 + 81 + 100 = 182

#### Example 2:

Input: n = 37
<br/>Output: 1478
<br/>Explanation: There are exactly 4 integers i in the range [1, 37] that satisfy the conditions in the statement:
- 1 since 1 * 1 = 1. 
- 9 since 9 * 9 = 81 and 81 can be partitioned into 8 + 1. 
- 10 since 10 * 10 = 100 and 100 can be partitioned into 10 + 0. 
- 36 since 36 * 36 = 1296 and 1296 can be partitioned into 1 + 29 + 6.
Hence, the punishment number of 37 is 1 + 81 + 100 + 1296 = 1478
 

Constraints:

1 <= n <= 1000

# Code
```java []
class Solution {
  public int punishmentNumber(int n) {
    int ans = 0;
    for (int i = 1; i <= n; ++i)
      if (isPossible(0, 0, String.valueOf(i * i), 0, i))
        ans += i * i;

    return ans;
  }

  // Returns true if the sum of any split of `numChars` equals to the target.
  private boolean isPossible(int accumulate, int running, String numChars, int s, int target) {
    if (s == numChars.length())
      return target == accumulate + running;
    final int d = numChars.charAt(s) - '0';
    return isPossible(accumulate, running * 10 + d, numChars, s + 1, target) ||
        isPossible(accumulate + running, d, numChars, s + 1, target);
  }
}
```
---
# 1718. Construct the Lexicographically Largest Valid Sequence
Solved
Medium

Given an integer n, find a sequence that satisfies all of the following:

The integer 1 occurs once in the sequence.
Each integer between 2 and n occurs twice in the sequence.
For every integer i between 2 and n, the distance between the two occurrences of i is exactly i.
The distance between two numbers on the sequence, a[i] and a[j], is the absolute difference of their indices, |j - i|.

Return the lexicographically largest sequence. It is guaranteed that under the given constraints, there is always a solution.

A sequence a is lexicographically larger than a sequence b (of the same length) if in the first position where a and b differ, sequence a has a number greater than the corresponding number in b. For example, [0,1,9,0] is lexicographically larger than [0,1,5,6] because the first position they differ is at the third number, and 9 is greater than 5.

 

#### Example 1:

Input: n = 3 
<br/>Output: [3,1,2,3,2] 

<br/>Explanation: [2,3,2,1,3] is also a valid sequence, but [3,1,2,3,2] is the lexicographically largest valid sequence.

#### Example 2:

Input: n = 5
<br/>Output: [5,3,1,4,3,5,2,4,2]
 

Constraints:

1 <= n <= 20

# Code
```java []
class Solution {
  public int[] constructDistancedSequence(int n) {
    int[] ans = new int[2 * n - 1];
    dfs(n, 0, 0, ans);
    return ans;
  }

  private boolean dfs(int n, int i, int mask, int[] ans) {
    if (i == ans.length)
      return true;
    if (ans[i] > 0)
      return dfs(n, i + 1, mask, ans);

    // Greedily fill in `ans` in descending order.
    for (int num = n; num >= 1; --num) {
      if ((mask >> num & 1) == 1)
        continue;
      if (num == 1) {
        ans[i] = num;
        if (dfs(n, i + 1, mask | 1 << num, ans))
          return true;
        ans[i] = 0;
      } else { // num in [2, n]
        if (i + num >= ans.length || ans[i + num] > 0)
          continue;
        ans[i] = num;
        ans[i + num] = num;
        if (dfs(n, i + 1, mask | 1 << num, ans))
          return true;
        ans[i + num] = 0;
        ans[i] = 0;
      }
    }

    return false;
  }
}
```
---
# 1079. Letter Tile Possibilities
Solved
Medium

You have n  tiles, where each tile has one letter tiles[i] printed on it.

Return the number of possible non-empty sequences of letters you can make using the letters printed on those tiles.

 

### Example 1:

Input: tiles = "AAB"
<br/>Output: 8
<br/>Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

### Example 2:

Input: tiles = "AAABBC"
<br/>Output: 188
<br/>Example 3:

<br/>Input: tiles = "V"
<br/>Output: 1
 

Constraints:

1 <= tiles.length <= 7 <br/>
tiles consists of uppercase English letters.

# Code
```java []
class Solution {
  public int numTilePossibilities(String tiles) {
    int[] count = new int[26];

    for (final char t : tiles.toCharArray())
      ++count[t - 'A'];

    return dfs(count);
  }

  private int dfs(int[] count) {
    int possibleSequences = 0;

    for (int i = 0; i < 26; ++i) {
      if (count[i] == 0)
        continue;
      // Put c in the current position. We only care about the number of possible
      // sequences of letters but don't care about the actual combination.
      --count[i];
      possibleSequences += 1 + dfs(count);
      ++count[i];
    }

    return possibleSequences;
  }
}
```
---
# 2375. Construct Smallest Number From DI String
Solved
Medium

You are given a 0-indexed string pattern of length n consisting of the characters 'I' meaning increasing and 'D' meaning decreasing.

A 0-indexed string num of length n + 1 is created using the following conditions:

num consists of the digits '1' to '9', where each digit is used at most once.
If pattern[i] == 'I', then num[i] < num[i + 1].
If pattern[i] == 'D', then num[i] > num[i + 1].
Return the lexicographically smallest possible string num that meets the conditions.

 

#### Example 1:

Input: pattern = "IIIDIDDD"<br/>
Output: "123549876"<br/>
Explanation:
At indices 0, 1, 2, and 4 we must have that num[i] < num[i+1].
At indices 3, 5, 6, and 7 we must have that num[i] > num[i+1].
Some possible values of num are "245639871", "135749862", and "123849765".
It can be proven that "123549876" is the smallest possible num that meets the conditions.
Note that "123414321" is not possible because the digit '1' is used more than once.

### Example 2:

Input: pattern = "DDD"
<br/>Output: "4321"
<br/>Explanation:
Some possible values of num are "9876", "7321", and "8742".
It can be proven that "4321" is the smallest possible num that meets the conditions.
 

Constraints:

1 <= pattern.length <= 8
pattern consists of only the letters 'I' and 'D'.

# Code
```java []
class Solution {
  public String smallestNumber(String pattern) {
    StringBuilder sb = new StringBuilder();
    Deque<Character> stack = new ArrayDeque<>(List.of('1'));

    for (final char c : pattern.toCharArray()) {
      char maxSorFar = stack.peek();
      if (c == 'I')
        while (!stack.isEmpty()) {
          maxSorFar = (char) Math.max(maxSorFar, stack.peek());
          sb.append(stack.pop());
        }
      stack.push((char) (maxSorFar + 1));
    }

    while (!stack.isEmpty())
      sb.append(stack.pop());

    return sb.toString();
  }
}
```
---
# 1415. The k-th Lexicographical String of All Happy Strings of Length n
Solved
Medium

A happy string is a string that:

consists only of letters of the set ['a', 'b', 'c'].
s[i] != s[i + 1] for all values of i from 1 to s.length - 1 (string is 1-indexed).
For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers n and k, consider a list of all happy strings of length n sorted in lexicographical order.

Return the kth string of this list or return an empty string if there are less than k happy strings of length n.

 

### Example 1:

Input: n = 1, k = 3 <br/>
Output: "c"<br/>
Explanation: The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".

### Example 2:

Input: n = 1, k = 4 <br/>
Output: "" <br/>
Explanation: There are only 3 happy strings of length 1.

### Example 3:

Input: n = 3, k = 9 <br/>
Output: "cab" <br/>
Explanation: There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9th string = "cab"
 

Constraints:

1 <= n <= 10
1 <= k <= 100

# Code
```java []
class Solution {
  public String getHappyString(int n, int k) {
    Map<Character, String> nextLetters = Map.of('a', "bc", 'b', "ac", 'c', "ab");
    Queue<String> q = new ArrayDeque<>(List.of("a", "b", "c"));

    while (q.peek().length() != n) {
      final String u = q.poll();
      for (final char nextLetter : nextLetters.get(u.charAt(u.length() - 1)).toCharArray())
        q.offer(u + nextLetter);
    }

    if (q.size() < k)
      return "";

    for (int i = 0; i < k - 1; ++i)
      q.poll();
    return q.poll();
  }
}
```
---
# 1980. Find Unique Binary String
Solved
Medium

Given an array of strings nums containing n unique binary strings each of length n, return a binary string of length n that does not appear in nums. If there are multiple answers, you may return any of them.

 

### Example 1:

Input: nums = ["01","10"] <br/>
Output: "11" <br/>
Explanation: "11" does not appear in nums. "00" would also be correct.

### Example 2:

Input: nums = ["00","01"] <br/>
Output: "11" <br/>
Explanation: "11" does not appear in nums. "10" would also be correct.

### Example 3:

Input: nums = ["111","011","001"] <br/>
Output: "101" <br/>
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
 

Constraints:

n == nums.length
1 <= n <= 16
nums[i].length == n
nums[i] is either '0' or '1'.
All the strings of nums are unique.

# Code
```java []
class Solution {
  public String findDifferentBinaryString(String[] nums) {
    final int bitSize = nums[0].length();
    final int maxNum = 1 << bitSize;
    Set<Integer> numsSet = Arrays.stream(nums)
                               .mapToInt(num -> Integer.parseInt(num, 2))
                               .boxed()
                               .collect(Collectors.toSet());

    for (int num = 0; num < maxNum; ++num)
      if (!numsSet.contains(num))
        return String.format("%" + bitSize + "s", Integer.toBinaryString(num)).replace(' ', '0');

    throw new IllegalArgumentException();
  }
}
```
---
# 1261. Find Elements in a Contaminated Binary Tree
Solved
Medium

Given a binary tree with the following rules:

root.val == 0
For any treeNode:
If treeNode.val has a value x and treeNode.left != null, then treeNode.left.val == 2 * x + 1
If treeNode.val has a value x and treeNode.right != null, then treeNode.right.val == 2 * x + 2
Now the binary tree is contaminated, which means all treeNode.val have been changed to -1.

Implement the FindElements class:

FindElements(TreeNode* root) Initializes the object with a contaminated binary tree and recovers it.
bool find(int target) Returns true if the target value exists in the recovered binary tree.
 

#### Example 1:


Input
["FindElements","find","find"] <br/>
[[[-1,null,-1]],[1],[2]] <br/>
Output <br/>
[null,false,true] <br/>
Explanation <br/>
FindElements findElements = new FindElements([-1,null,-1]); 
 <br/>findElements.find(1); // return False 
 <br/>findElements.find(2); // return True 

#### Example 2:


Input <br/>
["FindElements","find","find","find"] <br/>
[[[-1,-1,-1,-1,-1]],[1],[3],[5]] <br/>
Output <br/>
[null,true,true,false] <br/>
Explanation <br/>
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]); <br/>
findElements.find(1); // return True <br/>
findElements.find(3); // return True <br/>
findElements.find(5); // return False <br/>

### Example 3:


Input <br/>
["FindElements","find","find","find","find"] <br/>
[[[-1,null,-1,-1,null,-1]],[2],[3],[4],[5]] <br/>
Output <br/>
[null,true,false,false,true] <br/>
Explanation <br/>
FindElements findElements = new FindElements([-1,null,-1,-1,null,-1]); <br/>
findElements.find(2); // return True <br/>
findElements.find(3); // return False <br/>
findElements.find(4); // return False <br/>
findElements.find(5); // return True <br/>
 

Constraints:

TreeNode.val == -1
The height of the binary tree is less than or equal to 20
The total number of nodes is between [1, 104]
Total calls of find() is between [1, 104]
0 <= target <= 106

# Code
```java []
class FindElements {
  public FindElements(TreeNode root) {
    dfs(root, 0);
  }

  public boolean find(int target) {
    return vals.contains(target);
  }

  private Set<Integer> vals = new HashSet<>();

  private void dfs(TreeNode root, int val) {
    if (root == null)
      return;

    root.val = val;
    vals.add(val);
    dfs(root.left, val * 2 + 1);
    dfs(root.right, val * 2 + 2);
  }
}
```
---
# 889. Construct Binary Tree from Preorder and Postorder Traversal
Solved
Medium

Given two integer arrays, preorder and postorder where preorder is the preorder traversal of a binary tree of distinct values and postorder is the postorder traversal of the same tree, reconstruct and return the binary tree.

If there exist multiple answers, you can return any of them.

 

#### Example 1:


Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1] <br/>
Output: [1,2,3,4,5,6,7] <br/>

#### Example 2:

Input: preorder = [1], postorder = [1] <br/>
Output: [1] <br/>
 

Constraints:

1 <= preorder.length <= 30
1 <= preorder[i] <= preorder.length
All the values of preorder are unique.
postorder.length == preorder.length
1 <= postorder[i] <= postorder.length
All the values of postorder are unique.
It is guaranteed that preorder and postorder are the preorder traversal and postorder traversal of the same binary tree.

# Code
```java []
class Solution {
  public TreeNode constructFromPrePost(int[] pre, int[] post) {
    Map<Integer, Integer> postToIndex = new HashMap<>();

    for (int i = 0; i < post.length; ++i)
      postToIndex.put(post[i], i);

    return build(pre, 0, pre.length - 1, post, 0, post.length - 1, postToIndex);
  }

  private TreeNode build(int[] pre, int preStart, int preEnd, int[] post, int postStart,
                         int postEnd, Map<Integer, Integer> postToIndex) {
    if (preStart > preEnd)
      return null;
    if (preStart == preEnd)
      return new TreeNode(pre[preStart]);

    final int rootVal = pre[preStart];
    final int leftRootVal = pre[preStart + 1];
    final int leftRootPostIndex = postToIndex.get(leftRootVal);
    final int leftSize = leftRootPostIndex - postStart + 1;

    TreeNode root = new TreeNode(rootVal);
    root.left = build(pre, preStart + 1, preStart + leftSize, post, postStart, leftRootPostIndex,
                      postToIndex);
    root.right = build(pre, preStart + leftSize + 1, preEnd, post, leftRootPostIndex + 1,
                       postEnd - 1, postToIndex);
    return root;
  }
}
```
---
# 1028. Recover a Tree From Preorder Traversal
Solved
Hard

We run a preorder depth-first search (DFS) on the root of a binary tree.

At each node in this traversal, we output D dashes (where D is the depth of this node), then we output the value of this node.  If the depth of a node is D, the depth of its immediate child is D + 1.  The depth of the root node is 0.

If a node has only one child, that child is guaranteed to be the left child.

Given the output traversal of this traversal, recover the tree and return its root.

 

#### Example 1:


Input: traversal = "1-2--3--4-5--6--7" <br/>
Output: [1,2,5,3,4,6,7] <br/>

#### Example 2:


Input: traversal = "1-2--3---4-5--6---7" <br/>
Output: [1,2,5,3,null,6,null,4,null,7] <br/>

#### Example 3:


Input: traversal = "1-401--349---90--88" <br/>
Output: [1,401,null,349,88,90] <br/>
 

Constraints:

The number of nodes in the original tree is in the range [1, 1000].
1 <= Node.val <= 109

# Code
```java []
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 }
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left =inla left;
 *         this.right = right;
 *     }
 * }
 */


class Solution {
      public TreeNode recoverFromPreorder(String traversal) {
          return recoverFromPreorder(traversal, 0);
            }

              private int i = 0;

                private TreeNode recoverFromPreorder(final String traversal, int depth) {
                    int nDashes = 0;
                        while (i + nDashes < traversal.length() && traversal.charAt(i + nDashes) == '-')
                              ++nDashes;
                                  if (nDashes != depth)
                                        return null;

                                            i += depth;
                                                final int start = i;
                                                    while (i < traversal.length() && Character.isDigit(traversal.charAt(i)))
                                                          ++i;

                                                              return new TreeNode(Integer.valueOf(traversal.substring(start, i)),
                                                                                      recoverFromPreorder(traversal, depth + 1),
                                                                                                              recoverFromPreorder(traversal, depth + 1));
                                                                                                                }
                                                                                                                }

```
---


