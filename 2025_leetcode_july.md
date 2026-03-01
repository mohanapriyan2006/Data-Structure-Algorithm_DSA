
------------------------------------
# *LEETCODE* - **july-2025**
------------------------------------



# 3330. Find the Original Typed String I -> [Leetcode](https://leetcode.com/problems/find-the-original-typed-string-i/description/)

Easy

Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and may press a key for too long, resulting in a character being typed multiple times.

Although Alice tried to focus on her typing, she is aware that she may still have done this at most once.

You are given a string word, which represents the final output displayed on Alice's screen.

Return the total number of possible original strings that Alice might have intended to type.

 

Example 1:

Input: word = "abbcccc"

Output: 5

Explanation:

The possible strings are: "abbcccc", "abbccc", "abbcc", "abbc", and "abcccc".

Example 2:

Input: word = "abcd"

Output: 1

Explanation:

The only possible string is "abcd".

Example 3:

Input: word = "aaaa"

Output: 4

 

Constraints:

1 <= word.length <= 100
word consists only of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int possibleStringCount(string word) {
        int ans = 1;

        for(int i=1 ; i<word.length() ; ++i){
            if(word[i] == word[i-1]) ans++;
        }

        return ans;
    }
};
```


-----

# 344. Reverse String -> [LeetCode](https://leetcode.com/problems/reverse-string/description/)

Easy

Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.

 

Example 1:

Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]


Example 2:

Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
 

Constraints:

1 <= s.length <= 105
s[i] is a printable ascii character.

# Code
```cpp []
class Solution {
public:
    void reverseString(vector<char>& s) {
      int i=0 , j = s.size() -1;

      while(i<j){
        char c = s[i];
        s[i++] = s[j];
        s[j--] = c;
      }
    }
};
```

------

# 151. Reverse Words in a String -> [LeetCode](https://leetcode.com/problems/reverse-words-in-a-string/description/)
 
Medium

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

Example 1:

Input: s = "the sky is blue"
Output: "blue is sky the"


Example 2:

Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.


Example 3:

Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
 

Constraints:

1 <= s.length <= 104
s contains English letters (upper-case and lower-case), digits, and spaces ' '.
There is at least one word in s.
 

Follow-up: If the string data type is mutable in your language, can you solve it in-place with O(1) extra space?

# Code
```cpp []
class Solution {
public:
    string reverseWords(string s) {
        vector<string> words;

        string word;

        for(const char c:s){
            if(c != ' '){
                word+=c;
            }else if(c == ' ' && !word.empty() ){
                words.push_back(word);
                word.clear();
            }
        }

        if(!word.empty()){
           words.push_back(word);
           word.clear();
        }

        string ans;

        for(int i=words.size() - 1 ; i>=0 ; --i){
            ans+=words[i];
            if( i != 0) ans+=' ';
        }

        return ans;


    }
};
```


--------------------


# 3333. Find the Original Typed String II -> [LeetCode](https://leetcode.com/problems/find-the-original-typed-string-ii/description)

Hard

Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and may press a key for too long, resulting in a character being typed multiple times.

You are given a string word, which represents the final output displayed on Alice's screen. You are also given a positive integer k.

Return the total number of possible original strings that Alice might have intended to type, if she was trying to type a string of size at least k.

Since the answer may be very large, return it modulo 109 + 7.

 

Example 1:

Input: word = "aabbccdd", k = 7

Output: 5

Explanation:

The possible strings are: "aabbccdd", "aabbccd", "aabbcdd", "aabccdd", and "abbccdd".

Example 2:

Input: word = "aabbccdd", k = 8

Output: 1

Explanation:

The only possible string is "aabbccdd".

Example 3:

Input: word = "aaabbb", k = 3

Output: 8

 

Constraints:

1 <= word.length <= 5 * 105
word consists only of lowercase English letters.
1 <= k <= 2000


# Code
```cpp []
class Solution {
 public:
  int possibleStringCount(string word, int k) {
    const vector<int> groups = getConsecutiveLetters(word);
    const int totalCombinations =
        accumulate(groups.begin(), groups.end(), 1L,
                   [](long acc, int group) { return acc * group % kMod; });
    if (k <= groups.size())
      return totalCombinations;

    
    vector<int> dp(k);
    dp[0] = 1;  

    for (int i = 0; i < groups.size(); ++i) {
      vector<int> newDp(k);
      int windowSum = 0;
      int group = groups[i];
      for (int j = i; j < k; ++j) {
        newDp[j] = (newDp[j] + windowSum) % kMod;
        windowSum = (windowSum + dp[j]) % kMod;
        if (j >= group)
          windowSum = (windowSum - dp[j - group] + kMod) % kMod;
      }
      dp = std::move(newDp);
    }

    const int invalidCombinations =
        accumulate(dp.begin(), dp.end(), 0,
                   [](int acc, int count) { return (acc + count) % kMod; });
    return (totalCombinations - invalidCombinations + kMod) % kMod;
  }

 private:
  static constexpr int kMod = 1'000'000'007;

  
  vector<int> getConsecutiveLetters(const string& word) {
    vector<int> groups;
    int group = 1;
    for (int i = 1; i < word.length(); ++i)
      if (word[i] == word[i - 1]) {
        ++group;
      } else {
        groups.push_back(group);
        group = 1;
      }
    groups.push_back(group);
    return groups;
  }
};
```

--------

# 83. Remove Duplicates from Sorted List -> [LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/)

Easy

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

 

Example 1:


Input: head = [1,1,2]
Output: [1,2]


Example 2:


Input: head = [1,1,2,3,3]
Output: [1,2,3]
 

Constraints:

The number of nodes in the list is in the range [0, 300].
-100 <= Node.val <= 100
The list is guaranteed to be sorted in ascending order.

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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* temp = head;
        
        if(head == nullptr) return head;
        
        while(temp != nullptr){
            while(temp->next && temp->val == temp->next->val){
                temp->next = temp->next->next;
            }
            temp = temp->next;
        }
        
        return head;
    }
};
```


-----------------


# 876. Middle of the Linked List -> [LeetCode](https://leetcode.com/problems/middle-of-the-linked-list/description/)

Easy

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

 

Example 1:


Input: head = [1,2,3,4,5]
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

        while( fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

---------



# 2095. Delete the Middle Node of a Linked List -> [LeetCode](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/description/)

Medium

You are given the head of a linked list. Delete the middle node, and return the head of the modified linked list.

The middle node of a linked list of size n is the ⌊n / 2⌋th node from the start using 0-based indexing, where ⌊x⌋ denotes the largest integer less than or equal to x.

For n = 1, 2, 3, 4, and 5, the middle nodes are 0, 1, 1, 2, and 2, respectively.
 

Example 1:


Input: head = [1,3,4,7,1,2,6]
Output: [1,3,4,1,2,6]
Explanation:
The above figure represents the given linked list. The indices of the nodes are written below.
Since n = 7, node 3 with value 7 is the middle node, which is marked in red.
We return the new list after removing this node. 


Example 2:


Input: head = [1,2,3,4]
Output: [1,2,4]
Explanation:
The above figure represents the given linked list.
For n = 4, node 2 with value 3 is the middle node, which is marked in red.


Example 3:


Input: head = [2,1]
Output: [2]
Explanation:
The above figure represents the given linked list.
For n = 2, node 1 with value 1 is the middle node, which is marked in red.
Node 0 with value 2 is the only node remaining after removing node 1.
 

Constraints:

The number of nodes in the list is in the range [1, 105].
1 <= Node.val <= 105

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
    ListNode* deleteMiddle(ListNode* head) {

        if( head == nullptr || head->next == nullptr ){
            return nullptr;
        }

        int c = 0;

        ListNode* temp = head;

        while(temp != nullptr){
            c++;
            temp = temp->next;
        }

        int mid = c/2;
        ListNode* cur = head;
        for(int i=1 ; i<mid ; ++i){
            cur = cur->next;
        }

        ListNode* toDel = cur->next;
        cur->next = cur->next->next;
        delete toDel;

        return head;
    }
};
```

-----


# 206. Reverse Linked List -> [LeetCode](https://leetcode.com/problems/reverse-linked-list/description/)

Easy

Given the head of a singly linked list, reverse the list, and return the reversed list.

 

Example 1:


Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]


Example 2:


Input: head = [1,2]
Output: [2,1]


Example 3:

Input: head = []
Output: []
 

Constraints:

The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000
 

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?

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
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
         while(head != nullptr){
            ListNode* next = head->next;
            head->next = prev;
            prev = head;
            head = next;
         }

         return prev;
    }
};
```

-----------

# 3304. Find the K-th Character in String Game I -> [LeetCode](https://leetcode.com/problems/find-the-k-th-character-in-string-game-i/description/)

Easy

Alice and Bob are playing a game. Initially, Alice has a string word = "a".

You are given a positive integer k.

Now Bob will ask Alice to perform the following operation forever:

Generate a new string by changing each character in word to its next character in the English alphabet, and append it to the original word.
For example, performing the operation on "c" generates "cd" and performing the operation on "zb" generates "zbac".

Return the value of the kth character in word, after enough operations have been done for word to have at least k characters.

Note that the character 'z' can be changed to 'a' in the operation.

 

Example 1:

Input: k = 5

Output: "b"

Explanation:

Initially, word = "a". We need to do the operation three times:

Generated string is "b", word becomes "ab".
Generated string is "bc", word becomes "abbc".
Generated string is "bccd", word becomes "abbcbccd".


Example 2:

Input: k = 10

Output: "c"

 

Constraints:

1 <= k <= 500

# Code
```cpp []
class Solution {
public:
    char kthCharacter(unsigned k) {
        return 'a' + popcount(k-1);
    }
};
```


-----


# 20. Valid Parentheses -> [LeetCode](https://leetcode.com/problems/valid-parentheses/description/)

Easy

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

Input: s = "()"

Output: true

Example 2:

Input: s = "()[]{}"

Output: true

Example 3:

Input: s = "(]"

Output: false

Example 4:

Input: s = "([])"

Output: true

 

Constraints:

1 <= s.length <= 104
s consists of parentheses only '()[]{}'.

# Code
```cpp []
class Solution {
public:
    bool isValid(string s) {

        stack<char> st;

        for(const char c:s){
           if(!st.empty()){
            if(st.top() == '(' && c == ')'){
                st.pop();
            }
            else if(st.top() == '{' && c == '}'){
                st.pop();
            }
            else if(st.top() == '[' && c == ']'){
                st.pop();
            }else{
                st.push(c);
            }
           }else{
            st.push(c);
           }

        }

        return st.empty();
    }
};
```

--------


# 3307. Find the K-th Character in String Game II -> [LeetCode](https://leetcode.com/problems/find-the-k-th-character-in-string-game-ii/description/)

Hard

Alice and Bob are playing a game. Initially, Alice has a string word = "a".

You are given a positive integer k. You are also given an integer array operations, where operations[i] represents the type of the ith operation.

Now Bob will ask Alice to perform all operations in sequence:

If operations[i] == 0, append a copy of word to itself.
If operations[i] == 1, generate a new string by changing each character in word to its next character in the English alphabet, and append it to the original word. For example, performing the operation on "c" generates "cd" and performing the operation on "zb" generates "zbac".
Return the value of the kth character in word after performing all the operations.

Note that the character 'z' can be changed to 'a' in the second type of operation.

 

Example 1:

Input: k = 5, operations = [0,0,0]

Output: "a"

Explanation:

Initially, word == "a". Alice performs the three operations as follows:

Appends "a" to "a", word becomes "aa".
Appends "aa" to "aa", word becomes "aaaa".
Appends "aaaa" to "aaaa", word becomes "aaaaaaaa".


Example 2:

Input: k = 10, operations = [0,1,0,1]

Output: "b"

Explanation:

Initially, word == "a". Alice performs the four operations as follows:

Appends "a" to "a", word becomes "aa".
Appends "bb" to "aa", word becomes "aabb".
Appends "aabb" to "aabb", word becomes "aabbaabb".
Appends "bbccbbcc" to "aabbaabb", word becomes "aabbaabbbbccbbcc".
 

Constraints:

1 <= k <= 1014
1 <= operations.length <= 100
operations[i] is either 0 or 1.
The input is generated such that word has at least k characters after all operations.

# Code
```cpp []
class Solution {
 public:
  char kthCharacter(long long k, vector<int>& operations) {
    const int operationsCount = ceil(log2(k));
    int increases = 0;

    for (int i = operationsCount - 1; i >= 0; --i) {
      const long halfSize = 1L << i;
      if (k > halfSize) {
        k -= halfSize;
        increases += operations[i];
      }
    }

    return 'a' + increases % 26;
  }
};
```

------



# 155. Min Stack -> [LeetCode](https://leetcode.com/problems/min-stack/description/)

Medium

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

MinStack() initializes the stack object.
void push(int val) pushes the element val onto the stack.
void pop() removes the element on the top of the stack.
int top() gets the top element of the stack.
int getMin() retrieves the minimum element in the stack.
You must implement a solution with O(1) time complexity for each function.

 

Example 1:

Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
 

Constraints:

-231 <= val <= 231 - 1
Methods pop, top and getMin operations will always be called on non-empty stacks.
At most 3 * 104 calls will be made to push, pop, top, and getMin.

# Code
```cpp []
class MinStack {
public:
    
    void push(int val) {
        if(st.empty()) st.emplace(val,val);
        else{
            int mini = min(val,st.top().second);
            st.emplace(val,mini);
        }
    }
    
    void pop() {
        st.pop();
    }
    
    int top() {
        return st.top().first;
    }
    
    int getMin() {
        return st.top().second;
    }

private:
    stack<pair<int,int>> st;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```


------------

# 225. Implement Stack using Queues -> [LeetCode](https://leetcode.com/problems/implement-stack-using-queues/description)

Easy

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

void push(int x) Pushes element x to the top of the stack.
int pop() Removes the element on the top of the stack and returns it.
int top() Returns the element on the top of the stack.
boolean empty() Returns true if the stack is empty, false otherwise.
Notes:

You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.
Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.
 

Example 1:

Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

Explanation
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False
 

Constraints:

1 <= x <= 9
At most 100 calls will be made to push, pop, top, and empty.
All the calls to pop and top are valid.
 

Follow-up: Can you implement the stack using only one queue?

# Code
```cpp []
class MyStack {
public:
    
    void push(int x) {
        st.push(x);
        for(int i=0 ; i<st.size() - 1 ; ++i){
            st.push(st.front());
            st.pop();
        }
    }
    
    int pop() {
        int n = st.front();
        st.pop();
        return n;
    }
    
    int top() {
        return st.front();
    }
    
    bool empty() {
        return st.empty();
    }

private:
    queue<int> st;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

----

# 1394. Find Lucky Integer in an Array -> [LeetCode](https://leetcode.com/problems/find-lucky-integer-in-an-array/description/)

Easy

Given an array of integers arr, a lucky integer is an integer that has a frequency in the array equal to its value.

Return the largest lucky integer in the array. If there is no lucky integer return -1.

 

Example 1:

Input: arr = [2,2,3,4]
Output: 2
Explanation: The only lucky number in the array is 2 because frequency[2] == 2.


Example 2:

Input: arr = [1,2,2,3,3,3]
Output: 3
Explanation: 1, 2 and 3 are all lucky numbers, return the largest of them.


Example 3:

Input: arr = [2,2,2,3,3]
Output: -1
Explanation: There are no lucky numbers in the array.
 

Constraints:

1 <= arr.length <= 500
1 <= arr[i] <= 500

# Code
```cpp []
class Solution {
public:
    int findLucky(vector<int>& arr) {

        int ans = -1;

        unordered_map<int,int> freq;
        for(const int num:arr){
            freq[num]++;
        }

        for(auto it:freq){
            if(it.first == it.second) ans = max(it.first,ans);
        }

        return ans;
    }
};
```

-----------

# 169. Majority Element -> [LeetCode](https://leetcode.com/problems/majority-element/description/)

Easy

### amazon

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 

Example 1:

Input: nums = [3,2,3]
Output: 3
Example 2:

Input: nums = [2,2,1,1,1,2,2]
Output: 2
 

Constraints:

n == nums.length
1 <= n <= 5 * 104
-109 <= nums[i] <= 109
 

Follow-up: Could you solve the problem in linear time and in O(1) space?

# Code
```cpp []
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int x = nums.size()/2;
        unordered_map<int,int> freq;
        for(const int num:nums){
            freq[num]++;
        }

        int ans = -1;

        for(auto it:freq){
            if(it.second > x) ans = it.first;
        }

        return ans;
    }
};
```

-----



# 1865. Finding Pairs With a Certain Sum -> [LeetCode](https://leetcode.com/problems/finding-pairs-with-a-certain-sum/description)

Medium

You are given two integer arrays nums1 and nums2. You are tasked to implement a data structure that supports queries of two types:

Add a positive integer to an element of a given index in the array nums2.
Count the number of pairs (i, j) such that nums1[i] + nums2[j] equals a given value (0 <= i < nums1.length and 0 <= j < nums2.length).
Implement the FindSumPairs class:

FindSumPairs(int[] nums1, int[] nums2) Initializes the FindSumPairs object with two integer arrays nums1 and nums2.
void add(int index, int val) Adds val to nums2[index], i.e., apply nums2[index] += val.
int count(int tot) Returns the number of pairs (i, j) such that nums1[i] + nums2[j] == tot.
 

Example 1:

Input
["FindSumPairs", "count", "add", "count", "count", "add", "add", "count"]
[[[1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]], [7], [3, 2], [8], [4], [0, 1], [1, 1], [7]]
Output
[null, 8, null, 2, 1, null, null, 11]

Explanation

FindSumPairs findSumPairs = new FindSumPairs([1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]);
findSumPairs.count(7);  // return 8; pairs (2,2), (3,2), (4,2), (2,4), (3,4), (4,4) make 2 + 5 and pairs (5,1), (5,5) make 3 + 4
findSumPairs.add(3, 2); // now nums2 = [1,4,5,4,5,4]
findSumPairs.count(8);  // return 2; pairs (5,2), (5,4) make 3 + 5
findSumPairs.count(4);  // return 1; pair (5,0) makes 3 + 1
findSumPairs.add(0, 1); // now nums2 = [2,4,5,4,5,4]
findSumPairs.add(1, 1); // now nums2 = [2,5,5,4,5,4]
findSumPairs.count(7);  // return 11; pairs (2,1), (2,2), (2,4), (3,1), (3,2), (3,4), (4,1), (4,2), (4,4) make 2 + 5 and pairs (5,3), (5,5) make 3 + 4
 

Constraints:

1 <= nums1.length <= 1000
1 <= nums2.length <= 105
1 <= nums1[i] <= 109
1 <= nums2[i] <= 105
0 <= index < nums2.length
1 <= val <= 105
1 <= tot <= 109
At most 1000 calls are made to add and count each.

# Code
```cpp []
class FindSumPairs {
 public:
  FindSumPairs(vector<int>& nums1, vector<int>& nums2)
      : nums1(nums1), nums2(nums2) {
    for (const int num : nums2)
      ++count2[num];
  }

