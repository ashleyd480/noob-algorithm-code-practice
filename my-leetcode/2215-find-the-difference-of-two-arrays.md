# 2215. Find the Difference of Two Arrays

---


# Problem 

## Tags: 
#hashset, #boolean-flag 

**Link:**  https://leetcode.com/problems/find-the-difference-of-two-arrays/description/?envType=study-plan-v2&envId=leetcode-75

**Problem Text:**   

Given two 0-indexed integer arrays nums1 and nums2, return a list answer of size 2 where:  

answer[0] is a list of all distinct integers in nums1 which are not present in nums2.  
answer[1] is a list of all distinct integers in nums2 which are not present in nums1. 
Note that the integers in the lists may be returned in any order.  

Example 1:  
Input: nums1 = [1,2,3], nums2 = [2,4,6]  
Output: [[1,3],[4,6]]  
Explanation:  
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3]. 
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6]. 

Example 2:  
Input: nums1 = [1,2,3,3], nums2 = [1,1,2,2]  
Output: [[3],[]]  

Explanation:  
For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3]. 
Every integer in nums2 is present in nums1. Therefore, answer[1] = [].  


---

# Scratch Pad/ Pseudocode

// 2 arrays : nums1 nums2
// {answer0 list , answer1 list }
// answer 0 contain all distinct ints in nums1 not in nums 2
// answer 1 all distinct ints in nums2 not in nums 1
initialize InnerList1 = Set<>  // this starts as empty set and we .add numbers but this way we dont have to check for duplicate as set only allow you toadd ounique numbers
initialize InnerList2 = Set <>



## Ask what are the constraints:
1 <= nums1.length, nums2.length <= 1000
-1000 <= nums1[i], nums2[i] <= 1000



## Initial Approach

```
//doing it at the same time
for (// fitler through nums 1) {
    boolean innerList2NotDistinct = false;
    boolean innerList1NotDistinct = false;


    // check inner boudns 
    for (// fitler through nums2)


    if (nums1[i] == nums2[j] ){
       innerList1NotDistinct = true;
    }
    if (nums2[i] == nums1[j]) {
        innerList2NotDistinct= true; 
    }
    if (nums1.length== j && innerList1NotDistinct == false) // means we started with value false and the other 2 if weren't "hit" so no numbers were equal {
        // add to first set - innerList1.add(nums1[i])
    }
     if (nums2.length== i && innerList1NotDistinct == false) // means we started with value false and the other 2 if weren't "hit" so no numbers were equal {
        // add to first set - innerList1.add(nums1[i])
    }

   else if (//j at the end - then we know we've iterated through whole loop) {
    InnerList1.add(nums1[i])
}
      

// do the same as above for List 2
....
InnerList2.add(nums1[i])


//duplicates

        return List<
        
    }
}
```

1. In this initial approach, we focus on iterating through the longer of the two arrays, nums1 and nums2, to avoid going out of bounds. (Note:  If you were to iterate through the shorter array and try to access elements of the longer array, you might run into out-of-bounds errors because the shorter array wouldn't cover all elements of the longer one.)

2. We use boolean flags, innerList1NotDistinct and innerList2NotDistinct, to check whether the current number we are iterating over in nums1 and nums2 is found in the other array. 

3. ^ These boolean flags are initialized to false, indicating that we assume the number is unique unless proven otherwise. As we compare each number from nums1 with all numbers in nums2, if we find a match, the corresponding flag is set to true, meaning the number is not unique.

4. For each element num1[i] in the longer array (nums1), start an inner loop to compare it with all elements in the shorter array (nums2). If, after checking all possibilities, the flag remains false, it indicates that the number was not found in the other array, and we add it to the result list for nums1, specifically `answers[0]`. The same logic applies when checking nums2 against nums1, where unique numbers from nums2 are added to `answers[1]`.

^ This method ensures that only distinct elements from each array are captured in the respective lists, providing us with the unique elements from nums1 and nums2.


---

# Java Solution


```
class Solution {
    public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        // initialize new hashset
        HashSet<Integer> set1 = new HashSet<>();
        HashSet<Integer> set2 = new HashSet<>();

        // Fill hashsets with elements from nums1 and nums2
        for (int num : nums1)
            set1.add(num);
        for (int num : nums2)
            set2.add(num);

        List<Integer> nonDuplicatedList1 = new ArrayList<>();
        List<Integer> nonDuplicatedList2 = new ArrayList<>();

        // Check unique elements in nums1 that are not in nums2
        for (int num : set1) {
            if (!set2.contains(num)) {
                nonDuplicatedList1.add(num);
            }
        }

        // Check unique elements in nums2 that are not in nums1
        for (int num : set2) {
            if (!set1.contains(num)) {
                nonDuplicatedList2.add(num);
            }
        }

        // Combine the two lists into one
        List<List<Integer>> combinedList = new ArrayList<>();
        combinedList.add(nonDuplicatedList1);
        combinedList.add(nonDuplicatedList2);

        return combinedList;
    }

}
```
Here is the approach: 
1. We use two hashsets (`set1` and `set2`) to write the unique values of the of the two arrays `num1` and `num2`. Eseentially for each array, we iterate through each num and `.add` it to an array. `.add` will only allow unique values.

2. Then, we iterate through each set, first through `set1` and if `set2` does not contain that number, that means that number in `nums1` is distinct so we can `.add` it to `nonDuplicatedList1`.

3.  The same goes for `set2`  , except we would check it against `set1`- and then we would know that number in `nums2` is distinct if it's not `.contain` in `set1`. Those distinct numbers would be added to `nonDuplicatedList2`. 

4. Then, we can just combine the two lists. 


---


# What I Learned
I also learned this ternary way to check longer length for the inital approach from my mushroom friend Sal who is also like another one of those 

`longerLength = nums1.length > nums2.lenght ? nums1.length : nums2.length`
`longerLength` is a boolean. 

