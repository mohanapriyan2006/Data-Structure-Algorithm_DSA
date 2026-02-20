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


# [1653. Minimum Deletions to Make String Balanced](https://leetcode.com/problems/minimum-deletions-to-make-string-balanced)

Medium
 
You are given a string s consisting only of characters 'a' and 'b'​​​​.

You can delete any number of characters in s to make s balanced. s is balanced if there is no pair of indices (i,j) such that i < j and s[i] = 'b' and s[j]= 'a'.

Return the minimum number of deletions needed to make s balanced.

 

Example 1:

Input: s = "aababbab"

Output: 2

Explanation: You can either:
Delete the characters at 0-indexed positions 2 and 6 ("aababbab" -> "aaabbb"), or
Delete the characters at 0-indexed positions 3 and 6 ("aababbab" -> "aabbbb").



Example 2:

Input: s = "bbaaaaabb"

Output: 2

Explanation: The only solution is to delete the first two characters.
 

Constraints:

1 <= s.length <= 105
s[i] is 'a' or 'b'​​.


# 
# Code
```cpp []
class Solution {
public:
    int minimumDeletions(string s) {
        int res = 0 , cnt = 0;
        for(char c:s){
            if(c == 'b') cnt++;
            else if(cnt) res++, cnt--;
        }
        return res;
    }
};
```

-------------------------------------------------------------------------------------------------------------------------

# [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description)

Easy

Given a binary tree, determine if it is height-balanced.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

Input: root = [3,9,20,null,null,15,7]

Output: true



Example 2:

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

Input: root = [1,2,2,3,3,null,null,4,4]

Output: false



Example 3:

Input: root = []

Output: true
 

Constraints:

The number of nodes in the tree is in the range [0, 5000].
-104 <= Node.val <= 104


# Code
```cpp []
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
private:
    bool ans;
    int checkBalanceTree(TreeNode* root){
        if(!root) return 0;
        if(!ans) return 0;
        int lf = checkBalanceTree(root->left);
        int rt = checkBalanceTree(root->right);
        if(abs(lf - rt) > 1) ans = false;
        return 1 + max(lf , rt);
    }
public:
    bool isBalanced(TreeNode* root) {
        ans = true;
        checkBalanceTree(root);
        return ans;
    }
};

```

-------------------------------------------------------------------------------------------------------------------


# [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree/description)
Medium
 
Given the root of a binary search tree, return a balanced binary search tree with the same node values. If there is more than one answer, return any of them.

A binary search tree is balanced if the depth of the two subtrees of every node never differs by more than 1.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg)

Input: root = [1,null,2,null,3,null,4,null,null]

Output: [2,1,3,null,null,null,4]

Explanation: This is not the only correct answer, [3,1,4,null,2] is also correct.



Example 2:

![img](https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg)

Input: root = [2,1,3]

Output: [2,1,3]
 

Constraints:

The number of nodes in the tree is in the range [1, 104].
1 <= Node.val <= 105


# Code
```cpp []
class Solution {
public:
    void inorder(TreeNode* node, vector<int>& vals) {
        if(!node) return;
        inorder(node->left, vals);
        vals.push_back(node->val);
        inorder(node->right, vals);
    }
    TreeNode* build(const vector<int>& vals, int l, int r) {
        if(l > r) return nullptr;
        int mid = (l + r) / 2;
        TreeNode* node = new TreeNode(vals[mid]);
        node->left  = build(vals, l, mid - 1);
        node->right = build(vals, mid + 1, r);
        return node;
    }
    TreeNode* balanceBST(TreeNode* root) {
        vector<int> vals;
        inorder(root, vals);
        return build(vals, 0, (int)vals.size() - 1);
    }
};
```

---------------------------------------------------------------------------------------------------


# [3719. Longest Balanced Subarray I](https://leetcode.com/problems/longest-balanced-subarray-i)

Medium
 
You are given an integer array nums.

A subarray is called balanced if the number of distinct even numbers in the subarray is equal to the number of distinct odd numbers.

