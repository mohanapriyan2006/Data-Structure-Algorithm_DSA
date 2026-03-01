
----------------------------------
# LeetCode - ***september*** 2025
----------------------------------


# # 1792. Maximum Average Pass Ratio -> [LEETCODE](https://leetcode.com/problems/maximum-average-pass-ratio/description/)
 
Medium
 
There is a school that has classes of students and each class will be having a final exam. You are given a 2D integer array classes, where classes[i] = [passi, totali]. You know beforehand that in the ith class, there are totali total students, but only passi number of students will pass the exam.

You are also given an integer extraStudents. There are another extraStudents brilliant students that are guaranteed to pass the exam of any class they are assigned to. You want to assign each of the extraStudents students to a class in a way that maximizes the average pass ratio across all the classes.

The pass ratio of a class is equal to the number of students of the class that will pass the exam divided by the total number of students of the class. The average pass ratio is the sum of pass ratios of all the classes divided by the number of the classes.

Return the maximum possible average pass ratio after assigning the extraStudents students. Answers within 10-5 of the actual answer will be accepted.

 

Example 1:

Input: classes = [[1,2],[3,5],[2,2]], extraStudents = 2
Output: 0.78333
Explanation: You can assign the two extra students to the first class. The average pass ratio will be equal to (3/4 + 3/5 + 2/2) / 3 = 0.78333.


Example 2:

Input: classes = [[2,4],[3,9],[4,5],[2,10]], extraStudents = 4
Output: 0.53485
 

Constraints:

1 <= classes.length <= 105
classes[i].length == 2
1 <= passi <= totali <= 105
1 <= extraStudents <= 105


# Code
```cpp []
class Solution {
 public:
  double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) { 
    priority_queue<tuple<double, int, int>> maxHeap;

    for (const vector<int>& c : classes) {
      const int pass = c[0];
      const int total = c[1];
      maxHeap.emplace(extraPassRatio(pass, total), pass, total);
    }

    for (int i = 0; i < extraStudents; ++i) {
      const auto [_, pass, total] = maxHeap.top();
      maxHeap.pop();
      maxHeap.emplace(extraPassRatio(pass + 1, total + 1), pass + 1, total + 1);
    }

    double ratioSum = 0;

    while (!maxHeap.empty()) {
      const auto [_, pass, total] = maxHeap.top();
      maxHeap.pop();
      ratioSum += pass / static_cast<double>(total);
    }

    return ratioSum / classes.size();
  }

 private: 
  double extraPassRatio(int pass, int total) {
    return (pass + 1) / static_cast<double>(total + 1) -
           pass / static_cast<double>(total);
  }
};
```


-----------------


# 3025. Find the Number of Ways to Place People I -> [LeetCode](https://leetcode.com/problems/find-the-number-of-ways-to-place-people-i/description)
 
Medium
 
You are given a 2D array points of size n x 2 representing integer coordinates of some points on a 2D plane, where points[i] = [xi, yi].

Count the number of pairs of points (A, B), where

A is on the upper left side of B, and
there are no other points in the rectangle (or line) they make (including the border).
Return the count.

 

Example 1:

Input: points = [[1,1],[2,2],[3,3]]

Output: 0

Explanation:

![img](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

There is no way to choose A and B so A is on the upper left side of B.



Example 2:

Input: points = [[6,2],[4,4],[2,6]]

Output: 2

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/25/t2.jpg)

The left one is the pair (points[1], points[0]), where points[1] is on the upper left side of points[0] and the rectangle is empty.
The middle one is the pair (points[2], points[1]), same as the left one it is a valid pair.
The right one is the pair (points[2], points[0]), where points[2] is on the upper left side of points[0], but points[1] is inside the rectangle so it's not a valid pair.


Example 3:

Input: points = [[3,1],[1,3],[1,1]]

Output: 2

Explanation:

![img](https://assets.leetcode.com/uploads/2024/06/25/t3.jpg)

The left one is the pair (points[2], points[0]), where points[2] is on the upper left side of points[0] and there are no other points on the line they form. Note that it is a valid state when the two points form a line.
The middle one is the pair (points[1], points[2]), it is a valid pair same as the left one.
The right one is the pair (points[1], points[0]), it is not a valid pair as points[2] is on the border of the rectangle.
 

Constraints:

2 <= n <= 50
points[i].length == 2
0 <= points[i][0], points[i][1] <= 50
All points[i] are distinct.

# Code
```cpp []
class Solution {
 public:
  int numberOfPairs(vector<vector<int>>& points) {
    int ans = 0;

    ranges::sort(points, ranges::less{}, [](const vector<int>& point) {
      const int x = point[0];
      const int y = point[1];
      return pair<int, int>{x, -y};
    });

    for (int i = 0; i < points.size(); ++i) {
      int maxY = INT_MIN;
      for (int j = i + 1; j < points.size(); ++j)
        if (points[i][1] >= points[j][1] && points[j][1] > maxY) {
          ++ans;
          maxY = points[j][1];
        }
    }

    return ans;
  }
};
```

-------------------------


# 3027. Find the Number of Ways to Place People II -> [LeetCode](https://leetcode.com/problems/find-the-number-of-ways-to-place-people-ii/description)
 
Hard

You are given a 2D array points of size n x 2 representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

We define the right direction as positive x-axis (increasing x-coordinate) and the left direction as negative x-axis (decreasing x-coordinate). Similarly, we define the up direction as positive y-axis (increasing y-coordinate) and the down direction as negative y-axis (decreasing y-coordinate)

You have to place n people, including Alice and Bob, at these points such that there is exactly one person at every point. Alice wants to be alone with Bob, so Alice will build a rectangular fence with Alice's position as the upper left corner and Bob's position as the lower right corner of the fence (Note that the fence might not enclose any area, i.e. it can be a line). If any person other than Alice and Bob is either inside the fence or on the fence, Alice will be sad.

Return the number of pairs of points where you can place Alice and Bob, such that Alice does not become sad on building the fence.

Note that Alice can only build a fence with Alice's position as the upper left corner, and Bob's position as the lower right corner. For example, Alice cannot build either of the fences in the picture below with four corners (1, 1), (1, 3), (3, 1), and (3, 3), because:

With Alice at (3, 3) and Bob at (1, 1), Alice's position is not the upper left corner and Bob's position is not the lower right corner of the fence.
With Alice at (1, 3) and Bob at (1, 1), Bob's position is not the lower right corner of the fence.

![img](https://assets.leetcode.com/uploads/2024/01/04/example0alicebob-1.png)
 

Example 1:


Input: points = [[1,1],[2,2],[3,3]]
Output: 0
Explanation: There is no way to place Alice and Bob such that Alice can build a fence with Alice's position as the upper left corner and Bob's position as the lower right corner. Hence we return 0. 

![img](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

Example 2:


Input: points = [[6,2],[4,4],[2,6]]
Output: 2
Explanation: There are two ways to place Alice and Bob such that Alice will not be sad:
- Place Alice at (4, 4) and Bob at (6, 2).
- Place Alice at (2, 6) and Bob at (4, 4).
You cannot place Alice at (2, 6) and Bob at (6, 2) because the person at (4, 4) will be inside the fence.

![img](https://assets.leetcode.com/uploads/2024/02/04/example2alicebob.png)

Example 3:


Input: points = [[3,1],[1,3],[1,1]]
Output: 2
Explanation: There are two ways to place Alice and Bob such that Alice will not be sad:
- Place Alice at (1, 1) and Bob at (3, 1).
- Place Alice at (1, 3) and Bob at (1, 1).
You cannot place Alice at (1, 3) and Bob at (3, 1) because the person at (1, 1) will be on the fence.
Note that it does not matter if the fence encloses any area, the first and second fences in the image are valid.


![img](https://assets.leetcode.com/uploads/2024/02/04/example4alicebob.png)

Constraints:

2 <= n <= 1000
points[i].length == 2
-109 <= points[i][0], points[i][1] <= 109
All points[i] are distinct.


# Code
```cpp []
class Solution {
 public:
 
  int numberOfPairs(vector<vector<int>>& points) {
    int ans = 0;

    ranges::sort(points, ranges::less{}, [](const vector<int>& point) {
      const int x = point[0];
      const int y = point[1];
      return pair<int, int>{x, -y};
    });

    for (int i = 0; i < points.size(); ++i) {
      int maxY = INT_MIN;
      for (int j = i + 1; j < points.size(); ++j)
        if (points[i][1] >= points[j][1] && points[j][1] > maxY) {
          ++ans;
          maxY = points[j][1];
        }
    }

    return ans;
  }
};



```

--------------------


# 3516. Find Closest Person -> [LeetCode](https://leetcode.com/problems/find-closest-person/description/)
 
Easy

You are given three integers x, y, and z, representing the positions of three people on a number line:

x is the position of Person 1.
y is the position of Person 2.
z is the position of Person 3, who does not move.
Both Person 1 and Person 2 move toward Person 3 at the same speed.

Determine which person reaches Person 3 first:

Return 1 if Person 1 arrives first.
Return 2 if Person 2 arrives first.
Return 0 if both arrive at the same time.
Return the result accordingly.

 

Example 1:

Input: x = 2, y = 7, z = 4

Output: 1

Explanation:

Person 1 is at position 2 and can reach Person 3 (at position 4) in 2 steps.
Person 2 is at position 7 and can reach Person 3 in 3 steps.
Since Person 1 reaches Person 3 first, the output is 1.


Example 2:

Input: x = 2, y = 5, z = 6

Output: 2

Explanation:

Person 1 is at position 2 and can reach Person 3 (at position 6) in 4 steps.
Person 2 is at position 5 and can reach Person 3 in 1 step.
Since Person 2 reaches Person 3 first, the output is 2.


Example 3:

Input: x = 1, y = 5, z = 3

Output: 0

Explanation:

Person 1 is at position 1 and can reach Person 3 (at position 3) in 2 steps.
Person 2 is at position 5 and can reach Person 3 in 2 steps.
Since both Person 1 and Person 2 reach Person 3 at the same time, the output is 0.

 

Constraints:

1 <= x, y, z <= 100


# Code
```cpp []
class Solution {
public:
    int findClosest(int x, int y, int z) {
        int p1 = abs(z-x);
        int p2 = abs(z-y);
        if(p1 < p2) return 1;
        if(p1 > p2) return 2;
        return 0;
    }
};
```

---------------------


# 2749. Minimum Operations to Make the Integer Zero -> [LeetCode](https://leetcode.com/problems/minimum-operations-to-make-the-integer-zero/description)
 
Medium
 
You are given two integers num1 and num2.

In one operation, you can choose integer i in the range [0, 60] and subtract 2i + num2 from num1.

Return the integer denoting the minimum number of operations needed to make num1 equal to 0.

If it is impossible to make num1 equal to 0, return -1.

 

Example 1:

Input: num1 = 3, num2 = -2

Output: 3

Explanation: We can make 3 equal to 0 with the following operations:
- We choose i = 2 and subtract 22 + (-2) from 3, 3 - (4 + (-2)) = 1.
- We choose i = 2 and subtract 22 + (-2) from 1, 1 - (4 + (-2)) = -1.
- We choose i = 0 and subtract 20 + (-2) from -1, (-1) - (1 + (-2)) = 0.
It can be proven, that 3 is the minimum number of operations that we need to perform.


Example 2:

Input: num1 = 5, num2 = 7

Output: -1

Explanation: It can be proven, that it is impossible to make 5 equal to 0 with the given operation.

 

Constraints:

1 <= num1 <= 109
-109 <= num2 <= 109


# Code
```cpp []
class Solution {
 public:
  int makeTheIntegerZero(int num1, int num2) {
    for (long ops = 0; ops <= 60; ++ops) {
      const long target = num1 - ops * num2;
      if (__builtin_popcountl(target) <= ops && ops <= target)
        return ops;
    }

    return -1;
  }
};
```


----------------------------------


# 3495. Minimum Operations to Make Array Elements Zero -> [Leetcode](https://leetcode.com/problems/minimum-operations-to-make-array-elements-zero/description/)
 
Hard

You are given a 2D array queries, where queries[i] is of the form [l, r]. Each queries[i] defines an array of integers nums consisting of elements ranging from l to r, both inclusive.

In one operation, you can:

Select two integers a and b from the array.
Replace them with floor(a / 4) and floor(b / 4).
Your task is to determine the minimum number of operations required to reduce all elements of the array to zero for each query. Return the sum of the results for all queries.

 

Example 1:

Input: queries = [[1,2],[2,4]]

Output: 3

Explanation:

For queries[0]:

The initial array is nums = [1, 2].
In the first operation, select nums[0] and nums[1]. The array becomes [0, 0].
The minimum number of operations required is 1.
For queries[1]:

The initial array is nums = [2, 3, 4].
In the first operation, select nums[0] and nums[2]. The array becomes [0, 3, 1].
In the second operation, select nums[1] and nums[2]. The array becomes [0, 0, 0].
The minimum number of operations required is 2.
The output is 1 + 2 = 3.

Example 2:

Input: queries = [[2,6]]

Output: 4

Explanation:

For queries[0]:

The initial array is nums = [2, 3, 4, 5, 6].
In the first operation, select nums[0] and nums[3]. The array becomes [0, 3, 4, 1, 6].
In the second operation, select nums[2] and nums[4]. The array becomes [0, 3, 1, 1, 1].
In the third operation, select nums[1] and nums[2]. The array becomes [0, 0, 0, 1, 1].
In the fourth operation, select nums[3] and nums[4]. The array becomes [0, 0, 0, 0, 0].
The minimum number of operations required is 4.
The output is 4.

 

Constraints:

1 <= queries.length <= 105
queries[i].length == 2
queries[i] == [l, r]
1 <= l < r <= 109


# Code

```cpp []
class Solution {
public:
    long long expSum4[18]={1};
    long long expSum(unsigned x){
        if (x==0) return 0;
        int log4=(31-countl_zero(x))/2;
        int r=x-(1<<(2*log4));
        return expSum4[log4]+r*(log4+1LL);
    }
    long long minOperations(vector<vector<int>>& queries) {
        for(int i=1; i<18; i++){
            expSum4[i]=expSum4[i-1]+3LL*i*(1LL<<(2*(i-1)))+1;
       
        }
        long long op=0;
        for(auto& q: queries){
            int l=q[0]-1, r=q[1];
            op+=(expSum(r)-expSum(l)+1)/2;
        }
        return op;
    }
};
```

-----------------------------------------------------


# 1304. Find N Unique Integers Sum up to Zero -> [LeetCode](https://leetcode.com/problems/find-n-unique-integers-sum-up-to-zero/description/)
 
Easy
 
Given an integer n, return any array containing n unique integers such that they add up to 0.

 

Example 1:

Input: n = 5

Output: [-7,-1,1,3,4]

Explanation: These arrays also are accepted [-5,-1,1,2,3] , [-3,-1,2,-2,4].


Example 2:

Input: n = 3

Output: [-1,0,1]


Example 3:

Input: n = 1

Output: [0]
 

Constraints:

1 <= n <= 1000



# Code
```cpp []
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> res(n);
        res[0] = n * (1 - n) / 2;
        for (int i = 1; i < n; ++i)
            res[i] = i;
        return res;
    }
};
```

------------------------------


# 1317. Convert Integer to the Sum of Two No-Zero Integers -> [LeetCode](https://leetcode.com/problems/convert-integer-to-the-sum-of-two-no-zero-integers/description/)
 
Easy
 
No-Zero integer is a positive integer that does not contain any 0 in its decimal representation.

Given an integer n, return a list of two integers [a, b] where:

a and b are No-Zero integers.
a + b = n
The test cases are generated so that there is at least one valid solution. If there are many valid solutions, you can return any of them.

 

Example 1:

Input: n = 2

Output: [1,1]

Explanation: Let a = 1 and b = 1.
Both a and b are no-zero integers, and a + b = 2 = n.


Example 2:

Input: n = 11

Output: [2,9]

Explanation: Let a = 2 and b = 9.
Both a and b are no-zero integers, and a + b = 11 = n.
Note that there are other valid answers as [8, 3] that can be accepted.
 

Constraints:

2 <= n <= 104


# Code
```cpp []
class Solution {
public:
    vector<int> getNoZeroIntegers(int n) {
        auto check = [](int num){
            while(num > 0){
                if(num % 10 == 0) return false;
                num /= 10;
            }
            return true;
        };

        for(int i=1 ; i<n ; ++i){
            int j = n - i;
            if(check(i) && check(j)) return {i,j};
        }

        return {};
    }
};
```

---------------------------------------------


# 2327. Number of People Aware of a Secret -> [LeetCode](https://leetcode.com/problems/number-of-people-aware-of-a-secret/description)
 
Medium
 
On day 1, one person discovers a secret.

You are given an integer delay, which means that each person will share the secret with a new person every day, starting from delay days after discovering the secret. You are also given an integer forget, which means that each person will forget the secret forget days after discovering it. A person cannot share the secret on the same day they forgot it, or on any day afterwards.

Given an integer n, return the number of people who know the secret at the end of day n. Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: n = 6, delay = 2, forget = 4

Output: 5

Explanation:

Day 1: Suppose the first person is named A. (1 person)
Day 2: A is the only person who knows the secret. (1 person)
Day 3: A shares the secret with a new person, B. (2 people)
Day 4: A shares the secret with a new person, C. (3 people)
Day 5: A forgets the secret, and B shares the secret with a new person, D. (3 people)
Day 6: B shares the secret with E, and C shares the secret with F. (5 people)


Example 2:

Input: n = 4, delay = 1, forget = 3

Output: 6

Explanation:

Day 1: The first person is named A. (1 person)
Day 2: A shares the secret with B. (2 people)
Day 3: A and B share the secret with 2 new people, C and D. (4 people)
Day 4: A forgets the secret. B, C, and D share the secret with 3 new people. (6 people)
 


Constraints:

2 <= n <= 1000
1 <= delay < forget <= n



# Code
```cpp []
class Solution {
public:
    int peopleAwareOfSecret(int n, int delay, int forget) {
        vector<long long> dp(n + 1, 0);
        dp[1] = 1;
        long long share = 0, MOD = 1000000007;
        for (int t = 2; t <= n; t++) {
            if (t - delay > 0)
                share = (share + dp[t - delay] + MOD) % MOD;
            if (t - forget > 0)
                share = (share - dp[t - forget] + MOD) % MOD;
            dp[t] = share;
        }
        long long know = 0;
        for (int i = n - forget + 1; i <= n; i++)
            know = (know + dp[i]) % MOD;
        return (int)know;
    }
};
```

-----------------------------------------------------------

# 1733. Minimum Number of People to Teach -> [LeetCode](https://leetcode.com/problems/minimum-number-of-people-to-teach/description)
 
Medium

On a social network consisting of m users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer n, an array languages, and an array friendships where:

There are n languages numbered 1 through n,
languages[i] is the set of languages the i​​​​​​th​​​​ user knows, and
friendships[i] = [u​​​​​​i​​​, v​​​​​​i] denotes a friendship between the users u​​​​​​​​​​​i​​​​​ and vi.
You can choose one language and teach it to some users so that all friends can communicate with each other. Return the minimum number of users you need to teach.

Note that friendships are not transitive, meaning if x is a friend of y and y is a friend of z, this doesn't guarantee that x is a friend of z.
 

Example 1:

Input: n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]

Output: 1

Explanation: You can either teach user 1 the second language or user 2 the first language.


Example 2:

Input: n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]

Output: 2

Explanation: Teach the third language to users 1 and 3, yielding two users to teach.
 

Constraints:

2 <= n <= 500
languages.length == m
1 <= m <= 500
1 <= languages[i].length <= n
1 <= languages[i][j] <= n
1 <= u​​​​​​i < v​​​​​​i <= languages.length
1 <= friendships.length <= 500
All tuples (u​​​​​i, v​​​​​​i) are unique
languages[i] contains only unique values


# Code

```cpp []
// use unordered_set for comparison
class Solution {
public:
    static bool intersection(unordered_set<int>& X, unordered_set<int>& Y){
        if (X.size()>Y.size()) return intersection(Y, X);
        for(int x: X){
            if (Y.count(x)) return 1;
        }
        return 0;
    }
    static int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        int m=languages.size(); // number of people

        // store known languages for each person
        vector<unordered_set<int>> know(m);
        for (int i=0; i<m; i++) 
            for (int l : languages[i]) know[i].insert(l);
        

        // people need be taught
        unordered_set<int> need;
        need.reserve(500);
        for (auto& f : friendships) {
            int a=f[0]-1, b=f[1]-1;
            if (intersection(know[a],know[b])) continue; // can talk
            need.insert(a);
            need.insert(b);
        }

        // if no need
        if (need.size()==0) return 0;

        int ans=INT_MAX;
        for (int lang=1; lang<=n; lang++) {   // languages for 1..n
            int cnt=0;
            for (int i: need) {
                if (!know[i].count(lang)) cnt++;
            }
            ans=min(ans, cnt);
        }

        return ans;
    }
};
```

---------------------------------------------



# 2785. Sort Vowels in a String -> [LeetCode](https://leetcode.com/problems/sort-vowels-in-a-string/description)
 
Medium
 
Given a 0-indexed string s, permute s to get a new string t such that:

All consonants remain in their original places. More formally, if there is an index i with 0 <= i < s.length such that s[i] is a consonant, then t[i] = s[i].
The vowels must be sorted in the nondecreasing order of their ASCII values. More formally, for pairs of indices i, j with 0 <= i < j < s.length such that s[i] and s[j] are vowels, then t[i] must not have a higher ASCII value than t[j].
Return the resulting string.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in lowercase or uppercase. Consonants comprise all letters that are not vowels.

 

Example 1:

Input: s = "lEetcOde"

Output: "lEOtcede"

Explanation: 'E', 'O', and 'e' are the vowels in s; 'l', 't', 'c', and 'd' are all consonants. The vowels are sorted according to their ASCII values, and the consonants remain in the same places.


Example 2:

Input: s = "lYmpH"

Output: "lYmpH"

Explanation: There are no vowels in s (all characters in s are consonants), so we return "lYmpH".
 

Constraints:

1 <= s.length <= 105
s consists only of letters of the English alphabet in uppercase and lowercase.


# Code
```cpp []
class Solution {
public:
    bool isVowel(char ch){
        if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u' ||
        ch=='A' || ch=='E' || ch=='I' || ch=='O' || ch=='U')return true;

        return false;
    }
    string sortVowels(string s) {
        int freq[128]={0};

        for(int i=0;i<s.size();i++){
            if(isVowel(s[i]))freq[(int)s[i]]++;
        }

        int idx=0;
        for(int i=0;i<s.size();i++){
            if(isVowel(s[i])){
                while(freq[idx]==0)idx++;
                s[i]=(char)idx;
                freq[idx]--;
            }
        }

        return s;
    }
};
```

---------------------------------------------------------------------

# 3227. Vowels Game in a String -> [LeetCode](https://leetcode.com/problems/vowels-game-in-a-string/description)
 
Medium

Alice and Bob are playing a game on a string.

You are given a string s, Alice and Bob will take turns playing the following game where Alice starts first:

On Alice's turn, she has to remove any non-empty substring from s that contains an odd number of vowels.
On Bob's turn, he has to remove any non-empty substring from s that contains an even number of vowels.
The first player who cannot make a move on their turn loses the game. We assume that both Alice and Bob play optimally.

Return true if Alice wins the game, and false otherwise.

The English vowels are: a, e, i, o, and u.

 

Example 1:

Input: s = "leetcoder"

Output: true

Explanation:
Alice can win the game as follows:

Alice plays first, she can delete the underlined substring in s = "leetcoder" which contains 3 vowels. The resulting string is s = "der".
Bob plays second, he can delete the underlined substring in s = "der" which contains 0 vowels. The resulting string is s = "er".
Alice plays third, she can delete the whole string s = "er" which contains 1 vowel.
Bob plays fourth, since the string is empty, there is no valid play for Bob. So Alice wins the game.



Example 2:

Input: s = "bbcd"

Output: false

Explanation:
There is no valid play for Alice in her first turn, so Alice loses the game.

 

Constraints:

1 <= s.length <= 105
s consists only of lowercase English letters.


# Code
```cpp []
class Solution {
public:
    bool doesAliceWin(string s) {
        for(const char c:s){
            if(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') return true;
        }
        return false;
    }
};
```


-------------------------------------------------------------------------


# 3541. Find Most Frequent Vowel and Consonant -> [LeetCode](https://leetcode.com/problems/find-most-frequent-vowel-and-consonant/description)
 
Easy
 
You are given a string s consisting of lowercase English letters ('a' to 'z').

Your task is to:

Find the vowel (one of 'a', 'e', 'i', 'o', or 'u') with the maximum frequency.
Find the consonant (all other letters excluding vowels) with the maximum frequency.
Return the sum of the two frequencies.

Note: If multiple vowels or consonants have the same maximum frequency, you may choose any one of them. If there are no vowels or no consonants in the string, consider their frequency as 0.

The frequency of a letter x is the number of times it occurs in the string.
 

Example 1:

Input: s = "successes"

Output: 6

Explanation:

The vowels are: 'u' (frequency 1), 'e' (frequency 2). The maximum frequency is 2.
The consonants are: 's' (frequency 4), 'c' (frequency 2). The maximum frequency is 4.
The output is 2 + 4 = 6.



Example 2:

Input: s = "aeiaeia"

Output: 3

Explanation:

The vowels are: 'a' (frequency 3), 'e' ( frequency 2), 'i' (frequency 2). The maximum frequency is 3.
There are no consonants in s. Hence, maximum consonant frequency = 0.
The output is 3 + 0 = 3.
 

Constraints:

1 <= s.length <= 100
s consists of lowercase English letters only.


# Code

```cpp []
class Solution {
private:
    bool isVowel(char c){
        return (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u');
    }
public:
    int maxFreqSum(string s) {
        unordered_map<char,int> freq;
        for(const char c:s) freq[c]++;

        int maxV = 0 , maxC = 0;

        for(auto it:freq){
            if(isVowel(it.first)) maxV = max(maxV , it.second);
            else maxC = max(maxC , it.second);
        }

        return maxV + maxC;

    }
};
```

--------------------------------------------------------------

# 966. Vowel Spellchecker -> [LeetCode](https://leetcode.com/problems/vowel-spellchecker/description)
 
Medium

Given a wordlist, we want to implement a spellchecker that converts a query word into a correct word.

For a given query word, the spell checker handles two categories of spelling mistakes:

Capitalization: If the query matches a word in the wordlist (case-insensitive), then the query word is returned with the same case as the case in the wordlist.
Example: wordlist = ["yellow"], query = "YellOw": correct = "yellow"
Example: wordlist = ["Yellow"], query = "yellow": correct = "Yellow"
Example: wordlist = ["yellow"], query = "yellow": correct = "yellow"
Vowel Errors: If after replacing the vowels ('a', 'e', 'i', 'o', 'u') of the query word with any vowel individually, it matches a word in the wordlist (case-insensitive), then the query word is returned with the same case as the match in the wordlist.
Example: wordlist = ["YellOw"], query = "yollow": correct = "YellOw"
Example: wordlist = ["YellOw"], query = "yeellow": correct = "" (no match)
Example: wordlist = ["YellOw"], query = "yllw": correct = "" (no match)
In addition, the spell checker operates under the following precedence rules:

When the query exactly matches a word in the wordlist (case-sensitive), you should return the same word back.
When the query matches a word up to capitlization, you should return the first such match in the wordlist.
When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
If the query has no matches in the wordlist, you should return the empty string.
Given some queries, return a list of words answer, where answer[i] is the correct word for query = queries[i].

 

Example 1:

Input: wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]

Output: ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]


Example 2:

Input: wordlist = ["yellow"], queries = ["YellOw"]

Output: ["yellow"]
 

Constraints:

1 <= wordlist.length, queries.length <= 5000
1 <= wordlist[i].length, queries[i].length <= 7
wordlist[i] and queries[i] consist only of only English letters.


# Code
```cpp []
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        unordered_set<string> exact(wordlist.begin(), wordlist.end());
        unordered_map<string, string> caseMap;
        unordered_map<string, string> vowelMap;

        for (string w : wordlist) {
            string lower = toLower(w);
            string devowel = deVowel(lower);
            if (!caseMap.count(lower)) caseMap[lower] = w;
            if (!vowelMap.count(devowel)) vowelMap[devowel] = w;
        }
        vector<string> result;
        for (string q : queries) {
            if (exact.count(q)) {
                result.push_back(q);
            } else {
                string lower = toLower(q);
                string devowel = deVowel(lower);

                if (caseMap.count(lower)) result.push_back(caseMap[lower]);
                else if (vowelMap.count(devowel)) result.push_back(vowelMap[devowel]);
                else result.push_back("");
            }
        }
        return result;
    }
private:
    string toLower(string s) {
        for (char &c : s) c = tolower(c);
        return s;
    }
    string deVowel(string s) {
        for (char &c : s) {
            if (isVowel(c)) c = '*';
        }
        return s;
    }
    bool isVowel(char c) {
        c = tolower(c);
        return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
    }
};
```

---------------------------------------------------

# 1935. Maximum Number of Words You Can Type -> [LeetCode](https://leetcode.com/problems/maximum-number-of-words-you-can-type/description)
 
Easy
 
There is a malfunctioning keyboard where some letter keys do not work. All other keys on the keyboard work properly.

Given a string text of words separated by a single space (no leading or trailing spaces) and a string brokenLetters of all distinct letter keys that are broken, return the number of words in text you can fully type using this keyboard.

 

Example 1:

Input: text = "hello world", brokenLetters = "ad"

Output: 1

Explanation: We cannot type "world" because the 'd' key is broken.


Example 2:

Input: text = "leet code", brokenLetters = "lt"

Output: 1

Explanation: We cannot type "leet" because the 'l' and 't' keys are broken.


Example 3:

Input: text = "leet code", brokenLetters = "e"

Output: 0

Explanation: We cannot type either word because the 'e' key is broken.
 


Constraints:

1 <= text.length <= 104
0 <= brokenLetters.length <= 26
text consists of words separated by a single space without any leading or trailing spaces.
Each word only consists of lowercase English letters.
brokenLetters consists of distinct lowercase English letters.


# Code

```cpp []
class Solution {
public:
    int canBeTypedWords(string text, string brokenLetters) {
        vector<string> arr;
        string str;
        for(char c:text){
            if(c == ' ' && !str.empty()){
                arr.push_back(str);
                str.clear();
            }else str += c;
        }

        if(!str.empty()) arr.push_back(str);

        int res = 0;

        for(auto s:arr){
            res++;
            for(char c:s){
                if(brokenLetters.find(c) < brokenLetters.size()){
                    res--;
                    break;
                }
            }
        }

        return res;

    }
};
```

-----------------------------------------------------------

# 2197. Replace Non-Coprime Numbers in Array -> [LeetCode](https://leetcode.com/problems/replace-non-coprime-numbers-in-array/description)
 
Hard
 
You are given an array of integers nums. Perform the following steps:

Find any two adjacent numbers in nums that are non-coprime.
If no such numbers are found, stop the process.
Otherwise, delete the two numbers and replace them with their LCM (Least Common Multiple).
Repeat this process as long as you keep finding two adjacent non-coprime numbers.
Return the final modified array. It can be shown that replacing adjacent non-coprime numbers in any arbitrary order will lead to the same result.

The test cases are generated such that the values in the final array are less than or equal to 108.

Two values x and y are non-coprime if GCD(x, y) > 1 where GCD(x, y) is the Greatest Common Divisor of x and y.

 

Example 1:

Input: nums = [6,4,3,2,7,6,2]

Output: [12,7,6]

Explanation: 
- (6, 4) are non-coprime with LCM(6, 4) = 12. Now, nums = [12,3,2,7,6,2].
- (12, 3) are non-coprime with LCM(12, 3) = 12. Now, nums = [12,2,7,6,2].
- (12, 2) are non-coprime with LCM(12, 2) = 12. Now, nums = [12,7,6,2].
- (6, 2) are non-coprime with LCM(6, 2) = 6. Now, nums = [12,7,6].
There are no more adjacent non-coprime numbers in nums.
Thus, the final modified array is [12,7,6].
Note that there are other ways to obtain the same resultant array.



Example 2:

Input: nums = [2,2,1,1,3,3,3]

Output: [2,1,1,3]

Explanation: 
- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,3,3].
- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,3].
- (2, 2) are non-coprime with LCM(2, 2) = 2. Now, nums = [2,1,1,3].
There are no more adjacent non-coprime numbers in nums.
Thus, the final modified array is [2,1,1,3].
Note that there are other ways to obtain the same resultant array.
 


Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 105
The test cases are generated such that the values in the final array are less than or equal to 108.


