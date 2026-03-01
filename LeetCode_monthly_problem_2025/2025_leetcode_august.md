
---------------------------------------------------
# ***LeetCode problems - ( Aug - 2025 )***
---------------------------------------------------


# 2561. Rearranging Fruits -> [LeetCode](https://leetcode.com/problems/rearranging-fruits/description/)
 
Hard
 
You have two fruit baskets containing n fruits each. You are given two 0-indexed integer arrays basket1 and basket2 representing the cost of fruit in each basket. You want to make both baskets equal. To do so, you can use the following operation as many times as you want:

Chose two indices i and j, and swap the ith fruit of basket1 with the jth fruit of basket2.
The cost of the swap is min(basket1[i],basket2[j]).
Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return the minimum cost to make both the baskets equal or -1 if impossible.

 

Example 1:

Input: basket1 = [4,2,2,2], basket2 = [1,4,1,2]
Output: 1
Explanation: Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.


Example 2:

Input: basket1 = [2,3,4,1], basket2 = [3,2,5,1]
Output: -1
Explanation: It can be shown that it is impossible to make both the baskets equal.
 

Constraints:

basket1.length == basket2.length
1 <= basket1.length <= 105
1 <= basket1[i],basket2[i] <= 109

# Code
```cpp []
class Solution {
 public:
  long long minCost(vector<int>& basket1, vector<int>& basket2) {
    vector<int> swapped;
    unordered_map<int, int> count;

    for (const int b : basket1)
      ++count[b];

    for (const int b : basket2)
      --count[b];

    for (const auto& [num, freq] : count) {
      if (freq % 2 != 0)
        return -1;
      for (int i = 0; i < abs(freq) / 2; ++i)
        swapped.push_back(num);
    }

    const int minNum = min(ranges::min(basket1), ranges::min(basket2));
    const auto midIt = swapped.begin() + swapped.size() / 2;
    nth_element(swapped.begin(), midIt, swapped.end());
    return accumulate(swapped.begin(), midIt, 0L, [minNum](long acc, int num) {
      return acc + min(2 * minNum, num);
    });
  }
};
```

----------


# 2106. Maximum Fruits Harvested After at Most K Steps -> [LeetCode](https://leetcode.com/problems/maximum-fruits-harvested-after-at-most-k-steps/description)
 
Hard
 
Fruits are available at some positions on an infinite x-axis. You are given a 2D integer array fruits where fruits[i] = [positioni, amounti] depicts amounti fruits at the position positioni. fruits is already sorted by positioni in ascending order, and each positioni is unique.

You are also given an integer startPos and an integer k. Initially, you are at the position startPos. From any position, you can either walk to the left or right. It takes one step to move one unit on the x-axis, and you can walk at most k steps in total. For every position you reach, you harvest all the fruits at that position, and the fruits will disappear from that position.

Return the maximum total number of fruits you can harvest.

 

Example 1:

![example1](https://assets.leetcode.com/uploads/2021/11/21/1.png)

Input: fruits = [[2,8],[6,3],[8,6]], startPos = 5, k = 4
Output: 9
Explanation: 
The optimal way is to:
- Move right to position 6 and harvest 3 fruits
- Move right to position 8 and harvest 6 fruits
You moved 3 steps and harvested 3 + 6 = 9 fruits in total.


Example 2:

![example1](https://assets.leetcode.com/uploads/2021/11/21/2.png)

Input: fruits = [[0,9],[4,1],[5,7],[6,2],[7,4],[10,9]], startPos = 5, k = 4
Output: 14
Explanation: 
You can move at most k = 4 steps, so you cannot reach position 0 nor 10.
The optimal way is to:
- Harvest the 7 fruits at the starting position 5
- Move left to position 4 and harvest 1 fruit
- Move right to position 6 and harvest 2 fruits
- Move right to position 7 and harvest 4 fruits
You moved 1 + 3 = 4 steps and harvested 7 + 1 + 2 + 4 = 14 fruits in total.


Example 3:


Input: fruits = [[0,3],[6,4],[8,5]], startPos = 3, k = 2
Output: 0
Explanation:
You can move at most k = 2 steps and cannot reach any position with fruits.
 

Constraints:

1 <= fruits.length <= 105
fruits[i].length == 2
0 <= startPos, positioni <= 2 * 105
positioni-1 < positioni for any i > 0 (0-indexed)
1 <= amounti <= 104
0 <= k <= 2 * 105

# Code
```cpp []
class Solution {
 public:
  int maxTotalFruits(vector<vector<int>>& fruits, int startPos, int k) {
    const int maxRight = max(startPos, fruits.back()[0]);
    int ans = 0;
    vector<int> amounts(1 + maxRight);
    vector<int> prefix(2 + maxRight);

    for (const vector<int>& f : fruits)
      amounts[f[0]] = f[1];

    partial_sum(amounts.begin(), amounts.end(), prefix.begin() + 1);

    auto getFruits = [&](int leftSteps, int rightSteps) {
      const int l = max(0, startPos - leftSteps);
      const int r = min(maxRight, startPos + rightSteps);
      return prefix[r + 1] - prefix[l];
    };

    // Go right first.
    const int maxRightSteps = min(maxRight - startPos, k);
    for (int rightSteps = 0; rightSteps <= maxRightSteps; ++rightSteps) {
      const int leftSteps = max(0, k - 2 * rightSteps);  // Turn left
      ans = max(ans, getFruits(leftSteps, rightSteps));
    }

    // Go left first.
    const int maxLeftSteps = min(startPos, k);
    for (int leftSteps = 0; leftSteps <= maxLeftSteps; ++leftSteps) {
      const int rightSteps = max(0, k - 2 * leftSteps);  // Turn right
      ans = max(ans, getFruits(leftSteps, rightSteps));
    }

    return ans;
  }
};
```

----------------


# 904. Fruit Into Baskets -> [LeetCode](https://leetcode.com/problems/fruit-into-baskets/description)
 
Medium

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
Given the integer array fruits, return the maximum number of fruits you can pick.

 

Example 1:

Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.


Example 2:

Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].


Example 3:

Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
 

Constraints:

1 <= fruits.length <= 105
0 <= fruits[i] < fruits.length


# Code
```cpp []
class Solution {
 public:
  int totalFruit(vector<int>& fruits) {
    int ans = 0;
    unordered_map<int, int> count;

    for (int l = 0, r = 0; r < fruits.size(); ++r) {
      ++count[fruits[r]];
      while (count.size() > 2) {
        if (--count[fruits[l]] == 0)
          count.erase(fruits[l]);
        ++l;
      }
      ans = max(ans, r - l + 1);
    }

    return ans;
  }
};

```

---------------


# 3477. Fruits Into Baskets II -> [LeetCode](https://leetcode.com/problems/fruits-into-baskets-ii/description/)
 
Easy
 
You are given two arrays of integers, fruits and baskets, each of length n, where fruits[i] represents the quantity of the ith type of fruit, and baskets[j] represents the capacity of the jth basket.

From left to right, place the fruits according to these rules:

Each fruit type must be placed in the leftmost available basket with a capacity greater than or equal to the quantity of that fruit type.
Each basket can hold only one type of fruit.
If a fruit type cannot be placed in any basket, it remains unplaced.
Return the number of fruit types that remain unplaced after all possible allocations are made.

 

Example 1:

Input: fruits = [4,2,5], baskets = [3,5,4]

Output: 1

Explanation:

fruits[0] = 4 is placed in baskets[1] = 5.
fruits[1] = 2 is placed in baskets[0] = 3.
fruits[2] = 5 cannot be placed in baskets[2] = 4.
Since one fruit type remains unplaced, we return 1.

Example 2:

Input: fruits = [3,6,1], baskets = [6,4,7]

Output: 0

Explanation:

fruits[0] = 3 is placed in baskets[0] = 6.
fruits[1] = 6 cannot be placed in baskets[1] = 4 (insufficient capacity) but can be placed in the next available basket, baskets[2] = 7.
fruits[2] = 1 is placed in baskets[1] = 4.
Since all fruits are successfully placed, we return 0.

 

Constraints:

n == fruits.length == baskets.length
1 <= n <= 100
1 <= fruits[i], baskets[i] <= 1000

# Code
```cpp []

class SegmentTree {
 public:
  explicit SegmentTree(const vector<int>& nums) : n(nums.size()), tree(n * 4) {
    build(nums, 0, 0, n - 1);
  }

  // Updates nums[i] to val.
  void update(int i, int val) {
    update(0, 0, n - 1, i, val);
  }

  // Returns the first index i where baskets[i] >= target, or -1 if not found.
  int queryFirst(int target) {
    return queryFirst(0, 0, n - 1, target);
  }

 private:
  const int n;       // the size of the input array
  vector<int> tree;  // the segment tree

  void build(const vector<int>& nums, int treeIndex, int lo, int hi) {
    if (lo == hi) {
      tree[treeIndex] = nums[lo];
      return;
    }
    const int mid = (lo + hi) / 2;
    build(nums, 2 * treeIndex + 1, lo, mid);
    build(nums, 2 * treeIndex + 2, mid + 1, hi);
    tree[treeIndex] = merge(tree[2 * treeIndex + 1], tree[2 * treeIndex + 2]);
  }

  void update(int treeIndex, int lo, int hi, int i, int val) {
    if (lo == hi) {
      tree[treeIndex] = val;
      return;
    }
    const int mid = (lo + hi) / 2;
    if (i <= mid)
      update(2 * treeIndex + 1, lo, mid, i, val);
    else
      update(2 * treeIndex + 2, mid + 1, hi, i, val);
    tree[treeIndex] = merge(tree[2 * treeIndex + 1], tree[2 * treeIndex + 2]);
  }

  int queryFirst(int treeIndex, int lo, int hi, int target) {
    if (tree[treeIndex] < target)
      return -1;
    if (lo == hi) {
      // Found a valid position, mark it as used by setting to -1.
      update(lo, -1);
      return lo;
    }
    const int mid = (lo + hi) / 2;
    const int leftChild = tree[2 * treeIndex + 1];
    return leftChild >= target
               ? queryFirst(2 * treeIndex + 1, lo, mid, target)
               : queryFirst(2 * treeIndex + 2, mid + 1, hi, target);
  }

  int merge(int left, int right) const {
    return max(left, right);
  }
};

class Solution {
 public:
  int numOfUnplacedFruits(vector<int>& fruits, vector<int>& baskets) {
    int ans = 0;
    SegmentTree tree(baskets);

    for (const int fruit : fruits)
      if (tree.queryFirst(fruit) == -1)
        ++ans;

    return ans;
  }
};
```

-----------


# 387. First Unique Character in a String -> [LeetCode](https://leetcode.com/problems/first-unique-character-in-a-string/description/)
 
Easy
 
Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.

 

Example 1:

Input: s = "leetcode"

Output: 0

Explanation:

The character 'l' at index 0 is the first character that does not occur at any other index.

Example 2:

Input: s = "loveleetcode"

Output: 2

Example 3:

Input: s = "aabb"

Output: -1

 

Constraints:

1 <= s.length <= 105
s consists of only lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> freq;
        for(const char i:s) freq[i]++;
        for(int i=0 ; i<s.size() ; ++i){
            if(freq[s[i]] == 1) return i;
        }
        return -1;
    }
};
```

-----------


# 3479. Fruits Into Baskets III -> [LeetCode](https://leetcode.com/problems/fruits-into-baskets-iii/description/)
 
Medium
 
You are given two arrays of integers, fruits and baskets, each of length n, where fruits[i] represents the quantity of the ith type of fruit, and baskets[j] represents the capacity of the jth basket.

From left to right, place the fruits according to these rules:

Each fruit type must be placed in the leftmost available basket with a capacity greater than or equal to the quantity of that fruit type.
Each basket can hold only one type of fruit.
If a fruit type cannot be placed in any basket, it remains unplaced.
Return the number of fruit types that remain unplaced after all possible allocations are made.

 

Example 1:

Input: fruits = [4,2,5], baskets = [3,5,4]

Output: 1

Explanation:

fruits[0] = 4 is placed in baskets[1] = 5.
fruits[1] = 2 is placed in baskets[0] = 3.
fruits[2] = 5 cannot be placed in baskets[2] = 4.
Since one fruit type remains unplaced, we return 1.

Example 2:

Input: fruits = [3,6,1], baskets = [6,4,7]

Output: 0

Explanation:

fruits[0] = 3 is placed in baskets[0] = 6.
fruits[1] = 6 cannot be placed in baskets[1] = 4 (insufficient capacity) but can be placed in the next available basket, baskets[2] = 7.
fruits[2] = 1 is placed in baskets[1] = 4.
Since all fruits are successfully placed, we return 0.

 

Constraints:

n == fruits.length == baskets.length
1 <= n <= 105
1 <= fruits[i], baskets[i] <= 109

# Code
```cpp []
class SegmentTree {
 public:
  explicit SegmentTree(const vector<int>& nums) : n(nums.size()), tree(n * 4) {
    build(nums, 0, 0, n - 1);
  }

