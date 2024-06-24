# 704. Binary Search

---

# Problem

Link: https://leetcode.com/problems/binary-search/
Problem Text:
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

Example 1:  
Input: nums = [-1,0,3,5,9,12], target = 9  
Output: 4  
Explanation: 9 exists in nums and its index is 4  

Example 2:  
Input: nums = [-1,0,3,5,9,12], target = 2  
Output: -1  
Explanation: 2 does not exist in nums so return -1  

---

# Java Solution

## Initial Solution

My inital thought was to use a for loop.
We would iterate through each number in the array to find see if the number at that index position matches the target, and if so return the `i`.
If there is no match after iterating through the entire loop, we return -1.

```
class Solution {
   public int search(int[] nums, int target) {
       for (int i = 0; i < nums.length; i++) {
           if (nums[i] == target) {
               return i;
           }
       }
       return -1;
   }
}
```

## Binary Search

Notice however this means looping through the entire array.length which can be not so efficient- just like you don't need to open every drawer to find your socks.
Let's pretend you are a neat freak and you alphabetize your eight drawers, so the drawers stack top down A to Z. You start from the middle drawer, and if you see hoodies, then you know you go to search drawers further down for socks. S goes after H.
Now imagine the time saved from doing that vs opening all your drawers.

That is the general idea behind binary search. Note: binary search only works with sorted arrays.

Here is the solution with binary search.

```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1; // because arrays start at 0

        while (left <= right) {
            int middle = (left + right) / 2;
            if (nums[middle] == target) {
                return middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                right = middle - 1;
            }
        }
        return -1;
    }
}
```

We initialize with the following index values: `left` value of 0- the start of the array, and the `right` is the end of the array.
We divide those two values to find the `middle`, and `int` helps to round that number down to an integer data type.
Then, while the left side of where we are searching is less than or equal to to the right, we will check:

- Did we find the target? Are my socks there?
  If no...
  - We check to see if the middle is less than the target, and if so, we got to search further up. Therefore, the `left` value becomes the one higher than the middle. This allows us to search the upper half.
  - Or... if the middle is greater than the target, then we have aimed too high and we got to search further down, so the `right` becomes one less than the middle. This allows us to search the lower half.


## Example Breakdown 
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4

There are 6 numbers in the array.
1. We start with an index value of `left: 0` and `right: 5`. The `middle` is int of 2.5 or 2. 
At index 2, we have the number 3.
3 is less than the target of 9, so we go to search higher.

2. Now, we have `left: 3` and `right: 5`.  The `middle` becomes 4. 
At index 4, we have the number 9 which is equal to 9. 


---

# Javascript Solution

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

const search = (nums, target) => {
    let left = 0;
    let right = nums.length - 1; // because arrays start at 0

    while (left <= right) {
        let middle = Math.floor((left + right) / 2);
        if (nums[middle] === target) {
            return middle;
        } else if (nums[middle] < target) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }
    return -1;
};

```
We use an arrow function to solve it in Javascript with the parameters in parenthesis. 
In Javascript, remember `let` is used when a variable's value can change. In this case, `left` and `right` does change as we search. 

Also, note the comments. @param is used to annotate the parameters. The stuff in brackets? That's to show the data type. For example, `{number[]} nums` is sayng the variable `nums` is of data type array of numbers. My two cents is Leetcode probably does this because Java is strictly typed, vs Javascript is a free spirit kind of guy, so we have to put in comments the context. 