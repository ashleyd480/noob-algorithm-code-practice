# 2475. Number of Unequal Triplets in Array

---


# Problem 

## Tags: 
#nested-for-loop

**Link:** https://leetcode.com/problems/number-of-unequal-triplets-in-array/description/

**Problem Text:**   
You are given a 0-indexed array of positive integers nums. Find the number of triplets (i, j, k) that meet the following conditions:  

0 <= i < j < k < nums.length  
nums[i], nums[j], and nums[k] are pairwise distinct.  
In other words, nums[i] != nums[j], nums[i] != nums[k], and nums[j] != nums[k]. 
Return the number of triplets that meet the conditions.  


Example 1:  
Input: nums = [4,4,2,4,3]  
Output: 3  
Explanation: The following triplets meet the conditions: 
- (0, 2, 4) because 4 != 2 != 3  
- (1, 2, 4) because 4 != 2 != 3  
- (2, 3, 4) because 2 != 4 != 3  
Since there are 3 triplets, we return 3. 
Note that (2, 0, 4) is not a valid triplet because 2 > 0.  

Example 2:  
Input: nums = [1,1,1,1,1]  
Output: 0  
Explanation: No triplets meet the conditions so we return 0. 

---

# Scratch Pad/ Pseudocode
// nums : positive ints
// i, j,k - indices
// how many tiems you can pick 3 numbers from list and they are distinct
// 3 for loop


## Ask what are the constraints:
3 <= nums.length <= 100
1 <= nums[i] <= 1000



---

# Java Solution

```
class Solution {
    public int unequalTriplets(int[] nums) {

        // nums : positive ints
        // i, j,k - indices
        // how many tiems you can pick 3 numbers from list and they are distinct
        // 3 for loop

        int countTriplet = 0;

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] != nums[j]) {
                        if (nums[i] != nums[j] && nums[i] != nums[k] && nums[j] != nums[k]) {
                            countTriplet++;
                        }

                    }
                }
            }

        }
        return countTriplet;
    }
}

```

In the outer loop with index i, we start with our first number say 4 from Example 1. 
Then, we compare it against the next number in the inner loop. We will see if it's distinct or not. The moment we see one that is distinct, then we can go into the third for loop and check next other ones 