  void add(int index, int val) {
    --count2[nums2[index]];
    nums2[index] += val;
    ++count2[nums2[index]];
  }

  int count(int tot) {
    int ans = 0;
    for (const int num : nums1) {
      const int target = tot - num;
      if (const auto it = count2.find(target); it != count2.cend())
        ans += it->second;
    }
    return ans;
  }

 private:
  vector<int> nums1;
  vector<int> nums2;
  unordered_map<int, int> count2;
};
```

-------

# I1353. Maximum Number of Events That Can Be Attended -> [LeetCode](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/description/)

Topics

You are given an array of events where events[i] = [startDayi, endDayi]. Every event i starts at startDayi and ends at endDayi.

You can attend an event i at any day d where startTimei <= d <= endTimei. You can only attend one event at any time d.

Return the maximum number of events you can attend.

 

Example 1:


Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.

Example 2:

Input: events= [[1,2],[2,3],[3,4],[1,2]]
Output: 4
 

Constraints:

1 <= events.length <= 105
events[i].length == 2
1 <= startDayi <= endDayi <= 105

# Code
```cpp []
class Solution {
public:
    int maxEvents(vector<vector<int>>& events) {
        int ans = 0;
        int d = 0; // the current day
        int i = 0; // events' index
        priority_queue<int, vector<int>, greater<>> minHeap;

        ranges::sort(events);

        while (!minHeap.empty() || i < events.size()) {
            // If no events are available to attend today, let time flies to the
            // next available event.
            if (minHeap.empty())
                d = events[i][0];
            // All the events starting from today are newly available.
            while (i < events.size() && events[i][0] == d)
                minHeap.push(events[i++][1]);
            // Greedily attend the event that'll end the earliest since it has
            // higher chance can't be attended in the future.
            minHeap.pop();
            ++ans;
            ++d;
            // Pop the events that can't be attended.
            while (!minHeap.empty() && minHeap.top() < d)
                minHeap.pop();
        }

        return ans;
    }
};
```

-------



# 203. Remove Linked List Elements -> [LeetCode](https://leetcode.com/problems/remove-linked-list-elements/description/)
Easy

Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

 

Example 1:


Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]


Example 2:

Input: head = [], val = 1
Output: []


Example 3:

Input: head = [7,7,7,7], val = 7
Output: []
 

Constraints:

The number of nodes in the list is in the range [0, 104].
1 <= Node.val <= 50
0 <= val <= 50

# Code
```cpp []
class Solution {
 public:
  ListNode* removeElements(ListNode* head, int val) {
    ListNode dummy(0, head);
    ListNode* prev = &dummy;

    for (; head; head = head->next)
      if (head->val != val) {
        prev->next = head;
        prev = prev->next;
      }
    prev->next = nullptr;  // In case that the last value equals `val`.

    return dummy.next;
  }
};
```

-------



# 1751. Maximum Number of Events That Can Be Attended II -> [LeetCode](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended-ii/description)

Hard

You are given an array of events where events[i] = [startDayi, endDayi, valuei]. The ith event starts at startDayi and ends at endDayi, and if you attend this event, you will receive a value of valuei. You are also given an integer k which represents the maximum number of events you can attend.

You can only attend one event at a time. If you choose to attend an event, you must attend the entire event. Note that the end day is inclusive: that is, you cannot attend two events where one of them starts and the other ends on the same day.

Return the maximum sum of values that you can receive by attending events.

 

Example 1:



Input: events = [[1,2,4],[3,4,3],[2,3,1]], k = 2
Output: 7
Explanation: Choose the green events, 0 and 1 (0-indexed) for a total value of 4 + 3 = 7.


Example 2:



Input: events = [[1,2,4],[3,4,3],[2,3,10]], k = 2
Output: 10
Explanation: Choose event 2 for a total value of 10.
Notice that you cannot attend any other event as they overlap, and that you do not have to attend k events.


Example 3:



Input: events = [[1,1,1],[2,2,2],[3,3,3],[4,4,4]], k = 3
Output: 9
Explanation: Although the events do not overlap, you can only attend 3 events. Pick the highest valued three.
 

Constraints:

1 <= k <= events.length
1 <= k * events.length <= 106
1 <= startDayi <= endDayi <= 109
1 <= valuei <= 106

# Code
```cpp []
class Solution {
public:
    int maxValue(vector<vector<int>>& events, int k) {
        vector<vector<int>> mem(events.size(), vector<int>(k + 1, -1));
        ranges::sort(events);
        return maxValue(events, 0, k, mem);
    }

private:
    // Returns the maximum sum of values that you can receive by attending
    // events[i..n), where k is the maximum number of attendancevents.
    int maxValue(const vector<vector<int>>& events, int i, int k,
                 vector<vector<int>>& mem) {
        if (k == 0 || i == events.size())
            return 0;
        if (mem[i][k] != -1)
            return mem[i][k];

        // Binary search `events` to find the first index j
        // s.t. events[j][0] > events[i][1].
        const auto it = upper_bound(
            events.begin() + i, events.end(), events[i][1],
            [](int end, const vector<int>& event) { return event[0] > end; });
        const int j = distance(events.begin(), it);
        return mem[i][k] = max(events[i][2] + maxValue(events, j, k - 1, mem),
                               maxValue(events, i + 1, k, mem));
    }
};
```

------


# 735. Asteroid Collision -> [LeetCode](https://leetcode.com/problems/asteroid-collision/description/)

Medium

We are given an array asteroids of integers representing asteroids in a row. The indices of the asteriod in the array represent their relative position in space.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

 

Example 1:

Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.


Example 2:

Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.


Example 3:

Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
 

Constraints:

2 <= asteroids.length <= 104
-1000 <= asteroids[i] <= 1000
asteroids[i] != 0

# Code
```cpp []
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& nums) {
        vector<int> ans;

        for(const int n:nums){
            bool isExplode = false;
            while(!ans.empty() && n < 0 && ans.back() > 0){
                if(abs(ans.back()) < abs(n)) ans.pop_back();
                else if(abs(ans.back()) == abs(n)){
                    ans.pop_back();
                    isExplode = true;
                    break;
                }else{
                    isExplode = true;
                    break;
                }
            }

            if(!isExplode) ans.push_back(n);
        }

        return ans;
    }
};
```

---------


# 150. Evaluate Reverse Polish Notation -> [LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

Medium

You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are '+', '-', '*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.
 

Example 1:

Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9


Example 2:

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6


Example 3:

Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
 

Constraints:

1 <= tokens.length <= 104
tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200].

# Code
```cpp []
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> ans;

        for (const string& s : tokens) {
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                int y = ans.top();
                ans.pop();
                int x = ans.top();
                ans.pop();

                if (s == "+") {
                    ans.push((x + y));
                    continue;
                }
                if (s == "-") {
                    ans.push((x - y));
                    continue;
                }
                if (s == "*") {
                    ans.push((x * y));
                    continue;
                }
                if (s == "/") {
                    ans.push((x / y));
                    continue;
                }

            } else {
                ans.push(stoi(s));
            }
        }

        return ans.top();
    }
};
```

-------



# 3439. Reschedule Meetings for Maximum Free Time I -> [LeetCode](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-i/description/)

Medium

You are given an integer eventTime denoting the duration of an event, where the event occurs from time t = 0 to time t = eventTime.

You are also given two integer arrays startTime and endTime, each of length n. These represent the start and end time of n non-overlapping meetings, where the ith meeting occurs during the time [startTime[i], endTime[i]].

You can reschedule at most k meetings by moving their start time while maintaining the same duration, to maximize the longest continuous period of free time during the event.

The relative order of all the meetings should stay the same and they should remain non-overlapping.

Return the maximum amount of free time possible after rearranging the meetings.

Note that the meetings can not be rescheduled to a time outside the event.

 

Example 1:

Input: eventTime = 5, k = 1, startTime = [1,3], endTime = [2,5]

Output: 2

Explanation:



Reschedule the meeting at [1, 2] to [2, 3], leaving no meetings during the time [0, 2].

Example 2:

Input: eventTime = 10, k = 1, startTime = [0,2,9], endTime = [1,4,10]

Output: 6

Explanation:



Reschedule the meeting at [2, 4] to [1, 3], leaving no meetings during the time [3, 9].

Example 3:

Input: eventTime = 5, k = 2, startTime = [0,1,2,3,4], endTime = [1,2,3,4,5]

Output: 0

Explanation:

There is no time during the event not occupied by meetings.

 

Constraints:

1 <= eventTime <= 109
n == startTime.length == endTime.length
2 <= n <= 105
1 <= k <= n
0 <= startTime[i] < endTime[i] <= eventTime
endTime[i] <= startTime[i + 1] where i lies in the range [0, n - 2].


# Code
```cpp []
class Solution {
public:
    int maxFreeTime(int eventTime, int k, vector<int>& startTime,
                    vector<int>& endTime) {
        const vector<int> gaps = getGaps(eventTime, startTime, endTime);
        int windowSum = accumulate(gaps.begin(), gaps.begin() + k + 1, 0);
        int ans = windowSum;

        for (int i = k + 1; i < gaps.size(); ++i) {
            windowSum += gaps[i] - gaps[i - k - 1];
            ans = max(ans, windowSum);
        }

        return ans;
    }

private:
    vector<int> getGaps(int eventTime, const vector<int>& startTime,
                        const vector<int>& endTime) {
        vector<int> gaps{startTime[0]};
        for (int i = 1; i < startTime.size(); ++i)
            gaps.push_back(startTime[i] - endTime[i - 1]);
        gaps.push_back(eventTime - endTime.back());
        return gaps;
    }
};
```

------




# 3440. Reschedule Meetings for Maximum Free Time II -> [LeetCode](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-ii/description)

Medium

You are given an integer eventTime denoting the duration of an event. You are also given two integer arrays startTime and endTime, each of length n.

These represent the start and end times of n non-overlapping meetings that occur during the event between time t = 0 and time t = eventTime, where the ith meeting occurs during the time [startTime[i], endTime[i]].

You can reschedule at most one meeting by moving its start time while maintaining the same duration, such that the meetings remain non-overlapping, to maximize the longest continuous period of free time during the event.

Return the maximum amount of free time possible after rearranging the meetings.

Note that the meetings can not be rescheduled to a time outside the event and they should remain non-overlapping.

Note: In this version, it is valid for the relative ordering of the meetings to change after rescheduling one meeting.

 

Example 1:

Input: eventTime = 5, startTime = [1,3], endTime = [2,5]

Output: 2

Explanation:



Reschedule the meeting at [1, 2] to [2, 3], leaving no meetings during the time [0, 2].

Example 2:

Input: eventTime = 10, startTime = [0,7,9], endTime = [1,8,10]

Output: 7

Explanation:



Reschedule the meeting at [0, 1] to [8, 9], leaving no meetings during the time [0, 7].

Example 3:

Input: eventTime = 10, startTime = [0,3,7,9], endTime = [1,4,8,10]

Output: 6

Explanation:



Reschedule the meeting at [3, 4] to [8, 9], leaving no meetings during the time [1, 7].

Example 4:

Input: eventTime = 5, startTime = [0,1,2,3,4], endTime = [1,2,3,4,5]

Output: 0

Explanation:

There is no time during the event not occupied by meetings.

 

Constraints:

1 <= eventTime <= 109
n == startTime.length == endTime.length
2 <= n <= 105
0 <= startTime[i] < endTime[i] <= eventTime
endTime[i] <= startTime[i + 1] where i lies in the range [0, n - 2].

# Code
```cpp []
class Solution {
public:
    int maxFreeTime(int eventTime, vector<int>& startTime,
                    vector<int>& endTime) {
        const int n = startTime.size();
        const vector<int> gaps = getGaps(eventTime, startTime, endTime);
        int ans = 0;
        vector<int> maxLeft(n + 1);  
        vector<int> maxRight(n + 1); 

        maxLeft[0] = gaps[0];
        maxRight[n] = gaps[n];

        for (int i = 1; i < n + 1; ++i)
            maxLeft[i] = max(gaps[i], maxLeft[i - 1]);

        for (int i = n - 1; i >= 0; --i)
            maxRight[i] = max(gaps[i], maxRight[i + 1]);

        for (int i = 0; i < n; ++i) {
            const int currMeetingTime = endTime[i] - startTime[i];
            const int adjacentGapsSum = gaps[i] + gaps[i + 1];
            const bool canMoveMeeting =
                currMeetingTime <= max(i > 0 ? maxLeft[i - 1] : 0, //
                                       i + 2 < n + 1 ? maxRight[i + 2] : 0);
            ans = max(ans,
                      adjacentGapsSum + (canMoveMeeting ? currMeetingTime : 0));
        }

        return ans;
    }

private:
    vector<int> getGaps(int eventTime, const vector<int>& startTime,
                        const vector<int>& endTime) {
        vector<int> gaps{startTime[0]};
        for (int i = 1; i < startTime.size(); ++i)
            gaps.push_back(startTime[i] - endTime[i - 1]);
        gaps.push_back(eventTime - endTime.back());
        return gaps;
    }
};
```

-------

# 692. Top K Frequent Words -> [LeetCode](https://leetcode.com/problems/top-k-frequent-words/description/)

Medium

Given an array of strings words and an integer k, return the k most frequent strings.

Return the answer sorted by the frequency from highest to lowest. Sort the words with the same frequency by their lexicographical order.

 

Example 1:

Input: words = ["i","love","leetcode","i","love","coding"], k = 2
Output: ["i","love"]
Explanation: "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.

Example 2:

Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
Output: ["the","is","sunny","day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.
 

Constraints:

1 <= words.length <= 500
1 <= words[i].length <= 10
words[i] consists of lowercase English letters.
k is in the range [1, The number of unique words[i]]
 

Follow-up: Could you solve it in O(n log(k)) time and O(n) extra space?

# Code
```cpp []
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {

        map<string, int> freq;

        for (auto i : words) {
            freq[i]++;
        }

        vector<string> ans;

        vector<pair<string, int>> pp(freq.begin(), freq.end());

        sort(pp.begin(), pp.end(),
             [](pair<string, int>& a, pair<string, int>& b) {
                if(a.second == b.second){
                    return a.first < b.first;
                }
                 return a.second > b.second;
             });

        int count = 0;

        for (auto it : pp) {
            if (count == k)
                break;
            // cout << it.first << " : " << it.second << endl;
            ans.push_back(it.first);
            count++;
        }

        return ans;
    }
};
```

-------


# 2402. Meeting Rooms III -> [Leetcode](https://leetcode.com/problems/meeting-rooms-iii/description/)

Hard

You are given an integer n. There are n rooms numbered from 0 to n - 1.

You are given a 2D integer array meetings where meetings[i] = [starti, endi] means that a meeting will be held during the half-closed time interval [starti, endi). All the values of starti are unique.

Meetings are allocated to rooms in the following manner:

Each meeting will take place in the unused room with the lowest number.
If there are no available rooms, the meeting will be delayed until a room becomes free. The delayed meeting should have the same duration as the original meeting.
When a room becomes unused, meetings that have an earlier original start time should be given the room.
Return the number of the room that held the most meetings. If there are multiple rooms, return the room with the lowest number.

A half-closed interval [a, b) is the interval between a and b including a and not including b.

 

Example 1:

Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
Output: 0
Explanation:
- At time 0, both rooms are not being used. The first meeting starts in room 0.
- At time 1, only room 1 is not being used. The second meeting starts in room 1.
- At time 2, both rooms are being used. The third meeting is delayed.
- At time 3, both rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 1 finishes. The third meeting starts in room 1 for the time period [5,10).
- At time 10, the meetings in both rooms finish. The fourth meeting starts in room 0 for the time period [10,11).
Both rooms 0 and 1 held 2 meetings, so we return 0. 


Example 2:

Input: n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
Output: 1
Explanation:
- At time 1, all three rooms are not being used. The first meeting starts in room 0.
- At time 2, rooms 1 and 2 are not being used. The second meeting starts in room 1.
- At time 3, only room 2 is not being used. The third meeting starts in room 2.
- At time 4, all three rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 2 finishes. The fourth meeting starts in room 2 for the time period [5,10).
- At time 6, all three rooms are being used. The fifth meeting is delayed.
- At time 10, the meetings in rooms 1 and 2 finish. The fifth meeting starts in room 1 for the time period [10,12).
Room 0 held 1 meeting while rooms 1 and 2 each held 2 meetings, so we return 1. 
 

Constraints:

1 <= n <= 100
1 <= meetings.length <= 105
meetings[i].length == 2
0 <= starti < endi <= 5 * 105
All the values of starti are unique.

# Code
```cpp []
struct T {
    long endTime;
    int roomId;
};

