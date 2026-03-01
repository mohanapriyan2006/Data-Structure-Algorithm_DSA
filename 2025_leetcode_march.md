# ***leetcode_problems - (march-2025)***

----

# 1749. Maximum Absolute Sum of Any Subarray
Solved
Medium

You are given an integer array nums. The absolute sum of a subarray [numsl, numsl+1, ..., numsr-1, numsr] is abs(numsl + numsl+1 + ... + numsr-1 + numsr).

Return the maximum absolute sum of any (possibly empty) subarray of nums.

Note that abs(x) is defined as follows:

If x is a negative integer, then abs(x) = -x.
If x is a non-negative integer, then abs(x) = x.
 

#### Example 1:

Input: nums = [1,-3,2,3,-4] <br/>
Output: 5 <br/>
Explanation: The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5. <br/>

#### Example 2:

Input: nums = [2,-5,1,-4,3,-2] <br/>
Output: 8 <br/>
Explanation: The subarray [-5,1,-4] has absolute sum = abs(-5+1-4) = abs(-8) = 8.
 

Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104

# Code
```java []
class Solution {
  public int maxAbsoluteSum(int[] nums) {
    int ans = Integer.MIN_VALUE;
    int maxSum = 0;
    int minSum = 0;

    for (final int num : nums) {
      maxSum = Math.max(num, maxSum + num);
      minSum = Math.min(num, minSum + num);
      ans = Math.max(ans, Math.max(maxSum, -minSum));
    }

    return ans;
  }
}
```
---
# 873. Length of Longest Fibonacci Subsequence
Solved
Medium

A sequence x1, x2, ..., xn is Fibonacci-like if:

n >= 3
xi + xi+1 == xi+2 for all i + 2 <= n
Given a strictly increasing array arr of positive integers forming a sequence, return the length of the longest Fibonacci-like subsequence of arr. If one does not exist, return 0.

A subsequence is derived from another sequence arr by deleting any number of elements (including none) from arr, without changing the order of the remaining elements. For example, [3, 5, 8] is a subsequence of [3, 4, 5, 6, 7, 8].

 

#### Example 1:

Input: arr = [1,2,3,4,5,6,7,8] <br/>
Output: 5 <br/>
Explanation: The longest subsequence that is fibonacci-like: [1,2,3,5,8]. <br/>

#### Example 2:

Input: arr = [1,3,7,11,12,14,18] <br/>
Output: 3 <br/>
Explanation: The longest subsequence that is fibonacci-like: [1,11,12], [3,11,14] or [7,11,18]. <br/>
 

Constraints:

3 <= arr.length <= 1000
1 <= arr[i] < arr[i + 1] <= 109

# Code
```java []
class Solution {
  public int lenLongestFibSubseq(int[] arr) {
    final int n = arr.length;
    int ans = 0;
    int[][] dp = new int[n][n];
    Arrays.stream(dp).forEach(A -> Arrays.fill(A, 2));
    Map<Integer, Integer> numToIndex = new HashMap<>();

    for (int i = 0; i < n; ++i)
      numToIndex.put(arr[i], i);

    for (int j = 0; j < n; ++j)
      for (int k = j + 1; k < n; ++k) {
        final int ai = arr[k] - arr[j];
        if (ai < arr[j] && numToIndex.containsKey(ai)) {
          final int i = numToIndex.get(ai);
          dp[j][k] = dp[i][j] + 1;
          ans = Math.max(ans, dp[j][k]);
        }
      }

    return ans;
  }
}
```
---
# 2570. Merge Two 2D Arrays by Summing Values
Solved
Easy

You are given two 2D integer arrays nums1 and nums2.

nums1[i] = [idi, vali] indicate that the number with the id idi has a value equal to vali.
nums2[i] = [idi, vali] indicate that the number with the id idi has a value equal to vali.
Each array contains unique ids and is sorted in ascending order by id.

Merge the two arrays into one array that is sorted in ascending order by id, respecting the following conditions:

Only ids that appear in at least one of the two arrays should be included in the resulting array.
Each id should be included only once and its value should be the sum of the values of this id in the two arrays. If the id does not exist in one of the two arrays, then assume its value in that array to be 0.
Return the resulting array. The returned array must be sorted in ascending order by id.

 

#### Example 1:

Input: nums1 = [[1,2],[2,3],[4,5]], nums2 = [[1,4],[3,2],[4,1]] <br/>
Output: [[1,6],[2,3],[3,2],[4,6]] <br/>
Explanation: The resulting array contains the following: <br/>
- id = 1, the value of this id is 2 + 4 = 6. <br/>
- id = 2, the value of this id is 3. <br/>
- id = 3, the value of this id is 2. <br/>
- id = 4, the value of this id is 5 + 1 = 6. <br/>

#### Example 2:

Input: nums1 = [[2,4],[3,6],[5,5]], nums2 = [[1,3],[4,3]] <br/>
Output: [[1,3],[2,4],[3,6],[4,3],[5,5]] <br/>
Explanation: There are no common ids, so we just include each id with its value in the resulting list. <br/>
 

##### Constraints:

1 <= nums1.length, nums2.length <= 200
nums1[i].length == nums2[j].length == 2
1 <= idi, vali <= 1000
Both arrays contain unique ids.
Both arrays are in strictly ascending order by id.

# Code
```java []
class Solution {
  public int[][] mergeArrays(int[][] nums1, int[][] nums2) {
    final int kMax = 1000;
    List<int[]> ans = new ArrayList<>();
    int[] count = new int[kMax + 1];

    addCount(nums1, count);
    addCount(nums2, count);

    for (int i = 1; i <= kMax; ++i)
      if (count[i] > 0)
        ans.add(new int[] {i, count[i]});

    return ans.stream().toArray(int[][] ::new);
  }

  private void addCount(int[][] nums, int[] count) {
    for (int[] idAndVal : nums) {
      final int id = idAndVal[0];
      final int val = idAndVal[1];
      count[id] += val;
    }
  }
}
```
---
# 2965. Find Missing and Repeated Values
Solved
Easy

You are given a 0-indexed 2D integer matrix grid of size n * n with values in the range [1, n2]. Each integer appears exactly once except a which appears twice and b which is missing. The task is to find the repeating and missing numbers a and b.

Return a 0-indexed integer array ans of size 2 where ans[0] equals to a and ans[1] equals to b.

 

#### Example 1:

Input: grid = [[1,3],[2,2]] <br/>
Output: [2,4] <br/>
Explanation: Number 2 is repeated and number 4 is missing so the answer is [2,4]. <br/>

#### Example 2:

Input: grid = [[9,1,7],[8,9,2],[3,4,6]] <br/>
Output: [9,5] <br/>
Explanation: Number 9 is repeated and number 5 is missing so the answer is [9,5].
 

Constraints:

2 <= n == grid.length == grid[i].length <= 50
1 <= grid[i][j] <= n * n
For all x that 1 <= x <= n * n there is exactly one x that is not equal to any of the grid members.
For all x that 1 <= x <= n * n there is exactly one x that is equal to exactly two of the grid members.
For all x that 1 <= x <= n * n except two of them there is exatly one pair of i, j that 0 <= i, j <= n - 1 and grid[i][j] == x.

# Code
```java []
class Solution {
  public int[] findMissingAndRepeatedValues(int[][] grid) {
    final int n = grid.length;
    final int nSquared = n * n;
    int[] count = new int[nSquared + 1];

    for (int[] row : grid)
      for (final int num : row)
        ++count[num];

    int repeated = -1;
    int missing = -1;

    for (int i = 1; i <= nSquared; ++i) {
      if (count[i] == 2)
        repeated = i;
      if (count[i] == 0)
        missing = i;
    }

    return new int[] {repeated, missing};
  }
}
```
----
# 2467. Most Profitable Path in a Tree
Solved
Medium

There is an undirected tree with n nodes labeled from 0 to n - 1, rooted at node 0. You are given a 2D integer array edges of length n - 1 where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

At every node i, there is a gate. You are also given an array of even integers amount, where amount[i] represents:

the price needed to open the gate at node i, if amount[i] is negative, or,
the cash reward obtained on opening the gate at node i, otherwise.
The game goes on as follows:

Initially, Alice is at node 0 and Bob is at node bob.
At every second, Alice and Bob each move to an adjacent node. Alice moves towards some leaf node, while Bob moves towards node 0.
For every node along their path, Alice and Bob either spend money to open the gate at that node, or accept the reward. Note that:
If the gate is already open, no price will be required, nor will there be any cash reward.
If Alice and Bob reach the node simultaneously, they share the price/reward for opening the gate there. In other words, if the price to open the gate is c, then both Alice and Bob pay c / 2 each. Similarly, if the reward at the gate is c, both of them receive c / 2 each.
If Alice reaches a leaf node, she stops moving. Similarly, if Bob reaches node 0, he stops moving. Note that these events are independent of each other.
Return the maximum net income Alice can have if she travels towards the optimal leaf node.

 