# Code
```cpp []
#include <vector>
#include <numeric>
using namespace std;

class Solution {
public:
    vector<int> replaceNonCoprimes(vector<int>& nums) {
        vector<int> result;
        for (int num : nums) {
            result.push_back(num);
            while (result.size() > 1) {
                int a = result.back();
                int b = result[result.size() - 2];
                int g = gcd(a, b);
                if (g > 1) {
                    result.pop_back();
                    result.pop_back();
                    long long lcm = (long long)a / g * b;
                    result.push_back((int)lcm);
                } else {
                    break;
                }
            }
        }
        return result;
    }
};
```

-----------------------------------------------------------

# 2353. Design a Food Rating System -> [LeetCode](https://leetcode.com/problems/design-a-food-rating-system/description)
 
Medium
 
Design a food rating system that can do the following:

Modify the rating of a food item listed in the system.
Return the highest-rated food item for a type of cuisine in the system.

Implement the FoodRatings class:

FoodRatings(String[] foods, String[] cuisines, int[] ratings) Initializes the system. The food items are described by foods, cuisines and ratings, all of which have a length of n.
foods[i] is the name of the ith food,
cuisines[i] is the type of cuisine of the ith food, and
ratings[i] is the initial rating of the ith food.
void changeRating(String food, int newRating) Changes the rating of the food item with the name food.
String highestRated(String cuisine) Returns the name of the food item that has the highest rating for the given type of cuisine. If there is a tie, return the item with the lexicographically smaller name.
Note that a string x is lexicographically smaller than string y if x comes before y in dictionary order, that is, either x is a prefix of y, or if i is the first position such that x[i] != y[i], then x[i] comes before y[i] in alphabetic order.

 


