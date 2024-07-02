1. Two Sum
---

# Problem

## Tags:
#array #hashmap #set


**Link:** https://leetcode.com/problems/sorting-the-sentence/

**Problem Text:**    
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

Example 1:  

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].  

Example 2:  
Input: nums = [3,2,4], target = 6  
Output: [1,2]  

Example 3:  
Input: nums = [3,3], target = 6  
Output: [0,1]  


---

# Scratch Pad/ Pseudocode

// When we look at [2, 7, 11, 15]- my thought is we want to go through each number, say start at 2 and then add it to the next number 7, then 11, then 15
// Then, we do 7, and add it to 11, 15, etc 

My pseudocode as follows:
One thing to note is we can't use the same variable `i` due to scoping issues, so in the second nested for loop- we use the variable `j`. 

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // look through nums array and see which one two add up to target
        for (int i = 0; i < nums.length; i++) {
            int outerValue = nums[i];
            for (int j = i + 1; j < nums.length; j++) {
        }
           return null;
    }
```

So we iterate 4 times through the array in first 4 loop- and for each of the 4 times we iterate: 
// The first iteration, we iterate through 3 in `j`- excluding the first element since we can't use the same element.
// Next, we start with i=1, and then iterate through the array for a match but we start at j=2, meaning we iterate through 2 elements.  
// Third iteration, we iterate through 1 element.
// Fourth iteration, `j` becomes 4 which is no longer less than `nums.length` of 4. 


## Initial Thoughts
3 + 2  + 1 means 6 iterations in total.   
You can image that this means that as the array size increases, the number of iterations would increase. Hence, it is O(n^2) complexity. However, the problem challenges us to solve it more efficiently.

Also, Leo (our mentor teacher) showed us a tip that we are to do s.out and to test as we write our code to see if we can pass the tests with the "naive" solution first. If that can pass (which in Leo's class)- he showed how due to the increasing time complexity growing as the array size grows- our nested for loop was not able to be timely enough to pass.

## Edge Case(s)
I don't see any edge case, and Leetcode mentions that we are to ensure that we can't use the same element. 

---

# Java Solution

## Attempt 1: 
Based on the above, Leo (mentor teacher) showed us some approaches that can be more efficient than a nested for loop. 
The first approach is to use a "set". HashSets allow you to directly check whether it contains an item, without having to iterating through all objects. Yeah, imagine not having to check all your socks for a match and magically you know where the matching pair is hidden. 


Thus, a pseudocode attempt of a set is as follows:

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        allNums.add(nums[i]);
    }
    for (int i = 0; i < nums.length; i++) {
    //
    }
```

From here, we can call `.contains` to see if the set contains the number- which means say we are at the number 2, we would see if the set contains the number 7 (which is the target - nums[i])...  
However, we realized during the class that we need to also have the index number, which we means we need key value pairs...

## Attempt 2: 
Continuing from the first attempt of hashset, we decided we need not just the value of the number, but also the index position.

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> theMap = new HashMap(); // initialize new hashamp

        for (int i = 0; i < nums.length; i++) {
            theMap.put(nums[i], i); // iterate through array and write the number and index as key-value
        }
        for (int i = 0; i < nums.length; i++) {
            int currentValue = nums[i];
            int needsValue = target - currentValue; // number we searching for to get to target
            if (theMap.containsKey(needsValue)) {
                int[] output = new int[2]; // initialize new array to hold the 2 nums that sum up
                output[0] = theMap.get(needsValue); // we can call .get to get value (index) associated with key 
                output[1] = i;
                if (output[0] != output[1]) {
                    return output;
                }
            }
        }
        return null;
    }

}

```

1. We intializes a new HashMap() that contains key-value pairs of type integer, integer.   
2. Then, we iterate through the array of numbers and write each number and its respective position as key-value pairs.
So why make the number as the key? Well, because we want to ensure that whatever we're looking for is the key. If we're looking for a missing sock, that would be our key.. and in this case- we're looking for the number- that would be our key! The number's index posiiton is the value.
3. Now that we are done with that first iteration which wrote the array data to a hashmap, then we we iterate through the array again. As we iterate through, the number we are on is called `currentValue`. the `needsValue` is the number we are finding that should be paired with `currentValue` in order to sum up to the `target`. 
4. If that `needsValue` key is found, then we initialize an array to hold the `currentValue` and the `needsValue`. However, Leo showed us how when he ran it- a test case failed and it was because `currentValue` was equal to `needsValue` and we are not able to use the same element- hence we added a conditional check that the outputs don't match- and if so then we can return that output array. 


### Time Complexity
We iterate twice through the array as you can see in step 2 and 3 above. This means we iterate n + n times. 
This is more efficient than the nested for loop, due to the hashmap - in the second iteration- we can use the .containsKey which allows us to quickly check if a key (`needsValue`) exists without having to check each number. 