  // Updates nums[i] to val.
  void update(int i, int val) {
    update(0, 0, n - 1, i, val);
  }

  // Returns the first index i where baskets[i] >= target, or -1 if not found.
  int queryFirst(int target) {
    return queryFirst(0, 0, n - 1, target);
  }

 private:
  const int n;       // the size of the input array
  vector<int> tree;  // the segment tree

  void build(const vector<int>& nums, int treeIndex, int lo, int hi) {
    if (lo == hi) {
      tree[treeIndex] = nums[lo];
      return;
    }
    const int mid = (lo + hi) / 2;
    build(nums, 2 * treeIndex + 1, lo, mid);
    build(nums, 2 * treeIndex + 2, mid + 1, hi);
    tree[treeIndex] = merge(tree[2 * treeIndex + 1], tree[2 * treeIndex + 2]);
  }

  void update(int treeIndex, int lo, int hi, int i, int val) {
    if (lo == hi) {
      tree[treeIndex] = val;
      return;
    }
    const int mid = (lo + hi) / 2;
    if (i <= mid)
      update(2 * treeIndex + 1, lo, mid, i, val);
    else
      update(2 * treeIndex + 2, mid + 1, hi, i, val);
    tree[treeIndex] = merge(tree[2 * treeIndex + 1], tree[2 * treeIndex + 2]);
  }

  int queryFirst(int treeIndex, int lo, int hi, int target) {
    if (tree[treeIndex] < target)
      return -1;
    if (lo == hi) {
      // Found a valid position, mark it as used by setting to -1.
      update(lo, -1);
      return lo;
    }
    const int mid = (lo + hi) / 2;
    const int leftChild = tree[2 * treeIndex + 1];
    return leftChild >= target
               ? queryFirst(2 * treeIndex + 1, lo, mid, target)
               : queryFirst(2 * treeIndex + 2, mid + 1, hi, target);
  }

  int merge(int left, int right) const {
    return max(left, right);
  }
};

class Solution {
 public:
  // Same as 3477. Fruits Into Baskets II
  int numOfUnplacedFruits(vector<int>& fruits, vector<int>& baskets) {
    int ans = 0;
    SegmentTree tree(baskets);

    for (const int fruit : fruits)
      if (tree.queryFirst(fruit) == -1)
        ++ans;

    return ans;
  }
};
```

-------------


# 3363. Find the Maximum Number of Fruits Collected -> [LeetCode](https://leetcode.com/problems/find-the-maximum-number-of-fruits-collected/description)
 
Hard
 
There is a game dungeon comprised of n x n rooms arranged in a grid.

You are given a 2D array fruits of size n x n, where fruits[i][j] represents the number of fruits in the room (i, j). Three children will play in the game dungeon, with initial positions at the corner rooms (0, 0), (0, n - 1), and (n - 1, 0).

The children will make exactly n - 1 moves according to the following rules to reach the room (n - 1, n - 1):

The child starting from (0, 0) must move from their current room (i, j) to one of the rooms (i + 1, j + 1), (i + 1, j), and (i, j + 1) if the target room exists.
The child starting from (0, n - 1) must move from their current room (i, j) to one of the rooms (i + 1, j - 1), (i + 1, j), and (i + 1, j + 1) if the target room exists.
The child starting from (n - 1, 0) must move from their current room (i, j) to one of the rooms (i - 1, j + 1), (i, j + 1), and (i + 1, j + 1) if the target room exists.
When a child enters a room, they will collect all the fruits there. If two or more children enter the same room, only one child will collect the fruits, and the room will be emptied after they leave.

Return the maximum number of fruits the children can collect from the dungeon.

 

Example 1:

Input: fruits = [[1,2,3,4],[5,6,8,7],[9,10,11,12],[13,14,15,16]]

Output: 100

Explanation:
![example](https://assets.leetcode.com/uploads/2024/10/15/example_1.gif)


In this example:

The 1st child (green) moves on the path (0,0) -> (1,1) -> (2,2) -> (3, 3).
The 2nd child (red) moves on the path (0,3) -> (1,2) -> (2,3) -> (3, 3).
The 3rd child (blue) moves on the path (3,0) -> (3,1) -> (3,2) -> (3, 3).
In total they collect 1 + 6 + 11 + 16 + 4 + 8 + 12 + 13 + 14 + 15 = 100 fruits.

Example 2:

Input: fruits = [[1,1],[1,1]]

Output: 4

Explanation:

In this example:

The 1st child moves on the path (0,0) -> (1,1).
The 2nd child moves on the path (0,1) -> (1,1).
The 3rd child moves on the path (1,0) -> (1,1).
In total they collect 1 + 1 + 1 + 1 = 4 fruits.

 

Constraints:

2 <= n == fruits.length == fruits[i].length <= 1000
0 <= fruits[i][j] <= 1000

# Code
```cpp []
class Solution {
 public:
  int maxCollectedFruits(vector<vector<int>>& fruits) {
    return getTopLeft(fruits) + getTopRight(fruits) + getBottomLeft(fruits) -
           2 * fruits.back().back();
  }

