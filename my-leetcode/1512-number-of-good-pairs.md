# 1512. Number of Good Pairs
---


# Problem 

## Tags: 
#hashmap

**Link:** https://leetcode.com/problems/number-of-good-pairs/description/

**Problem Text:**   

Given an array of integers nums, return the number of good pairs.  

A pair (i, j) is called good if nums[i] == nums[j] and i < j.  


Example 1:  
Input: nums = [1,2,3,1,1,3]  
Output: 4  
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.

Example 2:  
Input: nums = [1,1,1,1]    
Output: 6    
Explanation: Each pair in the array are good.    

Example 3:  
Input: nums = [1,2,3]  
Output: 0  
 

---

# Scratch Pad/ Pseudocode

// it's a good pair if index i to left of j  
// AND nums[i] has to equal nums[j]  
// so are we just essentially looking for how many numbers in the array have count of 2 or more  
// my thought of a naive approach is a nested for loop where we go through each  
// num and then we can increment the count each time we see another num that is alike  
// however that is worse than linear because nested loop  
// how about we do hashmap   
// so we iterate through the array and we keep count and if that num has > 2 and is even we just divide by 2 to get number of pairs   
// we know if the count of each is 1 then no pairs   
// we want to iterate up until we reach the 2nd to last index since we need pairs   
// the indices can be the keys and value would be at that key- how many other nums  

## Ask what are the constraints:
1 <= nums.length <= 100
1 <= nums[i] <= 100


---

# Java Solution

```
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int numGoodPairs = 0;
        HashMap <Integer, Integer> trackingGoodMap = new HashMap <> ();
        for (int num : nums) {
            if (!trackingGoodMap.containsKey(num)) {
                trackingGoodMap.put(num,1);
            }
            else {
                trackingGoodMap.put(num,trackingGoodMap.get(num)+1);
            }

        }

        for (Integer value: trackingGoodMap.values()) {
            if (value >= 2) {
                int incrementalGoodPair = value * (value-1)/2;
                numGoodPairs += incrementalGoodPair;
            }
        }

        return numGoodPairs;
    }

}

```

Here is the approach: 
1. Since I know we need to return number of good pairs, I initialize a variable called `numGoodPairs` with 0.

2. I also initialize a hashmap `trackingGoodMap` which tracks the number and its count. We know that a good pair is possible if the number appears at least twice. 

3. We iterate through the array keeping track. If we've not seen the `num`- we add the new key of `num` and a value count of 1. If we have seen it, then we update the value count of that key `num` by 1. 

4. Once we have those values, we need a way to find how many pairs can we get. To do that, we want to examine the examples that Leetcode provided:
- In Leetcode's first provided example- you have 3 one's - if you draw that out, the first 1, would be able to pair with the 2nd 1 and 3rd 1. Then, with the second 1, we can pair that with 3rd one. That makes 3 pairs. 
- Similarly for Example 2, with the four 1's. The first 1 can pair with second, third, or fourth 1. The second 1 can pair with third or fourth 1. The third one can pair with fourth. That's 6 total - or 4*3 /2 . 
- ^So we see how its equal to the number of times the num appears multipled by 2 and then divided by 2 to get numbers of pairs. `n * (n – 1) // 2`. (And yes, I admit I found that from the Leetcode hint, but later I did learn that it's related a combination formula in mathetmatics, which is a formula for number of wayts to choose 2 items from set of n items.)

5. We know if the value count of the `num` key is > 2, then we can find the `incrementalGoodPair` that that `num` can generate with the formula we found in Step 4. We can then add that `incrementalGoodPair` amount to the `numGoodPairs` as we iterate through each `num` that has a value count > 2. 

6. Lastly, return `numGoodPairs`.