#### Example 1:


Input: edges = [[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6] <br/>
Output: 6 <br/>
Explanation:  <br/>
The above diagram represents the given tree. The game goes as follows:
- Alice is initially on node 0, Bob on node 3. They open the gates of their respective nodes.
  Alice's net income is now -2.
- Both Alice and Bob move to node 1. 
  Since they reach here simultaneously, they open the gate together and share the reward.
  Alice's net income becomes -2 + (4 / 2) = 0.
- Alice moves on to node 3. Since Bob already opened its gate, Alice's income remains unchanged.
  Bob moves on to node 0, and stops moving.
- Alice moves on to node 4 and opens the gate there. Her net income becomes 0 + 6 = 6.
Now, neither Alice nor Bob can make any further moves, and the game ends.
It is not possible for Alice to get a higher net income.

#### Example 2:


Input: edges = [[0,1]], bob = 1, amount = [-7280,2350] <br/>
Output: -7280 <br/>
Explanation:  <br/>
Alice follows the path 0->1 whereas Bob follows the path 1->0.
Thus, Alice opens the gate at node 0 only. Hence, her net income is -7280. 
 

Constraints:

2 <= n <= 105
edges.length == n - 1
edges[i].length == 2
0 <= ai, bi < n
ai != bi
edges represents a valid tree.
1 <= bob < n
amount.length == n
amount[i] is an even integer in the range [-104, 104].

# Code
```java []
class Solution {
  public int mostProfitablePath(int[][] edges, int bob, int[] amount) {
    final int n = amount.length;
    List<Integer>[] tree = new List[n];
    int[] parent = new int[n];
    int[] aliceDist = new int[n];
    Arrays.fill(aliceDist, -1);

    for (int i = 0; i < n; ++i)
      tree[i] = new ArrayList<>();

    for (int[] edge : edges) {
      final int u = edge[0];
      final int v = edge[1];
      tree[u].add(v);
      tree[v].add(u);
    }

    dfs(tree, 0, -1, 0, parent, aliceDist);

    // Modify the amount along the path from node Bob to node 0.
    // For each node,
    //   1. If Bob reaches earlier than Alice does, change the amount to 0.
    //   2. If Bob and Alice reach simultaneously, devide the amount by 2.
    for (int u = bob, bobDist = 0; u != 0; u = parent[u], ++bobDist)
      if (bobDist < aliceDist[u])
        amount[u] = 0;
      else if (bobDist == aliceDist[u])
        amount[u] /= 2;

    return getMoney(tree, 0, -1, amount);
  }

  // Fills `parent` and `dist`.
  private void dfs(List<Integer>[] tree, int u, int prev, int d, int[] parent, int[] dist) {
    parent[u] = prev;
    dist[u] = d;
    for (final int v : tree[u]) {
      if (dist[v] == -1)
        dfs(tree, v, u, d + 1, parent, dist);
    }
  }

  private int getMoney(List<Integer>[] tree, int u, int prev, int[] amount) {
    // a leaf node
    if (tree[u].size() == 1 && tree[u].get(0) == prev)
      return amount[u];

    int maxPath = Integer.MIN_VALUE;
    for (final int v : tree[u])
      if (v != prev)
        maxPath = Math.max(maxPath, getMoney(tree, v, u, amount));

    return amount[u] + maxPath;
  }
}
```
---

# 13. Roman to Integer
Solved
Easy

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol    -   Value
I         =  1
V         =    5
X         =    10
L         =    50
C         =    100
D         =    500
M         =    1000

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

 

#### Example 1:

Input: s = "III" <br/>
Output: 3 <br/>
Explanation: III = 3. <br/>

#### Example 2:

Input: s = "LVIII" <br/>
Output: 58 <br/>
Explanation: L = 50, V= 5, III = 3. <br/>

#### Example 3:

Input: s = "MCMXCIV" <br/>
Output: 1994 <br/>
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4. <br/>
 

Constraints:

1 <= s.length <= 15
s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
It is guaranteed that s is a valid roman numeral in the range [1, 3999].

# Code
```java []
class Solution {
    public int romanToInt(String s) {
        int[] arr = new int[128];

        arr['I'] = 1;
        arr['V'] = 5;
        arr['X'] = 10;
        arr['L'] = 50;
        arr['C'] = 100;
        arr['D'] = 500;
        arr['M'] = 1000;

        int a = 0;

        for (int i = 0; i + 1 < s.length(); ++i) {
            if (arr[s.charAt(i)] >= arr[s.charAt(i + 1)]) {
                a += arr[s.charAt(i)];
            } else {
                a -= arr[s.charAt(i)];
            }
        }

        return a + arr[s.charAt(s.length() - 1)];
    }
}
```
----

# 3306. Count of Substrings Containing Every Vowel and K Consonants II
Medium

You are given a string word and a non-negative integer k.

Return the total number of substrings of word that contain every vowel ('a', 'e', 'i', 'o', and 'u') at least once and exactly k consonants.

 

##### Example 1:

Input: word = "aeioqq", k = 1 <br/>

Output: 0 <br/>

Explanation: <br/>

There is no substring with every vowel. <br/>

##### Example 2:

Input: word = "aeiou", k = 0 <br/>

Output: 1 <br/>

Explanation: <br/>

The only substring with every vowel and zero consonants is word[0..4], which is "aeiou". <br/>

#### Example 3:

Input: word = "ieaouqqieaouqq", k = 1 <br/>

Output: 3 <br/>

Explanation: <br/>

The substrings with every vowel and one consonant are: <br/>

word[0..5], which is "ieaouq". <br/>
word[6..11], which is "qieaou". <br/>
word[7..12], which is "ieaouq". <br/>
 

Constraints:

5 <= word.length <= 2 * 105
word consists only of lowercase English letters.
0 <= k <= word.length - 5

# Code
```java []
class Solution {
  // Same as 3305. Count of Substrings Containing Every Vowel and K Consonants I
  public long countOfSubstrings(String word, int k) {
    return substringsWithAtMost(word, k) - substringsWithAtMost(word, k - 1);
  }

  // Return the number of substrings containing every vowel with at most k
  // consonants.
  private long substringsWithAtMost(String word, int k) {
    if (k == -1)
      return 0;

    long res = 0;
    int vowels = 0;
    int uniqueVowels = 0;
    Map<Character, Integer> vowelLastSeen = new HashMap<>();

    for (int l = 0, r = 0; r < word.length(); ++r) {
      if (isVowel(word.charAt(r))) {
        ++vowels;
        if (!vowelLastSeen.containsKey(word.charAt(r)) || vowelLastSeen.get(word.charAt(r)) < l)
          ++uniqueVowels;
        vowelLastSeen.put(word.charAt(r), r);
      }
      while (r - l + 1 - vowels > k) {
        if (isVowel(word.charAt(l))) {
          --vowels;
          if (vowelLastSeen.get(word.charAt(l)) == l)
            --uniqueVowels;
        }
        ++l;
      }
      if (uniqueVowels == 5) {
        // Add substrings containing every vowel with at most k consonants to
        // the answer. They are
        // word[l..r], word[l + 1..r], ..., word[min(vowelLastSeen[vowel])..r]
        final int minVowelLastSeen = Arrays.asList('a', 'e', 'i', 'o', 'u')
                                         .stream()
                                         .mapToInt(vowel -> vowelLastSeen.get(vowel))
                                         .min()
                                         .getAsInt();
        res += minVowelLastSeen - l + 1;
      }
    }

    return res;
  }

  private boolean isVowel(char c) {
    return "aeiou".indexOf(c) != -1;
  }
}
```
-----

# 1358. Number of Substrings Containing All Three Characters
Medium

Given a string s consisting only of characters a, b and c.

Return the number of substrings containing at least one occurrence of all these characters a, b and c.

 

#### Example 1:

Input: s = "abcabc" <br/>
Output: 10 <br/>
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again).  <br/>

#### Example 2:

Input: s = "aaacb" <br/>
Output: 3 <br/>
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb".  <br/>

#### Example 3:

Input: s = "abc" <br/>
Output: 1 <br/>
 

Constraints:

3 <= s.length <= 5 x 10^4 <br/>
s only consists of a, b or c characters.

# Code
```java []
class Solution {
  // Similar to 3. Longest SubString Without Repeating Characters
  public int numberOfSubstrings(String s) {
    int ans = 0;
    int[] count = new int[3];

    int l = 0;
    for (final char c : s.toCharArray()) {
      ++count[c - 'a'];
      while (count[0] > 0 && count[1] > 0 && count[2] > 0)
        --count[s.charAt(l++) - 'a'];
      // s[0..r], s[1..r], ..., s[l - 1..r] are satified strings.
      ans += l;
    }

    return ans;
  }
}
```
-----