 private:
  int getTopLeft(const vector<vector<int>>& fruits) {
    const int n = fruits.size();
    int res = 0;
    for (int i = 0; i < n; ++i)
      res += fruits[i][i];
    return res;
  }

  int getTopRight(const vector<vector<int>>& fruits) {
    const int n = fruits.size();
    vector<vector<int>> dp(n, vector<int>(n));
    dp[0][n - 1] = fruits[0][n - 1];
    for (int x = 0; x < n; ++x) {
      for (int y = 0; y < n; ++y) {
        if (x >= y && !(x == n - 1 && y == n - 1))
          continue;
        for (const auto& [dx, dy] :
             vector<pair<int, int>>{{1, -1}, {1, 0}, {1, 1}}) {
          const int i = x - dx;
          const int j = y - dy;
          if (i < 0 || i == n || j < 0 || j == n)
            continue;
          if (i < j && j < n - 1 - i)
            continue;
          dp[x][y] = max(dp[x][y], dp[i][j] + fruits[x][y]);
        }
      }
    }

    return dp[n - 1][n - 1];
  }

  int getBottomLeft(const vector<vector<int>>& fruits) {
    const int n = fruits.size();
    vector<vector<int>> dp(n, vector<int>(n));
    dp[n - 1][0] = fruits[n - 1][0];
    for (int y = 0; y < n; ++y) {
      for (int x = 0; x < n; ++x) {
        if (x <= y && !(x == n - 1 && y == n - 1))
          continue;
        for (const auto& [dx, dy] :
             vector<pair<int, int>>{{-1, 1}, {0, 1}, {1, 1}}) {
          const int i = x - dx;
          const int j = y - dy;
          if (i < 0 || i == n || j < 0 || j == n)
            continue;
          if (j < i && i < n - 1 - j)
            continue;
          dp[x][y] = max(dp[x][y], dp[i][j] + fruits[x][y]);
        }
      }
    }
    return dp[n - 1][n - 1];
  }
};
```

------------------



# 808. Soup Servings -> [LeetCode](https://leetcode.com/problems/soup-servings/description/)
 
Medium
 
You have two soups, A and B, each starting with n mL. On every turn, one of the following four serving operations is chosen at random, each with probability 0.25 independent of all previous turns:

pour 100 mL from type A and 0 mL from type B
pour 75 mL from type A and 25 mL from type B
pour 50 mL from type A and 50 mL from type B
pour 25 mL from type A and 75 mL from type B
Note:

There is no operation that pours 0 mL from A and 100 mL from B.
The amounts from A and B are poured simultaneously during the turn.
If an operation asks you to pour more than you have left of a soup, pour all that remains of that soup.
The process stops immediately after any turn in which one of the soups is used up.

Return the probability that A is used up before B, plus half the probability that both soups are used up in the same turn. Answers within 10-5 of the actual answer will be accepted.

 

Example 1:

Input: n = 50
Output: 0.62500
Explanation: 
If we perform either of the first two serving operations, soup A will become empty first.
If we perform the third operation, A and B will become empty at the same time.
If we perform the fourth operation, B will become empty first.
So the total probability of A becoming empty first plus half the probability that A and B become empty at the same time, is 0.25 * (1 + 1 + 0.5 + 0) = 0.625.


Example 2:

Input: n = 100
Output: 0.71875
Explanation: 
If we perform the first serving operation, soup A will become empty first.
If we perform the second serving operations, A will become empty on performing operation [1, 2, 3], and both A and B become empty on performing operation 4.
If we perform the third operation, A will become empty on performing operation [1, 2], and both A and B become empty on performing operation 3.
If we perform the fourth operation, A will become empty on performing operation 1, and both A and B become empty on performing operation 2.
So the total probability of A becoming empty first plus half the probability that A and B become empty at the same time, is 0.71875.
 

Constraints:

0 <= n <= 109

# Code
```cpp []
class Solution {
 public:
  double soupServings(int n) {
    return n >= 4800 ? 1.0 : dfs((n + 24) / 25, (n + 24) / 25);
  }

 private:
  vector<vector<double>> mem =
      vector<vector<double>>(4800 / 25, vector<double>(4800 / 25));

  double dfs(int a, int b) {
    if (a <= 0 && b <= 0)
      return 0.5;
    if (a <= 0)
      return 1.0;
    if (b <= 0)
      return 0.0;
    if (mem[a][b] > 0)
      return mem[a][b];
    return mem[a][b] = 0.25 * (dfs(a - 4, b) + dfs(a - 3, b - 1) +
                               dfs(a - 2, b - 2) + dfs(a - 1, b - 3));
  }
};
```

------------


# 2438. Range Product Queries of Powers -> [LeetCode](https://leetcode.com/problems/range-product-queries-of-powers/description/)
 
Medium
 
Given a positive integer n, there exists a 0-indexed array called powers, composed of the minimum number of powers of 2 that sum to n. The array is sorted in non-decreasing order, and there is only one way to form the array.

You are also given a 0-indexed 2D integer array queries, where queries[i] = [lefti, righti]. Each queries[i] represents a query where you have to find the product of all powers[j] with lefti <= j <= righti.

Return an array answers, equal in length to queries, where answers[i] is the answer to the ith query. Since the answer to the ith query may be too large, each answers[i] should be returned modulo 109 + 7.

 

Example 1:

Input: n = 15, queries = [[0,1],[2,2],[0,3]]
Output: [2,4,64]
Explanation:
For n = 15, powers = [1,2,4,8]. It can be shown that powers cannot be a smaller size.
Answer to 1st query: powers[0] * powers[1] = 1 * 2 = 2.
Answer to 2nd query: powers[2] = 4.
Answer to 3rd query: powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64.
Each answer modulo 109 + 7 yields the same answer, so [2,4,64] is returned.


Example 2:

Input: n = 2, queries = [[0,0]]
Output: [2]
Explanation:
For n = 2, powers = [2].
The answer to the only query is powers[0] = 2. The answer modulo 109 + 7 is the same, so [2] is returned.
 

Constraints:

1 <= n <= 109
1 <= queries.length <= 105
0 <= starti <= endi < powers.length
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```cpp []
class Solution {
 public:
  vector<int> productQueries(int n, vector<vector<int>>& queries) {
    constexpr int kMod = 1'000'000'007;
    constexpr int kMaxBit = 30;
    vector<int> ans;
    vector<int> pows;

    for (int i = 0; i < kMaxBit; ++i)
      if (n >> i & 1)
        pows.push_back(1 << i);

    for (const vector<int>& query : queries) {
      const int left = query[0];
      const int right = query[1];
      long prod = 1;
      for (int i = left; i <= right; ++i) {
        prod *= pows[i];
        prod %= kMod;
      }
      ans.push_back(prod);
    }

    return ans;
  }
};
```

--------


# 869. Reordered Power of 2 -> [LeetCode](https://leetcode.com/problems/reordered-power-of-2/description/)
 
Medium

You are given an integer n. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return true if and only if we can do this so that the resulting number is a power of two.

 

Example 1:

Input: n = 1
Output: true


Example 2:

Input: n = 10
Output: false
 

Constraints:

1 <= n <= 109

# Code
```cpp []
class Solution {
 public:
  bool reorderedPowerOf2(int n) {
    int count = counter(n);

    for (int i = 0; i < 30; ++i)
      if (counter(1 << i) == count)
        return true;

    return false;
  }