class Solution {
public:
    int mostBooked(int n, vector<vector<int>>& meetings) {
        vector<int> count(n);

        ranges::sort(meetings);

        auto compare = [](const T& a, const T& b) {
            return a.endTime == b.endTime ? a.roomId > b.roomId
                                          : a.endTime > b.endTime;
        };
        priority_queue<T, vector<T>, decltype(compare)> occupied(compare);
        priority_queue<int, vector<int>, greater<>> availableRoomIds;

        for (int i = 0; i < n; ++i)
            availableRoomIds.push(i);

        for (const vector<int>& meeting : meetings) {
            const int start = meeting[0];
            const int end = meeting[1];
            // Push meetings ending before this `meeting` in occupied to the
            // `availableRoomsIds`.
            while (!occupied.empty() && occupied.top().endTime <= start)
                availableRoomIds.push(occupied.top().roomId), occupied.pop();
            if (availableRoomIds.empty()) {
                const auto [newStart, roomId] = occupied.top();
                occupied.pop();
                ++count[roomId];
                occupied.push({newStart + (end - start), roomId});
            } else {
                const int roomId = availableRoomIds.top();
                availableRoomIds.pop();
                ++count[roomId];
                occupied.push({end, roomId});
            }
        }

        return ranges::max_element(count) - count.begin();
    }
};
```

------


# 706. Design HashMap -> [Leetcode](https://leetcode.com/problems/design-hashmap/description/)

Easy

Design a HashMap without using any built-in hash table libraries.

Implement the MyHashMap class:

MyHashMap() initializes the object with an empty map.
void put(int key, int value) inserts a (key, value) pair into the HashMap. If the key already exists in the map, update the corresponding value.
int get(int key) returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
void remove(key) removes the key and its corresponding value if the map contains the mapping for the key.
 

Example 1:

Input
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
Output
[null, null, null, 1, -1, null, 1, null, -1]

Explanation
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]
 

Constraints:

0 <= key, value <= 106
At most 104 calls will be made to put, get, and remove.

# Code
```cpp []
class MyHashMap {
    static const int SIZE = 1000001;
    int mp[SIZE];

public:
    MyHashMap() {
        fill(mp,mp + SIZE , -1);
    }

    void put(int key, int value) { mp[key] = value; }

    int get(int key) {
        return (mp[key]);
    }

    void remove(int key) { mp[key] = -1; }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

----



# 144. Binary Tree Preorder Traversal -> [LeetCode](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

Easy

Given the root of a binary tree, return the preorder traversal of its nodes' values.

 

Example 1:

Input: root = [1,null,2,3]

Output: [1,2,3]

Explanation:



Example 2:

Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]

Output: [1,2,4,5,6,7,3,8,9]

Explanation:



Example 3:

Input: root = []

Output: []

Example 4:

Input: root = [1]

Output: [1]

 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100
 

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
    void preOrder(TreeNode* root , vector<int> &ans){
        if(root == nullptr) return;
        ans.push_back(root->val);
        preOrder(root->left , ans);        
        preOrder(root->right , ans);        
    }
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;

        preOrder(root , ans);

        return ans;
    }
};
```

-----


# 958. Check Completeness of a Binary Tree -> [LeetCode](https://leetcode.com/problems/check-completeness-of-a-binary-tree/description/)

Medium

Given the root of a binary tree, determine if it is a complete binary tree.

In a complete binary tree, every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

 

Example 1:


Input: root = [1,2,3,4,5,6]
Output: true
Explanation: Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.


Example 2:


Input: root = [1,2,3,4,5,null,7]
Output: false
Explanation: The node with value 7 isn't as far left as possible.
 

Constraints:

The number of nodes in the tree is in the range [1, 100].
1 <= Node.val <= 1000

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
public:
    bool isCompleteTree(TreeNode* root) {
        if(root == nullptr) return true;

        queue<TreeNode*> q;
        q.push(root);

        while(q.front() != nullptr){
            TreeNode* cur = q.front(); q.pop();
            q.push(cur->left);
            q.push(cur->right);
        }

        while(q.front() == nullptr){
            q.pop();
        }

        return q.empty();

    }
};
```

-----


# 622. Design Circular Queue -> [LeetCode](https://leetcode.com/problems/design-circular-queue/description/)

Medium

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle, and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implement the MyCircularQueue class:

MyCircularQueue(k) Initializes the object with the size of the queue to be k.
int Front() Gets the front item from the queue. If the queue is empty, return -1.
int Rear() Gets the last item from the queue. If the queue is empty, return -1.
boolean enQueue(int value) Inserts an element into the circular queue. Return true if the operation is successful.
boolean deQueue() Deletes an element from the circular queue. Return true if the operation is successful.
boolean isEmpty() Checks whether the circular queue is empty or not.
boolean isFull() Checks whether the circular queue is full or not.
You must solve the problem without using the built-in queue data structure in your programming language. 

 

Example 1:

Input
["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
Output
[null, true, true, true, false, 3, true, true, true, 4]

Explanation
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
 

Constraints:

1 <= k <= 1000
0 <= value <= 1000
At most 3000 calls will be made to enQueue, deQueue, Front, Rear, isEmpty, and isFull.

# Code
```cpp []
class MyCircularQueue {
    int size;
    int *arr;
    int front;
    int rear;
    int count;
public:
    MyCircularQueue(int k) {
        size = k;
        arr = new int[size];
        front = -1;
        rear = -1;
        count = 0;
    }
    
    bool enQueue(int value) {
        if(isFull()) return false;
        if(front == -1) front = 0;
        rear = (rear + 1) % size;
        arr[rear] = value;
        count++;
        return true;
    }
    
    bool deQueue() {
        if(isEmpty()) return false;
        front = (front + 1) % size;
        count--;
        return true;
    }
    
    int Front() {
        return isEmpty() ? -1 : arr[front];
    }
    
    int Rear() {
        return isEmpty() ? -1 : arr[rear];
    }
    
    bool isEmpty() {
        if(count == 0) return true;
        return false;
    }
    
    bool isFull() {
        if(count == size) return true;
        return false;
    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```

------

# 1342. Number of Steps to Reduce a Number to Zero -> [LeetCode](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/description/)
Easy

Given an integer num, return the number of steps to reduce it to zero.

In one step, if the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.

 

Example 1:

Input: num = 14
Output: 6
Explanation: 
Step 1) 14 is even; divide by 2 and obtain 7. 
Step 2) 7 is odd; subtract 1 and obtain 6.
Step 3) 6 is even; divide by 2 and obtain 3. 
Step 4) 3 is odd; subtract 1 and obtain 2. 
Step 5) 2 is even; divide by 2 and obtain 1. 
Step 6) 1 is odd; subtract 1 and obtain 0.


Example 2:

Input: num = 8
Output: 4
Explanation: 
Step 1) 8 is even; divide by 2 and obtain 4. 
Step 2) 4 is even; divide by 2 and obtain 2. 
Step 3) 2 is even; divide by 2 and obtain 1. 
Step 4) 1 is odd; subtract 1 and obtain 0.


Example 3:

Input: num = 123
Output: 12
 

Constraints:

0 <= num <= 106

# Code
```cpp []
class Solution {
public:
    int numberOfSteps(int num) {
        int ans = 0;

        while(num){
            if(num % 2 == 0){
                num /= 2;
                ans++;
                continue;
            }
            num--;
            ans++;
        }

        return ans;
    }
};
```

------

# 1900. The Earliest and Latest Rounds Where Players Compete -> [LeetCode](https://leetcode.com/problems/the-earliest-and-latest-rounds-where-players-compete/description/)

Hard

There is a tournament where n players are participating. The players are standing in a single row and are numbered from 1 to n based on their initial standing position (player 1 is the first player in the row, player 2 is the second player in the row, etc.).

The tournament consists of multiple rounds (starting from round number 1). In each round, the ith player from the front of the row competes against the ith player from the end of the row, and the winner advances to the next round. When the number of players is odd for the current round, the player in the middle automatically advances to the next round.

For example, if the row consists of players 1, 2, 4, 6, 7
Player 1 competes against player 7.
Player 2 competes against player 6.
Player 4 automatically advances to the next round.
After each round is over, the winners are lined back up in the row based on the original ordering assigned to them initially (ascending order).

The players numbered firstPlayer and secondPlayer are the best in the tournament. They can win against any other player before they compete against each other. If any two other players compete against each other, either of them might win, and thus you may choose the outcome of this round.

Given the integers n, firstPlayer, and secondPlayer, return an integer array containing two values, the earliest possible round number and the latest possible round number in which these two players will compete against each other, respectively.

 

Example 1:

Input: n = 11, firstPlayer = 2, secondPlayer = 4
Output: [3,4]
Explanation:
One possible scenario which leads to the earliest round number:
First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Second round: 2, 3, 4, 5, 6, 11
Third round: 2, 3, 4
One possible scenario which leads to the latest round number:
First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Second round: 1, 2, 3, 4, 5, 6
Third round: 1, 2, 4
Fourth round: 2, 4

Example 2:

Input: n = 5, firstPlayer = 1, secondPlayer = 5
Output: [1,1]
Explanation: The players numbered 1 and 5 compete in the first round.
There is no way to make them compete in any other round.
 

Constraints:

2 <= n <= 28
1 <= firstPlayer < secondPlayer <= n

# Code
```cpp []
class Solution {
 public:
  vector<int> earliestAndLatest(int n, int firstPlayer, int secondPlayer) {
    using P = pair<int, int>;
    vector<vector<vector<P>>> mem(n + 1,
                                  vector<vector<P>>(n + 1, vector<P>(n + 1)));
    const auto [a, b] = solve(firstPlayer, n - secondPlayer + 1, n, mem);
    return {a, b};
  }

 private:
  // Returns the (earliest, latest) pair, the first player is the l-th player
  // from the front, the second player is the r-th player from the end, and
  // there're k people.
  pair<int, int> solve(int l, int r, int k,
                       vector<vector<vector<pair<int, int>>>>& mem) {
    if (l == r)
      return {1, 1};
    if (l > r)
      swap(l, r);
    if (mem[l][r][k] != pair<int, int>{0, 0})
      return mem[l][r][k];

    int a = INT_MAX;
    int b = INT_MIN;

    // Enumerate all the possible positions.
    for (int i = 1; i <= l; ++i)
      for (int j = l - i + 1; j <= r - i; ++j) {
        if (i + j > (k + 1) / 2 || i + j < l + r - k / 2)
          continue;
        const auto [x, y] = solve(i, j, (k + 1) / 2, mem);
        a = min(a, x + 1);
        b = max(b, y + 1);
      }

    return mem[l][r][k] = {a, b};
  }
};
```

-----


# 102. Binary Tree Level Order Traversal -> [LeetCode](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

Medium

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]


Example 2:

Input: root = [1]
Output: [[1]]


Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 2000].
-1000 <= Node.val <= 1000


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
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (root == nullptr)
            return {};

        vector<vector<int>> ans;
        queue<TreeNode*> q;

        q.push(root);

        while (!q.empty()) {

            vector<int> curLevel;
            int size =  q.size();

            for(int i=0 ; i<size ; ++i){

                TreeNode* cur = q.front(); q.pop();
                curLevel.push_back(cur->val);

                if (cur->left)
                    q.push(cur->left);
                if (cur->right)
                    q.push(cur->right);
            }

            ans.push_back(curLevel);
        }

        return ans;
    }
};
```

-----


# 235. Lowest Common Ancestor of a Binary Search Tree -> [leetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
Medium

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

 

Example 1:

Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.

Example 2:


Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

Example 3:

Input: root = [2,1], p = 2, q = 1
Output: 2
 

Constraints:

The number of nodes in the tree is in the range [2, 105].
-109 <= Node.val <= 109
All Node.val are unique.
p != q
p and q will exist in the BST.

# Code
```cpp []
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return nullptr;
        if(p->val < root->val && q->val < root->val) return lowestCommonAncestor(root->left , p , q);
        if(p->val > root->val && q->val > root->val) return lowestCommonAncestor(root->right , p , q);
        return root;
    }
};
```

-------

# 349. Intersection of Two Arrays -> [LeetCode](https://leetcode.com/problems/intersection-of-two-arrays/description/)
Easy

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

 

Example 1:

Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]

Example 2:

Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
 

Constraints:

1 <= nums1.length, nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 1000

# Code
```cpp []
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {

        unordered_set<int> s(nums1.begin() , nums1.end());

        vector<int> ans;

        for(const int i:nums2){
            if(s.count(i)){
                ans.push_back(i);
                s.erase(i);
            }
        }

        return ans;
    }
};
```

-----


# 350. Intersection of Two Arrays II -> [LeetCode](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/)

Easy

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

 

Example 1:

Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]

Example 2:

Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
Explanation: [9,4] is also accepted.
 

Constraints:

1 <= nums1.length, nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 1000
 

Follow up:

What if the given array is already sorted? How would you optimize your algorithm?
What if nums1's size is small compared to nums2's size? Which algorithm is better?
What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

# Code
```cpp []
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        
        vector<int> ans;

        unordered_map<int,int> freq;

        for(const int i:nums1) freq[i]++;

        for(const int i: nums2){
            if(freq[i]){
                ans.push_back(i);
                freq[i]--;
            }
        }

        return ans;
    }
};
```

------

# 704. Binary Search -> [LeetCode](https://leetcode.com/problems/binary-search/description/)
Easy

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
 

Constraints:

1 <= nums.length <= 104
-104 < nums[i], target < 104
All the integers in nums are unique.
nums is sorted in ascending order.

# Code
```cpp []
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0 , r = nums.size() - 1;
        while(l<=r){
            int mid = (l + r) / 2;
            if(nums[mid] == target) return mid;
            if(nums[mid] < target) l = mid + 1;
            else r = mid - 1;
        }

        return -1;
    }
};
```

-----


# 1346. Check If N and Its Double Exist -> [LeetCode](https://leetcode.com/problems/check-if-n-and-its-double-exist/description/)

Easy

Given an array arr of integers, check if there exist two indices i and j such that :

i != j
0 <= i, j < arr.length
arr[i] == 2 * arr[j]
 

Example 1:

Input: arr = [10,2,5,3]
Output: true
Explanation: For i = 0 and j = 2, arr[i] == 10 == 2 * 5 == 2 * arr[j]

Example 2:

Input: arr = [3,1,7,11]
Output: false
Explanation: There is no i and j that satisfy the conditions.
 

Constraints:

2 <= arr.length <= 500
-103 <= arr[i] <= 103

# Code
```cpp []
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        sort(arr.begin() , arr.end());

        for(int i=0 ; i<arr.size() - 1 ; ++i){
            int t = arr[i] * 2;
            int l = 0 , r = arr.size() - 1;
            while(l<=r){
                int m = (l+r) / 2;
                if(arr[m] == t && m != i) return true;
                if(arr[m] > t) r = m - 1;
                else l = m + 1;
            }
        }

        return false;
    }
};
```

------


# 2410. Maximum Matching of Players With Trainers -> [LeetCode](https://leetcode.com/problems/maximum-matching-of-players-with-trainers/description/)

Medium

You are given a 0-indexed integer array players, where players[i] represents the ability of the ith player. You are also given a 0-indexed integer array trainers, where trainers[j] represents the training capacity of the jth trainer.

The ith player can match with the jth trainer if the player's ability is less than or equal to the trainer's training capacity. Additionally, the ith player can be matched with at most one trainer, and the jth trainer can be matched with at most one player.

Return the maximum number of matchings between players and trainers that satisfy these conditions.

 

Example 1:

Input: players = [4,7,9], trainers = [8,2,5,8]
Output: 2
Explanation:
One of the ways we can form two matchings is as follows:
- players[0] can be matched with trainers[0] since 4 <= 8.
- players[1] can be matched with trainers[3] since 7 <= 8.
It can be proven that 2 is the maximum number of matchings that can be formed.

Example 2:

Input: players = [1,1,1], trainers = [10]
Output: 1
Explanation:
The trainer can be matched with any of the 3 players.
Each player can only be matched with one trainer, so the maximum answer is 1.
 

Constraints:

1 <= players.length, trainers.length <= 105
1 <= players[i], trainers[j] <= 109
 

Note: This question is the same as 445: Assign Cookies.

# Code
```cpp []
class Solution {
public:
    int matchPlayersAndTrainers(vector<int>& players, vector<int>& trainers) {
        int ans = 0;

        ranges::sort(players);
        ranges::sort(trainers);

        for (int i = 0; i < trainers.size(); ++i)
            if (players[ans] <= trainers[i] && ++ans == players.size())
                return ans;

        return ans;
    }
};
```

-------

# 94. Binary Tree Inorder Traversal -> [LeetCode](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)
Easy

Given the root of a binary tree, return the inorder traversal of its nodes' values.

 

Example 1:

Input: root = [1,null,2,3]

Output: [1,3,2]

Explanation:



Example 2:

Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]

Output: [4,2,6,5,7,1,3,9,8]

Explanation:



Example 3:

Input: root = []

Output: []

Example 4:

Input: root = [1]

Output: [1]

 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100
 

Follow up: Recursive solution is trivial, could you do it iteratively?

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
    void inOrder(TreeNode* root, vector<int> &ans){
        if(root == nullptr) return;
        inOrder(root->left , ans);
        ans.push_back(root->val);
        inOrder(root->right , ans);

    }
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        inOrder(root,ans);
        return ans;
    }
};
```

-----


# 145. Binary Tree Postorder Traversal -> [LeetCode](https://leetcode.com/problems/binary-tree-postorder-traversal/description)
Easy

Given the root of a binary tree, return the postorder traversal of its nodes' values.

 

Example 1:

Input: root = [1,null,2,3]

Output: [3,2,1]

Explanation:



Example 2:

Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]

Output: [4,6,7,5,2,9,8,3,1]

Explanation:



Example 3:

Input: root = []

Output: []

Example 4:

Input: root = [1]

Output: [1]

 

Constraints:

The number of the nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100
 