Example 1:

Input

["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]
[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]

Output

[null, "kimchi", "ramen", null, "sushi", null, "ramen"]

Explanation

FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated("korean"); // return "kimchi"
                                    // "kimchi" is the highest rated korean food with a rating of 9.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // "ramen" is the highest rated japanese food with a rating of 14.
foodRatings.changeRating("sushi", 16); // "sushi" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "sushi"
                                      // "sushi" is the highest rated japanese food with a rating of 16.
foodRatings.changeRating("ramen", 16); // "ramen" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // Both "sushi" and "ramen" have a rating of 16.
                                      // However, "ramen" is lexicographically smaller than "sushi".
 


Constraints:

1 <= n <= 2 * 104
n == foods.length == cuisines.length == ratings.length
1 <= foods[i].length, cuisines[i].length <= 10
foods[i], cuisines[i] consist of lowercase English letters.
1 <= ratings[i] <= 108
All the strings in foods are distinct.
food will be the name of a food item in the system across all calls to changeRating.
cuisine will be a type of cuisine of at least one food item in the system across all calls to highestRated.
At most 2 * 104 calls in total will be made to changeRating and highestRated.


# Code
```cpp []
#include <set>
#include <string>
using namespace std;

struct Info {
    string name;
    string cuisine;
    int rating{};

    Info() = default;
    Info(string name, string cuisine, int rating) : name{name}, cuisine{cuisine}, rating{rating}{}

    bool operator<(const Info& first) const {
        if (rating == first.rating) {
            return name < first.name;
        }
        return rating > first.rating;
    }
};

class FoodRatings {
    
    unordered_map<string, Info> foodNameToInfo;
    unordered_map<string, set<Info>> cuisineNameToSortedInfo;

public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for (size_t i = 0; i < foods.size(); ++i) {
            foodNameToInfo.emplace(foods[i], Info(foods[i], cuisines[i], ratings[i]));
            cuisineNameToSortedInfo[cuisines[i]].emplace(foods[i], cuisines[i], ratings[i]);
        }
    }

    void changeRating(const string& food, int newRating) {
        Info& toUpdate = foodNameToInfo[food];
        cuisineNameToSortedInfo[toUpdate.cuisine].erase(toUpdate);
        toUpdate.rating = newRating;
        cuisineNameToSortedInfo[toUpdate.cuisine].insert(toUpdate);
    }

    string highestRated(const string& cuisine) const {
        return cuisineNameToSortedInfo.at(cuisine).begin()->name;
    }
};
```

---------------------------------------------------------------

# 3408. Design Task Manager -> [LeetCode](https://leetcode.com/problems/design-task-manager/description)
 
Medium

There is a task management system that allows users to manage their tasks, each associated with a priority. The system should efficiently handle adding, modifying, executing, and removing tasks.

Implement the TaskManager class:

TaskManager(vector<vector<int>>& tasks) initializes the task manager with a list of user-task-priority triples. Each element in the input list is of the form [userId, taskId, priority], which adds a task to the specified user with the given priority.

void add(int userId, int taskId, int priority) adds a task with the specified taskId and priority to the user with userId. It is guaranteed that taskId does not exist in the system.

void edit(int taskId, int newPriority) updates the priority of the existing taskId to newPriority. It is guaranteed that taskId exists in the system.

void rmv(int taskId) removes the task identified by taskId from the system. It is guaranteed that taskId exists in the system.

int execTop() executes the task with the highest priority across all users. If there are multiple tasks with the same highest priority, execute the one with the highest taskId. After executing, the taskId is removed from the system. Return the userId associated with the executed task. If no tasks are available, return -1.

Note that a user may be assigned multiple tasks.

 

Example 1:

Input:

["TaskManager", "add", "edit", "execTop", "rmv", "add", "execTop"]
[[[[1, 101, 10], [2, 102, 20], [3, 103, 15]]], [4, 104, 5], [102, 8], [], [101], [5, 105, 15], []]

Output:

[null, null, null, 3, null, null, 5]

Explanation

TaskManager taskManager = new TaskManager([[1, 101, 10], [2, 102, 20], [3, 103, 15]]); // Initializes with three tasks for Users 1, 2, and 3.
taskManager.add(4, 104, 5); // Adds task 104 with priority 5 for User 4.
taskManager.edit(102, 8); // Updates priority of task 102 to 8.
taskManager.execTop(); // return 3. Executes task 103 for User 3.
taskManager.rmv(101); // Removes task 101 from the system.
taskManager.add(5, 105, 15); // Adds task 105 with priority 15 for User 5.
taskManager.execTop(); // return 5. Executes task 105 for User 5.
 

Constraints:

1 <= tasks.length <= 105
0 <= userId <= 105
0 <= taskId <= 105
0 <= priority <= 109
0 <= newPriority <= 109
At most 2 * 105 calls will be made in total to add, edit, rmv, and execTop methods.
The input is generated such that taskId will be valid.


# Code
```cpp []
#include <iostream>
#include <vector>
#include <map>
#include <unordered_map>

using namespace std;

class TaskManager {
private:
    struct Task {
        int userId, taskId, priority;

        // Default constructor
        Task() : userId(0), taskId(0), priority(0) {}

        // Parameterized constructor
        Task(int userId, int taskId, int priority)
            : userId(userId), taskId(taskId), priority(priority) {}

        // Define the comparison operator to maintain the correct order in the sorted container
        bool operator<(const Task& other) const {
            if (priority != other.priority) {
                return priority > other.priority; // Higher priority comes first
            }
            return taskId > other.taskId; // If priority is the same, higher taskId comes first
        }

        // Define the copy constructor
        Task(const Task& other) = default;
        // Define the move constructor
        Task(Task&& other) noexcept = default;

        // Define the copy assignment operator
        Task& operator=(const Task& other) = default;

        // Define the move assignment operator
        Task& operator=(Task&& other) noexcept = default;
    };

    map<Task, int> sortedTasks;
    unordered_map<int, Task> taskMap;

public:
    TaskManager(vector<vector<int>>& tasks) {
        for (auto& task : tasks) {
            int userId = task[0], taskId = task[1], priority = task[2];
            add(userId, taskId, priority);
        }
    }

    void add(int userId, int taskId, int priority) {
        Task task(userId, taskId, priority);
        sortedTasks[task] = userId;
        taskMap[taskId] = task;
    }

    void edit(int taskId, int newPriority) {
        auto taskIt = taskMap.find(taskId);
        if (taskIt != taskMap.end()) {
            Task task = taskIt->second;
            sortedTasks.erase(task);
            task.priority = newPriority;
            sortedTasks[task] = task.userId;
            taskMap[taskId] = task;
        }
    }

    void rmv(int taskId) {
        auto taskIt = taskMap.find(taskId);
        if (taskIt != taskMap.end()) {
            Task task = taskIt->second;
            sortedTasks.erase(task);
            taskMap.erase(taskId);
        }
    }

    int execTop() {
        if (sortedTasks.empty()) {
            return -1;
        }
        auto topTask = sortedTasks.begin();
        Task top = topTask->first;
        sortedTasks.erase(topTask);
        taskMap.erase(top.taskId);
        return top.userId;
    }
};
```

--------------------------------------------------------------------------

# Next Greater Element in Circular Array -> [GFG](https://www.geeksforgeeks.org/problems/next-greater-element/1)

Difficulty: Medium 

Given a circular integer array arr[], the task is to determine the next greater element (NGE) for each element in the array.

The next greater element of an element arr[i] is the first element that is greater than arr[i] when traversing circularly. If no such element exists, return -1 for that position.

Note: Since the array is circular, after reaching the last element, the search continues from the beginning until we have looked at all elements once.

Examples: 

Input: arr[] = [1, 3, 2, 4]

Output: [3, 4, 4, -1]

Explanation:

The next greater element for 1 is 3.
The next greater element for 3 is 4.
The next greater element for 2 is 4.
The next greater element for 4 does not exist, so return -1.


Input: arr[] = [0, 2, 3, 1, 1]

Output: [2, 3, -1, 2, 2]

Explanation:

The next greater element for 0 is 2.
The next greater element for 2 is 3.
The next greater element for 3 does not exist, so return -1.
The next greater element for 1 is 2 (from circular traversal).
The next greater element for 1 is 2 (from circular traversal).


Constraints:

1 ≤ arr.size() ≤ 105
0 ≤ arr[i] ≤ 106


Expected Complexities

Time Complexity: O(n)
Auxiliary Space: O(n)


Company Tags

FlipkartAmazonMicrosoft


Topic Tags

StackData Structures

### code
```cpp []
class Solution {
  public:
    vector<int> nextGreater(vector<int> &arr) {
        int n = arr.size();
        
        vector<int> ans(n,-1);
        stack<int> st;
        
        for(int i=n*2-1 ; i>=0 ; --i){
            int ind = i % n;
            while(!st.empty() && st.top() <= arr[ind]) st.pop();
            if(!st.empty()) ans[ind] = st.top();
            st.push(arr[ind]);
        }
        
        
        return ans;
    }
};
```

----------------------------------------------------------------------

# 3484. Design Spreadsheet -> [LeetCode](https://leetcode.com/problems/design-spreadsheet/description/)
 
Medium
 
A spreadsheet is a grid with 26 columns (labeled from 'A' to 'Z') and a given number of rows. Each cell in the spreadsheet can hold an integer value between 0 and 105.

Implement the Spreadsheet class:

Spreadsheet(int rows) Initializes a spreadsheet with 26 columns (labeled 'A' to 'Z') and the specified number of rows. All cells are initially set to 0.
void setCell(String cell, int value) Sets the value of the specified cell. The cell reference is provided in the format "AX" (e.g., "A1", "B10"), where the letter represents the column (from 'A' to 'Z') and the number represents a 1-indexed row.
void resetCell(String cell) Resets the specified cell to 0.
int getValue(String formula) Evaluates a formula of the form "=X+Y", where X and Y are either cell references or non-negative integers, and returns the computed sum.
Note: If getValue references a cell that has not been explicitly set using setCell, its value is considered 0.

 

Example 1:

Input:
["Spreadsheet", "getValue", "setCell", "getValue", "setCell", "getValue", "resetCell", "getValue"]
[[3], ["=5+7"], ["A1", 10], ["=A1+6"], ["B2", 15], ["=A1+B2"], ["A1"], ["=A1+B2"]]

Output:
[null, 12, null, 16, null, 25, null, 15]

Explanation

Spreadsheet spreadsheet = new Spreadsheet(3); // Initializes a spreadsheet with 3 rows and 26 columns
spreadsheet.getValue("=5+7"); // returns 12 (5+7)
spreadsheet.setCell("A1", 10); // sets A1 to 10
spreadsheet.getValue("=A1+6"); // returns 16 (10+6)
spreadsheet.setCell("B2", 15); // sets B2 to 15
spreadsheet.getValue("=A1+B2"); // returns 25 (10+15)
spreadsheet.resetCell("A1"); // resets A1 to 0
spreadsheet.getValue("=A1+B2"); // returns 15 (0+15)
 

Constraints:

1 <= rows <= 103
0 <= value <= 105
The formula is always in the format "=X+Y", where X and Y are either valid cell references or non-negative integers with values less than or equal to 105.
Each cell reference consists of a capital letter from 'A' to 'Z' followed by a row number between 1 and rows.
At most 104 calls will be made in total to setCell, resetCell, and getValue.

# Code
```cpp []
class Spreadsheet {
    unordered_map<string, int> cells;
public:
    Spreadsheet(int rows) {
        
    }
    
    void setCell(string cell, int value) {
        cells[cell] = value;
    }
    
    void resetCell(string cell) {
        cells.erase(cell);
    }
    
    int getValue(string formula) {
        int idx = formula.find('+');
        string left = formula.substr(1, idx - 1);
        string right = formula.substr(idx + 1);

        int valLeft = 
            isalpha(left[0]) 
            ? (cells.count(left) ? cells[left] : 0) 
            : stoi(left);

        int valRight = 
            isalpha(right[0]) 
            ? (cells.count(right) ? cells[right] : 0) 
            : stoi(right);

        return valLeft + valRight;
    }
};
```

---------------------------------------------------------------------

# 3508. Implement Router -> [LeetCode](https://leetcode.com/problems/implement-router/description/)
 
Medium
 
Design a data structure that can efficiently manage data packets in a network router. Each data packet consists of the following attributes:

source: A unique identifier for the machine that generated the packet.
destination: A unique identifier for the target machine.
timestamp: The time at which the packet arrived at the router.
Implement the Router class:

Router(int memoryLimit): Initializes the Router object with a fixed memory limit.

memoryLimit is the maximum number of packets the router can store at any given time.
If adding a new packet would exceed this limit, the oldest packet must be removed to free up space.
bool addPacket(int source, int destination, int timestamp): Adds a packet with the given attributes to the router.

A packet is considered a duplicate if another packet with the same source, destination, and timestamp already exists in the router.
Return true if the packet is successfully added (i.e., it is not a duplicate); otherwise return false.
int[] forwardPacket(): Forwards the next packet in FIFO (First In First Out) order.

Remove the packet from storage.
Return the packet as an array [source, destination, timestamp].
If there are no packets to forward, return an empty array.
int getCount(int destination, int startTime, int endTime):

Returns the number of packets currently stored in the router (i.e., not yet forwarded) that have the specified destination and have timestamps in the inclusive range [startTime, endTime].
Note that queries for addPacket will be made in increasing order of timestamp.

 

Example 1:

Input:

["Router", "addPacket", "addPacket", "addPacket", "addPacket", "addPacket", "forwardPacket", "addPacket", "getCount"]
[[3], [1, 4, 90], [2, 5, 90], [1, 4, 90], [3, 5, 95], [4, 5, 105], [], [5, 2, 110], [5, 100, 110]]

Output:

[null, true, true, false, true, true, [2, 5, 90], true, 1]

Explanation

Router router = new Router(3); // Initialize Router with memoryLimit of 3.
router.addPacket(1, 4, 90); // Packet is added. Return True.
router.addPacket(2, 5, 90); // Packet is added. Return True.
router.addPacket(1, 4, 90); // This is a duplicate packet. Return False.
router.addPacket(3, 5, 95); // Packet is added. Return True
router.addPacket(4, 5, 105); // Packet is added, [1, 4, 90] is removed as number of packets exceeds memoryLimit. Return True.
router.forwardPacket(); // Return [2, 5, 90] and remove it from router.
router.addPacket(5, 2, 110); // Packet is added. Return True.
router.getCount(5, 100, 110); // The only packet with destination 5 and timestamp in the inclusive range [100, 110] is [4, 5, 105]. Return 1.


Example 2:

Input:

["Router", "addPacket", "forwardPacket", "forwardPacket"]
[[2], [7, 4, 90], [], []]

Output:

[null, true, [7, 4, 90], []]

Explanation

Router router = new Router(2); // Initialize Router with memoryLimit of 2.
router.addPacket(7, 4, 90); // Return True.
router.forwardPacket(); // Return [7, 4, 90].
router.forwardPacket(); // There are no packets left, return [].
 

Constraints:

2 <= memoryLimit <= 105
1 <= source, destination <= 2 * 105
1 <= timestamp <= 109
1 <= startTime <= endTime <= 109
At most 105 calls will be made to addPacket, forwardPacket, and getCount methods altogether.
queries for addPacket will be made in increasing order of timestamp.


# Code
```cpp []
class Router {
public:
    map<vector<int>, int> mpp; // to track duplicates
    queue<vector<int>> queue; // to store packets in FIFO order
    unordered_map<int, vector<int>> timestamps; // for timestamps tracking
    unordered_map<int, int> st; 
    int maxSize = 0; // maxSize allowed

    Router(int memoryLimit) { 
        maxSize = memoryLimit; 
    }

    bool addPacket(int source, int destination, int timestamp) {
        vector<int> packet = {source, destination, timestamp};
        // checking for duplicate
        if (mpp.count(packet))
            return false;
        if (queue.size() == maxSize) { // remove the first element if queue is full
            vector<int> res = queue.front();
            mpp.erase(res);
            int temp = res[1];
            st[temp]++;  
            queue.pop();
        }
        queue.push(packet);
        mpp[packet]++;
        timestamps[destination].push_back(timestamp);
        return true;
    }

    vector<int> forwardPacket() {
        if(queue.empty()) return {};
        vector<int> res = queue.front();
        queue.pop();
        mpp.erase(res);
        int temp = res[1];
        st[temp]++;
        return res;
    }

    int getCount(int destination, int startTime, int endTime) {
        if(timestamps.find(destination) == timestamps.end())
            return 0;
        auto &p = timestamps[destination];
        int temp = st[destination];
        auto right = lower_bound(p.begin() + temp, p.end(), startTime);
        auto left = upper_bound(p.begin() + temp, p.end(), endTime);
        return int(left - right);
    }
};
```

--------------------------------------------------------------------------

# 1912. Design Movie Rental System -> [LeetCode](https://leetcode.com/problems/design-movie-rental-system/description)
 
Hard
 
You have a movie renting company consisting of n shops. You want to implement a renting system that supports searching for, booking, and returning movies. The system should also support generating a report of the currently rented movies.

Each movie is given as a 2D integer array entries where entries[i] = [shopi, moviei, pricei] indicates that there is a copy of movie moviei at shop shopi with a rental price of pricei. Each shop carries at most one copy of a movie moviei.

The system should support the following functions:

Search: Finds the cheapest 5 shops that have an unrented copy of a given movie. The shops should be sorted by price in ascending order, and in case of a tie, the one with the smaller shopi should appear first. If there are less than 5 matching shops, then all of them should be returned. If no shop has an unrented copy, then an empty list should be returned.
Rent: Rents an unrented copy of a given movie from a given shop.
Drop: Drops off a previously rented copy of a given movie at a given shop.
Report: Returns the cheapest 5 rented movies (possibly of the same movie ID) as a 2D list res where res[j] = [shopj, moviej] describes that the jth cheapest rented movie moviej was rented from the shop shopj. The movies in res should be sorted by price in ascending order, and in case of a tie, the one with the smaller shopj should appear first, and if there is still tie, the one with the smaller moviej should appear first. If there are fewer than 5 rented movies, then all of them should be returned. If no movies are currently being rented, then an empty list should be returned.
Implement the MovieRentingSystem class:

MovieRentingSystem(int n, int[][] entries) Initializes the MovieRentingSystem object with n shops and the movies in entries.
List<Integer> search(int movie) Returns a list of shops that have an unrented copy of the given movie as described above.
void rent(int shop, int movie) Rents the given movie from the given shop.
void drop(int shop, int movie) Drops off a previously rented movie at the given shop.
List<List<Integer>> report() Returns a list of cheapest rented movies as described above.
Note: The test cases will be generated such that rent will only be called if the shop has an unrented copy of the movie, and drop will only be called if the shop had previously rented out the movie.

 

Example 1:

Input

["MovieRentingSystem", "search", "rent", "rent", "report", "drop", "search"]
[[3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]], [1], [0, 1], [1, 2], [], [1, 2], [2]]