Return the length of the longest balanced subarray.

 


Example 1:

Input: nums = [2,5,4,3]

Output: 4

Explanation:

The longest balanced subarray is [2, 5, 4, 3].
It has 2 distinct even numbers [2, 4] and 2 distinct odd numbers [5, 3]. Thus, the answer is 4.



Example 2:

Input: nums = [3,2,2,5,4]

Output: 5

Explanation:

The longest balanced subarray is [3, 2, 2, 5, 4].
It has 2 distinct even numbers [2, 4] and 2 distinct odd numbers [3, 5]. Thus, the answer is 5.


Example 3:

Input: nums = [1,2,3,2]

Output: 3

Explanation:

The longest balanced subarray is [2, 3, 2].
It has 1 distinct even number [2] and 1 distinct odd number [3]. Thus, the answer is 3.
 

Constraints:

1 <= nums.length <= 1500
1 <= nums[i] <= 105


# Code
```cpp []
class Solution {
public:
    inline static uint32_t seen[100001] = {};
    inline static uint32_t leet = 0;
    int longestBalanced(vector<int>& nums) {
        leet++;
        int n = nums.size();
        int res = 0;

        for (int i = 0; i < n && n - i > res; i++) {
            int A[2] = {0, 0};
            uint32_t marker = (leet << 16) | (uint32_t)(i + 1);
            for (int j = i; j < n; j++) {
                int val = nums[j];
                if (seen[val] != marker) {
                    seen[val] = marker;
                    A[val & 1]++;
                }
                if (A[0] == A[1])
                    res = max(res, j - i + 1);
            }
        }

        return res;
    }
};

```

--------------------------------------------------------------------------------------------------------

# [3721. Longest Balanced Subarray II](https://leetcode.com/problems/longest-balanced-subarray-ii)

Hard
 
You are given an integer array nums.

A subarray is called balanced if the number of distinct even numbers in the subarray is equal to the number of distinct odd numbers.

Return the length of the longest balanced subarray.

 

Example 1:

Input: nums = [2,5,4,3]

Output: 4

Explanation:

The longest balanced subarray is [2, 5, 4, 3].
It has 2 distinct even numbers [2, 4] and 2 distinct odd numbers [5, 3]. Thus, the answer is 4.



Example 2:

Input: nums = [3,2,2,5,4]

Output: 5

Explanation:

The longest balanced subarray is [3, 2, 2, 5, 4].
It has 2 distinct even numbers [2, 4] and 2 distinct odd numbers [3, 5]. Thus, the answer is 5.


Example 3:

Input: nums = [1,2,3,2]

Output: 3

Explanation:

The longest balanced subarray is [2, 3, 2].
It has 1 distinct even number [2] and 1 distinct odd number [3]. Thus, the answer is 3.
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 105



