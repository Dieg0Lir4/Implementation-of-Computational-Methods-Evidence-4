# Implementation-of-Computational-Methods-Evidence-4


'''
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
'''