Output

[null, [1, 0, 2], null, null, [[0, 1], [1, 2]], null, [0, 1]]

Explanation

MovieRentingSystem movieRentingSystem = new MovieRentingSystem(3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]);
movieRentingSystem.search(1);  // return [1, 0, 2], Movies of ID 1 are unrented at shops 1, 0, and 2. Shop 1 is cheapest; shop 0 and 2 are the same price, so order by shop number.
movieRentingSystem.rent(0, 1); // Rent movie 1 from shop 0. Unrented movies at shop 0 are now [2,3].
movieRentingSystem.rent(1, 2); // Rent movie 2 from shop 1. Unrented movies at shop 1 are now [1].
movieRentingSystem.report();   // return [[0, 1], [1, 2]]. Movie 1 from shop 0 is cheapest, followed by movie 2 from shop 1.
movieRentingSystem.drop(1, 2); // Drop off movie 2 at shop 1. Unrented movies at shop 1 are now [1,2].
movieRentingSystem.search(2);  // return [0, 1]. Movies of ID 2 are unrented at shops 0 and 1. Shop 0 is cheapest, followed by shop 1.
 

Constraints:

1 <= n <= 3 * 105
1 <= entries.length <= 105
0 <= shopi < n
1 <= moviei, pricei <= 104
Each shop carries at most one copy of a movie moviei.
At most 105 calls in total will be made to search, rent, drop and report.


