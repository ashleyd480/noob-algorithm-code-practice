# 345. Reverse Vowels of a String

---


# Problem 

## Tags: 
#String #nested-while #two-pointer

**Link:** https://leetcode.com/problems/reverse-vowels-of-a-string/description/?envType=study-plan-v2&envId=leetcode-75

**Problem Text:**   

Given a string s, reverse only all the vowels in the string and return it. 

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both lower and upper cases, more than once. 

 

Example 1:  
Input: s = "hello"  
Output: "holle" 

Example 2:  
Input: s = "leetcode"  
Output: "leotcede"  


---

# Scratch Pad/ Pseudocode

// reverse only vowels  
// iterate through, if the letter == `a` , e i o , u  
// keep track of their position  
// firstVowelPosition = 1    
// secondVowel = 4   
// s[4]= e;    
// s[1] = o;    
// palindrome: array of characters: two indexes left and right, compare if the same, we can apply that way here  
// right index decrease by one at start of each loop  
// at start of loop - check if right index char is a vowel- if not break out of the loop  
// else we take that right index and compare it against the left index which increases by one until we find  another vowel  
// if s[leftIndex] is vowel && s[rightIndex] is vowel --> swap  



## Ask what are the constraints:
1 <= s.length <= 3 * 105  
s consist of printable ASCII characters. 


## Edge Case(s)
I was going to say capital letters as in maybe we have to do `toLowerCase`, and appears that was mentioned in the problem that they can be lower or upper.


---

# Java Solution

```
class Solution {
    public String reverseVowels(String s) {

        // Convert the string to a character array as strings are immutable and we need
        // to swap
        char[] charArray = s.toCharArray();
        int leftIndex = 0;
        int rightIndex = s.length() - 1;
        char cL = charArray[leftIndex];
        char cR = charArray[rightIndex];
        while (leftIndex < rightIndex) {// becuase when left and right are equal it's the same letter so nothing to swap and don't want to go further because don't want to undo the swaps
            while (!isVowel(cL) && leftIndex < rightIndex) { // put the array length check in the inner while too 
                leftIndex++;
                cL = charArray[leftIndex];
            }
            while (!isVowel(cR) && leftIndex < rightIndex) { // put the array length check in the inner while too
                rightIndex--;
                cR = charArray[rightIndex];
            }
            // swap here
            // check if left < right and check if both are vowels
            if (leftIndex < rightIndex && isVowel(cL) && isVowel(cR)) {

                // Swap characters
                char temp = charArray[leftIndex];
                charArray[leftIndex] = charArray[rightIndex];
                charArray[rightIndex] = temp;

                // Move indices inward and update cL and cR
                leftIndex++;
                rightIndex--;
                if (leftIndex < rightIndex) {
                    cL = charArray[leftIndex];
                    cR = charArray[rightIndex];
                }
            }
            // while loop while s left and s right - one is not vowel, then we keep right
            // the same and just keep increasing index of 1
            // and also while left index is less than right
        }
        // Convert the character array back to a string
        return new String(charArray);

    }

    private static boolean isVowel(char c) {
        // Convert the character to lowercase and check if it's a vowel
        char cLower = Character.toLowerCase(c);
        return cLower == 'a' || cLower == 'e' || cLower == 'i' || cLower == 'o' || cLower == 'u';
    }

}

```
Here is the approach:
1. Firstly, we know that we need to swap letters and in Java, Strings are immutable. This means we need to convert it to an array of Characters using `.toCharArray()` method. 

2. Then, we can keep track of the `leftIndex` (initialized at 0) and `rightIndex` (initialized to the end of the  character array represented by `s.length()-1`). This is a "two-pointer" approach with two indexes. This way we can advance from the left side first to check for a vowel, and once found- advance down the right side to check for a vowel. If both sides have a vowel- we can swap. 

3. I also initialized two variables `cL` and `cR` to represent respectively the current character at `leftIndex` and `rightIndex`. (Note: Doing it like this means we also should ensure that we are updating their values). We also have a helper function called `isVowel` which converts those characters to lower case and then returns a boolean to see if it is a vowel. 

4. The outer while loop is `while (leftIndex < rightIndex)`. This is because we want to keep going until `leftIndex` is equal to `rightIndex`. When they're equal, we're on the same letter, and there's no point to swap. When `right` > `left`, then that would mean potentially undoing the swaps.

5. The inner while loops then respectively check the character array from the left and right. We first loop through the left side with: `while (!isVowel(cL) && leftIndex < rightIndex)`. We will continue going up one index with each iteration. With that, we also update the value of `cL` with each iteration as well, as that updated value is needed in the next inner loop iteration. The loop continues going until either we find a vowel, or until we've looped all the way up to the `rightIndex`- at which point we exit the inner loop.

6. The same logic as discussed in step 5 applies to to the second while loop through the right side with: ` while (!isVowel(cR) && leftIndex < rightIndex)`. 

7. Once both are exited (either due to a vowel being found or leftIndex getting up to rightIndex), then we reach the if statement. The if statement checks that the character from both inner loops is indeed a vowel and that the leftIndex < rightIndex. 
( This is so we ensure we are only swappping vowels, i.e. imagine exiting out of the first inner while loop with a vowel, but then the second while loop is exited because left and right equal as no vowel from the right. Also, the index check ensures that we are swapping only if left < right - so we don't unswap characters or swap the same character as explained in Step 4.)

8. When that `if` check passes, then we create a variable called `temp`, and it's assigned the value of the character at `leftIndex`. Without the `temp` - we would lose the value of the character at `leftIndex` if we immediately reassign it to value of character at `rightIndex`.

9.  Then, we say that character at `leftIndex` will now be assigned the value of character at `rightIndex`. Finally, we say character at `rightIndex` will get assiged the value of `temp` which had held the value of `leftIndex`. 


10. Finally, after the swap, we want to move onto the next potential pair, so we want to move away from the current left and right index we are on. As such, we will increment `leftIndex` and decrement `rightIndex`. 


---


# What I Learned
When doing a nested while loop (or this could even apply to nested for loop), you need an exit condition for the inner while loop too. For example, you can see above how the inner while loop will stop when either we've reached a vowel or leftIndex < rightIndex. This is because say what if you iterate through all the left-hand characters and you don't see a vowel. Then you'll keep iterating. 


I also learned that using the below way to swap doesn't work because we are working with copies. 
```
 char temp = cL;
 cL = cR;
 cR = temp;
 ```

 Doing it this way means we are assigning `temp` to the value of `cL`. then `cL` gets assigned the value of `cR`. However, they are not actually moving around in the array. 