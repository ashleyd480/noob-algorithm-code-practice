# 136. Single Number

---


# Problem 

## Tags: 
#hashmap, #hset

**Link:** https://leetcode.com/problems/single-number/description/

**Problem Text:**   

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

 

Example 1:  
Input: nums = [2,2,1]   
Output: 1 

Example 2:  
Input: nums = [4,1,2,1,2]  
Output: 4  

Example 3:  
Input: nums = [1]  
Output: 1 
 



---

# Scratch Pad/ Pseudocode

// every element appears twice except for one
// i want to know are they sorted? no
// nums can be 1 element as well
// my thought could be sorting the array- if current num and next num are the
// same then it's a dupe, move on to the next
// however that involves swapping stuff which is not ideal
// how about we use a hashmap- where the values are the key and the numbers are


## Ask what are the constraints:

1 <= nums.length <= 3 * 104
-3 * 104 <= nums[i] <= 3 * 104
Each element in the array appears twice except for one element which appears only once.


---

# Java Solution

## Attempt 1

```
 HashMap<Integer, Integer> numMap = new HashMap<>();

        int theSingleNum = 0;

        for (int i = 0; i < nums.length; i++) {
            int currentNum = nums[i];

            if (numMap.containsKey(currentNum)) {
                numMap.put(currentNum, numMap.get(currentNum) + 1);
            } else {
                numMap.put(currentNum, 1);
            }
        }

        for (Map.Entry<Integer, Integer> entry : numMap.entrySet()) {
            if (entry.getValue() == 1) {
                theSingleNum = entry.getKey();
            }

        }
        return theSingleNum;

    }
}
```

Initially I went with a hashmap to keep track of each unique number (key) and its count (value).
I initialize a `singleNum` with a value of 0, and then only if the value of the key (number) is 1 - then that key is the `singleNum`.

We iterate through the array to write the numbers to a map, and this a linear time complexity.

As we iterate through, we check if the hashmaps contains they key with the `currentNum` we are iterating over.
If so, then we increment that key's value by 1. We can update the key's value by 1 by:  `numMap.get(key) + 1`.
However, if the hashmap doesn't yet contain the key- we just set the value of the count to 1. 
One mistake I made was at first just setting it to `numMap.get(currentNum)`but that meant the value of  `currentNum` was never initialized. 
Thus I had to set it as 1 as the inital count the first time we see a number. 

Then, we iterate through the key value pairs and see if the entry we are iterating over- if its value is 1, and if so, we set the `singleNum` value to the key of that entry. This is another linear operation.


## Attempt 2
I was thinking if we're dealing with duplicates, why not use a hashset. 

```
class Solution {
    public int singleNumber(int[] nums) {
        HashSet<Integer> numbersSet = new HashSet<Integer>();
        int singleNum = 0;
        for (int i = 0; i < nums.length; i++) {
            if (numbersSet.add(nums[i])) {
                singleNum = nums[i];

            }

        }
        return singleNum;
    }
}
```

However, this was not correct as for this input [4,1,2,1,2], it output 2.
This is because hashsets aren't able to see the full list of numbers at once. So that's why using the .add to check isn't going to work since it doesn't yet know the full list so it can't check against duplicates. 
So we are able to add 4, 1, and 2 (and we can't add the other 1 and 2).
With each number added the value of `singleNum` is added to the value of that number so that's why we wind up with a value of 2. 

The problem is that 2 is a duplicate and 1 is a duplicate but we had no way of handling that.


## Attempt 3

The next idea is to go through the set and then remove the duplicates. 
We would know if it's a duplicate if we've seen it before. 
We use the `.contains` to check if we've seen it and if so we will remove all of the nums that match that num from the hashset. In this case the 1, 2 will be removed.
This means whatever is left would be the single num.
Just like checking if a line of people are taken and if they're taken- tell them to step aside.
Then, the last one left is the single one... 

```
class Solution {
    public int singleNumber(int[] nums) {
        HashSet<Integer> numbersSet = new HashSet<Integer>();
        int singleNum = 0;
        for (int i = 0; i < nums.length; i++) {
            int currentNum = nums[i];
            if (numbersSet.contains(currentNum)) {
                numbersSet.remove(currentNum);

            } else {
                numbersSet.add(currentNum);
            }

        }
        for (int num : numbersSet) {
            singleNum = num; // iterate through the set to return the num left
        }
        return singleNum;
    }
}
```

To match the required return type of `int` we iterate through the hashset and set the singleNum to the num that is left. 
I also learned we could return use this too `numbersSet.iterator().next();`


## Side Note
Iterator allows you to traverse over the hashset. The .next method returns the next element in the iteration. If there are no more elements, it will throw a NoSuchElementException.

As a side note: I learned that to iterate through a set with iterator- it looks like this:

```
  while (iterator.hasNext()) {
            int number = iterator.next();  // retrieves the single element
            System.out.println(number);    // prints the single element
        }
```

If you think of an Iterator as similar to accessing elements in an array, iterator.next() would indeed start at the first element, analogous to array[0].

The `hasNext` checks to see if there is another element to iterate over. 

Note also HashSet does not maintain any order of elements, and also HashSet does not provide a way to access elements by index. Instead, elements are accessed through iterators or for-loops