# Code
```cpp []
class Solution {
public:
    struct SegTree {
        int n;
        vector<int> sum, mn, mx;
        vector<int>  lazyVal;
        vector<bool> hasLazy;

        SegTree(int _n){
            n = _n;
            sum.assign(4 * n + 5, 0);
            mn.assign(4 * n + 5, 0);
            mx.assign(4 * n + 5, 0);
            lazyVal.assign(4 * n + 5, 0);
            hasLazy.assign(4 * n + 5, 0);
            
        }

        void pull(int v){
            sum[v] = sum[v << 1] + sum[v << 1 | 1];
            mn[v] = min(mn[v << 1], sum[v << 1] + mn[v << 1 | 1]);
            mx[v] = max(mx[v << 1], sum[v << 1] + mx[v << 1 | 1]);
        }

        void applySet(int v, int l, int r, int val){
            int len = r - l + 1;
            sum[v] = 1LL * val * len;

            if(val == 0){
                mn[v] = 0;
                mx[v] = 0;
            }
            else if(val > 0){
                mn[v] = val;
                mx[v] = 1LL * val * len;
            }
            else{
                mn[v] = 1LL * val * len;
                mx[v] = val;
            }

            hasLazy[v] = true;
            lazyVal[v] = val;
        }

        void push(int v, int l, int r){
            if(!hasLazy[v] || l == r) return;
            int m = (l + r) >> 1;
            applySet(v << 1, l, m, lazyVal[v]);
            applySet(v << 1 | 1, m + 1, r, lazyVal[v]);
            hasLazy[v] = false;
        }


        void update(int pos, int newval){
            update(1, 0, n - 1, pos, pos, newval);
        }

        void update(int v, int l, int r, int ql, int qr, int val){
            if(ql <= l && r <= qr){
                applySet(v, l, r, val);
                return;
            }
            push(v, l, r);
            int m = (l + r) >> 1;
            if(ql <= m) update(v << 1, l, m, ql, qr, val);
            if(qr >  m) update(v << 1 | 1, m + 1, r, ql, qr, val);
            pull(v);
        }

        int query(int x){
            if(x == 0) return -1;
            if(x < mn[1] || x > mx[1]) return n;
            int pref = 0;
            return query(1, 0, n - 1, x, pref);
        }

        int query(int v, int l, int r, int x, int& pref){
            if(l == r){
                if(pref + sum[v] == x) return l;
                return n;
            }

            push(v, l, r);

            int m = (l + r) >> 1;
            int L = v << 1;
            int R = v << 1 | 1;

            int leftMin = pref + mn[L];
            int leftMax = pref + mx[L];

            if(x >= leftMin && x <= leftMax){
                return query(L, l, m, x, pref);
            }
            else{
                pref += sum[L];
                return query(R, m + 1, r, x, pref);
            }
        }
    };

    int longestBalanced(vector<int>& nums) {
        int n = (int)nums.size();
        int m = 0;
        for(auto x : nums) m = max(m, x);
        m += 1;

        vector<int> lastPos(m, -1);
        SegTree S(n);

        int sum = 0;
        int ans = 0;

        for(int i = 0; i < n; i ++){
            if(lastPos[nums[i]] != -1){
                S.update(lastPos[nums[i]], 0);
            }
            else{
                if(nums[i] % 2){
                    sum += 1;
                }
                else{
                    sum -= 1;
                }
            }

            lastPos[nums[i]] = i;

            if(nums[i] % 2){
                S.update(i, +1);
            }
            else{
                S.update(i, -1);
            }

            int p = S.query(sum);
            ans = max(ans, i - p);
        }

        return ans;
    }
};
```

------------------------------------------------------------------------------------------------------------------------------



# [3713. Longest Balanced Substring I](https://leetcode.com/problems/longest-balanced-substring-i/)

Medium
 
You are given a string s consisting of lowercase English letters.

A substring of s is called balanced if all distinct characters in the substring appear the same number of times.

Return the length of the longest balanced substring of s.

 

Example 1:

Input: s = "abbac"

Output: 4

Explanation:

The longest balanced substring is "abba" because both distinct characters 'a' and 'b' each appear exactly 2 times.


Example 2:

Input: s = "zzabccy"

Output: 4

Explanation:

The longest balanced substring is "zabc" because the distinct characters 'z', 'a', 'b', and 'c' each appear exactly 1 time.​​​​​​​



Example 3:

Input: s = "aba"

Output: 2

Explanation:

​​​​​​​One of the longest balanced substrings is "ab" because both distinct characters 'a' and 'b' each appear exactly 1 time. Another longest balanced substring is "ba".

 

Constraints:

1 <= s.length <= 1000
s consists of lowercase English letters.



# Code
```cpp []
class Solution {
public:
    int longestBalanced(string s) {
        int n = s.size();

        // Transform char -> int
        vector<int> a(n);
        for (int i = 0; i < n; ++i) 
            a[i] = s[i] - 'a';

        int result = 0;
        for (int l = 0; l < n; ++l) {
            if (n - l <= result) 
                break;

            int cnt[26] = {0};          
            int uniq = 0, maxfreq = 0;  
            for (int r = l; r < n; ++r) {
                int i = a[r];

                if (cnt[i] == 0) 
                    ++uniq;

                ++cnt[i];
                if (cnt[i] > maxfreq) 
                    maxfreq = cnt[i];

                int cur = r - l + 1;
                if (uniq * maxfreq == cur && cur > result)
                    result = cur;
            }
        }
        return result;
    }
};
```