 private:
  int counter(int n) {
    int count = 0;

    for (; n > 0; n /= 10)
      count += pow(10, n % 10);

    return count;
  }
};
```

-------



# 2787. Ways to Express an Integer as Sum of Powers -> [LeetCode](https://leetcode.com/problems/ways-to-express-an-integer-as-sum-of-powers/description/)
 
Medium
 
Given two positive integers n and x.

Return the number of ways n can be expressed as the sum of the xth power of unique positive integers, in other words, the number of sets of unique integers [n1, n2, ..., nk] where n = n1x + n2x + ... + nkx.

Since the result can be very large, return it modulo 109 + 7.

For example, if n = 160 and x = 3, one way to express n is n = 23 + 33 + 53.

 

Example 1:

Input: n = 10, x = 2
Output: 1
Explanation: We can express n as the following: n = 32 + 12 = 10.
It can be shown that it is the only way to express 10 as the sum of the 2nd power of unique integers.

Example 2:

Input: n = 4, x = 1
Output: 2
Explanation: We can express n in the following ways:
- n = 41 = 4.
- n = 31 + 11 = 4.
 

Constraints:

1 <= n <= 300
1 <= x <= 5

# Code
```cpp []
class Solution {
 public:
  int numberOfWays(int n, int x) {
    constexpr int kMod = 1'000'000'007;
    // dp[i] := the number of ways to express i
    vector<int> dp(n + 1);
    int ax;  // a^x

    dp[0] = 1;

    for (int a = 1; (ax = pow(a, x)) <= n; ++a)
      for (int i = n; i >= ax; --i) {
        dp[i] += dp[i - ax];
        dp[i] %= kMod;
      }

    return dp[n];
  }
};
```

------------



# 326. Power of Three -> [LeetCode](https://leetcode.com/problems/power-of-three/description/)
 
Easy
 
Given an integer n, return true if it is a power of three. Otherwise, return false.

An integer n is a power of three, if there exists an integer x such that n == 3x.

 

Example 1:

Input: n = 27
Output: true
Explanation: 27 = 33


Example 2:

Input: n = 0
Output: false
Explanation: There is no x where 3x = 0.

Example 3:

Input: n = -1
Output: false
Explanation: There is no x where 3x = (-1).
 

Constraints:

-231 <= n <= 231 - 1
 

Follow up: Could you solve it without loops/recursion?

# Code
```cpp []
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 &&  ((int)pow(3,19)) % n == 0;
    }
};
```

-----------


# 2264. Largest 3-Same-Digit Number in String -> [LeetCode](https://leetcode.com/problems/largest-3-same-digit-number-in-string/)
  
Easy
 
You are given a string num representing a large integer. An integer is good if it meets the following conditions:

It is a substring of num with length 3.
It consists of only one unique digit.
Return the maximum good integer as a string or an empty string "" if no such integer exists.

Note:

A substring is a contiguous sequence of characters within a string.
There may be leading zeroes in num or a good integer.
 

Example 1:

Input: num = "6777133339"
Output: "777"
Explanation: There are two distinct good integers: "777" and "333".
"777" is the largest, so we return "777".

Example 2:

Input: num = "2300019"
Output: "000"
Explanation: "000" is the only good integer.


Example 3:

Input: num = "42352338"
Output: ""
Explanation: No substring of length 3 consists of only one unique digit. Therefore, there are no good integers.
 

Constraints:

3 <= num.length <= 1000
num only consists of digits.

# Code
```cpp []
class Solution {
public:
    string largestGoodInteger(string num) {
        int result = -1;
        int n = num.size() ;
        for(int i=0 ; i + 2 < n ; ++i){
            if(num[i] == num[i+1] && num[i+1] == num[i+2]) result = max(result , num[i] - '0');
        }
        return (result == -1 ? "" : string(3,result + '0'));
    }
};
```

--------


# 342. Power of Four -> [LeetCode](https://leetcode.com/problems/power-of-four/description)
 
Easy
 
Given an integer n, return true if it is a power of four. Otherwise, return false.

An integer n is a power of four, if there exists an integer x such that n == 4x.

 

Example 1:

Input: n = 16
Output: true


Example 2:

Input: n = 5
Output: false


Example 3:

Input: n = 1
Output: true
 

Constraints:

-231 <= n <= 231 - 1
 

Follow up: Could you solve it without loops/recursion?

# Code
```cpp []
class Solution {
 public:
  bool isPowerOfFour(unsigned n) {
    return n > 0 && popcount(n) == 1 && (n - 1) % 3 == 0;
  }
};
```

------------------



# 1323. Maximum 69 Number -> [LeetCode](https://leetcode.com/problems/maximum-69-number/description/)
 
Easy
 
You are given a positive integer num consisting only of digits 6 and 9.

Return the maximum number you can get by changing at most one digit (6 becomes 9, and 9 becomes 6).

 

Example 1:

Input: num = 9669
Output: 9969
Explanation: 
Changing the first digit results in 6669.
Changing the second digit results in 9969.
Changing the third digit results in 9699.
Changing the fourth digit results in 9666.
The maximum number is 9969.

Example 2:

Input: num = 9996
Output: 9999
Explanation: Changing the last digit 6 to 9 results in the maximum number.

Example 3:

Input: num = 9999
Output: 9999
Explanation: It is better not to apply any change.
 

Constraints:

1 <= num <= 104
num consists of only 6 and 9 digits.

# Code
```cpp []
class Solution {
 public:
  int maximum69Number(int num) {
    string ans = to_string(num);

    for (char& c : ans)
      if (c == '6') {
        c = '9';
        break;
      }

    return stoi(ans);
  }
};
```



-----------------------------



# 837. New 21 Game -> [LeetCode](https://leetcode.com/problems/new-21-game/description)
 
Medium

Alice plays the following game, loosely based on the card game "21".

Alice starts with 0 points and draws numbers while she has less than k points. During each draw, she gains an integer number of points randomly from the range [1, maxPts], where maxPts is an integer. Each draw is independent and the outcomes have equal probabilities.

Alice stops drawing numbers when she gets k or more points.

Return the probability that Alice has n or fewer points.

Answers within 10-5 of the actual answer are considered accepted.

 

Example 1:

Input: n = 10, k = 1, maxPts = 10
Output: 1.00000
Explanation: Alice gets a single card, then stops.


Example 2:

Input: n = 6, k = 1, maxPts = 10
Output: 0.60000
Explanation: Alice gets a single card, then stops.
In 6 out of 10 possibilities, she is at or below 6 points.


Example 3:

Input: n = 21, k = 17, maxPts = 10
Output: 0.73278
 

Constraints:

0 <= k <= n <= 104
1 <= maxPts <= 104

# Code
```cpp []
class Solution {
 public:
  double new21Game(int n, int k, int maxPts) {
    // When the game ends, the point is in [k..k - 1 maxPts].
    //   P = 1, if n >= k - 1 + maxPts
    //   P = 0, if n < k (note that the constraints already have k <= n)
    if (k == 0 || n >= k - 1 + maxPts)
      return 1.0;

    double ans = 0.0;
    vector<double> dp(n + 1);  // dp[i] := the probability to have i points
    dp[0] = 1.0;
    double windowSum = dp[0];  // P(i - 1) + P(i - 2) + ... + P(i - maxPts)

    for (int i = 1; i <= n; ++i) {
      // The probability to get i points is
      // P(i) = [P(i - 1) + P(i - 2) + ... + P(i - maxPts)] / maxPts
      dp[i] = windowSum / maxPts;
      if (i < k)
        windowSum += dp[i];
      else  // The game ends.
        ans += dp[i];
      if (i - maxPts >= 0)
        windowSum -= dp[i - maxPts];
    }

    return ans;
  }
};
```

--------------------


# 679. 24 Game -> [LeetCode](https://leetcode.com/problems/24-game/description)
 
Hard
 
You are given an integer array cards of length 4. You have four cards, each containing a number in the range [1, 9]. You should arrange the numbers on these cards in a mathematical expression using the operators ['+', '-', '*', '/'] and the parentheses '(' and ')' to get the value 24.

You are restricted with the following rules:

The division operator '/' represents real division, not integer division.
For example, 4 / (1 - 2 / 3) = 4 / (1 / 3) = 12.
Every operation done is between two numbers. In particular, we cannot use '-' as a unary operator.
For example, if cards = [1, 1, 1, 1], the expression "-1 - 1 - 1 - 1" is not allowed.
You cannot concatenate numbers together
For example, if cards = [1, 2, 1, 2], the expression "12 + 12" is not valid.
Return true if you can get such expression that evaluates to 24, and false otherwise.

 

Example 1:

Input: cards = [4,1,8,7]
Output: true
Explanation: (8-4) * (7-1) = 24


Example 2:

Input: cards = [1,2,1,2]
Output: false
 

Constraints:

cards.length == 4
1 <= cards[i] <= 9

# Code
```cpp []
class Solution {
 public:
  bool judgePoint24(vector<int>& nums) {
    vector<double> doubleNums;

    for (const int num : nums)
      doubleNums.push_back(num);

    return dfs(doubleNums);
  }

