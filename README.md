# Implementation-of-Computational-Methods-Evidence-4

## Description:

For this evidence, I chose the problem "Trapping Rain Water." In this challenge, the goal is to determine how much rainwater can be trapped given an array of non-negative integers representing an elevation map. Each element of the array corresponds to the height of a bar, and the width of each bar is assumed to be 1 unit. Visualizing this problem is crucial for understanding how rainwater can accumulate between the bars. Below is an image and example provided by LeetCode illustrating the problem:

![https://github.com/Dieg0Lir4/Implementation-of-Computational-Methods-Evidence-4/blob/main/leetcodeExample.jpg]

Example:
* Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
* Output: 6
* Explanation: The configuration represented by the array can trap 6 units of rainwater.

Constraints:
* The maximum number of bars is 20,000.
* The maximum height of a bar is 100,000 units.


I chose this problem not only because it is challenging but also highly relevant. It demonstrates how different spaces in the same map can have different results and how these differences can affect other spaces. For example, the problem illustrates how varying elevations can lead to different amounts of trapped water, similar to how real world terrains affect water accumulation. Additionally, this problem has various ways of solving it, making it a great test for parallel programming and sequential programming. 







```
class Solution
{
public:
    
    // Function to get the max height of the left wall
    int maxLeftWall(vector<int> &height, int i)
    {
        int max = height[i - 1];
        for (int j = 0; j < i; ++j)
        {
            if (height[j] > max)
            {
                max = height[j];
            }
        }
        return max;
    }

    // Function to get the max height of the right wall
    int maxRightWall(vector<int> &height, int i)
    {
        int max = height[i + 1];
        for (int j = i + 1; j < height.size(); ++j)
        {
            if (height[j] > max)
            {
                max = height[j];
            }
        }
        return max;
    }

    // Function to get the min height of the two walls
    int min(int a, int b)
    {
        return a < b ? a : b;
    }

    int trap(vector<int> &height)
    {

        // Get the number of elements in the vector
        int n = height.size();

        // Create variable for water trapped
        int water_trapped = 0;

        // Check if the at least space for two walls and space for water
        if (n < 3)
            return 0;

        // Loop for each position expect first and last
        for (int i = 1; i < n-1; ++i)
        {
            // Call the function to get the height of its max right and left walls
            int leftWall = maxLeftWall(height, i);
            int rightWall = maxRightWall(height, i);

            // Get the minimum height of the two walls
            int minWall = min(leftWall, rightWall);

            // Calculate if the current wall is less than the minimum wall
            if (height[i] < minWall)
            {
                // Add the difference between the minimum wall and the current wall 
                // to the water trapped
                water_trapped += minWall - height[i];
            }
        }

        return water_trapped;
    }
};
```