----------------------------------------------------------------------------------------------------

# [3714. Longest Balanced Substring II](https://leetcode.com/problems/longest-balanced-substring-ii)

Medium
 
You are given a string s consisting only of the characters 'a', 'b', and 'c'.

A substring of s is called balanced if all distinct characters in the substring appear the same number of times.

Return the length of the longest balanced substring of s.

 

Example 1:

Input: s = "abbac"

Output: 4

Explanation:

The longest balanced substring is "abba" because both distinct characters 'a' and 'b' each appear exactly 2 times.

Example 2:

Input: s = "aabcc"

Output: 3

Explanation:

The longest balanced substring is "abc" because all distinct characters 'a', 'b' and 'c' each appear exactly 1 time.

Example 3:

Input: s = "aba"

Output: 2

Explanation:

One of the longest balanced substrings is "ab" because both distinct characters 'a' and 'b' each appear exactly 1 time. Another longest balanced substring is "ba".

 

Constraints:

1 <= s.length <= 105
s contains only the characters 'a', 'b', and 'c'.


# Code
```cpp []
class Solution {
public:
    int mono(const string& s){
        if(s.empty()) return 0;
        int cnt = 1;
        int ans = 1;
        for(int i = 1; i < (int)s.size(); i ++){
            if(s[i] == s[i - 1]) cnt++;
            else cnt = 1;
            ans = max(ans, cnt);
        }
        return ans;
    }

    int duo(const string& s, char c1, char c2){
        map<int, int> pos;
        pos[0] = -1;
        int ans = 0;
        int delta = 0;
        for(int i = 0; i < (int)s.size(); i ++){
            if(s[i] != c1 && s[i] != c2){
                pos.clear();
                pos[0] = i;
                delta = 0;
                continue;
            }
            if(s[i] == c1){
                delta++;
            }
            else{
                delta--;
            }
            if(pos.find(delta) != pos.end()){
                ans = max(ans, i - pos[delta]);
            }
            else{
                pos[delta] = i;
            }
        }
        return ans;
    }

    int trio(const string& s){
        vector<int> cnt(3, 0);

        map<vector<int>, int> pos;
        pos[{0, 0}] = -1;

        int ans = 0;

        for(int i = 0; i < (int)s.size(); i++){
            cnt[s[i] - 'a']++;

            vector<int> key = {cnt[1] - cnt[0], cnt[2] - cnt[0]};

            if(pos.find(key) != pos.end()){
                ans = max(ans, i - pos[key]);
            }
            else{
                pos[key] = i;
            }
        }
        return ans;
    }
    int longestBalanced(string s) {
        return max({
            mono(s),
            duo(s, 'a', 'b'),
            duo(s, 'a', 'c'),
            duo(s, 'b', 'c'),
            trio(s)
        });
    }
};
```

------------------------------------------------------------------------------------------------------


# [799. Champagne Tower](https://leetcode.com/problems/champagne-tower)

Medium
 
We stack glasses in a pyramid, where the first row has 1 glass, the second row has 2 glasses, and so on until the 100th row.  Each glass holds one cup of champagne.

Then, some champagne is poured into the first glass at the top.  When the topmost glass is full, any excess liquid poured will fall equally to the glass immediately to the left and right of it.  When those glasses become full, any excess champagne will fall equally to the left and right of those glasses, and so on.  (A glass at the bottom row has its excess champagne fall on the floor.)

For example, after one cup of champagne is poured, the top most glass is full.  After two cups of champagne are poured, the two glasses on the second row are half full.  After three cups of champagne are poured, those two cups become full - there are 3 full glasses total now.  After four cups of champagne are poured, the third row has the middle glass half full, and the two outside glasses are a quarter full, as pictured below.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/09/tower.png)


