# 2441. Largest Positive Integer That Exists With Its Negative

---


# Problem 

## Tags: 
#hashset, #arraylist

**Link:** https://leetcode.com/problems/largest-positive-integer-that-exists-with-its-negative/description/

**Problem Text:**   

Given an integer array nums that does not contain any zeros, find the largest positive integer k such that -k also exists in the array.

Return the positive integer k. If there is no such integer, return -1.

Example 1:  
Input: nums = [-1,2,-3,3]  
Output: 3  
Explanation: 3 is the only valid k we can find in the array.  

Example 2:  
Input: nums = [-1,10,6,7,-7,1]  
Output: 7  
Explanation: Both 1 and 7 have their corresponding negative values in the array. 7 has a larger value. 

Example 3:  
Input: nums = [-10,8,6,7,-2,-3]  
Output: -1 
Explanation: There is no a single valid k, we return -1.  

---

# Scratch Pad/ Pseudocode

// nums does not contain any 0  
// k = largest positive integer, where -k also exist  
// we are on the 10  
// we check if the set .contains(the negative of it )  
// if so - then we woudl update k = num  
// if not then we continue to move to num on the left and repeat that check  
// memory to save time 

## Ask what are the constraints:
1 <= nums.length <= 1000
-1000 <= nums[i] <= 1000
nums[i] != 0

## What clarifying questions do I have:


## Edge Case(s)
//edge case for if array null  
//edge case if there were no negatives- before the for loop    
The above two are handled by the constraints, however it is good to think of edge cases. For instance, if there 


## Initial Approach
I started out with a bit of a naive approach with arrays.
I knew this was linear and then I was like ok, convert the array of nums into all positives and then sort it. The highest one at the top and then just check if one to its left matches..but edge case there is what if the one to the left wasn't a negative converted to positive- thus leading to false positive answers. 

```
...
if (num < 0 ) {
                num * -1
            }
        }

        Arrays.sort(nums)
        for (i = lastIndex; i >0 ; i --) {
            num we are on is 10
            we check the one before it to see if they are the same number 
            if so then we return that on 

            else 
            ...
}
```


---

# Java Solution

```
class Solution {
    public int findMaxK(int[] nums) {
        int k = -1; // let's just initialize it with the false value

        HashSet<Integer> trackingSet = new HashSet<>();
        List<Integer> positiveList = new ArrayList<>();
        // Arrays.sort(num)
        for (int num : nums) { // if nums < 0 {write those nums to a set)
            if (num < 0) {
                trackingSet.add(num);

            } else { // if nums > 0 {add it to a ArrayList}
                positiveList.add(num);
            }
        }

        Collections.sort(positiveList, (a, b) -> a - b);

        for (int i = positiveList.size() - 1; i >= 0; i--) {
            int negativeNum = positiveList.get(i) * -1;
            if (trackingSet.contains(negativeNum)) {
                k = positiveList.get(i);
                break;
            }

        }

        return k;

    }
}
```


Here is the approach and I'm smiling writing this thinking about my awesome mushroom friend Sal who gave me pointers. He, among others, make my week, especially when it storms. 

1. We start off by initializing `k`. We initialize it with the value it would have if "false"- or -1.

2. We use a `trackingSet` (hashset) to hold just the negative numbers of the array, and the `positiveList` (arraylist) to hold just the positives. We can write the array of `nums` to both at the same time. When iterating through `nums`, if it's a negative number, we can add it to the set, else add it to the `positiveList`.


3. We then sort the `positiveList` with `Collections.sort` in ascending order so that the highest numbers are at the end. 

4. Then, we can just iterate through the `positiveList` in reverse order as we chose to sort our list by ascending. This is because highest numbers would be at the end. Then, we check each positive number that we iterate over in the list against the `trackingSet` to see if the set `.contains` the negative version of that number. If so, then we can update the value of k to that number. 

5. We want to make sure to `break` as soon as we find that first match for "k" so it doesn't keep iterating.

6. We know if we iterated through the whole list and no match was found, then k would not be updated and  would retain its initial value of -1. Our last line says `return k`. So that would either return the updated value of k if we found that largest positive integer that exists with its negative or -1 if not found. 


---


# What I Learned
- By initializing the tracking variable  `k` with the value that it would have when false, then we can return `k` at the end without having to use an if statement. That is because in the case we find a "largest positive integer that exists with its negative", then we can simply update the value of k. 

- In Step 2 above, we once again revisit the concept of how can we just do it in "one pass". We iterate through the array in one pass to write to both the set for negative numbers, and a list for positive numbers. 