Follow up: Recursive solution is trivial, could you do it iteratively?

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
    void postOrder(TreeNode *root, vector<int> &ans){
        if(root == nullptr) return;
        postOrder(root->left,ans);
        postOrder(root->right,ans);
        ans.push_back(root->val);
    }
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        postOrder(root,ans);
        return ans;
    }
};
```

------


# 1290. Convert Binary Number in a Linked List to Integer -> [LeetCode](https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer/description/)
Easy

Given head which is a reference node to a singly-linked list. The value of each node in the linked list is either 0 or 1. The linked list holds the binary representation of a number.

Return the decimal value of the number in the linked list.

The most significant bit is at the head of the linked list.

 

Example 1:


Input: head = [1,0,1]
Output: 5
Explanation: (101) in base 2 = (5) in base 10

Example 2:

Input: head = [0]
Output: 0
 

Constraints:

The Linked List is not empty.
Number of nodes will not exceed 30.
Each node's value is either 0 or 1.
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
    int getDecimalValue(ListNode* head) {
        int ans = 0;

        for( ; head ; head = head->next) ans = ans * 2 + head->val;

        return ans;
    }
};
```

-------------


# 75. Sort Colors -> [LeetCode](https://leetcode.com/problems/sort-colors/)
Medium

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

 

Example 1:

Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

Example 2:

Input: nums = [2,0,1]
Output: [0,1,2]
 

Constraints:

n == nums.length
1 <= n <= 300
nums[i] is either 0, 1, or 2.
 

Follow up: Could you come up with a one-pass algorithm using only constant extra space?

# Code
```cpp []
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int l = 0 , m = 0 , h = nums.size() - 1;
        while(m<=h){
            if(nums[m] == 0) swap(nums[m++],nums[l++]);
            else if(nums[m] == 1) m++;
            else swap(nums[m] , nums[h--]);
        }
    }
};
```

------

# 108. Convert Sorted Array to Binary Search Tree -> [LeetCode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

Easy

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

 

Example 1:


Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:

Example 2:


Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
 

Constraints:

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums is sorted in a strictly increasing order.

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
    TreeNode* sortTree(vector<int> &nums , int l ,int h){
        if(l>h) return nullptr;
        int mid = l + (h-l) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = sortTree(nums,l,mid - 1);
        root->right = sortTree(nums,mid + 1 , h);
        return root;
    }

public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size() - 1;
        return sortTree(nums,0,n);
    }
};
```

-----


# 905. Sort Array By Parity -> [LeetCode](https://leetcode.com/problems/sort-array-by-parity/description/)

Easy

Given an integer array nums, move all the even integers at the beginning of the array followed by all the odd integers.

Return any array that satisfies this condition.

 

Example 1:

Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

Example 2:

Input: nums = [0]
Output: [0]
 

Constraints:

1 <= nums.length <= 5000
0 <= nums[i] <= 5000

# Code
```cpp []
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int l =0 , r = nums.size() - 1;
        while(l<r){
            if(nums[l] % 2 == 0) l++;
            else if(nums[r] % 2 == 1) r--;
            else swap(nums[r--],nums[l++]);
        }

        return nums;
    }
};
```

------



# 48. Rotate Image -> [LeetCode](https://leetcode.com/problems/rotate-image/description/)

Medium

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

 

Example 1:


Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]


Example 2:


Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
 

Constraints:

n == matrix.length == matrix[i].length
1 <= n <= 20
-1000 <= matrix[i][j] <= 1000

# Code
```cpp []
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();

        for(int i=0 ; i<n ; ++i){
            for(int j = i + 1 ; j<n ; ++j) swap(matrix[i][j] , matrix[j][i]);
        }

        for(int i=0 ; i<n ; ++i) reverse(matrix[i].begin() , matrix[i].end());
    }
};
```

------


# 200. Number of Islands -> [LeetCode](https://leetcode.com/problems/number-of-islands/description/)

Medium

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1


Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] is '0' or '1'.

# Code
```cpp []
class Solution {
private:
    vector<vector<bool>> visit;
    int r, c;

    int dfs(int i, int j, vector<vector<char>>& grid) {
        if (i < 0 || i >= r || j < 0 || j >= c)
            return 0;
        if (grid[i][j] == '0' || visit[i][j])
            return 0;

        visit[i][j] = true;

        dfs(i + 1, j, grid);
        dfs(i - 1, j, grid);
        dfs(i, j + 1, grid);
        dfs(i, j - 1, grid);

        return 1;
    }

public:
    int numIslands(vector<vector<char>>& grid) {
        r = grid.size();
        c = grid[0].size();

        visit.resize(r, vector<bool>(c, false));

        int ans = 0;

        for (int i = 0; i < r; ++i) {
            for (int j = 0; j < c; ++j) {
                if (grid[i][j] == '1' && !visit[i][j]) {
                    ans += dfs(i, j, grid);
                }
            }
        }

        return ans;
    }
};
```

------



# 224. Basic Calculator -> [LeetCode](https://leetcode.com/problems/basic-calculator/)
Hard

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().

 

Example 1:

Input: s = "1 + 1"
Output: 2

Example 2:

Input: s = " 2-1 + 2 "
Output: 3

Example 3:

Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
 

Constraints:

1 <= s.length <= 3 * 105
s consists of digits, '+', '-', '(', ')', and ' '.
s represents a valid expression.
'+' is not used as a unary operation (i.e., "+1" and "+(2 + 3)" is invalid).
'-' could be used as a unary operation (i.e., "-1" and "-(2 + 3)" is valid).
There will be no two consecutive operators in the input.
Every number and running calculation will fit in a signed 32-bit integer.

# Code
```cpp []
#include<bits/stdc++.h>
class Solution {
private:
    int evaluate(string s){
        stack<int> st;
        int num = 0;
        int sign = 1;
        int res = 0;

        for(char c:s){
            if(isdigit(c)) num = num * 10 + (c - '0');
            else if(c == '+'){
                res += (sign * num);
                sign = 1;
                num = 0;
            }else if(c == '-'){
                res += (sign * num);
                sign = -1;
                num = 0;
            }else if(c == '('){
                st.push(res);
                st.push(sign);
                res = 0;
                num = 0;
                sign = 1;
            }else if(c == ')'){
                res += (sign * num);
                int prevS = st.top(); st.pop();
                int prevV = st.top(); st.pop();
                res = prevV + (prevS * res);
                num = 0;
            }
        }

        res += (sign * num);

        return res;
    }
public:
    int calculate(string s) {
        stack<char> st;
        
        return evaluate(s);
    }
};
```

----------


# 58. Length of Last Word -> [LeetCode](https://leetcode.com/problems/length-of-last-word/description/)
Easy

Given a string s consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

 

Example 1:

Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.

Example 2:

Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.

Example 3:

Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
 

Constraints:

1 <= s.length <= 104
s consists of only English letters and spaces ' '.
There will be at least one word in s.

# Code
```cpp []
class Solution {
public:
    int lengthOfLastWord(string s) {
        int i = s.length() - 1;
        while(i>=0 && s[i] == ' ') i--;
        int count = 0;
        while(i>=0 && s[i--] != ' ') count++;
        return count;
    }
};
```

-------


# 3136. Valid Word -> [LeetCode](https://leetcode.com/problems/valid-word/description/)

Easy

A word is considered valid if:

It contains a minimum of 3 characters.
It contains only digits (0-9), and English letters (uppercase and lowercase).
It includes at least one vowel.
It includes at least one consonant.
You are given a string word.

Return true if word is valid, otherwise, return false.

Notes:

'a', 'e', 'i', 'o', 'u', and their uppercases are vowels.
A consonant is an English letter that is not a vowel.
 

Example 1:

Input: word = "234Adas"

Output: true

Explanation:

This word satisfies the conditions.

Example 2:

Input: word = "b3"

Output: false

Explanation:

The length of this word is fewer than 3, and does not have a vowel.

Example 3:

Input: word = "a3$e"

Output: false

Explanation:

This word contains a '$' character and does not have a consonant.

 

Constraints:

1 <= word.length <= 20
word consists of English uppercase and lowercase letters, digits, '@', '#', and '$'.

# Code
```cpp []
class Solution {
 public:
  bool isValid(string word) {
    return word.length() >= 3 &&
           ranges::all_of(word, [](char c) { return isalnum(c); }) &&
           ranges::any_of(word, isVowel) && ranges::any_of(word, isConsonant);
  }

 private:
  static bool isVowel(char c) {
    static constexpr string_view kVowels = "aeiouAEIOU";
    return kVowels.find(c) != string_view::npos;
  }

  static bool isConsonant(char c) {
    return isalpha(c) && !isVowel(c);
  }
};
```

-----------



# 1047. Remove All Adjacent Duplicates In String -> [LeetCode](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)
Easy

You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.

We repeatedly make duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.

 

Example 1:

Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".


Example 2:

Input: s = "azxxzy"
Output: "ay"
 

Constraints:

1 <= s.length <= 105
s consists of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    string removeDuplicates(string s) {
        string ans = "";
        for(const char c:s){
            if( !ans.empty() && ans.back() == c) ans.pop_back();
            else ans.push_back(c);
        }

        return ans;
    }
};
```

-----


# 290. Word Pattern -> LeetCodehttps://leetcode.com/problems/word-pattern/description/

Easy

Given a pattern and a string s, find if s follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in s. Specifically:

Each letter in pattern maps to exactly one unique word in s.
Each unique word in s maps to exactly one letter in pattern.
No two letters map to the same word, and no two words map to the same letter.
 

Example 1:

Input: pattern = "abba", s = "dog cat cat dog"

Output: true

Explanation:

The bijection can be established as:

'a' maps to "dog".
'b' maps to "cat".
Example 2:

Input: pattern = "abba", s = "dog cat cat fish"

Output: false

Example 3:

Input: pattern = "aaaa", s = "dog cat cat dog"

Output: false

 

Constraints:

1 <= pattern.length <= 300
pattern contains only lower-case English letters.
1 <= s.length <= 3000
s contains only lowercase English letters and spaces ' '.
s does not contain any leading or trailing spaces.
All the words in s are separated by a single space.

# Code
```cpp []
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        unordered_map<char, string> cTw;
        unordered_map<string, char> wTc;

        int n = pattern.length();

        vector<string> str;

        string word;
        stringstream ss(s);
        int sC = 0;
        while (ss >> word) {
            sC++;
            str.push_back(word);
        }

        if(n != sC) return false;

        for (int i = 0; i < n; ++i) {
            char c = pattern[i];
            string w = str[i];

            if (cTw.count(c)) {
                if (cTw[c] != w)
                    return false;
            } else {
                cTw[c] = w;
            }

            if (wTc.count(w)) {
                if (wTc[w] != c)
                    return false;
            } else {
                wTc[w] = c;
            }
        }

        return true;
    }
};
```

-----


# 82. Remove Duplicates from Sorted List II -> [LeetCode](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)

Medium

Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

 

Example 1:


Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]


Example 2:


Input: head = [1,1,1,2,3]
Output: [2,3]
 

Constraints:

The number of nodes in the list is in the range [0, 300].
-100 <= Node.val <= 100
The list is guaranteed to be sorted in ascending order.

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
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return nullptr;

        unordered_map<int, int> freq;
        ListNode* temp = head;

        while (temp != nullptr) {
            freq[temp->val]++;
            temp = temp->next;
        }

        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        ListNode* prev = dummy;
        ListNode* cur = head;

        while (cur != nullptr) {
            if (freq[cur->val] > 1) {
                prev->next = cur->next;
                delete cur;
                cur = prev->next;
            } else {
                prev = cur;
                cur = cur->next;
            }
        }

        ListNode* newHead = dummy->next;
        delete dummy;
        return newHead;
    }
};

```

-----


# 3201. Find the Maximum Length of Valid Subsequence I -> [Leetcode](https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-i/description/)

Medium

You are given an integer array nums.
A subsequence sub of nums with length x is called valid if it satisfies:

(sub[0] + sub[1]) % 2 == (sub[1] + sub[2]) % 2 == ... == (sub[x - 2] + sub[x - 1]) % 2.
Return the length of the longest valid subsequence of nums.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

 

Example 1:

Input: nums = [1,2,3,4]

Output: 4

Explanation:

The longest valid subsequence is [1, 2, 3, 4].

Example 2:

Input: nums = [1,2,1,1,2,1,2]

Output: 6

Explanation:

The longest valid subsequence is [1, 2, 1, 2, 1, 2].

Example 3:

Input: nums = [1,3]

Output: 2

Explanation:

The longest valid subsequence is [1, 3].

 

Constraints:

2 <= nums.length <= 2 * 105
1 <= nums[i] <= 107

# Code
```cpp []
class Solution {
 public:
  int maximumLength(vector<int>& nums) {
    // dp[i][j] := the maximum length of a valid subsequence, where the last
    // number mod 2 equal to i and the next desired number mod 2 equal to j
    vector<vector<int>> dp(2, vector<int>(2));

    // Extend the pattern xyxyxy...xy.
    for (const int x : nums)
      for (int y = 0; y < 2; ++y)
        dp[x % 2][y] = dp[y][x % 2] + 1;

    return accumulate(dp.begin(), dp.end(), 0,
                      [](int acc, const vector<int>& row) {
      return max(acc, ranges::max(row));
    });
  }
};
```

--------------



# 707. Design Linked List -> [LeetCode](https://leetcode.com/problems/design-linked-list/description/)

Medium

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: val and next. val is the value of the current node, and next is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the MyLinkedList class:

MyLinkedList() Initializes the MyLinkedList object.
int get(int index) Get the value of the indexth node in the linked list. If the index is invalid, return -1.
void addAtHead(int val) Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
void addAtTail(int val) Append a node of value val as the last element of the linked list.
void addAtIndex(int index, int val) Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.
void deleteAtIndex(int index) Delete the indexth node in the linked list, if the index is valid.
 

Example 1:

Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
 

Constraints:

0 <= index, val <= 1000
Please do not use the built-in LinkedList library.
At most 2000 calls will be made to get, addAtHead, addAtTail, addAtIndex and deleteAtIndex.

# Code
```cpp []
class MyLinkedList {
public:
    struct Node {
        int val;
        Node *next;
        Node *prev;
        Node(int v) : val(v), next(nullptr), prev(nullptr) {}
    };

    Node* head;
    Node* tail;
    int size;

public:
    MyLinkedList() {
        head = nullptr;
        tail = nullptr;
        size = 0;
    }

    Node* getNode(int index) {
        if (index < 0 || index >= size) {
            return nullptr;
        }

        Node* temp = head;
        for (int i = 0; i < index; ++i) {
            temp = temp->next;
        }
        return temp;
    }

    int get(int index) {
        Node* node = getNode(index);
        if (node) {
            return node->val;
        }
        return -1;
    }

    void addAtHead(int val) {
        Node* newN = new Node(val);

        if (!head) {
            head = newN;
            tail = newN;
        } else {
            newN->next = head;
            head->prev = newN;
            head = newN;
        }
        size++;
    }

    void addAtTail(int val) {
        Node* newN = new Node(val);

        if (!tail) {
            head = newN;
            tail = newN;
        } else {
            tail->next = newN;
            newN->prev = tail;
            tail = newN;
        }
        size++;
    }

    void addAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }

        if (index == 0) {
            addAtHead(val);
            return;
        }

        if (index == size) {
            addAtTail(val);
            return;
        }

        Node* newN = new Node(val);
        Node* cur = getNode(index);

        if (!cur) return; 

        newN->next = cur;
        newN->prev = cur->prev;
        cur->prev->next = newN;
        cur->prev = newN;

        size++;
    }

    void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }

        Node* nodeToDelete = getNode(index);

        if (!nodeToDelete) return;

        if (nodeToDelete == head) {
            head = nodeToDelete->next;
            if (head) {
                head->prev = nullptr;
            } else {
                tail = nullptr;
            }
        } else if (nodeToDelete == tail) {
            tail = nodeToDelete->prev;
            if (tail) {
                tail->next = nullptr;
            } else {
                head = nullptr;
            }
        } else {
            nodeToDelete->prev->next = nodeToDelete->next;
            nodeToDelete->next->prev = nodeToDelete->prev;
        }

        delete nodeToDelete;
        size--;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

-----


# 19. Remove Nth Node From End of List -> [LeetCode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

Medium

Given the head of a linked list, remove the nth node from the end of the list and return its head.

 

Example 1:


Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Example 2:

Input: head = [1], n = 1
Output: []

Example 3:

Input: head = [1,2], n = 1
Output: [1]
 

Constraints:

The number of nodes in the list is sz.
1 <= sz <= 30
0 <= Node.val <= 100
1 <= n <= sz

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int size = 0;
        ListNode* temp = head;
        while(temp){
            size++;
            temp = temp->next;
        }

        if(size == n){
            ListNode* delN = head;
            head = head->next;
            delete delN;
            return head;
        }

        temp = head;

        for(int i=1 ; i<size-n ; ++i){
            temp = temp->next;
        }
        ListNode* del = temp->next;
        temp->next = del->next;
        delete del;

        return head;

    }
};
```

-----

# 141. Linked List Cycle -> [LeetCode](https://leetcode.com/problems/linked-list-cycle/description)

Easy

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

 

Example 1:


Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).


Example 2:


Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.


Example 3:


Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
 

Constraints:

The number of the nodes in the list is in the range [0, 104].
-105 <= Node.val <= 105
pos is -1 or a valid index in the linked-list.
 

Follow up: Can you solve it using O(1) (i.e. constant) memory?

# Code
```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) return true;
        }

        return false;
    }
};
```

-------



# 34. Find First and Last Position of Element in Sorted Array -> [LeetCode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

Medium

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]


Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]


Example 3:

Input: nums = [], target = 0
Output: [-1,-1]
 

Constraints:

0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums is a non-decreasing array.
-109 <= target <= 109

# Code
```cpp []
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int first = -1;
        int last = -1;

        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == target) {
                if (first == -1)
                    first = i;
                last = i;
            }
        }

        return {first, last};
    }
};
```

----

# 1995. Count Special Quadruplets -> [Leetcode](https://leetcode.com/problems/count-special-quadruplets/description/)

Easy

Given a 0-indexed integer array nums, return the number of distinct quadruplets (a, b, c, d) such that:

nums[a] + nums[b] + nums[c] == nums[d], and
a < b < c < d
 

Example 1:

Input: nums = [1,2,3,6]
Output: 1
Explanation: The only quadruplet that satisfies the requirement is (0, 1, 2, 3) because 1 + 2 + 3 == 6.


Example 2:

Input: nums = [3,3,6,4,5]
Output: 0
Explanation: There are no such quadruplets in [3,3,6,4,5].


Example 3:

Input: nums = [1,1,1,3,5]
Output: 4
Explanation: The 4 quadruplets that satisfy the requirement are:
- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5
 

Constraints:

4 <= nums.length <= 50
1 <= nums[i] <= 100

# Code
```cpp []
class Solution {
 public:
  int countQuadruplets(vector<int>& nums) {
    const int n = nums.size();
    int ans = 0;
    unordered_map<int,int> mp;

    for(int a = n-1 ; a>=0 ; --a){
        for(int b=a-1 ; b>=0 ; --b){
            if(mp.count(nums[a] + nums[b])){
                ans += mp[nums[a] + nums[b]];
            }
        }
        for(int d=n-1 ; d>a ; --d)  mp[nums[d] - nums[a]]++;
    }

    return ans;
  }
};
```

------


# 45. Jump Game II -> [LeetCode](https://leetcode.com/problems/jump-game-ii/description/)

Medium

You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

 

Example 1:

Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.


Example 2:

Input: nums = [2,3,0,1,4]
Output: 2
 

Constraints:

1 <= nums.length <= 104
0 <= nums[i] <= 1000
It's guaranteed that you can reach nums[n - 1].

# Code
```cpp []
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();

        int curEnd = 0 , far = 0 , jumb = 0;

        for(int i=0 ; i<n-1 ; ++i){
            far = max(far , i + nums[i]);
            if(i == curEnd){
                jumb++;
                curEnd = far;
            }
        }

        return jumb;
    }
};
```

------------



# 2163. Minimum Difference in Sums After Removal of Elements -> [Leetcode](https://leetcode.com/problems/minimum-difference-in-sums-after-removal-of-elements/)

Hard

You are given a 0-indexed integer array nums consisting of 3 * n elements.

You are allowed to remove any subsequence of elements of size exactly n from nums. The remaining 2 * n elements will be divided into two equal parts:

The first n elements belonging to the first part and their sum is sumfirst.
The next n elements belonging to the second part and their sum is sumsecond.
The difference in sums of the two parts is denoted as sumfirst - sumsecond.

For example, if sumfirst = 3 and sumsecond = 2, their difference is 1.
Similarly, if sumfirst = 2 and sumsecond = 3, their difference is -1.
Return the minimum difference possible between the sums of the two parts after the removal of n elements.

 

Example 1:

Input: nums = [3,1,2]
Output: -1
Explanation: Here, nums has 3 elements, so n = 1. 
Thus we have to remove 1 element from nums and divide the array into two equal parts.
- If we remove nums[0] = 3, the array will be [1,2]. The difference in sums of the two parts will be 1 - 2 = -1.
- If we remove nums[1] = 1, the array will be [3,2]. The difference in sums of the two parts will be 3 - 2 = 1.
- If we remove nums[2] = 2, the array will be [3,1]. The difference in sums of the two parts will be 3 - 1 = 2.
The minimum difference between sums of the two parts is min(-1,1,2) = -1. 


Example 2:

Input: nums = [7,9,5,8,1,3]
Output: 1
Explanation: Here n = 2. So we must remove 2 elements and divide the remaining array into two parts containing two elements each.
If we remove nums[2] = 5 and nums[3] = 8, the resultant array will be [7,9,1,3]. The difference in sums will be (7+9) - (1+3) = 12.
To obtain the minimum difference, we should remove nums[1] = 9 and nums[4] = 1. The resultant array becomes [7,5,8,3]. The difference in sums of the two parts is (7+5) - (8+3) = 1.
It can be shown that it is not possible to obtain a difference smaller than 1.
 

Constraints:

nums.length == 3 * n
1 <= n <= 105
1 <= nums[i] <= 105

# Code
```cpp []
class Solution {
 public:
  long long minimumDifference(vector<int>& nums) {
    const int n = nums.size() / 3;
    long ans = LONG_MAX;
    long leftSum = 0;
    long rightSum = 0;
    // The left part should be as small as possible.
    priority_queue<int> maxHeap;
    // The right part should be as big as possible.
    priority_queue<int, vector<int>, greater<>> minHeap;
    // minLeftSum[i] := the minimum of the sum of n nums in nums[0..i)
    vector<long> minLeftSum(nums.size());

    for (int i = 0; i < 2 * n; ++i) {
      maxHeap.push(nums[i]);
      leftSum += nums[i];
      if (maxHeap.size() == n + 1)
        leftSum -= maxHeap.top(), maxHeap.pop();
      if (maxHeap.size() == n)
        minLeftSum[i] = leftSum;
    }

    for (int i = nums.size() - 1; i >= n; --i) {
      minHeap.push(nums[i]);
      rightSum += nums[i];
      if (minHeap.size() == n + 1)
        rightSum -= minHeap.top(), minHeap.pop();
      if (minHeap.size() == n)
        ans = min(ans, minLeftSum[i - 1] - rightSum);
    }

    return ans;
  }
};
```

------


# 72. Edit Distance -> [LeetCode](https://leetcode.com/problems/edit-distance/)

Medium

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character
 

Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')


Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
 

Constraints:

0 <= word1.length, word2.length <= 500
word1 and word2 consist of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.length() , m = word2.length();
        vector<vector<int>> dp(n+1 , vector<int>(m+1));

        for(int i=0 ; i<=n ; ++i) dp[i][0] = i;
        for(int i=0 ; i<=m ; ++i) dp[0][i] = i;

        for(int i=1 ; i<=n ; ++i){
            for(int j=1 ; j<=m ; ++j){
                if(word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1];
                else{
                    dp[i][j] = 1 + min({
                        dp[i-1][j] , dp[i-1][j-1] , dp[i][j-1]
                    });
                }
            }
        }

        return dp[n][m];

    }
};
```

-----


# 100. Same Tree -> [LeetCode](https://leetcode.com/problems/same-tree/description/)

Easy

Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

 

Example 1:


Input: p = [1,2,3], q = [1,2,3]
Output: true


Example 2:


Input: p = [1,2], q = [1,null,2]
Output: false


Example 3:


Input: p = [1,2,1], q = [1,1,2]
Output: false
 

Constraints:

The number of nodes in both trees is in the range [0, 100].
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
    void preOrder(TreeNode* p,TreeNode* q,bool &ans){
        if(p == nullptr && q==nullptr) return;
        if(p == nullptr || q == nullptr){
            ans = false;
            return;
        }
        if(p->val != q->val){
            ans = false;
            return;
        }
        preOrder(p->left,q->left,ans);
        preOrder(p->right,q->right,ans);

    }
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        bool ans = true;
        preOrder(p,q,ans);
        return ans;
    }
};
```

------



# 101. Symmetric Tree -> [LeetCode](https://leetcode.com/problems/symmetric-tree/description)

Easy

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

 

Example 1:


Input: root = [1,2,2,3,4,4,3]
Output: true


Example 2:


Input: root = [1,2,2,null,3,null,3]
Output: false
 

Constraints:

The number of nodes in the tree is in the range [1, 1000].
-100 <= Node.val <= 100
 

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
    bool isIdentitical(TreeNode* a , TreeNode* b){
        if(!a && !b) return true;
        if(!a || !b) return false;
        return a->val == b->val && isIdentitical(a->left,b->right) && isIdentitical(a->right , b->left);
    }
public:
    bool isSymmetric(TreeNode* root) {
        return isIdentitical(root->left , root->right);
    }
};
```

-----


# 104. Maximum Depth of Binary Tree -> [Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

Easy

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: 3

Example 2:

Input: root = [1,null,2]
Output: 2
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
-100 <= Node.val <= 100

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
    int findDepth(TreeNode* root){
        if(!root) return 0;
        return 1 + max( findDepth(root->left) , findDepth(root->right));
    }
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        return findDepth(root);
    }
};
```

-----



# 103. Binary Tree Zigzag Level Order Traversal -> [LeetCode](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

Medium

Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]


Example 2:

Input: root = [1]
Output: [[1]]


Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 2000].
-100 <= Node.val <= 100
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
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(!root) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        bool leftToRight = true;

        while(!q.empty()){
            int n = q.size();
            vector<int> level(n);
            for(int i=0 ; i<n ; ++i){
                TreeNode* cur = q.front(); q.pop();
                int ind = leftToRight ? i : n - i - 1;
                level[ind] = cur->val;
                if(cur->left) q.push(cur->left);
                if(cur->right) q.push(cur->right);
            }
            ans.push_back(level);
            leftToRight = !leftToRight;
        }

        return ans;
    }
};
```

------



# 2. Add Two Numbers -> [LeetCode](https://leetcode.com/problems/add-two-numbers/)

Medium

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

Example 1:


Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.


Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]
Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
 

Constraints:

The number of nodes in each linked list is in the range [1, 100].
0 <= Node.val <= 9
It is guaranteed that the list represents a number that does not have leading zeros.

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* head = dummy;
        int sum = 0 , carry = 0;
        while(l1 || l2 || carry){
            sum = carry;
            if(l1){
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2){
                sum += l2->val;
                l2 = l2->next;
            }
            carry = sum / 10;
            dummy->next = new ListNode(sum%10);
            dummy = dummy->next;
        }

        return head->next;
    }
};
```

------------


# 222. Count Complete Tree Nodes -> [LeetCode](https://leetcode.com/problems/count-complete-tree-nodes/description/)

Easy

Given the root of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.

 

Example 1:


Input: root = [1,2,3,4,5,6]
Output: 6


Example 2:

Input: root = []
Output: 0


Example 3:

Input: root = [1]
Output: 1
 

Constraints:

The number of nodes in the tree is in the range [0, 5 * 104].
0 <= Node.val <= 5 * 104
The tree is guaranteed to be complete.

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
public:
    int countNodes(TreeNode* root) {
        if(!root) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

------


# 112. Path Sum -> [LeetCode](https://leetcode.com/problems/path-sum/description/)

Easy

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

A leaf is a node with no children.

 

Example 1:


Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.

Example 2:


Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There are two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.


Example 3:

Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.
 

Constraints:

The number of nodes in the tree is in the range [0, 5000].
-1000 <= Node.val <= 1000
-1000 <= targetSum <= 1000

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
    void check(TreeNode* root, int target, int sum , bool& ans){
        if(!root) return;
        sum += root->val;
        if(sum == target && !root->left && !root->right){
            ans = true;
        }
        check(root->left , target , sum , ans);
        check(root->right , target ,sum , ans);
    }

public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        bool ans = false;
        check( root , targetSum , 0 , ans);
        return ans;
    }
};
```

--------


# 111. Minimum Depth of Binary Tree -> [LeetCode](https://leetcode.com/problems/minimum-depth-of-binary-tree/description)

Easy

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: 2


Example 2:

Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
 

Constraints:

The number of nodes in the tree is in the range [0, 105].
-1000 <= Node.val <= 1000

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

public:
    int minDepth(TreeNode* root) {
        if(!root) return 0;
        int L = minDepth(root->left) , R =  minDepth(root->right);
        return 1 + (min(L,R) ? min(L,R) : max(L,R));
    }
};
```

-----------



# 226. Invert Binary Tree -> [Leetcode](https://leetcode.com/problems/invert-binary-tree/description)

Easy

Given the root of a binary tree, invert the tree, and return its root.

 

Example 1:


Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]


Example 2:


Input: root = [2,1,3]
Output: [2,3,1]


Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100

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
public:
    TreeNode* invertTree(TreeNode* root) {
       if(!root) return nullptr;

       invertTree(root->left);
       invertTree(root->right);

       TreeNode* temp = root->left;
       root->left = root->right;
       root->right = temp;

       return root;
    }
};
```

----


# 671. Second Minimum Node In a Binary Tree -> [LeetCode](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/description)

Easy

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property root.val = min(root.left.val, root.right.val) always holds.

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

 

 

Example 1:


Input: root = [2,2,5,null,null,5,7]
Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.


Example 2:

Input: root = [2,2,2]
Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
 

Constraints:

The number of nodes in the tree is in the range [1, 25].
1 <= Node.val <= 231 - 1
root.val == min(root.left.val, root.right.val) for each internal node of the tree.

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
    set<int> s;
    void preOrder(TreeNode* root){
        if(!root) return;
        s.insert(root->val);
        preOrder(root->left);
        preOrder(root->right);
    }
public:
    int findSecondMinimumValue(TreeNode* root) {
        if(!root) return -1;
        preOrder(root);
        if(s.size() < 2) return -1;
        return ( *++(s.begin()) );
    }
};
```

---------


# 1233. Remove Sub-Folders from the Filesystem -> [LeetCode](https://leetcode.com/problems/remove-sub-folders-from-the-filesystem/description/)
Medium

Given a list of folders folder, return the folders after removing all sub-folders in those folders. You may return the answer in any order.

If a folder[i] is located within another folder[j], it is called a sub-folder of it. A sub-folder of folder[j] must start with folder[j], followed by a "/". For example, "/a/b" is a sub-folder of "/a", but "/b" is not a sub-folder of "/a/b/c".

The format of a path is one or more concatenated strings of the form: '/' followed by one or more lowercase English letters.

For example, "/leetcode" and "/leetcode/problems" are valid paths while an empty string and "/" are not.
 

Example 1:

Input: folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]
Output: ["/a","/c/d","/c/f"]
Explanation: Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.


Example 2:

Input: folder = ["/a","/a/b/c","/a/b/d"]
Output: ["/a"]
Explanation: Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".


Example 3:

Input: folder = ["/a/b/c","/a/b/ca","/a/b/d"]
Output: ["/a/b/c","/a/b/ca","/a/b/d"]
 

Constraints:

1 <= folder.length <= 4 * 104
2 <= folder[i].length <= 100
folder[i] contains only lowercase letters and '/'.
folder[i] always starts with the character '/'.
Each folder name is unique.

# Code
```cpp []
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        vector<string> ans;
        string prev;
        sort(folder.begin() , folder.end());
        for(const string &f : folder){
            if( !prev.empty() && f.find(prev) == 0 && f[prev.size()] == '/' ) continue;
            ans.push_back(f);
            prev = f;
        }

        return ans;
    }
};
```

---------



# 530. Minimum Absolute Difference in BST -> [LeetCode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

Easy

Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

 

Example 1:

Input: root = [4,2,6,1,3]
Output: 1

Example 2:


Input: root = [1,0,48,null,null,12,49]
Output: 1
 

Constraints:

The number of nodes in the tree is in the range [2, 104].
0 <= Node.val <= 105
 

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
    int diff = INT_MAX;
    TreeNode* prev = nullptr;
    void dfs(TreeNode* root){
        if(root->left) dfs(root->left);
        if(prev) diff = min(diff , abs(prev->val - root->val));
        prev = root;
        if(root->right) dfs(root->right);
    }
public:
    int getMinimumDifference(TreeNode* root) {
        if(!root) return -1;
        // queue<TreeNode*> q;
        // q.push(root);
        // vector<int> nums;
        // while(!q.empty()){
        //     int n = q.size();
        //     for(int i=0 ; i<n  ; ++i){
        //         TreeNode* cur = q.front(); q.pop();
        //         nums.push_back(cur->val);
        //         if(cur->left){ 
        //             q.push(cur->left);
        //         }
        //         if(cur->right){ 
        //             q.push(cur->right);
        //         }
        //     }
        // }
        // ranges::sort(nums);
        // int diff = INT_MAX;
        // for(int i=1 ; i<nums.size() ; ++i){
        //     diff = min(diff , nums[i] - nums[i-1] );
        // }
        dfs(root);
        return diff;
    }
};
```

------



# 21. Merge Two Sorted Lists -> [LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/description/)

Easy

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

 

Example 1:


Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]


Example 2:

Input: list1 = [], list2 = []
Output: []


Example 3:

Input: list1 = [], list2 = [0]
Output: [0]
 

Constraints:

The number of nodes in both lists is in the range [0, 50].
-100 <= Node.val <= 100
Both list1 and list2 are sorted in non-decreasing order.

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* temp = dummy;
        while(l1 && l2){
            if(l1->val <= l2->val){
                temp->next = l1;
                l1 = l1->next;
            }else{
                temp->next = l2;
                l2 = l2->next;
            }
            temp = temp->next;
        }
        while(l1){
            temp->next = l1;
            l1 = l1->next;
            temp = temp->next;
        }
        while(l2){
            temp->next = l2;
            l2 = l2->next;
            temp = temp->next;
        }

        return dummy->next;
    }
};
```


------



# 1948. Delete Duplicate Folders in System -> [LeetCode](https://leetcode.com/problems/delete-duplicate-folders-in-system/description/)

Hard

Due to a bug, there are many duplicate folders in a file system. You are given a 2D array paths, where paths[i] is an array representing an absolute path to the ith folder in the file system.

For example, ["one", "two", "three"] represents the path "/one/two/three".
Two folders (not necessarily on the same level) are identical if they contain the same non-empty set of identical subfolders and underlying subfolder structure. The folders do not need to be at the root level to be identical. If two or more folders are identical, then mark the folders as well as all their subfolders.

For example, folders "/a" and "/b" in the file structure below are identical. They (as well as their subfolders) should all be marked:
/a
/a/x
/a/x/y
/a/z
/b
/b/x
/b/x/y
/b/z
However, if the file structure also included the path "/b/w", then the folders "/a" and "/b" would not be identical. Note that "/a/x" and "/b/x" would still be considered identical even with the added folder.
Once all the identical folders and their subfolders have been marked, the file system will delete all of them. The file system only runs the deletion once, so any folders that become identical after the initial deletion are not deleted.

Return the 2D array ans containing the paths of the remaining folders after deleting all the marked folders. The paths may be returned in any order.

 

Example 1:


Input: paths = [["a"],["c"],["d"],["a","b"],["c","b"],["d","a"]]
Output: [["d"],["d","a"]]
Explanation: The file structure is as shown.
Folders "/a" and "/c" (and their subfolders) are marked for deletion because they both contain an empty
folder named "b".


Example 2:


Input: paths = [["a"],["c"],["a","b"],["c","b"],["a","b","x"],["a","b","x","y"],["w"],["w","y"]]
Output: [["c"],["c","b"],["a"],["a","b"]]
Explanation: The file structure is as shown. 
Folders "/a/b/x" and "/w" (and their subfolders) are marked for deletion because they both contain an empty folder named "y".
Note that folders "/a" and "/c" are identical after the deletion, but they are not deleted because they were not marked beforehand.


Example 3:


Input: paths = [["a","b"],["c","d"],["c"],["a"]]
Output: [["c"],["c","d"],["a"],["a","b"]]
Explanation: All folders are unique in the file system.
Note that the returned array can be in a different order as the order does not matter.
 

Constraints:

1 <= paths.length <= 2 * 104
1 <= paths[i].length <= 500
1 <= paths[i][j].length <= 10
1 <= sum(paths[i][j].length) <= 2 * 105
path[i][j] consists of lowercase English letters.
No two paths lead to the same folder.
For any folder not at the root level, its parent folder will also be in the input.

# Code
```cpp []
struct TrieNode {
    unordered_map<string, shared_ptr<TrieNode>> children;
    bool deleted = false;
};