# Code
```cpp []
struct Node {
    int shop, movie, price;
    bool operator<(const Node& other) const {
        if (price != other.price) return price < other.price;
        if (shop != other.shop) return shop < other.shop;
        return movie < other.movie;
    }
};

class MovieRentingSystem {
    unordered_map<long long, Node> byPair;
    unordered_map<int, set<Node>> availableByMovie;
    set<Node> rentedSet;

    long long key(int shop, int movie) {
        return ((long long)shop << 32) ^ movie;
    }

public:
    MovieRentingSystem(int n, vector<vector<int>>& entries) {
        for (auto& e : entries) {
            int shop = e[0], movie = e[1], price = e[2];
            Node node{shop, movie, price};
            byPair[key(shop, movie)] = node;
            availableByMovie[movie].insert(node);
        }
    }

    vector<int> search(int movie) {
        vector<int> res;
        if (availableByMovie.count(movie) == 0) return res;
        auto& s = availableByMovie[movie];
        int count = 0;
        for (auto it = s.begin(); it != s.end() && count < 5; ++it, ++count) {
            res.push_back(it->shop);
        }
        return res;
    }

    void rent(int shop, int movie) {
        long long k = key(shop, movie);
        Node node = byPair[k];
        availableByMovie[movie].erase(node);
        rentedSet.insert(node);
    }

    void drop(int shop, int movie) {
        long long k = key(shop, movie);
        Node node = byPair[k];
        rentedSet.erase(node);
        availableByMovie[movie].insert(node);
    }

    vector<vector<int>> report() {
        vector<vector<int>> res;
        int count = 0;
        for (auto it = rentedSet.begin(); it != rentedSet.end() && count < 5; ++it, ++count) {
            res.push_back({it->shop, it->movie});
        }
        return res;
    }
};
```