 private:
  bool dfs(vector<double>& nums) {
    if (nums.size() == 1)
      return abs(nums[0] - 24) < 0.001;

    for (int i = 0; i < nums.size(); ++i)
      for (int j = 0; j < i; ++j) {
        for (const double num : generate(nums[i], nums[j])) {
          vector<double> nextRound{num};
          for (int k = 0; k < nums.size(); ++k) {
            if (k == i || k == j)  // It is used in `generate()`.
              continue;
            nextRound.push_back(nums[k]);
          }
          if (dfs(nextRound))
            return true;
        }
      }

    return false;
  }

  vector<double> generate(double a, double b) {
    return {a * b, a / b, b / a, a + b, a - b, b - a};
  }
};
```


--------------



# 2348. Number of Zero-Filled Subarrays -> [LeetCode](https://leetcode.com/problems/number-of-zero-filled-subarrays/description/)
 
Medium
 
Given an integer array nums, return the number of subarrays filled with 0.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,3,0,0,2,0,0,4]
Output: 6
Explanation: 
There are 4 occurrences of [0] as a subarray.
There are 2 occurrences of [0,0] as a subarray.
There is no occurrence of a subarray with a size more than 2 filled with 0. Therefore, we return 6.


Example 2:

Input: nums = [0,0,0,2,0,0]
Output: 9
Explanation:
There are 5 occurrences of [0] as a subarray.
There are 3 occurrences of [0,0] as a subarray.
There is 1 occurrence of [0,0,0] as a subarray.
There is no occurrence of a subarray with a size more than 3 filled with 0. Therefore, we return 9.


Example 3:

Input: nums = [2,10,2019]
Output: 0
Explanation: There is no subarray filled with 0. Therefore, we return 0.
 

Constraints:

1 <= nums.length <= 105
-109 <= nums[i] <= 109

# Code
```cpp []
class Solution {
 public:
  long long zeroFilledSubarray(vector<int>& nums) {
    long ans = 0;
    int indexBeforeZero = -1;

    for (int i = 0; i < nums.size(); ++i)
      if (nums[i])
        indexBeforeZero = i;
      else
        ans += i - indexBeforeZero;

    return ans;
  }
};
```

------------------



# 1277. Count Square Submatrices with All Ones -> [LeetCode](https://leetcode.com/problems/count-square-submatrices-with-all-ones/description)
 
Medium
 
Given a m * n matrix of ones and zeros, return how many square submatrices have all ones.

 

Example 1:

Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
Output: 15
Explanation: 
There are 10 squares of side 1.
There are 4 squares of side 2.
There is  1 square of side 3.
Total number of squares = 10 + 4 + 1 = 15.


Example 2:

Input: matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
Output: 7
Explanation: 
There are 6 squares of side 1.  
There is 1 square of side 2. 
Total number of squares = 6 + 1 = 7.
 

Constraints:

1 <= arr.length <= 300
1 <= arr[0].length <= 300
0 <= arr[i][j] <= 1

# Code
```cpp []
class Solution {
 public:
  int countSquares(vector<vector<int>>& matrix) {
    for (int i = 0; i < matrix.size(); ++i)
      for (int j = 0; j < matrix[0].size(); ++j)
        if (matrix[i][j] == 1 && i > 0 && j > 0)
          matrix[i][j] +=
              min({matrix[i - 1][j - 1], matrix[i - 1][j], matrix[i][j - 1]});
    return accumulate(matrix.begin(), matrix.end(), 0,
                      [](int subtotal, const vector<int>& row) {
      return subtotal + accumulate(row.begin(), row.end(), 0);
    });
  }
};
```

------------


# 1504. Count Submatrices With All Ones -> [LeetCode](https://leetcode.com/problems/count-submatrices-with-all-ones/description)
 
Medium
 
Given an m x n binary matrix mat, return the number of submatrices that have all ones.

 

Example 1:


Input: mat = [[1,0,1],[1,1,0],[1,1,0]]

Output: 13

![example](https://assets.leetcode.com/uploads/2021/10/27/ones1-grid.jpg)

Explanation: 
There are 6 rectangles of side 1x1.
There are 2 rectangles of side 1x2.
There are 3 rectangles of side 2x1.
There is 1 rectangle of side 2x2. 
There is 1 rectangle of side 3x1.
Total number of rectangles = 6 + 2 + 3 + 1 + 1 = 13.


Example 2:


Input: mat = [[0,1,1,0],[0,1,1,1],[1,1,1,0]]
Output: 24
Explanation: 
There are 8 rectangles of side 1x1.
There are 5 rectangles of side 1x2.
There are 2 rectangles of side 1x3. 
There are 4 rectangles of side 2x1.
There are 2 rectangles of side 2x2. 
There are 2 rectangles of side 3x1. 
There is 1 rectangle of side 3x2. 
Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24.
 

Constraints:

1 <= m, n <= 150
mat[i][j] is either 0 or 1.

# Code
```cpp []
class Solution {
 public:
  int numSubmat(vector<vector<int>>& mat) {
    const int m = mat.size();
    const int n = mat[0].size();
    int ans = 0;

    for (int baseRow = 0; baseRow < m; ++baseRow) {
      vector<int> row(n, 1);
      for (int i = baseRow; i < m; ++i) {
        for (int j = 0; j < n; ++j)
          row[j] &= mat[i][j];
        ans += count(row);
      }
    }

    return ans;
  }

 private:
  int count(vector<int>& row) {
    int res = 0;
    int length = 0;
    for (const int num : row) {
      length = num == 0 ? 0 : length + 1;
      res += length;
    }
    return res;
  }
};
```


--------------------------



# 560. Subarray Sum Equals K -> [LeetCode](https://leetcode.com/problems/subarray-sum-equals-k/description/)
 
Medium

Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,1,1], k = 2
Output: 2


Example 2:

Input: nums = [1,2,3], k = 3
Output: 2
 

Constraints:

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

# Code
```cpp []
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int ans = 0;
        int n = nums.size();
        unordered_map<long long , int> preSum;
        preSum[0] = 1;
        long long sum = 0;
        for(int i=0 ; i<n ; ++i){
            sum += nums[i];
            long long rem = sum - k;
            if(preSum.count(rem)) ans += preSum[rem];
            preSum[sum]++;
        }
        return ans;
    }
};
```

-----------



# 3195. Find the Minimum Area to Cover All Ones I -> [LeetCode](https://leetcode.com/problems/find-the-minimum-area-to-cover-all-ones-i/)
 
Medium
 
You are given a 2D binary array grid. Find a rectangle with horizontal and vertical sides with the smallest area, such that all the 1's in grid lie inside this rectangle.

Return the minimum possible area of the rectangle.

 

Example 1:

Input: grid = [[0,1,0],[1,0,1]]

Output: 6

Explanation:

![example](https://assets.leetcode.com/uploads/2024/05/08/examplerect0.png)


The smallest rectangle has a height of 2 and a width of 3, so it has an area of 2 * 3 = 6.

Example 2:

Input: grid = [[1,0],[0,0]]

Output: 1

Explanation:

![example](https://assets.leetcode.com/uploads/2024/05/08/examplerect1.png)


The smallest rectangle has both height and width 1, so its area is 1 * 1 = 1.

 

Constraints:

1 <= grid.length, grid[i].length <= 1000
grid[i][j] is either 0 or 1.
The input is generated such that there is at least one 1 in grid.

# Code
```cpp []
class Solution {
 public:
  int minimumArea(vector<vector<int>>& grid) {
    int x1 = INT_MAX;
    int y1 = INT_MAX;
    int x2 = 0;
    int y2 = 0;

    for (int i = 0; i < grid.size(); ++i)
      for (int j = 0; j < grid[0].size(); ++j)
        if (grid[i][j] == 1) {
          x1 = min(x1, i);
          y1 = min(y1, j);
          x2 = max(x2, i);
          y2 = max(y2, j);
        }

    return (x2 - x1 + 1) * (y2 - y1 + 1);
  }
};