Now after pouring some non-negative integer cups of champagne, return how full the jth glass in the ith row is (both i and j are 0-indexed.)

 

Example 1:

Input: poured = 1, query_row = 1, query_glass = 1

Output: 0.00000

Explanation: We poured 1 cup of champange to the top glass of the tower (which is indexed as (0, 0)). There will be no excess liquid so all the glasses under the top glass will remain empty.



Example 2:

Input: poured = 2, query_row = 1, query_glass = 1

Output: 0.50000

Explanation: We poured 2 cups of champange to the top glass of the tower (which is indexed as (0, 0)). There is one cup of excess liquid. The glass indexed as (1, 0) and the glass indexed as (1, 1) will share the excess liquid equally, and each will get half cup of champange.




Example 3:

Input: poured = 100000009, query_row = 33, query_glass = 17

Output: 1.00000
 

Constraints:

0 <= poured <= 109
0 <= query_glass <= query_row < 100


# Code
```cpp []
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        std::vector<std::vector<double>> tower(query_row + 1, std::vector<double>(query_row + 1, 0.0));
        tower[0][0] = static_cast<double>(poured);

        for (int row = 0; row < query_row; row++) {
            for (int glass = 0; glass <= row; glass++) {
                double excess = (tower[row][glass] - 1.0) / 2.0;
                if (excess > 0) {
                    tower[row + 1][glass] += excess;
                    tower[row + 1][glass + 1] += excess;
                }
            }
        }

        return std::min(1.0, tower[query_row][query_glass]);
    }
};
```

-------------------------------------------------------------------------------------------------------------------------

# [67. Add Binary](https://leetcode.com/problems/add-binary)
 
Easy
 
Given two binary strings a and b, return their sum as a binary string.

 

Example 1:

Input: a = "11", b = "1"

Output: "100"


Example 2:

Input: a = "1010", b = "1011"

Output: "10101"
 

Constraints:

1 <= a.length, b.length <= 104
a and b consist only of '0' or '1' characters.
Each string does not contain leading zeros except for the zero itself.



# Code
```cpp []
class Solution {
public:
    string addBinary(string a, string b) {
        string ans;
        int i=a.size()-1 , j=b.size()-1;
        int carry = 0;
        
        while(i>=0 || j>=0 || carry){
            if(i>=0) carry += a[i--] - '0';
            if(j>=0) carry += b[j--] - '0';
            ans.insert(ans.begin() , carry % 2 + '0');
            carry /= 2;
        }

        return ans;
    }
};
```

--------------------------------------------------------------------------------------------------


# [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/description)
 
Easy


Reverse bits of a given 32 bits signed integer.

 

Example 1:

Input: n = 43261596

Output: 964176192

Explanation:

Integer	Binary
43261596	00000010100101000001111010011100
964176192	00111001011110000010100101000000




Example 2:

Input: n = 2147483644

Output: 1073741822

Explanation:

Integer	Binary
2147483644	01111111111111111111111111111100
1073741822	00111111111111111111111111111110
 

Constraints:

0 <= n <= 231 - 2
n is even.
 

Follow up: If this function is called many times, how would you optimize it?


# Code
```cpp []
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t ans = 0;
        for (int i = 0; i < 32; i++) {
            ans <<= 1;
            ans |= (n & 1);
            n >>= 1;
        }
        return ans;
    }
};
```

-------------------------------------------------------------------------


# [401. Binary Watch](https://leetcode.com/problems/binary-watch)

Easy
 
A binary watch has 4 LEDs on the top to represent the hours (0-11), and 6 LEDs on the bottom to represent the minutes (0-59). Each LED represents a zero or one, with the least significant bit on the right.

For example, the below binary watch reads "4:51".

