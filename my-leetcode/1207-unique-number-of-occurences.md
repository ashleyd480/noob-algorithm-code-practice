# 1207. Unique Number of Occurences 

---


# Problem 

## Tags: 
#hashmap #hashset

**Link:** https://leetcode.com/problems/unique-number-of-occurrences/description/?envType=study-plan-v2&envId=leetcode-75 

 
**Problem Text:**  
Given an array of integers arr, return true if the number of occurrences of each value in the array is unique or false otherwise.  


Example 1:  
Input: arr = [1,2,2,1,1,3]  
Output: true  
Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.  

Example 2:  
Input: arr = [1,2]  
Output: false  

Example 3:  
Input: arr = [-3,0,1,-3,1,1,1,-3,10,0]  
Output: true 

---

# Scratch Pad/ Pseudocode

// int countOfOccurence can not be equal for the number keys   
// initalize hashset   
//  iterating through values of hashmamp  
// for each iteration write vlaue to hashset - so we can se if . 
// adding dulcate wil return false and we can inspect return value 
// we want to look at number of occurences if that is unique  

## Ask what are the constraints:
1 <= arr.length <= 1000 
-1000 <= arr[i] <= 1000 


---

# Java Solution

```
class Solution {
    public boolean uniqueOccurrences(int[] arr) {

        HashMap<Integer, Integer> countMap = new HashMap<>();

        for (int num : arr) {
            countMap.compute(num, (k, v) -> (v == null ? 1 : v + 1));
        }

        HashSet<Integer> countSet = new HashSet<>();

        for (Integer countValue : countMap.values()) {
            System.out.println(countValue);

            if (!countSet.add(countValue)) {
                return false;
            }

        }

        return true;

    }
}
```

Here are the steps:
1. Initialize a HashMap `countMap`.

2. Iterate through the array `arr` and as we iterate through, `num` represents the number at that index. For each `num` we iterate over, we add it to a hashmap with `.compute` where the `num` is the key. The count is computed with an anonymous function that take the `(k,v)` pair - and if that key hasn't been seen yet (it's null), then we set its count to 1, else increase its count by 1.

3. Since we want to check if counts are unique, we can use a HashSet which is a collection of unique elements. My thought is this way as we iterate through the HashMap's values, we can see if we can `.add` the value to the HashSet. If we can't .add it, then it's a duplicate in which case return false and exit the iteration. 

4. If we iterate through all the values without issue, then we know no duplicates and its true. 



