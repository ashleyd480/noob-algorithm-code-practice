# 2824. Count Pairs Whose Sum is Less than Target

---

# Problem 

## Tags: 
#sort, #hashmap, #hashset 


**Link:** https://leetcode.com/problems/contains-duplicate/description/

**Problem Text:**   

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

 

Example 1:  
Input: nums = [1,2,3,1]  
Output: true 

Example 2:  
Input: nums = [1,2,3,4]  
Output: false  

Example 3:  
Input: nums = [1,1,1,3,3,4,3,2,4,2]  
Output: true  


---

# Scratch Pad/ Pseudocode

// if value appear twice--> true 
// if value distinct --> false
// we need a way to count each value - let's initalize a variable 
// we could look at each number as we iterate through and then in a for loop see if it appears again
// basically if the number is equal then we would increment the count 
// but this would be incrementing the total count and not the count per element 

class Solution {
    public boolean containsDuplicate(int[] nums) {
        // int count = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
               
            }
          
        }
         return false;
        
    }
}


// initially had it as count > 2x - because I thought each number had to be a dupe to be true
// however a test case failed [1,2,3,1]- resulted in false when it's true- realized we only have to have at least one value be a dupe 
// but if count >= the number the amount of numbers in the array- this would mean there were at least 2 of one value-- like an array of 4 numbers, one of the number is a duplicate
// oh oops but then because i only increment count in my if statement if the value is found again, but in my else- i probably would need to increment count by 1 if not found (to say we have one of that element) and by 2 if found, and initialize count to 0 
// ok I was overcomplicating it- we simply need to just check if there's a match and return true which means yes we have a dupe 


## Ask what are the constraints:
My inital attempt did not pass due to time limit exceeded. This is because the constraint says the largest array size is 10^5 so maybe the app crashed due to the data size too large and using nested for loop. 

## Edge Case(s)
n/a


---

# Java Solution

## Attempt 1 
After researching some possible other solutions- kudos to https://leetcode.com/problems/contains-duplicate/solutions/3163705/java-best-solution-3-ways, I found that I could still use my for loop to iterate.  
However, to avoid a nested loop, we could simply make just one pass. To do that, we can make sure the numbers are sorted.
Then, as we iterate through- we can check to see if the current number we are on and the number after are duplicates.

```
class Solution {
    public boolean containsDuplicate(int[] nums) {
         Arrays.sort(nums); 
        for (int i = 0; i < nums.length-1; i++) {
                if (nums[i] == nums[i+1]) {
                    return true;
                }
               
            }
               
         return false;
        
    }
}
 ```           

I initially had it as `i < nums.length`, however I got an index out of bounds issue. 
This is because say I have [1, 2, 3, 3], then when I could iterate up to the 4th number - but then there is 5th number to compare it with. 
Hence, I have to put `i < nums.length -1`

## Attempt 2: 
// let's try this instead with a hashamp to keep track of the value and then the 
// we would first want to initialize the hashmap and write the array to a hash
// with a key of the number (which is the one we want to search) - and then value is count
// if the number is seen- we increment its count 
// then we can iterate through the hashmap and see if any keys has a value >=2 and if so we return true, which exits the loop

```
class Solution {
    public boolean containsDuplicate(int[] nums) {

        HashMap<Integer, Integer> numberWithCountMap = new HashMap<>();
     
        for (int i = 0; i < nums.length; i++) {
            Integer numberWeChecking = nums[i];
            if (numberWithCountMap.containsKey(numberWeChecking)) {
                numberWithCountMap.put(numberWeChecking, numberWithCountMap.get(numberWeChecking) + 1);
            } else {
                numberWithCountMap.put(numberWeChecking, 1);
            }
             // Print statement to debug
    // System.out.println("numberWeChecking: " + numberWeChecking + ", timesNumberShowsUp: " + numberWithCountMap.get(numberWeChecking));
    // had to comment out due to it was saying output limit exceeded haha
        }
        for (Integer countValue : numberWithCountMap.values()) {
            if (countValue>= 2 ) {
                return true;
            }

        }

        return false;

    }
}
```

## Attempt 3
I learned about hashset solution. In this case, though hashmap works, we aren't really searching for a key-- 
After I peeked into Leetcode solutions, I saw hashset was reccomended. 
A hashset is a collection of itemes where each item is unique and it handles duplicates.  
It handles duplicates?! Aha, perfect for our problem. 

So here were my thoughts (of course, with some help from my friend ChatGPT)
// initialize a new hashset of data type integer.
// we iterate through the array of nums- and we add each num to the set. 
// the seen number is contained, then return true. 

```
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> seenNumbers = new HashSet<>();
        for (int num : nums) {
          
            if (seenNumbers.contains(num)) {
                return true; // Duplicate found
            }
              seenNumbers.add(num); // Add number to set
            
        }
        return false; // No duplicates found
    }
}
```

At first, I had the logic reversed adding the num to `seenNumbers` and then checking if `seenNumbers` contans that num but this failed. 
This is because if we add the num first, then it will always contain that num.

Reversing the order to the one you see above- that means as we iterate through nums:  
- We check if that num is contained in the set already- and if so we return true. A return exits the loop. 
- Otherwise if that num is not contained in the set already- then we add that num to the set. 
- Else if num not contained in that set after checking through each num- outside of the for loop, we return false. 

## Attempt 4 
Ok and then I thought and was questioning ChatGPT so if a hashset doesn't allow duplicates by blocking .add if an item already exists, can't we use that built in functionality? 

From my research:
"If the element is not already in the set, it is added successfully, and add returns true.
If the element is already present, the HashSet does not allow duplicates, and add returns false."


So the below is saying if we aren't able to add the num (aka `seenNumbers.add(num)` is not true ), then return true because that means there's a dupe so we can't add. 

```
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> seenNumbers = new HashSet<>();
        for (int num : nums) {
            if (!seenNumbers.add(num)) {
                return true; // Duplicate found
            }
        }
        return false; // No duplicates found
    }
}
```