class Solution {
public:
    vector<vector<string>>
    deleteDuplicateFolder(vector<vector<string>>& paths) {
        vector<vector<string>> ans;
        vector<string> path;
        unordered_map<string, vector<shared_ptr<TrieNode>>> subtreeToNodes;

        ranges::sort(paths);

        for (const vector<string>& path : paths) {
            shared_ptr<TrieNode> node = root;
            for (const string& s : path) {
                if (!node->children.contains(s))
                    node->children[s] = make_shared<TrieNode>();
                node = node->children[s];
            }
        }

        buildSubtreeToRoots(root, subtreeToNodes);

        for (const auto& [_, nodes] : subtreeToNodes)
            if (nodes.size() > 1)
                for (shared_ptr<TrieNode> node : nodes)
                    node->deleted = true;

        constructPath(root, path, ans);
        return ans;
    }

private:
    shared_ptr<TrieNode> root = make_shared<TrieNode>();

    string buildSubtreeToRoots(
        shared_ptr<TrieNode> node,
        unordered_map<string, vector<shared_ptr<TrieNode>>>& subtreeToNodes) {
        string subtree = "(";
        for (const auto& [s, child] : node->children)
            subtree += s + buildSubtreeToRoots(child, subtreeToNodes);
        subtree += ")";
        if (subtree != "()")
            subtreeToNodes[subtree].push_back(node);
        return subtree;
    }

    void constructPath(shared_ptr<TrieNode> node, vector<string>& path,
                       vector<vector<string>>& ans) {
        for (const auto& [s, child] : node->children)
            if (!child->deleted) {
                path.push_back(s);
                constructPath(child, path, ans);
                path.pop_back();
            }
        if (!path.empty())
            ans.push_back(path);
    }
};
```

------


# 110. Balanced Binary Tree -> [LeetCode](https://leetcode.com/problems/balanced-binary-tree/description/)

Easy

Given a binary tree, determine if it is height-balanced.

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: true


Example 2:


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
        int left = checkBalanceTree(root->left);
        int right = checkBalanceTree(root->right);
        if(abs(left - right) > 1) ans = false;
        return 1 + max(left , right);
    }
public:
    bool isBalanced(TreeNode* root) {
        ans = true;
        checkBalanceTree(root);
        return ans;
    }
};

```


------



# 160. Intersection of Two Linked Lists -> [LeetCode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

Easy

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:


The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.

Custom Judge:

The inputs to the judge are given as follows (your program is not given these inputs):

intersectVal - The value of the node where the intersection occurs. This is 0 if there is no intersected node.
listA - The first linked list.
listB - The second linked list.
skipA - The number of nodes to skip ahead in listA (starting from the head) to get to the intersected node.
skipB - The number of nodes to skip ahead in listB (starting from the head) to get to the intersected node.
The judge will then create the linked structure based on these inputs and pass the two heads, headA and headB to your program. If you correctly return the intersected node, then your solution will be accepted.

 

Example 1:


Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.


Example 2:

Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.


Example 3:

Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
 

Constraints:

The number of nodes of listA is in the m.
The number of nodes of listB is in the n.
1 <= m, n <= 3 * 104
1 <= Node.val <= 105
0 <= skipA <= m
0 <= skipB <= n
intersectVal is 0 if listA and listB do not intersect.
intersectVal == listA[skipA] == listB[skipB] if listA and listB intersect.
 

Follow up: Could you write a solution that runs in O(m + n) time and use only O(1) memory?

# Code
```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {

        if( !headA || !headB ) return nullptr;

        ListNode* p1 = headA;
        ListNode* p2 = headB;

        while( p1 != p2 ){
            p1 = ( p1 ? p1->next : headA );
            p2 = ( p2 ? p2->next : headB );
        }
        
        return p1;
    }
};
```

-------



# 234. Palindrome Linked List -> [LeetCode](https://leetcode.com/problems/palindrome-linked-list/description/)

Easy

Given the head of a singly linked list, return true if it is a palindrome or false otherwise.

 

Example 1:


Input: head = [1,2,2,1]
Output: true


Example 2:


Input: head = [1,2]
Output: false
 

Constraints:

The number of nodes in the list is in the range [1, 105].
0 <= Node.val <= 9
 

Follow up: Could you do it in O(n) time and O(1) space?
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
private:
    ListNode* reverseList(ListNode* head){
        ListNode* prev = nullptr;
        while(head){
            ListNode* nxt = head->next;
            head->next = prev;
            prev = head;
            head = nxt;
        }

        return prev;
    }
public:
    bool isPalindrome(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }

        if(fast) slow = slow->next;
        slow = reverseList(slow);

        while(slow){
            if(slow->val != head->val) return false;
            slow = slow->next;
            head = head->next;
        }

        return true;
    }
};
```


--------



# 543. Diameter of Binary Tree -> [LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/description/)

Easy

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

 

Example 1:


Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].


Example 2:

Input: root = [1,2]
Output: 1
 

Constraints:

The number of nodes in the tree is in the range [1, 104].
-100 <= Node.val <= 100

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
    int res = 0;
    int diameter(TreeNode* root){
        if(!root) return 0;
        int lf = diameter(root->left);
        int rt = diameter(root->right);
        res = max(res , lf + rt);
        return 1 + max(lf,rt);
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        diameter(root);
        return res;
    }
};
```

--------



# 559. Maximum Depth of N-ary Tree -> [Leetcode](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/description/)

Easy

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

 

Example 1:

Input: root = [1,null,3,2,4,null,5,6]
Output: 3


Example 2:

Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: 5
 

Constraints:

The total number of nodes is in the range [0, 104].
The depth of the n-ary tree is less than or equal to 1000.

# Code
```cpp []
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if(!root) return 0;
        queue<Node*> q;
        q.push(root);
        int ans = 0;
        while(!q.empty()){
            ans++;
            int len = q.size();
            for(int i=0 ; i < len ; ++i){
                Node* cur = q.front(); q.pop();
                for(Node* child:cur->children){
                    if(child) q.push(child);
                }
            }
        }

        return ans;
    }
};
```

------------



# 24. Swap Nodes in Pairs -> [LeetCode](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

Medium

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

Example 1:

Input: head = [1,2,3,4]

Output: [2,1,4,3]

Explanation:



Example 2:

Input: head = []

Output: []

Example 3:

Input: head = [1]

Output: [1]

Example 4:

Input: head = [1,2,3]

Output: [2,1,3]

 

Constraints:

The number of nodes in the list is in the range [0, 100].
0 <= Node.val <= 100

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
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* cur = dummy;
        while(cur->next && cur->next->next){
            ListNode* f = cur->next;
            ListNode* s = cur->next->next;
            f->next = s->next;
            s->next = f;
            cur->next = s;
            cur = f;
        }
        return dummy->next;
    }
};
```

-------


# 42. Trapping Rain Water -> [LeetCode](https://leetcode.com/problems/trapping-rain-water/description/)

Hard

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

 

Example 1:


Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.


Example 2:

Input: height = [4,2,0,3,2,5]
Output: 9
 

Constraints:

n == height.length
1 <= n <= 2 * 104
0 <= height[i] <= 105

# Code
```cpp []
class Solution {
public:
    int trap(vector<int>& arr) {
        int l = 0 , r = arr.size() - 1;
        int lM = arr[l] , rM = arr[r];
        int water = 0;
        while(l<r){
            if(arr[l] < arr[r]){
                lM = max(lM , arr[l]);
                water += lM - arr[l++];
            }else{
                rM = max(rM , arr[r]);
                water += rM - arr[r--];
            }
        }
        return water;
    }
};
```


--------------



# 590. N-ary Tree Postorder Traversal -> [LeetCode](https://leetcode.com/problems/n-ary-tree-postorder-traversal/description/)

Easy

Given the root of an n-ary tree, return the postorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

 

Example 1:

Input: root = [1,null,3,2,4,null,5,6]
Output: [5,6,3,2,4,1]


Example 2:


Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [2,6,14,11,7,3,12,8,4,13,9,10,5,1]
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
0 <= Node.val <= 104
The height of the n-ary tree is less than or equal to 1000.
 

Follow up: Recursive solution is trivial, could you do it iteratively?

# Code
```cpp []
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    vector<int> ans;
    void travesal(Node* root){
        if(!root) return;
        for(Node* cur : root->children) travesal(cur);
        ans.push_back(root->val);
    }
public:
    vector<int> postorder(Node* root) {
        travesal(root);
        return ans;
    }
};
```

-----


# 589. N-ary Tree Preorder Traversal -> [LeetCode](https://leetcode.com/problems/n-ary-tree-preorder-traversal/description/)

Easy

Given the root of an n-ary tree, return the preorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

Example 1:

Input: root = [1,null,3,2,4,null,5,6]
Output: [1,3,5,6,2,4]


Example 2:

Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [1,2,3,6,7,11,14,4,8,12,5,9,13,10]
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
0 <= Node.val <= 104
The height of the n-ary tree is less than or equal to 1000.

# Code
```cpp []
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    vector<int> ans;
    void travesal(Node* root){
        if(!root) return;
        ans.push_back(root->val);
        for(Node* cur : root->children ){
            travesal(cur);
        }
    }
public:
    vector<int> preorder(Node* root) {
        travesal(root);
        return ans;
    }
};
```

--------


# 1957. Delete Characters to Make Fancy String -> [LeetCode](https://leetcode.com/problems/delete-characters-to-make-fancy-string/description)

Easy

A fancy string is a string where no three consecutive characters are equal.

Given a string s, delete the minimum possible number of characters from s to make it fancy.

Return the final string after the deletion. It can be shown that the answer will always be unique.

 

Example 1:

Input: s = "leeetcode"
Output: "leetcode"
Explanation:
Remove an 'e' from the first group of 'e's to create "leetcode".
No three consecutive characters are equal, so return "leetcode".
Example 2:

Input: s = "aaabaaaa"
Output: "aabaa"
Explanation:
Remove an 'a' from the first group of 'a's to create "aabaaaa".
Remove two 'a's from the second group of 'a's to create "aabaa".
No three consecutive characters are equal, so return "aabaa".
Example 3:

Input: s = "aab"
Output: "aab"
Explanation: No three consecutive characters are equal, so return "aab".
 

Constraints:

1 <= s.length <= 105
s consists only of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    string makeFancyString(string s) {
        
        string ans = "";
        ans.push_back(s[0]);
        int count = 1;
        for(int i=1 ; i<s.size() ; ++i){
          if(s[i] == ans.back() ){
            count++;
            if(count < 3) ans.push_back(s[i]);
          }else{
            count= 1;
            ans.push_back(s[i]);
          }
        }

        return ans;
    }
};
```

-------


# 28. Find the Index of the First Occurrence in a String -> [LeetCode](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

Easy

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

 
Example 1:

Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.


Example 2:

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
 

Constraints:

1 <= haystack.length, needle.length <= 104
haystack and needle consist of only lowercase English characters.

# Code
```cpp []
class Solution {
public:
    int strStr(string s1, string s2) {

        int n = s1.size() , m = s2.size();
        for(int i= 0 ; i <= n-m ; ++i){
            if(s1.substr(i,m) == s2) return i;
        }
        return -1;
    }
};
```

------------



# 67. Add Binary -> [LeetCode](https://leetcode.com/problems/add-binary/description/)

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
        int i = a.size() - 1 , j = b.size() - 1;
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

-------



# 1695. Maximum Erasure Value ->  [LeetCode](https://leetcode.com/problems/maximum-erasure-value/description)

Medium

You are given an array of positive integers nums and want to erase a subarray containing unique elements. The score you get by erasing the subarray is equal to the sum of its elements.

Return the maximum score you can get by erasing exactly one subarray.

An array b is called to be a subarray of a if it forms a contiguous subsequence of a, that is, if it is equal to a[l],a[l+1],...,a[r] for some (l,r).

 

Example 1:

Input: nums = [4,2,4,5,6]
Output: 17
Explanation: The optimal subarray here is [2,4,5,6].


Example 2:

Input: nums = [5,2,1,2,5,2,1,2,5]
Output: 8
Explanation: The optimal subarray here is [5,2,1] or [1,2,5].
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 104

# Code
```cpp []
class Solution {
 public:
  int maximumUniqueSubarray(vector<int>& nums) {
    int ans = 0;
    int score = 0;
    unordered_set<int> seen;

    for(int l=0 , r =0 ; r<nums.size() ; ++r){
        while(!seen.insert(nums[r]).second){
            score -= nums[l];
            seen.erase(nums[l++]);
        }
        score += nums[r];
        ans = max(ans , score);
    }

    return ans;
  }
};

```

---------------




# 1717. Maximum Score From Removing Substrings -> [LeetCode](https://leetcode.com/problems/maximum-score-from-removing-substrings/description/)

Medium

You are given a string s and two integers x and y. You can perform two types of operations any number of times.

Remove substring "ab" and gain x points.
For example, when removing "ab" from "cabxbae" it becomes "cxbae".
Remove substring "ba" and gain y points.
For example, when removing "ba" from "cabxbae" it becomes "cabxe".
Return the maximum points you can gain after applying the above operations on s.

 

Example 1:

Input: s = "cdbcbbaaabab", x = 4, y = 5
Output: 19
Explanation:
- Remove the "ba" underlined in "cdbcbbaaabab". Now, s = "cdbcbbaaab" and 5 points are added to the score.
- Remove the "ab" underlined in "cdbcbbaaab". Now, s = "cdbcbbaa" and 4 points are added to the score.
- Remove the "ba" underlined in "cdbcbbaa". Now, s = "cdbcba" and 5 points are added to the score.
- Remove the "ba" underlined in "cdbcba". Now, s = "cdbc" and 5 points are added to the score.
Total score = 5 + 4 + 5 + 5 = 19.


Example 2:

Input: s = "aabbaaxybbaabb", x = 5, y = 4
Output: 20
 

Constraints:

1 <= s.length <= 105
1 <= x, y <= 104
s consists of lowercase English letters.

# Code
```cpp []
class Solution {
 public:
  int maximumGain(string s, int x, int y) {
    return x > y ? gain(s, "ab", x, "ba", y) : gain(s, "ba", y, "ab", x);
  }

 private:

  int gain(const string& s, const string& sub1, int point1, const string& sub2,
           int point2) {
    int points = 0;
    vector<char> stack1;
    vector<char> stack2;

    for (const char c : s)
      if (!stack1.empty() && stack1.back() == sub1[0] && c == sub1[1]) {
        stack1.pop_back();
        points += point1;
      } else {
        stack1.push_back(c);
      }

    for (const char c : stack1)
      if (!stack2.empty() && stack2.back() == sub2[0] && c == sub2[1]) {
        stack2.pop_back();
        points += point2;
      } else {
        stack2.push_back(c);
      }

    return points;
  }
};
```


---------




# 1048. Longest String Chain -> [LeetCode](https://leetcode.com/problems/longest-string-chain/description/)

Medium

You are given an array of words where each word consists of lowercase English letters.

wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".
A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1.

Return the length of the longest possible word chain with words chosen from the given list of words.

 

Example 1:

Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chains is ["a","ba","bda","bdca"].


Example 2:

Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5
Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].


Example 3:

Input: words = ["abcd","dbqca"]
Output: 1
Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.
 

Constraints:

1 <= words.length <= 1000
1 <= words[i].length <= 16
words[i] only consists of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int longestStrChain(vector<string>& words) {
        int n = words.size();
        sort(words.begin(), words.end(), [](auto & a, auto & b) {
            return a.size() < b.size();
        });

        unordered_map<string, int> dp;
        int res = 1;
        for (const string& s : words) {
            dp[s] = 1;
            for(int i=0 ; i<s.size() ; ++i){
                string pre = s.substr(0,i) + s.substr(i+1);
                if(dp.count(pre)) dp[s] = max(dp[s] , dp[pre] +1 );
            }
            res = max(res , dp[s]);
        }

        return res;
    }
};
```

--------------------


# 516. Longest Palindromic Subsequence -> [LeetCode](https://leetcode.com/problems/longest-palindromic-subsequence/description/)

Medium

Given a string s, find the longest palindromic subsequence's length in s.

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

 

Example 1:

Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".

Example 2:

Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".
 

Constraints:

1 <= s.length <= 1000
s consists only of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int len = s.size();

        vector<int> cur(len,0) , prev(len,0);

        for(int i=len-1 ; i>=0 ; --i){
            cur[i] = 1;
            for(int j=i ; j<len ; ++j ){
                if(i == j) continue;
                if(s[i] == s[j]){
                    if(i+1 == j) cur[j] = 2;
                    else cur[j] = prev[j-1] + 2;
                }
                else{
                    cur[j] = max(prev[j] , cur[j-1]);
                }
            }
            prev = cur;
        }
        return prev[len-1];
    }
};
```