----------------------------------------------------------------------------------------


# 3005. Count Elements With Maximum Frequency -> [LeetCode](https://leetcode.com/problems/count-elements-with-maximum-frequency/description)
 
Easy
 
You are given an array nums consisting of positive integers.

Return the total frequencies of elements in nums such that those elements all have the maximum frequency.

The frequency of an element is the number of occurrences of that element in the array.

 

Example 1:

Input: nums = [1,2,2,3,1,4]

Output: 4

Explanation: The elements 1 and 2 have a frequency of 2 which is the maximum frequency in the array.
So the number of elements in the array with maximum frequency is 4.


Example 2:

Input: nums = [1,2,3,4,5]

Output: 5

Explanation: All elements of the array have a frequency of 1 which is the maximum.
So the number of elements in the array with maximum frequency is 5.
 

Constraints:

1 <= nums.length <= 100
1 <= nums[i] <= 100

# Code

```cpp []
class Solution {
public:
    int maxFrequencyElements(vector<int>& nums) {

        int ans = 0;
        int maxFreq = 0;

        unordered_map<int,int> freq;

        for(const int i:nums){ 
            freq[i]++;
            maxFreq = max(maxFreq , freq[i]);
        }

        for(auto it:freq){
            if(maxFreq == it.second) ans++;
        }

        return (ans * maxFreq);


    }
};
```

------------------------------------------------------


# 165. Compare Version Numbers -> [LeetCode](https://leetcode.com/problems/compare-version-numbers/description)
 
Medium
 
Given two version strings, version1 and version2, compare them. A version string consists of revisions separated by dots '.'. The value of the revision is its integer conversion ignoring leading zeros.

To compare version strings, compare their revision values in left-to-right order. If one of the version strings has fewer revisions, treat the missing revision values as 0.

Return the following:

If version1 < version2, return -1.
If version1 > version2, return 1.
Otherwise, return 0.
 

Example 1:

Input: version1 = "1.2", version2 = "1.10"

Output: -1

Explanation:

version1's second revision is "2" and version2's second revision is "10": 2 < 10, so version1 < version2.

Example 2:

Input: version1 = "1.01", version2 = "1.001"

Output: 0

Explanation:

Ignoring leading zeroes, both "01" and "001" represent the same integer "1".

Example 3:

Input: version1 = "1.0", version2 = "1.0.0.0"

Output: 0

Explanation:

version1 has less revisions, which means every missing revision are treated as "0".

 

Constraints:

1 <= version1.length, version2.length <= 500
version1 and version2 only contain digits and '.'.
version1 and version2 are valid version numbers.
All the given revisions in version1 and version2 can be stored in a 32-bit integer.

# Code
```cpp []
class Solution {
public:
    static int compareVersion(string& v1, string& v2) {
        const int n1=v1.size(), n2=v2.size();
        int x1=0, x2=0;
        for(int i=0, j=0; i<n1 || j<n2; i++, j++){
            while(i<n1 && v1[i]!='.'){
                x1=10*x1+(v1[i++]-'0');
            }
            while(j<n2 && v2[j]!='.'){
                x2=10*x2+(v2[j++]-'0');
            }
            if (x1<x2) return -1;
            else if (x1>x2) return 1;
            x1=0;
            x2=0;
        }
        return 0;
    }
};

```

--------------------------------------------------------------------------------------------------


# 166. Fraction to Recurring Decimal -> [LeetCode](https://leetcode.com/problems/fraction-to-recurring-decimal/description)
 
Medium
 
Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

If multiple answers are possible, return any of them.

It is guaranteed that the length of the answer string is less than 104 for all the given inputs.

 

Example 1:

Input: numerator = 1, denominator = 2

Output: "0.5"


Example 2:

Input: numerator = 2, denominator = 1

Output: "2"


Example 3:

Input: numerator = 4, denominator = 333

Output: "0.(012)"

 

Constraints:

-231 <= numerator, denominator <= 231 - 1
denominator != 0


# Code
```cpp []
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {
        if(!numerator) return "0";
        string ans = "";
        if (numerator > 0 ^ denominator > 0) ans += '-';
        long num = labs(numerator), den = labs(denominator);
        long q = num / den;
        long r = num % den;
        ans += to_string(q);
        
        if(r == 0) return ans;
        
        ans += '.';
        unordered_map<long, int> mp;
        while(r != 0){
            if(mp.find(r) != mp.end()){
                int pos = mp[r];
                ans.insert(pos, "(");
                ans += ')';
                break;
            }
            else{
                mp[r] = ans.length();
                r *= 10;
                q = r / den;
                r = r % den;
                ans += to_string(q);
            }
        }
        return ans;
    }
};
```

-------------------------------------------------------------

# 120. Triangle -> [LeetCode](https://leetcode.com/problems/triangle/description/)
 
Medium
 
Given a triangle array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.

 

Example 1:

Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]

Output: 11

Explanation: The triangle looks like:

   2
  
  3 4
 
 6 5 7

4 1 8 3


The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).


Example 2:

Input: triangle = [[-10]]

Output: -10
 

Constraints:

1 <= triangle.length <= 200
triangle[0].length == 1
triangle[i].length == triangle[i - 1].length + 1
-104 <= triangle[i][j] <= 104
 

Follow up: Could you do this using only O(n) extra space, where n is the total number of rows in the triangle?


# Code
```cpp []
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int row = triangle.size();
        vector<int> memo = triangle[row - 1];

        for(int r=row-2 ; r>=0 ; --r){
            for(int c=0 ; c<=r ; ++c){
                memo[c] = min(memo[c] , memo[c+1]) + triangle[r][c];
            }
        }

        return memo[0];
    }
};
```

------------------------------------

# 611. Valid Triangle Number -> [LeetCode](https://leetcode.com/problems/valid-triangle-number/description)
 
Medium
 
Given an integer array nums, return the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

 

Example 1:

Input: nums = [2,2,3,4]

Output: 3

Explanation: Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3


Example 2:

Input: nums = [4,2,3,4]

Output: 4
 

Constraints:

1 <= nums.length <= 1000
0 <= nums[i] <= 1000


# Code
```cpp []
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int ans = 0;

        sort(nums.begin() , nums.end());

        for(int r=nums.size()-1 ; r>=2 ; --r){
            int l = 0;
            int m = r - 1;
            while(l<m){
                if(nums[l] + nums[m] > nums[r]){
                    ans += (m - l);
                    m--;
                }else{
                    l++;
                }
            }
        }

        return ans;
    }
};
```

--------------------------------------------------

# 812. Largest Triangle Area -> [LeetCode](https://leetcode.com/problems/largest-triangle-area/description)
  
Easy
 
Given an array of points on the X-Y plane points where points[i] = [xi, yi], return the area of the largest triangle that can be formed by any three different points. Answers within 10-5 of the actual answer will be accepted.


![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png)
 

Example 1:


Input: points = [[0,0],[0,1],[1,0],[0,2],[2,0]]

Output: 2.00000

Explanation: The five points are shown in the above figure. The red triangle is the largest.


Example 2:

Input: points = [[1,0],[0,0],[0,1]]

Output: 0.50000
 

Constraints:

3 <= points.length <= 50
-50 <= xi, yi <= 50
All the given points are unique.