```

--------------



# 3197. Find the Minimum Area to Cover All Ones II -> [LeetCode](https://leetcode.com/problems/find-the-minimum-area-to-cover-all-ones-ii/description)
 
Hard
 
You are given a 2D binary array grid. You need to find 3 non-overlapping rectangles having non-zero areas with horizontal and vertical sides such that all the 1's in grid lie inside these rectangles.

Return the minimum possible sum of the area of these rectangles.

Note that the rectangles are allowed to touch.

 

Example 1:

Input: grid = [[1,0,1],[1,1,1]]

Output: 5

Explanation:

![image](https://assets.leetcode.com/uploads/2024/05/14/example0rect21.png)

The 1's at (0, 0) and (1, 0) are covered by a rectangle of area 2.
The 1's at (0, 2) and (1, 2) are covered by a rectangle of area 2.
The 1 at (1, 1) is covered by a rectangle of area 1.


Example 2:

Input: grid = [[1,0,1,0],[0,1,0,1]]

Output: 5

Explanation:

![image](https://assets.leetcode.com/uploads/2024/05/14/example1rect2.png)

The 1's at (0, 0) and (0, 2) are covered by a rectangle of area 3.
The 1 at (1, 1) is covered by a rectangle of area 1.
The 1 at (1, 3) is covered by a rectangle of area 1.
 

Constraints:

1 <= grid.length, grid[i].length <= 30
grid[i][j] is either 0 or 1.
The input is generated such that there are at least three 1's in grid.

# Code
```cpp []
class Solution {
 public:
  int minimumSum(vector<vector<int>>& grid) {
    const int m = grid.size();
    const int n = grid[0].size();
    int ans = m * n;

    for (int i = 0; i < m; ++i) {
      const int top = minimumArea(grid, 0, i, 0, n - 1);
      for (int j = 0; j < n; ++j)
        ans = min(ans,
                  top + /*left*/ minimumArea(grid, i + 1, m - 1, 0, j) +
                      /*right*/ minimumArea(grid, i + 1, m - 1, j + 1, n - 1));
    }

    for (int i = 0; i < m; ++i) {
      const int bottom = minimumArea(grid, i, m - 1, 0, n - 1);
      for (int j = 0; j < n; ++j)
        ans = min(ans, bottom + /*left*/ minimumArea(grid, 0, i - 1, 0, j) +
                           /*right*/ minimumArea(grid, 0, i - 1, j + 1, n - 1));
    }

    for (int j = 0; j < n; ++j) {
      const int left = minimumArea(grid, 0, m - 1, 0, j);
      for (int i = 0; i < m; ++i)
        ans = min(ans,
                  left + /*top*/ minimumArea(grid, 0, i, j + 1, n - 1) +
                      /*bottom*/ minimumArea(grid, i + 1, m - 1, j + 1, n - 1));
    }

    for (int j = 0; j < n; ++j) {
      const int right = minimumArea(grid, 0, m - 1, j, n - 1);
      for (int i = 0; i < m; ++i)
        ans =
            min(ans, right + /*top*/ minimumArea(grid, 0, i, 0, j - 1) +
                         /*bottom*/ minimumArea(grid, i + 1, m - 1, 0, j - 1));
    }

    for (int i1 = 0; i1 < m; ++i1)
      for (int i2 = i1 + 1; i2 < m; ++i2)
        ans =
            min(ans, /*top*/ minimumArea(grid, 0, i1, 0, n - 1) +
                         /*middle*/ minimumArea(grid, i1 + 1, i2, 0, n - 1) +
                         /*bottom*/ minimumArea(grid, i2 + 1, m - 1, 0, n - 1));

    for (int j1 = 0; j1 < n; ++j1)
      for (int j2 = j1 + 1; j2 < n; ++j2)
        ans =
            min(ans, /*left*/ minimumArea(grid, 0, m - 1, 0, j1) +
                         /*middle*/ minimumArea(grid, 0, m - 1, j1 + 1, j2) +
                         /*right*/ minimumArea(grid, 0, m - 1, j2 + 1, n - 1));

    return ans;
  }

 private:
  int minimumArea(vector<vector<int>>& grid, int si, int ei, int sj, int ej) {
    int x1 = INT_MAX;
    int y1 = INT_MAX;
    int x2 = 0;
    int y2 = 0;
    for (int i = si; i <= ei; ++i)
      for (int j = sj; j <= ej; ++j)
        if (grid[i][j] == 1) {
          x1 = min(x1, i);
          y1 = min(y1, j);
          x2 = max(x2, i);
          y2 = max(y2, j);
        }
    return x1 == INT_MAX ? 0 : (x2 - x1 + 1) * (y2 - y1 + 1);
  }
};
```

-----------




# 1493. Longest Subarray of 1's After Deleting One Element -> [LeetCode](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/description)
 
Medium
 
Given a binary array nums, you should delete one element from it.

Return the size of the longest non-empty subarray containing only 1's in the resulting array. Return 0 if there is no such subarray.

 

Example 1:

Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.


Example 2:

Input: nums = [0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].


Example 3:

Input: nums = [1,1,1]
Output: 2
Explanation: You must delete one element.
 

Constraints:

1 <= nums.length <= 105
nums[i] is either 0 or 1.

# Code
```cpp []
class Solution {
 public:
  int longestSubarray(vector<int>& nums) {
    int ans = 0;
    int zeros = 0;

    for (int l = 0, r = 0; r < nums.size(); ++r) {
      if (nums[r] == 0)
        ++zeros;
      while (zeros == 2)
        if (nums[l++] == 0)
          --zeros;
      ans = max(ans, r - l);
    }

    return ans;
  }
};
```


-------------




# 409. Longest Palindrome -> [LeetCode](https://leetcode.com/problems/longest-palindrome/description/)
 
Easy
 
Given a string s which consists of lowercase or uppercase letters, return the length of the longest palindrome that can be built with those letters.

Letters are case sensitive, for example, "Aa" is not considered a palindrome.

 

Example 1:

Input: s = "abccccdd"
Output: 7
Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.


Example 2:

Input: s = "a"
Output: 1
Explanation: The longest palindrome that can be built is "a", whose length is 1.
 

Constraints:

1 <= s.length <= 2000
s consists of lowercase and/or uppercase English letters only.

# Code
```cpp []
class Solution {
public:
    int longestPalindrome(string s) {
        int oddCount = 0;
        unordered_map<char,int> freq;
        for(const char c:s){
            freq[c]++;
            if(freq[c] % 2 == 1) oddCount++;
            else oddCount--;
        }
        return (oddCount > 1) ? (s.length() - oddCount + 1) : s.length();
    }
};
```

-------------




# 498. Diagonal Traverse -> [LeetCode](https://leetcode.com/problems/diagonal-traverse/description/)
 
Medium
 
Given an m x n matrix mat, return an array of all the elements of the array in a diagonal order.

 

Example 1:


Input: mat = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,4,7,5,3,6,8,9]

![image](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)
<br/>

Example 2:

Input: mat = [[1,2],[3,4]]
Output: [1,2,3,4]
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
-105 <= mat[i][j] <= 105

# Code
```cpp []
class Solution {
 public:
  vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
    const int m = matrix.size();
    const int n = matrix[0].size();
    vector<int> ans(m * n);
    int d = 1;  // left-bottom -> right-top
    int row = 0;
    int col = 0;

    for (int i = 0; i < m * n; ++i) {
      ans[i] = matrix[row][col];
      row -= d;
      col += d;
      // out-of-bounds
      if (row == m)
        row = m - 1, col += 2, d = -d;
      if (col == n)
        col = n - 1, row += 2, d = -d;
      if (row < 0)
        row = 0, d = -d;
      if (col < 0)
        col = 0, d = -d;
    }

