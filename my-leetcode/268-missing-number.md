# 268. Missing Number

---


# Problem 

## Tags: 
#sorted-array

**Link:** https://leetcode.com/problems/missing-number/description/

**Problem Text:**   

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.  

 
Example 1:  
Input: nums = [3,0,1]  
Output: 2  
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.  

Example 2:  
Input: nums = [0,1]  
Output: 2  
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums. 

Example 3: 
Input: nums = [9,6,4,2,3,5,7,0,1] 
Output: 8  
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums. 
 


---

# Scratch Pad/ Pseudocode

// so there are x numbers of array.length
// we want to get the one that 1 missing between 0 and array.length
// so we would want to sort
// as we iterate through we add one and if i[n+1] does not equal to to that
// number its the one that misisng
// i can do that subtraction check since we only dealing with positive numbers

## Ask what are the constraints:
n == nums.length
1 <= n <= 104
0 <= nums[i] <= n
All the numbers of nums are unique.

// numbers all start from 0 - they're all positive which is why I can use that subtraction to check if they're in order 
// since it starts with 0, and array can be of size 1- this means if we only have one element, the missing num would be 1 
// ^ I should have read that first, but it took me a failed test case to realize oh oops, yeah- what if there was only one array element. This is a great reminder on why it's so important to carefully read the constraints. 


---

# Java Solution

```
class Solution {
    public int missingNumber(int[] nums) {

        int missingNum = 0;
        int lastIndex = nums.length - 1;
        Arrays.sort(nums);
    

        for (int i = 0; i < nums.length; i++) {
        
            if (i <= nums.length - 2) {

                if ((nums[i + 1] - nums[i]) > 1) {
                    missingNum = nums[i] + 1;
                    break;
                }
            } else if (i == lastIndex) {
                if (nums[i] < nums.length) {
                    missingNum = nums.length;
                }
            }
            else if (nums.length == 1) {
                missingNum = 1; 
            }
        }
        return missingNum;
    }
}
```
We initialize the `missingNum` and also just for clarity, I make a variable `lastIndex` to represent the last index of the array, so that we can get the last element of the array. The reason for this is when we're at the last element, there is no next element to comapare with- and at that point, we instead must compare it with the length of the array. y the way, I realized that after a failed test case where it said index out of bounds-- and I was like oh yeah- because we can do i+1 when we're already at the end.

For numbers that are not the last element, we can compare to see if they are sequential - and to get those "other" numbers, we would just look for those at index position of i < nums.length-2. B

Also, as mentioned in my Constraints section- and again it's so important to read the constraints because I missed this at first, if there's only one element in the array and we always start at 0, then the missing num will be 1 in those cases.