--------------


# 2322. Minimum Score After Removals on a Tree -> [Leetcode](https://leetcode.com/problems/minimum-score-after-removals-on-a-tree/description/)
 
Hard
  
There is an undirected connected tree with n nodes labeled from 0 to n - 1 and n - 1 edges.

You are given a 0-indexed integer array nums of length n where nums[i] represents the value of the ith node. You are also given a 2D integer array edges of length n - 1 where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

Remove two distinct edges of the tree to form three connected components. For a pair of removed edges, the following steps are defined:

Get the XOR of all the values of the nodes for each of the three components respectively.
The difference between the largest XOR value and the smallest XOR value is the score of the pair.
For example, say the three components have the node values: [4,5,7], [1,9], and [3,3,3]. The three XOR values are 4 ^ 5 ^ 7 = 6, 1 ^ 9 = 8, and 3 ^ 3 ^ 3 = 3. The largest XOR value is 8 and the smallest XOR value is 3. The score is then 8 - 3 = 5.
Return the minimum score of any possible pair of edge removals on the given tree.

 

Example 1:


Input: nums = [1,5,5,4,11], edges = [[0,1],[1,2],[1,3],[3,4]]
Output: 9
Explanation: The diagram above shows a way to make a pair of removals.
- The 1st component has nodes [1,3,4] with values [5,4,11]. Its XOR value is 5 ^ 4 ^ 11 = 10.
- The 2nd component has node [0] with value [1]. Its XOR value is 1 = 1.
- The 3rd component has node [2] with value [5]. Its XOR value is 5 = 5.
The score is the difference between the largest and smallest XOR value which is 10 - 1 = 9.
It can be shown that no other pair of removals will obtain a smaller score than 9.


Example 2:


Input: nums = [5,5,2,4,4,2], edges = [[0,1],[1,2],[5,2],[4,3],[1,3]]
Output: 0
Explanation: The diagram above shows a way to make a pair of removals.
- The 1st component has nodes [3,4] with values [4,4]. Its XOR value is 4 ^ 4 = 0.
- The 2nd component has nodes [1,0] with values [5,5]. Its XOR value is 5 ^ 5 = 0.
- The 3rd component has nodes [2,5] with values [2,2]. Its XOR value is 2 ^ 2 = 0.
The score is the difference between the largest and smallest XOR value which is 0 - 0 = 0.
We cannot obtain a smaller score than 0.
 

Constraints:

n == nums.length
3 <= n <= 1000
1 <= nums[i] <= 108
edges.length == n - 1
edges[i].length == 2
0 <= ai, bi < n
ai != bi
edges represents a valid tree.

# Code
```cpp []
class Solution {
public:
    int minimumScore(vector<int>& nums, vector<vector<int>>& edges) {
        const int n = nums.size();
        const int xors = reduce(nums.begin(), nums.end(), 0, bit_xor());
        vector<int> subXors(nums);
        vector<vector<int>> tree(n);
        vector<unordered_set<int>> children(n);

        for (int i = 0; i < n; ++i)
            children[i].insert(i);

        for (const vector<int>& edge : edges) {
            const int u = edge[0];
            const int v = edge[1];
            tree[u].push_back(v);
            tree[v].push_back(u);
        }

        dfs(tree, 0, -1, subXors, children);

        int ans = INT_MAX;

        for (int i = 0; i < edges.size(); ++i) {
            int a = edges[i][0];
            int b = edges[i][1];
            if (children[a].contains(b))
                swap(a, b);
            for (int j = 0; j < i; ++j) {
                int c = edges[j][0];
                int d = edges[j][1];
                if (children[c].contains(d))
                    swap(c, d);
                vector<int> cands;
                if (a != c && children[a].contains(c))
                    cands = {subXors[c], subXors[a] ^ subXors[c],
                             xors ^ subXors[a]};
                else if (a != c && children[c].contains(a))
                    cands = {subXors[a], subXors[c] ^ subXors[a],
                             xors ^ subXors[c]};
                else
                    cands = {subXors[a], subXors[c],
                             xors ^ subXors[a] ^ subXors[c]};
                ans = min(ans, ranges::max(cands) - ranges::min(cands));
            }
        }

        return ans;
    }

private:
    pair<int, unordered_set<int>> dfs(const vector<vector<int>>& tree, int u,
                                      int prev, vector<int>& subXors,
                                      vector<unordered_set<int>>& children) {
        for (const int v : tree[u]) {
            if (v == prev)
                continue;
            const auto& [vXor, vChildren] = dfs(tree, v, u, subXors, children);
            subXors[u] ^= vXor;
            children[u].insert(vChildren.begin(), vChildren.end());
        }
        return {subXors[u], children[u]};
    }
};
```

-------------



# 5. Longest Palindromic Substring -> [LeetCode](https://leetcode.com/problems/longest-palindromic-substring/description/)
 
Medium
 
Given a string s, return the longest palindromic substring in s.

 

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
Example 2:

Input: s = "cbbd"
Output: "bb"
 

Constraints:

1 <= s.length <= 1000
s consist of only digits and English letters.

# Code
```cpp []
class Solution {

public:
    string longestPalindrome(string s) {
        int len  = s.size();

        vector<vector<bool>> dp(len , vector<bool>(len , false));

        for(int i=0 ; i<len ; ++i) dp[i][i] = true;

        int st = 0 , maxL = 1;

        for(int i=0 ; i<len-1 ; ++i){
            if(s[i] == s[i+1]){
                dp[i][i+1] = true;
                if(maxL<2){
                    st = i;
                    maxL = 2;
                }
            }
        }

        for(int k=3 ; k<=len ; ++k){
            for(int i=0 ; i<len - k +1 ; ++i){
                int j = i + k - 1;
                if(s[i] == s[j] && dp[i+1][j-1]){
                    dp[i][j] = true;
                    if(k>maxL){
                        maxL = k;
                        st = i;
                    }
                }
            }
        }

        return s.substr(st,maxL);
    }
};
```

--------------

# 647. Palindromic Substrings  -> [LeetCode](https://leetcode.com/problems/palindromic-substrings/description/)
 
Medium

Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

 

Example 1:

Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".


Example 2:

Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 

Constraints:

1 <= s.length <= 1000
s consists of lowercase English letters.

# Code
```cpp []
class Solution {
public:
    int countSubstrings(string s) {
        int res = 0;
        int len = s.size();

        vector<vector<bool>> dp(len , vector<bool>(len , false));

        for(int i=0 ; i<len ; ++i){ 
            dp[i][i] = true;
            res++;
        }

        for(int i=0 ; i<len-1 ; ++i){
            if(s[i] == s[i+1]){
                dp[i][i+1] = true;
                res++;
            }
        }


        for(int k=3 ; k<=len ; ++k){
            for(int i=0 ; i<=len-k ; ++i){
                int j = i + k - 1;
                if(s[i] == s[j] && dp[i+1][j-1]){
                    dp[i][j] = true;
                    res++;
                }
            }
        }

        return res;
    }
};
```

-------------

# 518. Coin Change II -> [leetCode](https://leetcode.com/problems/coin-change-ii/description/)
 
Medium
 
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

 

Example 1:

Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
Example 2:

Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
Example 3:

Input: amount = 10, coins = [10]
Output: 1
 

Constraints:

1 <= coins.length <= 300
1 <= coins[i] <= 5000
All the values of coins are unique.
0 <= amount <= 5000

# Code
```cpp []
class Solution {
public:
    int change(int sum, vector<int>& coins) {
        vector<unsigned long long> dp(sum+1 , 0);
        dp[0] = 1;
        for(const int& c:coins){
            for(int j=c ; j<=sum ; ++j){
                dp[j] += dp[j-c];
            }
        }
        return (int)(dp[sum]);
    }
};
```

--------


# 3487. Maximum Unique Subarray Sum After Deletion -> [LeetCode](https://leetcode.com/problems/maximum-unique-subarray-sum-after-deletion/description)
 
Easy
 
You are given an integer array nums.

You are allowed to delete any number of elements from nums without making it empty. After performing the deletions, select a subarray of nums such that:

All elements in the subarray are unique.
The sum of the elements in the subarray is maximized.
Return the maximum sum of such a subarray.

 

Example 1:

Input: nums = [1,2,3,4,5]

Output: 15

Explanation:

Select the entire array without deleting any element to obtain the maximum sum.

Example 2:

Input: nums = [1,1,0,1,1]

Output: 1

Explanation:

Delete the element nums[0] == 1, nums[1] == 1, nums[2] == 0, and nums[3] == 1. Select the entire array [1] to obtain the maximum sum.

Example 3:

Input: nums = [1,2,-1,-2,1,0,-1]

Output: 3

Explanation:

Delete the elements nums[2] == -1 and nums[3] == -2, and select the subarray [2, 1] from [1, 2, 1, 0, -1] to obtain the maximum sum.

 

Constraints:

1 <= nums.length <= 100
-100 <= nums[i] <= 100

# Code
```cpp []
class Solution {
public:
    int maxSum(vector<int>& nums) {
        bool isAllNeg = true;
        int maxV = INT_MIN;

        for(const int& n:nums){
            if(n>=0) isAllNeg = false;
            if(n > maxV) maxV = n;
        }

        if(isAllNeg) return maxV;

        unordered_set<int> arr(nums.begin() , nums.end());

        int sum = 0;

        for(const int & n:arr){
            if(n >=0 ) sum += n;
        }

        return sum;
    }
};
```

--------



# 55. Jump Game -> [LeetCode](https://leetcode.com/problems/jump-game/description/)
 
Medium
 
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

 

Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.


Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
 

Constraints:

1 <= nums.length <= 104
0 <= nums[i] <= 105

# Code
```cpp []
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int goal = nums.size() - 1;

        for(int i = nums.size() - 2 ; i>=0 ; --i ){
            if(i + nums[i] >= goal) goal = i;
        }

        return (goal == 0);
    }
};
```


------



# 3480. Maximize Subarrays After Removing One Conflicting Pair -> [LeetCode](https://leetcode.com/problems/maximize-subarrays-after-removing-one-conflicting-pair/description)
 
Hard
 
You are given an integer n which represents an array nums containing the numbers from 1 to n in order. Additionally, you are given a 2D array conflictingPairs, where conflictingPairs[i] = [a, b] indicates that a and b form a conflicting pair.

Remove exactly one element from conflictingPairs. Afterward, count the number of non-empty subarrays of nums which do not contain both a and b for any remaining conflicting pair [a, b].

Return the maximum number of subarrays possible after removing exactly one conflicting pair.

 

Example 1:

Input: n = 4, conflictingPairs = [[2,3],[1,4]]

Output: 9

Explanation:

Remove [2, 3] from conflictingPairs. Now, conflictingPairs = [[1, 4]].
There are 9 subarrays in nums where [1, 4] do not appear together. They are [1], [2], [3], [4], [1, 2], [2, 3], [3, 4], [1, 2, 3] and [2, 3, 4].
The maximum number of subarrays we can achieve after removing one element from conflictingPairs is 9.
Example 2:

Input: n = 5, conflictingPairs = [[1,2],[2,5],[3,5]]

Output: 12

Explanation:

Remove [1, 2] from conflictingPairs. Now, conflictingPairs = [[2, 5], [3, 5]].
There are 12 subarrays in nums where [2, 5] and [3, 5] do not appear together.
The maximum number of subarrays we can achieve after removing one element from conflictingPairs is 12.
 

Constraints:

2 <= n <= 105
1 <= conflictingPairs.length <= 2 * n
conflictingPairs[i].length == 2
1 <= conflictingPairs[i][j] <= n
conflictingPairs[i][0] != conflictingPairs[i][1]

# Code
```cpp []
class Solution {
public:
    long long maxSubarrays(int n, vector<vector<int>>& conflictingPairs) {
        long validSubarrays = 0;
        int maxLeft = 0;
        int secondMaxLeft = 0;
        vector<long> gains(n + 1);
        vector<vector<int>> conflicts(n + 1);

        for (const vector<int>& pair : conflictingPairs) {
            const int a = pair[0];
            const int b = pair[1];
            conflicts[max(a, b)].push_back(min(a, b));
        }

        for (int right = 1; right <= n; ++right) {
            for (const int left : conflicts[right]) {
                if (left > maxLeft) {
                    secondMaxLeft = maxLeft;
                    maxLeft = left;
                } else if (left > secondMaxLeft) {
                    secondMaxLeft = left;
                }
            } 
            validSubarrays += right - maxLeft; 
            gains[maxLeft] += maxLeft - secondMaxLeft;
        }

        return validSubarrays + ranges::max(gains);
    }
};
```

-----------


# 239. Sliding Window Maximum -> [LeetCode](https://leetcode.com/problems/sliding-window-maximum/description/)
 
Hard
 
You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

 

Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]

Explanation: 

Window position              |  Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7


Example 2:

Input: nums = [1], k = 1
Output: [1]
 

Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104
1 <= k <= nums.length

# Code
```cpp []
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        deque<int> dq;
        vector<int> ans;

        for(int i=0 ; i<k ; ++i){
            while(!dq.empty() && nums[i] >= nums[dq.back()]) dq.pop_back();
            dq.push_back(i);
        }

        for(int i=k ; i<n ; ++i){
            ans.push_back(nums[dq.front()]);
            while(!dq.empty() && dq.front() <= i - k) dq.pop_front();
            while(!dq.empty() && nums[i] >= nums[dq.back()]) dq.pop_back();
            dq.push_back(i);
        }
        ans.push_back(nums[dq.front()]);

        return ans;
    }
};
```

-----------------


# 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit -> [LeetCode](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/description/)
 
Medium
 
Given an array of integers nums and an integer limit, return the size of the longest non-empty subarray such that the absolute difference between any two elements of this subarray is less than or equal to limit.

 

Example 1:

Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.


Example 2:

Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.


Example 3:

Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
0 <= limit <= 109

# Code
```cpp []
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int n = nums.size();
        int st = 0, end = 0;
        int stA = 0, endA = 0;

        deque<int> minQ, maxQ;

        while (end < n) {
            while (!minQ.empty() && nums[end] < nums[minQ.back()])
                minQ.pop_back();
            while (!maxQ.empty() && nums[end] > nums[maxQ.back()])
                maxQ.pop_back();
            minQ.push_back(end);
            maxQ.push_back(end);

            while (nums[maxQ.front()] - nums[minQ.front()] > limit) {
                if (st == minQ.front())
                    minQ.pop_front();
                if (st == maxQ.front())
                    maxQ.pop_front();
                st++;
            }

            if (end - st > endA - stA) {
                stA = st;
                endA = end;
            }

            end++;
        }

        return (endA - stA) + 1;
    }
};
```


--------------



# 32. Longest Valid Parentheses -> [LeetCode](https://leetcode.com/problems/longest-valid-parentheses/description/)
 
Hard
 
Given a string containing just the characters '(' and ')', return the length of the longest valid (well-formed) parentheses substring.

 

Example 1:

Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".


Example 2:

Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".

Example 3:

Input: s = ""
Output: 0
 

Constraints:

0 <= s.length <= 3 * 104
s[i] is '(', or ')'.

# Code
```cpp []
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1);
        int ans = 0;

        for(int i=0 ; i<s.size() ; ++i){
            if(s[i] == '(') st.push(i);
            else{
                st.pop();
                if(st.empty()) st.push(i);
                else ans = max(ans, i - st.top());
            }
        }
        return ans;
    }
};
```

--------------


# 496. Next Greater Element I -> [LeetCode](https://leetcode.com/problems/next-greater-element-i/description/)
 
Easy
 
The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.

You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.

For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.

Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.

 

Example 1:

Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.


Example 2:

Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
 

Constraints:

1 <= nums1.length <= nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 104
All integers in nums1 and nums2 are unique.
All the integers of nums1 also appear in nums2.

# Code
```cpp []
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        
        stack<int> st;
        unordered_map<int,int> umap;

        for(int i=nums2.size() - 1 ; i>=0 ; --i){
            while(!st.empty() && st.top() <= nums2[i]) st.pop();
            if(st.empty()) umap[nums2[i]] = -1;
            else umap[nums2[i]] = st.top();
            st.push(nums2[i]);
        }

        vector<int> ans;
        for(int i:nums1) ans.push_back(umap[i]);

        return ans;
    }
};
```

----------------------------------



#  724. Find Pivot Index -> [LeetCode](https://leetcode.com/problems/find-pivot-index/description/)
 
Easy
 
Given an array of integers nums, calculate the pivot index of this array.

The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

If the index is on the left edge of the array, then the left sum is 0 because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return -1.

 

Example 1:

Input: nums = [1,7,3,6,5,6]
Output: 3
Explanation:
The pivot index is 3.
Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
Right sum = nums[4] + nums[5] = 5 + 6 = 11


Example 2:

Input: nums = [1,2,3]
Output: -1
Explanation:
There is no index that satisfies the conditions in the problem statement.


Example 3:

Input: nums = [2,1,-1]
Output: 0
Explanation:
The pivot index is 0.
Left sum = 0 (no elements to the left of index 0)
Right sum = nums[1] + nums[2] = 1 + -1 = 0
 


Constraints:

1 <= nums.length <= 104
-1000 <= nums[i] <= 1000
 


# Code
```cpp []
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int prefixSum = 0 , total = 0;
        for(int i:nums) total += i;
        int n = nums.size();
        for(int i=0 ; i<n ; ++i){
            int suffixSum = total - prefixSum - nums[i];
            if(suffixSum == prefixSum) return i;
            prefixSum += nums[i];
        }
        return -1;
    }
};
```

--------------


# 2044. Count Number of Maximum Bitwise-OR Subsets -> [LeetCode](https://leetcode.com/problems/count-number-of-maximum-bitwise-or-subsets/description)
 
Medium
 
Given an integer array nums, find the maximum possible bitwise OR of a subset of nums and return the number of different non-empty subsets with the maximum bitwise OR.

