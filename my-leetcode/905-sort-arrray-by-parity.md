# 905. Sort Array By Parity


---


# Problem 

## Tags: 
#sorted-array, #for-each, #const, #concat

**Link:** https://leetcode.com/problems/sort-array-by-parity/description/  

**Problem Text:**   
Given an integer array nums, move all the even integers at the beginning of the array followed by all the odd integers.

Return any array that satisfies this condition.  

 
Example 1:  
Input: nums = [3,1,2,4]  
Output: [2,4,3,1]  
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.  

Example 2:  
Input: nums = [0]  
Output: [0]  



---

# Scratch Pad/ Pseudocode

// need to move even first, then odd  
// my thought is loop through and get the even ones first and then get the odd ones  
// we can initalize a new array to hold these numbers but... if we use an array in Java we would need to know the length which we wouldn't know  
// also with array in java we can't use .add  
// also, at first I just used sortedArray and iterate through nums to check if  
// the num is even or odd, but then there's no way to force put them in order-  
// like add the even first so perhaps two seperate arrays
 

## Edge Case(s)
n/a

---

# Java Solution

```
class Solution {
    public int[] sortArrayByParity(int[] nums) {
    
        List<Integer> evenArray = new ArrayList<>();
        List<Integer> oddArray = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] % 2 == 0) {
                evenArray.add(nums[i]);
            } else {
                oddArray.add(nums[i]);
            }
        }

    
        int[] sortedParityArray = new int[nums.length];

       int index = 0; // had to do this- even with i and for loop with i, this would mean scoping issue with i; this index is to track specifically where the num should go


// Copy even numbers first
  // Copy even numbers first
        for (int i = 0; i < evenArray.size(); i++) {
            sortedParityArray[index++] = evenArray.get(i);
        }
        
        // Then copy odd numbers
        for (int i = 0; i < oddArray.size(); i++) {
            sortedParityArray[index++] = oddArray.get(i);
        }
        
        return sortedParityArray;
    }
}

```
Arrays in Java are immutable, hence we have to use array lists which allow me to add items. We use an even array list and add the even numbers to it.  
Then, we initialize an odd array, and add the odd numbers to it.  I tried to them combine both array lists, however Mr. Leetcode demands that the solution must be of return type array.  
As such,  I had to initalize an array to write the array list stuff back into it, called `sortedParityArray`. I iterated through the even numbers array first, and then the odd numbers array.  
Oh yeah, and note that with array lists, to get the index position, you have to use .get(i). This is how we get the element at that index position (i) from firstly the even array and save it to the `sortedParityArray`.

Another thing I found - yes I made a n00b mistake was using `i` also for my array, however that causes scoping issues as we can't use that same variable for two different places. Therefore, we had to initliaze a variable called `index`. With this, we use `index++` so that with each iteration through the array list, the index position where we are putting that array list element into the array increments too. That's how we put the odds after the evens.  


After I finished my first attempt, I asked chatGPT for idea for how to further improve my solution. It reccomended, that we could use a for-each loop as a cleaner code to iterate through the array lists. 


```
...
        // Add all even numbers first
        for (int num : evenArray) {
            sortedParityArray[index++] = num;
        }
        
        // Add all odd numbers next
        for (int num : oddArray) {
            sortedParityArray[index++] = num;
        }
        
        return sortedParityArray;
    }
}
```

---

# Javascript Solution

In this case, I believe Javascript would have been easier becauase I could just use .push to add

```
/**
 * @param {number[]} nums
 * @return {number[]}
 */
sortArrayByParity = (nums) => {
    const evenArray = [];
    const oddArray = [];
    
    nums.forEach((num) => {
        if (num % 2 === 0) {
            evenArray.push(num);  
        } else {
            oddArray.push(num);  
        }
    });

    const sortedArray = evenArray.concat(oddArray); 
    return sortedArray;
};
```

While writing this, one mistake I made initially was initializing a `sortedArray`that was blank with `const`and then trying to reassign it. And I oop.
`const` in Javascript means you can't change the value. Had I used `let` then I could reassign the value. 