    return ans;
  }
};
```


---------------



# 3000. Maximum Area of Longest Diagonal Rectangle -> [LeetCode](https://leetcode.com/problems/maximum-area-of-longest-diagonal-rectangle/description)
 
Easy
 
You are given a 2D 0-indexed integer array dimensions.

For all indices i, 0 <= i < dimensions.length, dimensions[i][0] represents the length and dimensions[i][1] represents the width of the rectangle i.

Return the area of the rectangle having the longest diagonal. If there are multiple rectangles with the longest diagonal, return the area of the rectangle having the maximum area.

 

Example 1:

Input: dimensions = [[9,3],[8,6]]
Output: 48
Explanation: 
For index = 0, length = 9 and width = 3. Diagonal length = sqrt(9 * 9 + 3 * 3) = sqrt(90) ≈ 9.487.
For index = 1, length = 8 and width = 6. Diagonal length = sqrt(8 * 8 + 6 * 6) = sqrt(100) = 10.
So, the rectangle at index 1 has a greater diagonal length therefore we return area = 8 * 6 = 48.


Example 2:

Input: dimensions = [[3,4],[4,3]]
Output: 12
Explanation: Length of diagonal is the same for both which is 5, so maximum area = 12.
 

Constraints:

1 <= dimensions.length <= 100
dimensions[i].length == 2
1 <= dimensions[i][0], dimensions[i][1] <= 100

# Code
```cpp []
class Solution {
 public:
  int areaOfMaxDiagonal(vector<vector<int>>& dimensions) {
    const vector<int> maxDimension = *ranges::max_element(
        dimensions, [](const vector<int>& a, const vector<int>& b) {
      return (a[0] * a[0] + a[1] * a[1] == b[0] * b[0] + b[1] * b[1])
                 ? (a[0] * a[1] < b[0] * b[1])
                 : (a[0] * a[0] + a[1] * a[1] < b[0] * b[0] + b[1] * b[1]);
    });
    return maxDimension[0] * maxDimension[1];
  }
};
```

---------------------



# 1283. Find the Smallest Divisor Given a Threshold -> [LeetCode](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/description/)
 
Medium
 
Given an array of integers nums and an integer threshold, we will choose a positive integer divisor, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to threshold.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).

The test cases are generated so that there will be an answer.

 

Example 1:

Input: nums = [1,2,5,9], threshold = 6
Output: 5
Explanation: We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2). 


Example 2:

Input: nums = [44,22,33,11,1], threshold = 5
Output: 44
 

Constraints:

1 <= nums.length <= 5 * 104
1 <= nums[i] <= 106
nums.length <= threshold <= 106

# Code
```cpp []
class Solution {
    bool isPossible(vector<int>& nums, int div , int th){
        int res = 0;
        for(const int i:nums) res += ceil((double)i/(double)div);
        return res <= th;
    } 
public:
    int smallestDivisor(vector<int>& nums, int th) {
        int n = nums.size();
        sort(nums.begin() , nums.end());
        int low = 1 , high = nums[n-1];
        
        while(low<=high){
            int mid = low + (high - low)/2;
            if(isPossible(nums,mid,th)) high = mid - 1;
            else low = mid + 1;
        }

        return low;

    }
};
```


------------------



# 1011. Capacity To Ship Packages Within D Days -> [LeetCode](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/)
 
Medium
 
A conveyor belt has packages that must be shipped from one port to another within days days.

The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within days days.

 

Example 1:

Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.


Example 2:

Input: weights = [3,2,2,4,1,4], days = 3
Output: 6
Explanation: A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4


Example 3:

Input: weights = [1,2,3,1,1], days = 4
Output: 3
Explanation:
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
 

Constraints:

1 <= days <= weights.length <= 5 * 104
1 <= weights[i] <= 500

# Code
```cpp []
class Solution {
private:
    bool isPoosible(vector<int>& arr, int cap , int days){
        int d = 1;
        int load = 0;
        for(const int i:arr){
            if(load + i > cap) d++ , load = i;
            else load += i;
        }
        return d <= days;
    }

public:
    int shipWithinDays(vector<int>& arr, int days) {
        int low = *max_element(arr.begin() , arr.end());
        int high = accumulate(arr.begin() , arr.end() , 0);
        while(low<=high){
            int mid = low + (high - low)/2;
            if(isPoosible(arr,mid,days)) high = mid - 1;
            else low = mid + 1;
        }
        return low;
    }
};
```

-----------------



# 3459. Length of Longest V-Shaped Diagonal Segment -> [LeetCode](https://leetcode.com/problems/length-of-longest-v-shaped-diagonal-segment/description/)

 
Hard
 
You are given a 2D integer matrix grid of size n x m, where each element is either 0, 1, or 2.

A V-shaped diagonal segment is defined as:

The segment starts with 1.
The subsequent elements follow this infinite sequence: 2, 0, 2, 0, ....
The segment:
Starts along a diagonal direction (top-left to bottom-right, bottom-right to top-left, top-right to bottom-left, or bottom-left to top-right).
Continues the sequence in the same diagonal direction.
Makes at most one clockwise 90-degree turn to another diagonal direction while maintaining the sequence.

![image](https://assets.leetcode.com/uploads/2025/01/11/length_of_longest3.jpg)


Return the length of the longest V-shaped diagonal segment. If no valid segment exists, return 0.

 

Example 1:

Input: grid = [[2,2,1,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

Output: 5
![image](https://assets.leetcode.com/uploads/2024/12/09/matrix_1-2.jpg)

Explanation:

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: (0,2) → (1,3) → (2,4), takes a 90-degree clockwise turn at (2,4), and continues as (3,3) → (4,2).


Example 2:

Input: grid = [[2,2,2,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

Output: 4
![image](https://assets.leetcode.com/uploads/2024/12/09/matrix_2.jpg)

Explanation:

The longest V-shaped diagonal segment has a length of 4 and follows these coordinates: (2,3) → (3,2), takes a 90-degree clockwise turn at (3,2), and continues as (2,1) → (1,0).


Example 3:

Input: grid = [[1,2,2,2,2],[2,2,2,2,0],[2,0,0,0,0],[0,0,2,2,2],[2,0,0,2,0]]

Output: 5
![image](https://assets.leetcode.com/uploads/2024/12/09/matrix_3.jpg)

Explanation:

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: (0,0) → (1,1) → (2,2) → (3,3) → (4,4).


Example 4:

Input: grid = [[1]]

Output: 1

Explanation:

The longest V-shaped diagonal segment has a length of 1 and follows these coordinates: (0,0).

 

Constraints:

n == grid.length
m == grid[i].length
1 <= n, m <= 500
grid[i][j] is either 0, 1 or 2.

# Code
```cpp []
constexpr int d[5] = {1, 1, -1, -1, 1};//(1,1),(1,-1),(-1, -1),(-1, 1)
int dp[500][500][4][2];
class Solution {
public:
    int n, m;
    bool isOutSide(int i, int j) { 
        return i<0 || i>=n || j<0 || j>=m; 
    }
    int dfs(int i, int j, int dir, bool turn, int nxt, vector<vector<int>>& grid) {
        if (dp[i][j][dir][turn]!=-1) return dp[i][j][dir][turn];
        
        int ans=1, x=grid[i][j];

        // Move in the current direction
        int s=i+d[dir], t=j+d[dir+1];
        if (!isOutSide(s, t)) { 
            int y=grid[s][t];
            if (nxt==y)  // continue moving in the same diagonal
                ans=max(ans, 1+dfs(s, t, dir, turn, nxt^2, grid));
        }

        // Try turning to the next diagonal direction
        if (!turn) { 
            const int newDir=(dir+1)&3;// same %4
            int u=i+d[newDir], v=j+d[newDir+1];
            if (!isOutSide(u, v)) {
                const int z=grid[u][v];
                if (nxt==z)  // to turn to the next diagonal
                    ans=max(ans, 1+dfs(u, v, newDir, 1, nxt^2, grid));
            }
        }

        return dp[i][j][dir][turn]=ans;
    }

