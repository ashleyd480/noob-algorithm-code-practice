# 905. Sort Array By Parity


---


# Problem 

## Tags: 
#sorted-array, #for-each, #const, #concat, #two-pointer

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

## Attenpt 1

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

## Attempt 2
Here is the way using two pointers.
We use the two pointers technique when we want to swap numbers, such as in this case to swap even and odd. 
With two pointers, we want to define our indices to represent left and right, and use the `while (left < right)` (we don't want to swap if we're on the same number). 
Then, we want to think of subproblem which is when do want to move our pointers, and when do we want to swap. When we swap, we also want to ensure that we're 

```
class Solution {
    public int[] sortArrayByParity(int[] nums) {
        // to represent indicees

        int left = 0;
        int right = nums.length - 1;

        while (left < right) { // no need to swap while equal
            
            // with 2 pointer we want to think when we want to move forward
            // Move left pointer forward if it's on an even number
            if (nums[left] % 2 == 0) {
                left++;
            }

            // Move right pointer backward if it's on an odd number
            if (nums[right] % 2 != 0) {
                right--;
            }

            // If left is odd and right is even, swap them
            else if (nums[left] % 2 != 0 && nums[right] % 2 == 0) {
                int temp = nums[left]; // swap values 1
                // when we swap just use the indices
                nums[left] = nums[right];
                nums[right] = temp;
                // because we swapped, we move both pointers down to see next two numbers 

                left++;
                right--;

            }

        }
        return nums;

    }
}
```

### What I Learned 

With two pointers we just want to make sure that we use the indices versus assigning the num at an indices to a variable.  
For example, "incorrect" way would be putting this in the beginning of the while loop:
```
int leftNum = nums[left];
int rightNum = nums[right];

```
Let's say we put this at the beginning of the while loop and let's say we're working with [0, 1, 2]
Now in our swap else statement, let's assume we wrote:

```
int placeholderNum = leftNum; // 0
nums[left] = rightNum; // assign value 2
nums[right] = placeholderNum; // 0
```

Because left 0 is even, our left indice would increase, however in our code we did NOT update the value of leftNum to say `leftNum = nums[left]`, so `leftNum` is still the initial value from before the for loop.
Thus, our leftNum would still be 0. `nums` at new left index position would be assigned value fo 2. Then, the `nums` and index position right would be assigned the placeholder which holds values of `leftNum` so we now have [0,1,0].

Had we also made sure to update the value of `leftNum` and `rightNum` with each poinnter movement, that would have worked- but that's just another extra step to remember!

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