### code
```cpp []

# Code
```cpp []
class Solution {
public:
    double largestTriangleArea(vector<vector<int>>& points) {
        const int n = points.size();
        double maxArea = 0;
        for (int i = 0; i < n - 2; i++) {
            const int a0 = points[i][0], a1 = points[i][1];
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    double area =
                        0.5 * ((points[j][0] - a0) * (points[k][1] - a1) -
                               (points[j][1] - a1) * (points[k][0] - a0));
                    if (area < 0)
                        area = -area;
                    maxArea = max(maxArea, area);
                }
            }
        }
        return maxArea;
    }
};
```

-------------------------------------------------------------------------------------

# 976. Largest Perimeter Triangle -> [LeetCode](https://leetcode.com/problems/largest-perimeter-triangle/description/)
 
Easy
 
Given an integer array nums, return the largest perimeter of a triangle with a non-zero area, formed from three of these lengths. If it is impossible to form any triangle of a non-zero area, return 0.

 

Example 1:

Input: nums = [2,1,2]

Output: 5

Explanation: You can form a triangle with three side lengths: 1, 2, and 2.


Example 2:

Input: nums = [1,2,1,10]

Output: 0

Explanation: 
You cannot use the side lengths 1, 1, and 2 to form a triangle.
You cannot use the side lengths 1, 1, and 10 to form a triangle.
You cannot use the side lengths 1, 2, and 10 to form a triangle.
As we cannot use any three side lengths to form a triangle of non-zero area, we return 0.
 

Constraints:

3 <= nums.length <= 104

1 <= nums[i] <= 106

# Code
```cpp []
class Solution {
public:
    int largestPerimeter(vector<int>& nums) {
        sort(nums.begin(), nums.end(), greater<int>());
        for (int i = 0; i < nums.size() - 2; i++)
            if (nums[i + 1] + nums[i + 2] > nums[i])
                return nums[i] + nums[i + 1] + nums[i + 2];
        return 0;
    }
};
```

-----------------------------------------------------------------------------------------

# 1039. Minimum Score Triangulation of Polygon -> [LeetCode](https://leetcode.com/problems/minimum-score-triangulation-of-polygon/description)
 
Medium
 
You have a convex n-sided polygon where each vertex has an integer value. You are given an integer array values where values[i] is the value of the ith vertex in clockwise order.

Polygon triangulation is a process where you divide a polygon into a set of triangles and the vertices of each triangle must also be vertices of the original polygon. Note that no other shapes other than triangles are allowed in the division. This process will result in n - 2 triangles.

You will triangulate the polygon. For each triangle, the weight of that triangle is the product of the values at its vertices. The total score of the triangulation is the sum of these weights over all n - 2 triangles.

Return the minimum possible score that you can achieve with some triangulation of the polygon.

 

Example 1:



Input: values = [1,2,3]

Output: 6

Explanation: The polygon is already triangulated, and the score of the only triangle is 6.

Example 2:



Input: values = [3,7,4,5]

Output: 144

Explanation: There are two triangulations, with possible scores: 3*7*5 + 4*5*7 = 245, or 3*4*5 + 3*4*7 = 144.
The minimum score is 144.

Example 3:



Input: values = [1,3,1,4,1,5]

Output: 13

Explanation: The minimum score triangulation is 1*1*3 + 1*1*4 + 1*1*5 + 1*1*1 = 13.

 

Constraints:

n == values.length
3 <= n <= 50
1 <= values[i] <= 100

# Code
```cpp []
class Solution {
public:
    int dp[50][50] = {};
    
    int minScoreTriangulation(vector<int>& values, int i = 0, int j = 0, int res = 0) {
        if (j == 0) j = values.size() - 1;
        if (dp[i][j] != 0) return dp[i][j];
        for (int k = i + 1; k < j; ++k) {
            res = min(res == 0 ? INT_MAX : res,
                minScoreTriangulation(values, i, k) + 
                values[i] * values[k] * values[j] + 
                minScoreTriangulation(values, k, j));
        }
        return dp[i][j] = res;
    }
};
```

-------------------------------------------------------------------------------------------

# 2221. Find Triangular Sum of an Array -> [LeetCode](https://leetcode.com/problems/find-triangular-sum-of-an-array/description)
 
Medium
 
You are given a 0-indexed integer array nums, where nums[i] is a digit between 0 and 9 (inclusive).

The triangular sum of nums is the value of the only element present in nums after the following process terminates:

Let nums comprise of n elements. If n == 1, end the process. Otherwise, create a new 0-indexed integer array newNums of length n - 1.
For each index i, where 0 <= i < n - 1, assign the value of newNums[i] as (nums[i] + nums[i+1]) % 10, where % denotes modulo operator.
Replace the array nums with newNums.
Repeat the entire process starting from step 1.
Return the triangular sum of nums.

![img](https://assets.leetcode.com/uploads/2022/02/22/ex1drawio.png) 

Example 1:


Input: nums = [1,2,3,4,5]

Output: 8

Explanation:

The above diagram depicts the process from which we obtain the triangular sum of the array.


Example 2:

Input: nums = [5]

Output: 5

Explanation:

Since there is only one element in nums, the triangular sum is the value of that element itself.
 

Constraints:

1 <= nums.length <= 1000
0 <= nums[i] <= 9

# Code
```cpp []
static array<int, 10> inv{};
class Solution {
public:
    //(exponent 2, exponent 5, factor coprime to 2, 5 mod 10)
    using int3=array<int, 3>; 
    static void inv_mod10(){
        if (inv[1]==1) return;
        inv[1]=1, inv[3]=7, inv[7]=3, inv[9]=9;
    }
    static int3 factor(unsigned x){
        int exp2=countr_zero(x);
        x>>=exp2;
        int exp5=0;
        for (;x%5==0; x/=5) exp5++;
        x%=10;
        return {exp2, exp5, int(x)};
    }
    static int3 mul(const int3& x, const int3& y){
        return {x[0]+y[0], x[1]+y[1], x[2]*y[2]%10};
    }
    static int3 div(const int3& x, const int3& y){
        return {x[0]-y[0], x[1]-y[1], x[2]*inv[y[2]]%10};
    }
    static int Pow(int b, int exp){
        if(exp==0) return 1;
        int y=(exp&1)?b:1;
        return Pow(b*b, exp>>1)*y;
    }
    static int toInt(const int3& x){
        return (Pow(5, x[1])<<x[0])*x[2]%10;
    }
    static vector<int> Comb(int n){
        vector<int3> a(n+1, {0, 0, 0});
        vector<int> A(n+1);
        a[0]=a[n]={0, 0, 1};
        A[0]=A[n]=1;
        for(int k=1; k<=n/2; k++){
            a[k]=a[n-k]=div(mul(a[k-1], factor(n-k+1)), factor(k));
            A[k]=A[n-k]=toInt(a[k]);
        }
        return  A;
    }
    static int triangularSum(vector<int>& nums) {
        const int n=nums.size()-1;
        inv_mod10();
        auto A=Comb(n);
        int ans=0;
        for(int i=0; i<=n; i++){
            ans=(ans+A[i]*nums[i])%10;
        }
        return ans;
    }
};
```

-------------------------------------------------------------------------

# 1518. Water Bottles -> [LeetCode](https://leetcode.com/problems/water-bottles/description)
 
Easy
 
There are numBottles water bottles that are initially full of water. You can exchange numExchange empty water bottles from the market with one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Given the two integers numBottles and numExchange, return the maximum number of water bottles you can drink.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/07/01/sample_1_1875.png)

Input: numBottles = 9, numExchange = 3

Output: 13

Explanation: You can exchange 3 empty bottles to get 1 full water bottle.
Number of water bottles you can drink: 9 + 3 + 1 = 13.


Example 2:

![img](https://assets.leetcode.com/uploads/2020/07/01/sample_2_1875.png)

Input: numBottles = 15, numExchange = 4

Output: 19

Explanation: You can exchange 4 empty bottles to get 1 full water bottle. 
Number of water bottles you can drink: 15 + 3 + 1 = 19.
 

Constraints:

1 <= numBottles <= 100
2 <= numExchange <= 100

# Code
```cpp []
class Solution {
public:
// //// Simple Math
    // int numWaterBottles(int numBottles, int numExchange) {
    //     return (numBottles*numExchange-1) / (numExchange-1);
    // }


    int numWaterBottles(int numBottles, int numExchange) {
        int res = 0;
        while(numBottles > 0){
            if(numBottles >= numExchange){
                res += numBottles - (numBottles % numExchange);
                numBottles = (numBottles % numExchange) + (numBottles / numExchange);
            }else{
                res += numBottles;
                numBottles = 0;
            }
        }

        return res;
    }
};
```

--------------------------------------------


# 3100. Water Bottles II -> [LeetCode](https://leetcode.com/problems/water-bottles-ii/description)
 
Medium
 
You are given two integers numBottles and numExchange.

numBottles represents the number of full water bottles that you initially have. In one operation, you can perform one of the following operations:

Drink any number of full water bottles turning them into empty bottles.
Exchange numExchange empty bottles with one full water bottle. Then, increase numExchange by one.
Note that you cannot exchange multiple batches of empty bottles for the same value of numExchange. For example, if numBottles == 3 and numExchange == 1, you cannot exchange 3 empty water bottles for 3 full bottles.

Return the maximum number of water bottles you can drink.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2024/01/28/exampleone1.png)

Input: numBottles = 13, numExchange = 6

Output: 15

Explanation: The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.


Example 2:

![img](https://assets.leetcode.com/uploads/2024/01/28/example231.png)

Input: numBottles = 10, numExchange = 3

Output: 13

Explanation: The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.
 

Constraints:

1 <= numBottles <= 100 
1 <= numExchange <= 100

# Code
```cpp []
class Solution {
public:
    int maxBottlesDrunk(int numBottles, int numExchange) {
        int res = numBottles;
        while(numBottles >= numExchange){
            numBottles -= numExchange - 1;
            numExchange++;
            res++;
        }

        return res;
    }
};
```

----------------------------------------------------------------------------
----------------------------------------------------------------------------

![badge](https://assets.leetcode.com/static_assets/marketing/202509.gif)
