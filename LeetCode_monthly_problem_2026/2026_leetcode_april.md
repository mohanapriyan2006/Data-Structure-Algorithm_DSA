# -------------------------------------------

# LeetCode Monthly Problems - April 2026

# -------------------------------------------

--------------------------------------------

# [2751. Robot Collisions](https://leetcode.com/problems/robot-collisions)

Hard
 
There are n 1-indexed robots, each having a position on a line, health, and movement direction.

You are given 0-indexed integer arrays positions, healths, and a string directions (directions[i] is either 'L' for left or 'R' for right). All integers in positions are unique.

All robots start moving on the line simultaneously at the same speed in their given directions. If two robots ever share the same position while moving, they will collide.

If two robots collide, the robot with lower health is removed from the line, and the health of the other robot decreases by one. The surviving robot continues in the same direction it was going. If both robots have the same health, they are both removed from the line.

Your task is to determine the health of the robots that survive the collisions, in the same order that the robots were given, i.e. final health of robot 1 (if survived), final health of robot 2 (if survived), and so on. If there are no survivors, return an empty array.

Return an array containing the health of the remaining robots (in the order they were given in the input), after no further collisions can occur.

Note: The positions may be unsorted.

 
 

Example 1:

![img](https://assets.leetcode.com/uploads/2023/05/15/image-20230516011718-12.png)

Input: positions = [5,4,3,2,1], healths = [2,17,9,15,10], directions = "RRRRR"

Output: [2,17,9,15,10]

Explanation: No collision occurs in this example, since all robots are moving in the same direction. So, the health of the robots in order from the first robot is returned, [2, 17, 9, 15, 10].



Example 2:

![img](https://assets.leetcode.com/uploads/2023/05/15/image-20230516004433-7.png)

Input: positions = [3,5,2,6], healths = [10,10,15,12], directions = "RLRL"

Output: [14]

Explanation: There are 2 collisions in this example. Firstly, robot 1 and robot 2 will collide, and since both have the same health, they will be removed from the line. Next, robot 3 and robot 4 will collide and since robot 4's health is smaller, it gets removed, and robot 3's health becomes 15 - 1 = 14. Only robot 3 remains, so we return [14].



Example 3:

![img](https://assets.leetcode.com/uploads/2023/05/15/image-20230516005114-9.png)

Input: positions = [1,2,5,6], healths = [10,10,11,11], directions = "RLRL"

Output: []

Explanation: Robot 1 and robot 2 will collide and since both have the same health, they are both removed. Robot 3 and 4 will collide and since both have the same health, they are both removed. So, we return an empty array, [].
 

Constraints:

1 <= positions.length == healths.length == directions.length == n <= 105
1 <= positions[i], healths[i] <= 109
directions[i] == 'L' or directions[i] == 'R'
All values in positions are distinct




# Code
```cpp []
class Solution {
public:
    vector<int> survivedRobotsHealths(vector<int>& pos, vector<int>& h, string d) {

        int n = pos.size();
        vector<int> order(n);
        iota(order.begin(), order.end(), 0);

        sort(order.begin(), order.end(),
             [&](int a,int b){ return pos[a] < pos[b]; });

        vector<bool> alive(n,true);
        vector<int> st;

        for(int idx:order){

            if(d[idx]=='R') st.push_back(idx);

            else{
                while(!st.empty()){

                    int top = st.back();

                    if(h[top] < h[idx]){
                        alive[top]=false;
                        st.pop_back();
                        h[idx]--;
                    }
                    else if(h[top] > h[idx]){
                        alive[idx]=false;
                        h[top]--;
                        break;
                    }
                    else{
                        alive[top]=false;
                        alive[idx]=false;
                        st.pop_back();
                        break;
                    }
                }
            }
        }

        vector<int> res;
        for(int i=0;i<n;i++)
            if(alive[i]) res.push_back(h[i]);

        return res;
    }
};
```

--------------------------------------------------------------------------------------------------


# [3418. Maximum Amount of Money Robot Can Earn](https://leetcode.com/problems/maximum-amount-of-money-robot-can-earn)

Medium
 
You are given an m x n grid. A robot starts at the top-left corner of the grid (0, 0) and wants to reach the bottom-right corner (m - 1, n - 1). The robot can move either right or down at any point in time.

The grid contains a value coins[i][j] in each cell:

If coins[i][j] >= 0, the robot gains that many coins.
If coins[i][j] < 0, the robot encounters a robber, and the robber steals the absolute value of coins[i][j] coins.
The robot has a special ability to neutralize robbers in at most 2 cells on its path, preventing them from stealing coins in those cells.

Note: The robot's total coins can be negative.

Return the maximum profit the robot can gain on the route.

 


Example 1:

Input: coins = [[0,1,-1],[1,-2,3],[2,-3,4]]

Output: 8

Explanation:

An optimal path for maximum coins is:

Start at (0, 0) with 0 coins (total coins = 0).
Move to (0, 1), gaining 1 coin (total coins = 0 + 1 = 1).
Move to (1, 1), where there's a robber stealing 2 coins. The robot uses one neutralization here, avoiding the robbery (total coins = 1).
Move to (1, 2), gaining 3 coins (total coins = 1 + 3 = 4).
Move to (2, 2), gaining 4 coins (total coins = 4 + 4 = 8).



Example 2:

Input: coins = [[10,10,10],[10,10,10]]

Output: 40

Explanation:

An optimal path for maximum coins is:

Start at (0, 0) with 10 coins (total coins = 10).
Move to (0, 1), gaining 10 coins (total coins = 10 + 10 = 20).
Move to (0, 2), gaining another 10 coins (total coins = 20 + 10 = 30).
Move to (1, 2), gaining the final 10 coins (total coins = 30 + 10 = 40).
 

Constraints:

m == coins.length
n == coins[i].length
1 <= m, n <= 500
-1000 <= coins[i][j] <= 1000













