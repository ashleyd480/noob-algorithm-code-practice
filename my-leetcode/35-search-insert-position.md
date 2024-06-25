# 35. Search Insert Position


---


# Problem 


**Link:** https://leetcode.com/problems/search-insert-position/description/  

**Problem Text:**   
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.  


Example 1:  
Input: nums = [1,3,5,6], target = 5  
Output: 2  

Example 2:  
Input: nums = [1,3,5,6], target = 2  
Output: 1  

Example 3:  
Input: nums = [1,3,5,6], target = 7  
Output: 4  


---

# Scratch Pad/ Pseudocode

// Let's try the simple way first. The good ol' loop de loop.  
// So [1, 3, 5, 6, 7] and we are looking for 2.  
// If we were to have to have found it, we'd return the index where it was found  
// But.... is 1 = 2, no --> So 3 = 2, no  
// assuming we loop through it, and we don't find the number, then we got to figure out where do we want to insert the number.  
// But wait, we would want to insert it after 1.   
// So let's do is 1 <=2? - yeah- so insert it after 1.  
// Ok, so this means we insert it at index 1- which is the number 3, and 3 is what?  
// 3 is greater than the target mmm...  
// which means .. if `nums[i]` == target or if it's `>` like in our example above... then we log `i`.  


## Initial Solution
Ok, so we love loops am I right? 
So, let's try it that way first.

```
class Solution {
   public int searchInsert(int[] nums, int target) {
       for (int i = 0; i < nums.length; i++) {
           if (nums[i] >= target) {
               return i;
           }
       }
       return nums.length;
   }
}
```

## Edge Case(s)
Alright, so if we iterated through all of it and didn't find it- you would know go down one door right? 
For example- `nums = [1,3,5,6], target = 7` - the output was 4. You knew none of those other doors would open up for you. So you'd go one door down aka the length of the array which would represent the nums[i] of last "door" + 1. 

---

# Java Solution

## Binary Search
Our friend Binary Search is back. Remember we can use binary search because hint Leetcode says it's a sorted array. 
Yay- we love neat freaks that sort everything out so it makes stuff easier to find. 

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int middle = (left + right) / 2;
            if (nums[middle] == target) {
                return middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else if (nums[middle] > target) {
                right = middle - 1;
            }

        }
    return left;
    }
}
```

The first chunk of code is the standard code for a general binary search. If the middle is less than the target, search the upper half and vice versa. You may refer to my detailed explanation [here](https://github.com/ashleyd480/noob-algorithm-code-practice/blob/master/my-leetcode/704-binary-search.md).

But... what if we don't find the number. 
Let's walk through this example.
`Input: nums = [1,3,5,6], target = 7`

1. Ok, so we start at index 1 which is the `middle`.  
The number is 3, and 3 is less than 7. We got to go higher up.  
`left` becomes index 2 and `right` is still index 3.    

2. Now, we are at index 2 for the `middle`.   
The number if 5, and 5 is less than 7...  
`left` becomes index 3 and `right` is still index 3.  


3. Our `middle` is index 3, which is 6 which is less than 7. That means we increase the left by 1, so it becomes index 4.  

4. Hold up! So our `left` and `right` were equal in step 2 and 3. So we can't go any further right? That's because the while loop is only for when `left <= right`. So.. we return `left` or index 4.   


---

# Javascript Solution

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
const searchInsert = (nums, target) => {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right) {
        let middle = Math.floor((left + right) / 2);
        if (nums[middle] === target) {
            return middle;
        } else if (nums[middle] < target) {
            left = middle + 1;
        }
        else {
            right = middle - 1;
        }
    }
    return left;
}
```