    int lenOfVDiagonal(vector<vector<int>>& grid) {
        n=grid.size(), m=grid[0].size();
        
        for (int i=0; i<n; i++)
            for(int j=0; j<m; j++)
                memset(dp[i][j], -1, sizeof(dp[i][j]));

        int ans=0;
        for (int i=0; i<n; i++) {
            for (int j=0; j<m; j++) {
                if (grid[i][j]==1) { // Start from cells that are part of the path
                    for (int d=0; d<4; d++) 
                        ans=max(ans, dfs(i, j, d, 0, 2,  grid));
                }
            }
        }
        return ans;
    }
};
```

-----------------------------


# 3446. Sort Matrix by Diagonals -> [LeetCode](https://leetcode.com/problems/sort-matrix-by-diagonals/description)
 
Medium

You are given an n x n square matrix of integers grid. Return the matrix such that:

The diagonals in the bottom-left triangle (including the middle diagonal) are sorted in non-increasing order.
The diagonals in the top-right triangle are sorted in non-decreasing order.
 

Example 1:

Input: grid = [[1,7,3],[9,8,2],[4,5,6]]

Output: [[8,2,3],[9,6,7],[4,5,1]]

Explanation:

![image](https://assets.leetcode.com/uploads/2024/12/29/4052example1drawio.png)

The diagonals with a black arrow (bottom-left triangle) should be sorted in non-increasing order:

[1, 8, 6] becomes [8, 6, 1].
[9, 5] and [4] remain unchanged.
The diagonals with a blue arrow (top-right triangle) should be sorted in non-decreasing order:

[7, 2] becomes [2, 7].
[3] remains unchanged.


Example 2:

Input: grid = [[0,1],[1,2]]

Output: [[2,1],[1,0]]

Explanation:

![image](https://assets.leetcode.com/uploads/2024/12/29/4052example2adrawio.png)


The diagonals with a black arrow must be non-increasing, so [0, 2] is changed to [2, 0]. The other diagonals are already in the correct order.

Example 3:

Input: grid = [[1]]

Output: [[1]]

Explanation:

Diagonals with exactly one element are already in order, so no changes are needed.

 

Constraints:

grid.length == grid[i].length == n
1 <= n <= 10
-105 <= grid[i][j] <= 105

# Code
```cpp []
class Solution {
 public:
  vector<vector<int>> sortMatrix(vector<vector<int>>& grid) {
    const int n = grid.size();
    vector<vector<int>> ans(n, vector<int>(n));
    vector<vector<int>> diag(2 * n + 1);

    for (int i = 0; i < n; ++i)
      for (int j = 0; j < n; ++j)
        diag[i - j + n].push_back(grid[i][j]);

    for (int i = 0; i < 2 * n + 1; ++i)
      if (i < n)
        ranges::sort(diag[i], greater<int>());
      else
        ranges::sort(diag[i]);

    for (int i = 0; i < n; ++i)
      for (int j = 0; j < n; ++j)
        ans[i][j] = diag[i - j + n].back(), diag[i - j + n].pop_back();

    return ans;
  }
};
```

---------------



# 3021. Alice and Bob Playing Flower Game -> [LeetCode](https://leetcode.com/problems/alice-and-bob-playing-flower-game/description/)
 
Medium
 
Alice and Bob are playing a turn-based game on a field, with two lanes of flowers between them. There are x flowers in the first lane between Alice and Bob, and y flowers in the second lane between them.



The game proceeds as follows:

![image](https://assets.leetcode.com/uploads/2025/08/27/3021.png)

Alice takes the first turn.
In each turn, a player must choose either one of the lane and pick one flower from that side.
At the end of the turn, if there are no flowers left at all, the current player captures their opponent and wins the game.
Given two integers, n and m, the task is to compute the number of possible pairs (x, y) that satisfy the conditions:

Alice must win the game according to the described rules.
The number of flowers x in the first lane must be in the range [1,n].
The number of flowers y in the second lane must be in the range [1,m].
Return the number of possible pairs (x, y) that satisfy the conditions mentioned in the statement.

 

Example 1:

Input: n = 3, m = 2
Output: 3
Explanation: The following pairs satisfy conditions described in the statement: (1,2), (3,2), (2,1).


Example 2:

Input: n = 1, m = 1
Output: 0
Explanation: No pairs satisfy the conditions described in the statement.
 

Constraints:

1 <= n, m <= 105


# Code
```cpp []
class Solution {
public:
    long long flowerGame(int n, int m) {
        const int xEven = n / 2;
        const int yEven = m / 2;
        const int xOdd = (n + 1) / 2;
        const int yOdd = (m + 1) / 2;
        return static_cast<long>(xEven) * yOdd +
               static_cast<long>(yEven) * xOdd;
    }
};
```

------------------


# 36. Valid Sudoku -> [LeetCode](https://leetcode.com/problems/valid-sudoku/description)
 
Medium
 
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
Note:

A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
 

Example 1:

![image](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true


Example 2:

Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
 

Constraints:

board.length == 9
board[i].length == 9
board[i][j] is a digit 1-9 or '.'.

# Code
```cpp []
class Solution {
 public:
  bool isValidSudoku(vector<vector<char>>& board) {
    unordered_set<string> seen;

    for (int i = 0; i < 9; ++i)
      for (int j = 0; j < 9; ++j) {
        if (board[i][j] == '.')
          continue;
        const string c(1, board[i][j]);
        if (!seen.insert(c + "@row" + to_string(i)).second ||
            !seen.insert(c + "@col" + to_string(j)).second ||
            !seen.insert(c + "@box" + to_string(i / 3) + to_string(j / 3))
                 .second)
          return false;
      }

    return true;
  }
};
```


------------------

# Reverse a Doubly Linked List -> [GFG](https://www.geeksforgeeks.org/problems/reverse-a-doubly-linked-list/1)

Difficulty: Easy

You are given the head of a doubly linked list. You have to reverse the doubly linked list and return its head.

Examples:

Input: head = 3 <-> 4 <-> 5
Output: 5 <-> 4 <-> 3

Input: head = 75 <-> 122 <-> 59 <-> 196
Output: 196 <-> 59 <-> 122 <-> 75


Constraints:

1 ≤ number of nodes ≤ 106
0 ≤ node->data ≤ 104


Expected Complexities

Time Complexity: O(n)
Auxiliary Space: O(1)


Company Tags

D-E-ShawAdobe


```cpp []
/*
class Node {
  public:
    int data;
    Node *next;
    Node *prev;
    Node(int val) {
        data = val;
        next = NULL;
        prev = NULL;
    }
};

*/
class Solution {
  public:
    Node *reverse(Node *head) {
        Node* dummy = new Node(-1);
        
        Node* rev = dummy;
        Node* cur = head;
        
        while(cur->next) cur = cur->next;
        
        while(cur->prev){
            Node* pre = cur->prev;
            rev->next = cur;
            cur->prev = rev;
            rev = cur;
            cur = pre;
        }
        
        rev->next = cur;
        cur->prev = rev;
        rev = cur;
        rev->next = nullptr;
        
        return dummy->next;
        
        
    }
};
```

---------------------------------



# 876. Middle of the Linked List -> [LeetCode](https://leetcode.com/problems/middle-of-the-linked-list/description/)
 
Easy

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

 

Example 1:


Input: head = [1,2,3,4,5]

![image](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

Output: [3,4,5]
Explanation: The middle node of the list is node 3.


Example 2:


Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
 

Constraints:

The number of nodes in the list is in the range [1, 100].
1 <= Node.val <= 100

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
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

-----------


# 37. Sudoku Solver -> [LeetCode](https://leetcode.com/problems/sudoku-solver/description)
 
Hard
 
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
The '.' character indicates empty cells.

 

Example 1:

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]


Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]

Explanation: The input board is shown above and the only valid solution is shown below:

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
 

Constraints:

board.length == 9
board[i].length == 9
board[i][j] is a digit or '.'.
It is guaranteed that the input board has only one solution.
 


# Code
```cpp []
class Solution {
 public:
  void solveSudoku(vector<vector<char>>& board) {
    solve(board, 0);
  }

 private:
  bool solve(vector<vector<char>>& board, int s) {
    if (s == 81)
      return true;

    const int i = s / 9;
    const int j = s % 9;

    if (board[i][j] != '.')
      return solve(board, s + 1);

    for (char c = '1'; c <= '9'; ++c)
      if (isValid(board, i, j, c)) {
        board[i][j] = c;
        if (solve(board, s + 1))
          return true;
        board[i][j] = '.';
      }

    return false;
  }

  bool isValid(vector<vector<char>>& board, int row, int col, char c) {
    for (int i = 0; i < 9; ++i)
      if (board[i][col] == c || board[row][i] == c ||
          board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c)
        return false;
    return true;
  }
};
```

---------------
-----------------------------------------------------------------------

![badge](https://assets.leetcode.com/static_assets/marketing/202508.gif)

-----------------------------------------------------------------------