![img](https://assets.leetcode.com/uploads/2021/04/08/binarywatch.jpg)

Given an integer turnedOn which represents the number of LEDs that are currently on (ignoring the PM), return all possible times the watch could represent. You may return the answer in any order.

The hour must not contain a leading zero.

For example, "01:00" is not valid. It should be "1:00".
The minute must consist of two digits and may contain a leading zero.

For example, "10:2" is not valid. It should be "10:02".
 


Example 1:

Input: turnedOn = 1

Output: ["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]



Example 2:

Input: turnedOn = 9

Output: []
 

Constraints:

0 <= turnedOn <= 10


# Code
```cpp []
class Solution {
public:
    vector<string> readBinaryWatch(int k) {
        if (k == 0) return {"0:00"};
        if (k > 8) return {};
        vector<string> res;
        int q = (1 << k) - 1;

        while (q < (1 << 10)) {
            string time = isValid(q);
            if (!time.empty()) res.push_back(time);
            int r = q & -q;
            int n = q + r;
            //Find the Next Bit Combination (with the same hamming weight)
            //0000000111 -> 0000001011 -> 0000001101 -> 0000001110 -> 0000010011
            //0000010101 -> 0000010110 -> 0000011001 -> 0000011010 -> 0000011100 
            q = (((n ^ q) >> 2) / r) | n;
        }
        return res;
    }

    string isValid(int q) {
        int hour = q >> 6;
        int min = q & 63;
        if (hour >= 12 || min >= 60) return "";
        //return format("{}:{:02}", hour, min);
        return to_string(hour) + ":" + (min < 10 ? "0" : "") + to_string(min);
    }
};

```

----------------------------------------------------------------------------------------------------------

# [693. Binary Number with Alternating Bits](https://leetcode.com/problems/binary-number-with-alternating-bits/)

Easy
 
Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

 

Example 1:

Input: n = 5

Output: true

Explanation: The binary representation of 5 is: 101



Example 2:

Input: n = 7

Output: false

Explanation: The binary representation of 7 is: 111.



Example 3:

Input: n = 11

Output: false

Explanation: The binary representation of 11 is: 1011.
 

Constraints:

1 <= n <= 231 - 1


# Code
```cpp []
class Solution {
public:
    bool hasAlternatingBits(int n) {
        unsigned int x = n ^ (n >> 1);
        return (x & (x + 1)) == 0;
    }
};
```

---------------------------------------------------------------------------------------------

# [696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings/)

Easy
 
Given a binary string s, return the number of non-empty substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

 

Example 1:

Input: s = "00110011"

Output: 6

Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
Notice that some of these substrings repeat and are counted the number of times they occur.
Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.



Example 2:

Input: s = "10101"

Output: 4

Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
 

Constraints:

1 <= s.length <= 105
s[i] is either '0' or '1'.


# Code
```cpp []
class Solution {
public:
    int countBinarySubstrings(string s) {
        int res = 0, prev = 0, strk = 1;

        for (int i = 1; i < s.size(); i++) {
            if (s[i] == s[i - 1]) strk++;
            else {
                prev = strk;
                strk = 1;
            }
            if (strk <= prev) res++;
        }
        return res;
    }
};

```


----------------------------------------------------------------------------------------------

# [761. Special Binary String](https://leetcode.com/problems/special-binary-string)

Hard
 
Special binary strings are binary strings with the following two properties:

The number of 0's is equal to the number of 1's.
Every prefix of the binary string has at least as many 1's as 0's.
You are given a special binary string s.

A move consists of choosing two consecutive, non-empty, special substrings of s, and swapping them. Two strings are consecutive if the last character of the first string is exactly one index before the first character of the second string.

Return the lexicographically largest resulting string possible after applying the mentioned operations on the string.

 

Example 1:

Input: s = "11011000"

Output: "11100100"

Explanation: The strings "10" [occuring at s[1]] and "1100" [at s[3]] are swapped.
This is the lexicographically largest string possible after some number of swaps.



Example 2:

Input: s = "10"

Output: "10"
 

Constraints:

1 <= s.length <= 50
s[i] is either '0' or '1'.
s is a special binary string.


