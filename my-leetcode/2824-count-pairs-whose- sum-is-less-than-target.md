# 2824. Count Pairs Whose Sum is Less than Target

---

# Problem 

## Tags: 
#nested-for-loop

**Link:** https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target/description/

**Problem Text:**   

Given a 0-indexed integer array nums of length n and an integer target, return the number of pairs (i, j) where 0 <= i < j < n and nums[i] + nums[j] < target.  
 

Example 1:  
Input: nums = [-1,1,2,3,1], target = 2  
Output: 3  
Explanation: There are 3 pairs of indices that satisfy the conditions in the statement:  
- (0, 1) since 0 < 1 and nums[0] + nums[1] = 0 < target
- (0, 2) since 0 < 2 and nums[0] + nums[2] = 1 < target 
- (0, 4) since 0 < 4 and nums[0] + nums[4] = 0 < target
Note that (0, 3) is not counted since nums[0] + nums[3] is not strictly less than the target.

Example 2:  
Input: nums = [-6,2,5,-2,-7,-1,3], target = -2  
Output: 10  
Explanation: There are 10 pairs of indices that satisfy the conditions in the statement:  
- (0, 1) since 0 < 1 and nums[0] + nums[1] = -4 < target
- (0, 3) since 0 < 3 and nums[0] + nums[3] = -8 < target
- (0, 4) since 0 < 4 and nums[0] + nums[4] = -13 < target
- (0, 5) since 0 < 5 and nums[0] + nums[5] = -7 < target
- (0, 6) since 0 < 6 and nums[0] + nums[6] = -3 < target
- (1, 4) since 1 < 4 and nums[1] + nums[4] = -5 < target
- (3, 4) since 3 < 4 and nums[3] + nums[4] = -9 < target
- (3, 5) since 3 < 5 and nums[3] + nums[5] = -3 < target
- (4, 5) since 4 < 5 and nums[4] + nums[5] = -8 < target
- (4, 6) since 4 < 6 and nums[4] + nums[6] = -4 < target


---

# Scratch Pad/ Pseudocode

// this reminds me of the two sum problem so i remembered the approach of the for loop  as "easy" way
// length n and target   
// return countOfPairs   
// i < j < array.length     

In this case, a hashamp would not be the most straighforward. Hashmaps are better for problesm where you need to store and quickly look up values or pairs, but this problem invovles counting pair of elements where order of elements is important i.e. i < j.


## Ask what are the constraints:


## What clarifying questions do I have:
Do i an j have to be distinct? Yes, 0 <= i < j < n. Notice how there is no equal sign between i and j which means they've got to be distinct
//[-1,1,2,3,1] and when we iterate through it the second time, b/c i and j have to be distinct - we start at the next index to not include i or j = i + 1 

## Edge Case(s)
n/a


---

# Java Solution

## Attempt 1 

```
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        
        int countOfPairs = 0;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums.get(i) + nums.get(j) < target) {
                    countOfPairs++;
                }
            }

        }
        return countOfPairs;
    }
}

```

We initalize a variable called `countOfPairs` and then as we iterate through the nested loop, we use the if statement to check if nums element at first for-loop array i position + the nums element at nested for-loop array j position is less than the target. If so, we increment  `countOfPairs` by one. I remmber this pattern when we did our Week 4 frontend site and my mentor Chad and I discussed how to increment the number of likes on a post, and from a mistake I made then- I learned to keep the initialized variable outside the loop. 

Ok, so that compiled and ran and Mr. Leet was happy- said it beats 80% or something. 
Note: at first, I was thinking we could have used a hashmap here as well, however as we see it exceed 80% in terms of efficiency, I left it as that for now. Also, hashmap wouldn't really be efficient here after I tried it out and my face was like the "huh cat meme". Hashmaps are used for finding a key typically. In this example we are not trying to find "specific complements or trying to find a single pair that meets a condition", therefore we did not continue with our attempts of hashmap.


## Attempt 2: 

This can also be solved with a two pointer approach, however the constraints do show the array size is small enough that it can be solved via brute fource. Below is how we would do this with two pointer.

1. You would want to go through the array, starting with `left` at index 0. 

2. We would then check for vaild pairs - using `while left < right to ensure we'e searching the valid range. 

3. Then, you would want to initialize the `right` pointer at the end each time. This is because if we simply moved the right pointer to the left each time, we could be missing other valid pairs. Another idea is using a value to track the value of the prior right index. Since it's sorted- if we increase our left pointer which means we're looking at a higher number, we know anything to the right of our current right pointer would result in a sum that is greater than target. However, in this case- we just reset right to the end each time just to keep the code more straightforward. 

4. After, we check pairs with our first left, we want to check our next left and go through that while loop again, checking for valid pairs. This would be repeated, until we've checked all the possibel left ones. 


```
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        Collections.sort(nums);  // Step 1: Sort the array
        int count = 0;           // To count valid pairs

        for (int left = 0; left < nums.size(); left++) {
            int right = nums.size() - 1;  // Step 2: Initialize right pointer at the end

            // Step 3: Iterate with the two pointers
            while (left < right) {
                // Fix the syntax for accessing the elements
                if (nums.get(left) + nums.get(right) >= target) {
                    // If the sum is too high, move the right pointer left
                    right--;
                } else {
                    // If the sum is valid, all pairs (left, left+1) to (left, right) are valid
                    count += right - left;  // Count valid pairs
                    break;  // Break since we found valid pairs for this left index
                }
            }
        }

        return count; // Return the count of valid pairs
    }
}
```