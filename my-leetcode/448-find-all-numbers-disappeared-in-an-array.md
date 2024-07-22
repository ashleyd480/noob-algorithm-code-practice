# 448. Find All Numbers Disappeared in an Array


---


# Problem 

## Tags: 
#set #array-list

**Link:** https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/

**Problem Text:**   

Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.  

 

Example 1:  
Input: nums = [4,3,2,7,8,2,3,1]  
Output: [5,6]  

Example 2:  
Input: nums = [1,1]  
Output: [2]  


---

# Scratch Pad/ Pseudocode

// return a list of numbers that do not appear in array of nums  
// nums can be 1 number long  
// naive solution would be sorting and then going through each  
// if next - current > 1- then we found missing one - but only issue is there 
// can be duplicates  

// n the max number is list.size  
// we would want to sort our array first since we can't sort hashamp and then  
// write that to a hashmap  
// so that would be a hashmap where we can .put the number if it's not contained  
// and its count  
// the number would be the key  
// and another list to see if it .contains by checking against that list 
// we would iterate through that list adding + 1 to a number until it gets up  
// oh or another idea is to get the count and if its key.value = 0 , then it means it's missing   
// or wait could i use hashset instead and see if a number is contained in that  
// set - also hashset don't allow duplicates so we could iterate through and do .add  



## Ask what are the constraints:
n == nums.length  
1 <= n <= 105  
1 <= nums[i] <= n 


---

# Java Solution

```
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int num = 0;
        HashSet<Integer> numSet = new HashSet<>();
        while (num < nums.length) {
            num++;
            numSet.add(num);

        }

        System.out.println(numSet);

        for (int i = 0; i < nums.length; i++) {
            int currentNum = nums[i];
            System.out.println(currentNum);
            if (numSet.contains(currentNum)) {
                numSet.remove(currentNum); // remove the ones that are found so the ones left are the numbers not in array 
            }

        }

        List<Integer> notInRangeNums = new ArrayList<>(numSet);
        return notInRangeNums;
    }

}
```

## Time Complexity 
While we still have to iterate through the nums array, in the inner code, we use the faster lookup of hashset to see if the set contains that number. 


