# Implementation-of-Computational-Methods-Evidence-4

## Description:

For this evidence, I chose the problem "Trapping Rain Water." In this challenge, the goal is to determine how much rainwater can be trapped given an array of non-negative integers representing an elevation map. Each element of the array corresponds to the height of a bar, and the width of each bar is assumed to be 1 unit. Visualizing this problem is crucial for understanding how rainwater can accumulate between the bars. Below is an image and example provided by LeetCode illustrating the problem:

![](https://github.com/Dieg0Lir4/Implementation-of-Computational-Methods-Evidence-4/blob/main/leetcodeExample.jpg)

Example:
* Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
* Output: 6
* Explanation: The configuration represented by the array can trap 6 units of rainwater.

Constraints:
* The maximum number of bars is 20,000.
* The maximum height of a bar is 100,000 units.

Note: I change the constraint of maximum height of a bar to 5 units and the amount of bars to 100,000, with the purpose to make a better test field to see the power of parallem programing.


I chose this problem not only because it is challenging but also highly relevant. It demonstrates how different spaces in the same map can have different results and how these differences can affect other spaces. For example, the problem illustrates how varying elevations can lead to different amounts of trapped water, similar to how real world terrains affect water accumulation. Additionally, this problem has various ways of solving it, making it a great test for parallel programming and sequential programming. 

Link to the problem: https://leetcode.com/problems/trapping-rain-water/description/

## Models:

Here are some diagrams of the problem:

![](https://github.com/Dieg0Lir4/Implementation-of-Computational-Methods-Evidence-4/blob/main/diagrama1.jpg)

The topmost row in the diagram represents the array of 100,000 elements. For simplicity, only the first few elements are shown. The diagram shows how the array elements are divided among threads and blocks. Each block (indicated by blockIdx.x) contains 32 threads (indicated by threadIdx.x). Threads within each block are responsible for processing specific elements of the array. Each thread in a block has a unique index (threadIdx.x), ranging from 0 to 31 and each block has a unique index (blockIdx.x).
For this solution you have an array of heights, and you want to compute the maximum height to the left and right of each element. This computation will be performed in parallel using CUDA.

![](https://github.com/Dieg0Lir4/Implementation-of-Computational-Methods-Evidence-4/blob/main/diagrama2.jpg)

The logic is quite simple. The array of heights is divided among multiple blocks and threads. Each thread is responsible for processing a specific element of the array. Each thread calculates its global index idx (a index in the problem’s array that only that thread has)

The diagram helps visualize how the array elements are mapped to threads and blocks. Each thread processes a specific element of the array to compute the left and right maximums in parallel, to then calculate the water that can be trapped in it. By dividing the work among multiple threads and blocks we make this problem way faster,  since we don't have to wait for other spaces to finish calculating their bars and water trapped.  By this approach parallel programming does a better job in large arrays like the one with 100,000 elements in this problem. 


## Implementation:

For the implementation I decided to use Google Colab since it's a free cloud service that provides GPU access and supports CUDA. That way there is no need to download and install cuda since Google Colab comes with several tools and libraries preinstalled, which include the necessary components for using CUDA, this allows you to write and execute CUDA code without needing to install CUDA manually and needing a NVIDIA GPU.

Here is the link to the google colab with my implementation: https://colab.research.google.com/drive/1uOyP-YfDH1NYUXzWE5uO0To_7b0UjXws?usp=s


NOTE: Make sure that your google colab account still have quote to connect to a GPU
* Click on "Runtime" in the top menu.
* Select "Change runtime type".
* In the "Hardware accelerator" dropdown, select "GPU".

NOTE: If you have a NVIDIA GPU, there is the code file in the repository if you want to try it locally


## Test:

In the google colab at the end of the notebook there is a part where the tests are, there are some arrays written directly into the code with the answer that should be printed.

Here are the arrays with the answer expected

* height = [0,1,0,2,1,0,1,3,2,1,2,1]
* output = 6

* height = [0,1,0,2,1,0,1,3,2,1,2,1]
* output = 9

* height = [2, 0, 2]
* Output: 2

* height = [3, 0, 1, 3, 0, 5]
* Output: 8


Have in mind that this test are only to show that the code works as expected, but due to the little amount of elements in the arrays there is no way to see the speed of parallem programming.

LINK to the google colab: https://colab.research.google.com/drive/1uOyP-YfDH1NYUXzWE5uO0To_7b0UjXws?usp=sharing

if that links is not working due to google colab's quote of time using GPU overuse try this one:
https://colab.research.google.com/drive/16qEzjSuVciS4hQMqwpwMaHLHwj9nZFf9?usp=sharing

if not able to use GPU use another account of google that hasn't use GPU.
Also



## Analysis:

Let's compare this solution with doing it by doing it sequentially with only one thread


```
%%writefile sequntial_trap_water.cpp
#include <iostream>
#include <vector>
#include <chrono>
#include <random>
using namespace std;

class Solution {
public:
    // Function to get the max height of the left wall
    int maxLeftWall(vector<int> &height, int i) {
        int max = height[i - 1];
        for (int j = 0; j < i; ++j) {
            if (height[j] > max) {
                max = height[j];
            }
        }
        return max;
    }

    // Function to get the max height of the right wall
    int maxRightWall(vector<int> &height, int i) {
        int max = height[i + 1];
        for (int j = i + 1; j < height.size(); ++j) {
            if (height[j] > max) {
                max = height[j];
            }
        }
        return max;
    }

    // Function to get the min height of the two walls
    int min(int a, int b) {
        return a < b ? a : b;
    }

    int trap(vector<int> &height) {
        // Get the number of elements in the vector
        int n = height.size();

        // Create variable for water trapped
        int water_trapped = 0;

        // Check if the at least space for two walls and space for water
        if (n < 3) return 0;

        // Loop for each position except first and last
        for (int i = 1; i < n-1; ++i) {
            // Call the function to get the height of its max right and left walls
            int leftWall = maxLeftWall(height, i);
            int rightWall = maxRightWall(height, i);

            // Get the minimum height of the two walls
            int minWall = min(leftWall, rightWall);

            // Calculate if the current wall is less than the minimum wall
            if (height[i] < minWall) {
                // Add the difference between the minimum wall and the current wall
                // to the water trapped
                water_trapped += minWall - height[i];
            }
        }

        return water_trapped;
    }
};
```

this is code does the same as the parallem, but here the diference is that in only has one thread and to past to the next position it has to calculate their max bars and its water trapped. Meanwhile in parallem you have multiple threads doing different position at the same time. So seen by time complexity the sequential way has a time complexity of O(n^2), that is because the thread has a loop to past to every position and has a nexted loop to check every position to its left and right. Meanwhile thanks to parallem programing each thread only havt to worry about the loop to check every position to its left and right. Making it a time complexity of 0(n).

Here is the link to the colab that have the sequntial code: https://colab.research.google.com/drive/1HzPf6cFma4sFE63JUJr9_timPwV7JOVv?usp=sharing

When I did the comparison it took 432167 miliseconds (43 seconds) using the sequential mode and 32 miliseconds using parallem programing

The approch use to solve this problems is not best for sequentialy, I use this approch to solve it in leetcode and eventhough it past all of the cases due that it took so much time leetcode rejected my solution. Thankfull due to how parallem programing works it makes using this approch way more efficient.