An array a is a subset of an array b if a can be obtained from b by deleting some (possibly zero) elements of b. Two subsets are considered different if the indices of the elements chosen are different.

The bitwise OR of an array a is equal to a[0] OR a[1] OR ... OR a[a.length - 1] (0-indexed).

 

Example 1:

Input: nums = [3,1]
Output: 2
Explanation: The maximum possible bitwise OR of a subset is 3. There are 2 subsets with a bitwise OR of 3:
- [3]
- [3,1]


Example 2:

Input: nums = [2,2,2]
Output: 7
Explanation: All non-empty subsets of [2,2,2] have a bitwise OR of 2. There are 23 - 1 = 7 total subsets.


Example 3:

Input: nums = [3,2,1,5]
Output: 6
Explanation: The maximum possible bitwise OR of a subset is 7. There are 6 subsets with a bitwise OR of 7:
- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]
 

Constraints:

1 <= nums.length <= 16
1 <= nums[i] <= 105

# Code
```cpp []
class Solution {
public:
    int countMaxOrSubsets(vector<int>& nums) {
        const int ors = accumulate(nums.begin(), nums.end(), 0, bit_or<>());
        int ans = 0;
        dfs(nums, 0, 0, ors, ans);
        return ans;
    }

private:
    void dfs(const vector<int>& nums, int i, int path, const int& ors,
             int& ans) {
        if (i == nums.size()) {
            if (path == ors)
                ++ans;
            return;
        }

        dfs(nums, i + 1, path, ors, ans);
        dfs(nums, i + 1, path | nums[i], ors, ans);
    }
};
```

--------------------

# 41. First Missing Positive -> [LeetCode](https://leetcode.com/problems/first-missing-positive/description/)
 
Hard
 
Given an unsorted integer array nums. Return the smallest positive integer that is not present in nums.

You must implement an algorithm that runs in O(n) time and uses O(1) auxiliary space.

 

Example 1:

Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.


Example 2:

Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.


Example 3:

Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
 

Constraints:

1 <= nums.length <= 105
-231 <= nums[i] <= 231 - 1

# Code
```cpp []
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();

        for (int i = 0; i < n; ++i) {
            while (nums[i] > 0 && nums[i] <= n &&
                   nums[i] != nums[nums[i]- 1] ) {
                    swap(nums[i] , nums[nums[i] - 1]);
            }
        }

        for(int i=0 ; i<n ; ++i){
            if(nums[i] != i + 1) return i+1;
        }

        return n+1;
    }
};
```

---------------


# 73. Set Matrix Zeroes - [LeetCode](https://leetcode.com/problems/set-matrix-zeroes/description/)
 
Medium
 
Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's.

You must do it in place.

 

Example 1:


Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]


Example 2:


Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
 

Constraints:

m == matrix.length
n == matrix[0].length
1 <= m, n <= 200
-231 <= matrix[i][j] <= 231 - 1

# Code
```cpp []
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int r = matrix.size(), c = matrix[0].size();
        vector<bool> rowZero(r, false), colZero(c, false);

        for (int i = 0; i < r ; ++i) {
            for (int j = 0; j < c; ++j) {
                if (matrix[i][j] == 0) {
                    rowZero[i] = true;
                    colZero[j] = true;
                }
            }
        }

        for (int i = 0; i < r; ++i) {
            for (int j = 0; j < c; ++j) {
                if (rowZero[i] || colZero[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

---------------


# 14. Longest Common Prefix -> [LeetCode](https://leetcode.com/problems/longest-common-prefix/description/)
 
Easy
 
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

 

Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"


Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
 

Constraints:

1 <= strs.length <= 200
0 <= strs[i].length <= 200
strs[i] consists of only lowercase English letters if it is non-empty.

# Code
```cpp []

class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ans = "";

        int minL = strs[0].size();
        for(int i =1 ; i<strs.size() ; ++i) minL = min(minL , (int)strs[i].length());

        for(int i=0 ; i<minL ; ++i){
            char c = strs[0][i];
            for(int j=1 ; j<strs.size() ; ++j){
                if(strs[j][i] != c) return ans;
            }
            ans += c;
        }

        return ans;
    }
};
```

--------------


# 2411. Smallest Subarrays With Maximum Bitwise OR -> [LeetCode](https://leetcode.com/problems/smallest-subarrays-with-maximum-bitwise-or/description/)
 
Medium
 
You are given a 0-indexed array nums of length n, consisting of non-negative integers. For each index i from 0 to n - 1, you must determine the size of the minimum sized non-empty subarray of nums starting at i (inclusive) that has the maximum possible bitwise OR.

In other words, let Bij be the bitwise OR of the subarray nums[i...j]. You need to find the smallest subarray starting at i, such that bitwise OR of this subarray is equal to max(Bik) where i <= k <= n - 1.
The bitwise OR of an array is the bitwise OR of all the numbers in it.

Return an integer array answer of size n where answer[i] is the length of the minimum sized subarray starting at i with maximum bitwise OR.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [1,0,2,1,3]
Output: [3,3,2,2,1]
Explanation:
The maximum possible bitwise OR starting at any index is 3. 
- Starting at index 0, the shortest subarray that yields it is [1,0,2].
- Starting at index 1, the shortest subarray that yields the maximum bitwise OR is [0,2,1].
- Starting at index 2, the shortest subarray that yields the maximum bitwise OR is [2,1].
- Starting at index 3, the shortest subarray that yields the maximum bitwise OR is [1,3].
- Starting at index 4, the shortest subarray that yields the maximum bitwise OR is [3].
Therefore, we return [3,3,2,2,1]. 


Example 2:

Input: nums = [1,2]
Output: [2,1]
Explanation:
Starting at index 0, the shortest subarray that yields the maximum bitwise OR is of length 2.
Starting at index 1, the shortest subarray that yields the maximum bitwise OR is of length 1.
Therefore, we return [2,1].
 

Constraints:

n == nums.length
1 <= n <= 105
0 <= nums[i] <= 109


# Code
```cpp []
class Solution {
public:
    vector<int> smallestSubarrays(vector<int>& nums) {
        constexpr int kMaxBit = 30;
        vector<int> ans(nums.size(), 1);
        // closest[j] := the closest index i s.t. the j-th bit of nums[i] is 1
        vector<int> closest(kMaxBit);

        for (int i = nums.size() - 1; i >= 0; --i)
            for (int j = 0; j < kMaxBit; ++j) {
                if (nums[i] >> j & 1)
                    closest[j] = i;
                ans[i] = max(ans[i], closest[j] - i + 1);
            }

        return ans;
    }
};
```

----------------


# 232. Implement Queue using Stacks -> [LeetCode](https://leetcode.com/problems/implement-queue-using-stacks/description/)
 
Easy
 
Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.
 

Example 1:

Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
 

Constraints:

1 <= x <= 9
At most 100 calls will be made to push, pop, peek, and empty.
All the calls to pop and peek are valid.

# Code
```cpp []
class MyQueue {
    stack<int> input;
    stack<int> output;
public:
    MyQueue() {
        
    }
    
    void push(int x) {
        input.push(x);
    }
    
    int pop() {
        if(output.empty()){
            while(!input.empty()){
                output.push(input.top());
                input.pop();
            }
        }
        int x = output.top(); output.pop();
        return x;
    }
    
    int peek() {
        if(output.empty()){
            while(!input.empty()){
                output.push(input.top());
                input.pop();
            }
        }
        int x = output.top(); 
        return x;
    }
    
    bool empty() {
        return input.empty() && output.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

---------------------


# 118. Pascal's Triangle -> [LeetCode](https://leetcode.com/problems/pascals-triangle/description/)
 
Easy
 
Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:


 

Example 1:

Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]


Example 2:

Input: numRows = 1
Output: [[1]]
 

Constraints:

1 <= numRows <= 30

# Code
```cpp []
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> triangle;

        for(int row=0 ; row<numRows ; ++row){
            vector<int> cur(row+1 , 1);
            for(int i=1 ; i<row ; ++i){
                cur[i] = triangle[row-1][i-1] + triangle[row-1][i];
            }
            triangle.push_back(cur);
        }

        return triangle;
    }
};
```

-----------



# 119. Pascal's Triangle II -> [LeetCode](https://leetcode.com/problems/pascals-triangle-ii/description/)
 
Easy
 
Given an integer rowIndex, return the rowIndexth (0-indexed) row of the Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:


 

Example 1:

Input: rowIndex = 3
Output: [1,3,3,1]

Example 2:

Input: rowIndex = 0
Output: [1]

Example 3:

Input: rowIndex = 1
Output: [1,1]
 

Constraints:

0 <= rowIndex <= 33

# Code
```cpp []
class Solution {
public:
    vector<int> getRow(int rowIndex) {
         vector<int> prev;

        for(int i=0 ; i<=rowIndex ; ++i){
            vector<int> cur(i+1 , 1);
            for(int j=1 ; j<i ; ++j){
                cur[j] = prev[j-1] + prev[j];
            }
            prev = cur;
        }

        return prev;
    }
};
```

-----------

# 258. Add Digits -> [LeetCode](https://leetcode.com/problems/add-digits/description/)
 
Easy
 
Given an integer num, repeatedly add all its digits until the result has only one digit, and return it.

 

Example 1:

Input: num = 38
Output: 2
Explanation: The process is
38 --> 3 + 8 --> 11
11 --> 1 + 1 --> 2 
Since 2 has only one digit, return it.


Example 2:

Input: num = 0
Output: 0
 

Constraints:

0 <= num <= 231 - 1



# Code
```cpp []
class Solution {
public:
    int addDigits(int num) {
        // while(num/10){
        //     num = (num % 10) + (num / 10);
        // }
        // return num;

        // using formula
        return 1 + (num-1) % 9;
    }
};
```

-----------------


# 2419. Longest Subarray With Maximum Bitwise AND -> [LeetCode](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/description)
 
Medium
 
You are given an integer array nums of size n.

Consider a non-empty subarray from nums that has the maximum possible bitwise AND.

In other words, let k be the maximum value of the bitwise AND of any subarray of nums. Then, only subarrays with a bitwise AND equal to k should be considered.
Return the length of the longest such subarray.

The bitwise AND of an array is the bitwise AND of all the numbers in it.

A subarray is a contiguous sequence of elements within an array.

 

Example 1:

Input: nums = [1,2,3,3,2,2]
Output: 2
Explanation:
The maximum possible bitwise AND of a subarray is 3.
The longest subarray with that value is [3,3], so we return 2.


Example 2:

Input: nums = [1,2,3,4]
Output: 1
Explanation:
The maximum possible bitwise AND of a subarray is 4.
The longest subarray with that value is [4], so we return 1.
 

Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 106

# Code
```cpp []
class Solution {
 public:
  int longestSubarray(vector<int>& nums) {
    int ans = 0;
    int maxIndex = 0;
    int sameNumLength = 0;

    for (int i = 0; i < nums.size(); ++i)
      if (nums[i] == nums[maxIndex]) {
        ans = max(ans, ++sameNumLength);
      } else if (nums[i] > nums[maxIndex]) {
        maxIndex = i;
        sameNumLength = 1;
        ans = 1;
      } else {
        sameNumLength = 0;
      }

    return ans;
  }
};
```

----------


# 168. Excel Sheet Column Title -> [LeetCode](https://leetcode.com/problems/excel-sheet-column-title/description/)
 
Easy
 
Given an integer columnNumber, return its corresponding column title as it appears in an Excel sheet.

For example:

A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
 

Example 1:

Input: columnNumber = 1
Output: "A"


Example 2:

Input: columnNumber = 28
Output: "AB"


Example 3:

Input: columnNumber = 701
Output: "ZY"
 

Constraints:

1 <= columnNumber <= 231 - 1

# Code
```cpp []
class Solution {

public:
    string convertToTitle(int columnNumber) {
        if(columnNumber == 0) return "";
        columnNumber--;
        char c = 'A' + columnNumber % 26;
        return convertToTitle(columnNumber/26) + c;
    }
};
```

----------------


# 171. Excel Sheet Column Number -> [LeetCode](https://leetcode.com/problems/excel-sheet-column-number/description/)
 
Easy
 
Given a string columnTitle that represents the column title as appears in an Excel sheet, return its corresponding column number.

For example:

A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
 

Example 1:

Input: columnTitle = "A"
Output: 1


Example 2:

Input: columnTitle = "AB"
Output: 28


Example 3:

Input: columnTitle = "ZY"
Output: 701
 

Constraints:

1 <= columnTitle.length <= 7
columnTitle consists only of uppercase English letters.
columnTitle is in the range ["A", "FXSHRXW"].

# Code
```cpp []
class Solution {
public:
    int titleToNumber(string columnTitle) {
        int ans = 0;
        for(char c:columnTitle){
            ans = (ans * 26) + (c - 'A' + 1);
        }
        return ans;
    }
};
```

-------------



# 257. Binary Tree Paths -> [LeetCode](https://leetcode.com/problems/binary-tree-paths/description/)
 
Easy
 
Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

 

Example 1:


Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]


Example 2:

Input: root = [1]
Output: ["1"]
 

Constraints:

The number of nodes in the tree is in the range [1, 100].
-100 <= Node.val <= 100

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
    vector<string> ans;

    void tracePath(TreeNode* root , string cur){
        if(!root) return;
        if(root->left || root->right) cur += to_string(root->val) + "->";
        else{
            cur+= to_string(root->val);
            ans.push_back(cur);
        }
        tracePath(root->left , cur);
        tracePath(root->right , cur);
    }
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        string cur = "";
        tracePath(root , cur);
        return ans;
    }
};
```


-------------

# 263. Ugly Number -> [LeetCode](https://leetcode.com/problems/ugly-number/description/)
 
Easy
 
An ugly number is a positive integer which does not have a prime factor other than 2, 3, and 5.

Given an integer n, return true if n is an ugly number.

 

Example 1:

Input: n = 6
Output: true
Explanation: 6 = 2 × 3


Example 2:

Input: n = 1
Output: true
Explanation: 1 has no prime factors.


Example 3:

Input: n = 14
Output: false
Explanation: 14 is not ugly since it includes the prime factor 7.
 

Constraints:

-231 <= n <= 231 - 1

# Code
```cpp []
class Solution {
public:
    bool isUgly(int num) {
        for(int i=2 ; i<=5 && num ; ++i){
            if(i==4) continue;
            while(num % i == 0){
                num /= i;
            }
        }
        return (num == 1);
    }
};
```

----------


# 278. First Bad Version -> [LeetCode](https://leetcode.com/problems/first-bad-version/description/)
Easy
 
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

 

Example 1:

Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.

Example 2:

Input: n = 1, bad = 1
Output: 1
 

Constraints:

1 <= bad <= n <= 231 - 1
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```cpp []
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 0 , r = n;
        while(l<=r){
            int mid = l + (r - l) / 2;
            if(isBadVersion(mid)) r = mid - 1;
            else l = mid + 1;
        }
        return l;
    }
};
```

----------


# 898. Bitwise ORs of Subarrays -> [LeetCode](https://leetcode.com/problems/bitwise-ors-of-subarrays/description/)
 
Medium
 
Given an integer array arr, return the number of distinct bitwise ORs of all the non-empty subarrays of arr.

The bitwise OR of a subarray is the bitwise OR of each integer in the subarray. The bitwise OR of a subarray of one integer is that integer.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: arr = [0]
Output: 1
Explanation: There is only one possible result: 0.


Example 2:

Input: arr = [1,1,2]
Output: 3
Explanation: The possible subarrays are [1], [1], [2], [1, 1], [1, 2], [1, 1, 2].
These yield the results 1, 1, 2, 1, 3, 3.
There are 3 unique values, so the answer is 3.


Example 3:

Input: arr = [1,2,4]
Output: 6
Explanation: The possible results are 1, 2, 3, 4, 6, and 7.
 

Constraints:

1 <= arr.length <= 5 * 104
0 <= arr[i] <= 109

# Code
```cpp []
class Solution {
 public:
  int subarrayBitwiseORs(vector<int>& arr) {
    vector<int> s;
    int l = 0;

    for (const int a : arr) {
      const int r = s.size();
      s.push_back(a);
      // s[l..r) are values generated in the previous iteration
      for (int i = l; i < r; ++i)
        if (s.back() != (s[i] | a))
          s.push_back(s[i] | a);
      l = r;
    }

    return unordered_set<int>(s.begin(), s.end()).size();
  }
};
```

-------------


# 367. Valid Perfect Square -> [LeetCode](https://leetcode.com/problems/valid-perfect-square/description/)
 
Easy
  
Given a positive integer num, return true if num is a perfect square or false otherwise.

A perfect square is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as sqrt.

 

Example 1:

Input: num = 16
Output: true
Explanation: We return true because 4 * 4 = 16 and 4 is an integer.


Example 2:

Input: num = 14
Output: false
Explanation: We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.
 

Constraints:

1 <= num <= 231 - 1


# Code
```cpp []
class Solution {
public:
    bool isPerfectSquare(int num) {
        int i=1;
        while(num>0){
            num-=i;
            i+=2;
            if(!num) return true;
        }
        return false;
    }
};
```

-----------



# 303. Range Sum Query - Immutable -> [LeetCode](https://leetcode.com/problems/range-sum-query-immutable/description/)
 
Easy
 
Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).
 

Example 1:

Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
 

Constraints:

1 <= nums.length <= 104
-105 <= nums[i] <= 105
0 <= left <= right < nums.length
At most 104 calls will be made to sumRange.

# Code
```cpp []
class NumArray {
    vector<int> arr;
public:
    NumArray(vector<int>& nums) {
        arr = nums;
        for(int i=1 ; i<arr.size() ; ++i) arr[i] += arr[i-1];
    }
    
    int sumRange(int left, int right) {
        if(left) return arr[right] - arr[left-1];
        return arr[right];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```


--------------------------------------------------------------------
--------------------------------------------------------------------

![badge](https://assets.leetcode.com/static_assets/marketing/202507.gif)

--------------------------------------------------------------------




