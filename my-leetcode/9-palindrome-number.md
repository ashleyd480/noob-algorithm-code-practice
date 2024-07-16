# 9. Palindrome Number


---


# Problem 

## Tags: 
#integer-to-string, #boolean, #integer-division 

**Link:** https://leetcode.com/problems/palindrome-number/description/

**Problem Text:**   
Given an integer x, return true if x is a 
palindrome, and false otherwise. 

 

Example 1:  
Input: x = 121  
Output: true  
Explanation: 121 reads as 121 from left to right and from right to left.  

Example 2:  
Input: x = -121  
Output: false  
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.  

Example 3:  
Input: x = 10  
Output: false  
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.  





---

# Scratch Pad/ Pseudocode

// must read the same left to right  
// so when we reverse the array of character- it should be the same  
// that means that ok what if we divide it in half then iterate one number up  
// and down from the middle and check they equal until no numbers left  


## Initial Approach

Note: I had to scrap this as what it didn't take into account is when we have an even count of numbers, the way the middle, top, and bottom are defined change.
I hadn't taken this into consideration so when I wrote the calculations for those variables, only numbers with an odd number of digits could pass. 

Below I included my thought process while working this inital approach. For example, you can see how I also failed a test case due to not taking into account the negative numbers. At the end of it, I realize, I'm overcomplicating it and try it another simpler way. 

// -101 returned true; this failed because the - sign - and so if it starts with
// - we can't have palindrome; use single quote for char
// now 10 failed ; we can't do with even number so i put
// if (integerToString.length() % 2 == 0) {
// return false;
// }

// but this failed because 11 is a palindrome - so when we have 2 numbers for  
// instance it would round down and start at 0 as the middle -- so if it's even   
// the middle - the top would be length/2 + 1 and bottom would be length/2 - but 
// if just 2 numbers we just ocmpare the two   

// ops im overcomplicating how about just iterate through the loop forwards  
// and backward to see if the string equals  

```
      String integerToString = String.valueOf(x); // can't use toString() because its a primitive type
        int middleIndex = (integerToString.length() - 1) / 2;

        int topIndex = middleIndex;
        int bottomIndex = middleIndex;

        char middleNumber = integerToString.charAt(middleIndex);
        char topNumber = integerToString.charAt(topIndex);
        char bottomNumber = integerToString.charAt(bottomIndex);

        if (integerToString.charAt (0) == integerToString.charAt (1)) {
            return true 
        }
          else return false;
        }

    
        while (topIndex < integerToString.length() && bottomIndex >= 0) {
            topIndex++;
            bottomIndex--;
            if (topNumber != bottomNumber || integerToString.charAt(0) ==('-')) {
                return false;

            }
            
        }
        return true; // if all of them equaled after we done with while loop then its a palindrome
    }
}
```

---

# Java Solution

## Attempt 1

After attempting my pseudocode by splitting the string in half and the code growing increasingly complex, I decided to go for an easier solutoin where we reverse the string and see if that equals the string. 

```
class Solution {
    public boolean isPalindrome(int x) {

        String integerToString = String.valueOf(x); // can't use toString() because its a primitive type

        int lastIndex = integerToString.length() - 1;
        String reversedIntegerToString = "";

        for (int i = lastIndex; i >= 0; i--) {// fixed out of bound error with the >= 0
            // return the the reversed string so we need to initalize it outside the loopo
            reversedIntegerToString = reversedIntegerToString + integerToString.charAt(i); // to update the value
        }

        if (reversedIntegerToString.equals(integerToString)) {
            return true;
        } else
            return false;
    }
}
```

One thing to note is that, I could have simplified that last return statement as follows. Since we're simply checking if the reversed equals the actual string, then we can just do:
` return reversedIntegerToString.equals(integerToString);`

## Attempt 2
The challenge was to not use strings, so I consulted with my friend ChatGPT to learn with him.
Here is what I learned.


```
class Solution {
    public boolean isPalindrome(int x) {
        // Negative numbers are not palindromes
        if (x < 0) {
            return false;
        }

        // Initialize variables
        int original = x;
        int reversed = 0;

        // Reverse the number
        while (x != 0) {
            int digit = x % 10; // Get the last digit - if 121 we have 1
            x = x/10;  // Remove the last digit ; this means 121/ 10 = 12 
            reversed = reversed * 10 + digit; // Add the digit to the reversed number
        }

        // Check if the original number and the reversed number are the same
        return original == reversed;
    }
}
```


1. Negative numbers can't be palindromes because of that - symbol. When reversed, it wouldn't be the same, hence that first if statement check.

2. Then, we initialize variables and the value of `original` is the provided number x. This way we can store that value, as `x`'s value change in the subsequent calculations.

3. Now, in the while loop we want to reverse the number. 
 - First iteration
     - Let's say we have 121. The last digit is the remainder of 121 divided by 10. That is 1 (12*10 + 1). ...Or even if we had 5858, the reminder would be 8-- (585*10 + 8)
    - Then, we want to remove the remove that last digit by dividing by 10. You can imagine when we draw this out, we draw an arrow to move up one decimal point to the left so 121 becomes 12.1... Of if we had 808, that would be 80.8 and the `int` type removes the decimal in both cases so we have all the numbers except the last. x is updated to 12.
    - The reverse of that number would start out with that last digit first- which is 1 for 121. (0*10 + 1) ---> `reversed = 1`
 - Second iteraton
    - The next iteration, we have `x=12`.  `digit` gets updated to 2. And then x = 1. 
    - reversed would now be (1*10 +2) = 12 (we multiply by 10 to set the "10^1" decimal point value's digit-- so that decimal point number is 1- and the 2 is added)
- Third iteration
    - Then, we have `x=1` and digit becomes 1 with integer division. 
    - x becomes 0 This means there are no other numbers to check. If the number is already one digit and we divide it by 10 and cast to `int`, it will equal to 0. *That's why we had that while loop check that x does not equal to 0 or else exit. 
    - reversed becomes 12*10 (we are now on the 10^2 decimal point place) and we add that last digit 1. 


4. Finally we can check if that reversed number equals the original. 