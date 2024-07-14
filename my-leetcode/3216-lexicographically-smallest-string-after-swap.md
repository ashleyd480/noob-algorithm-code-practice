# 3216. Lexicographically Smallest String After a Swap

---


# Problem 

## Tags: 
#stringbuilder

**Link:** https://leetcode.com/problems/lexicographically-smallest-string-after-a-swap/description/

**Problem Text:**   
Given a string s containing only digits, return the lexicographically smallest string that can be obtained after swapping adjacent digits in s with the same parity at most once.  

Digits have the same parity if both are odd or both are even. For example, 5 and 9, as well as 2 and 4, have the same parity, while 6 and 9 do not.  

Example 1:  
Input: s = "45320"  
Output: "43520"  

Explanation:  
s[1] == '5' and s[2] == '3' both have the same parity, and swapping them results in the lexicographically smallest string.  


Example 2:  
Input: s = "001"  
Output: "001" 

Explanation:  
There is no need to perform a swap because s is already the lexicographically smallest. 


---

# Scratch Pad/ Pseudocode

// only digits  
// A string a is lexicographically smaller than a string b if in the first  
// position where a and b differ, string a has a letter that appears earlier in  
// the alphabet than the corresponding letter in b.  
// If the first min(a.length, b.length) characters do not differ, then the 
// shorter string is the lexicographical  
// want to see if same parity (if both odd and even as we iterate through)  
// want to track if even or odd 
// maybe we get all the even ones and odd ones as we iterate through   
// sort the evens and odds  
// and then compare the first two   
// no but we can only swap adjacent digits and if they have the same parity so 
// as we check adjacent digit, we see if same parity   
// so check parity first and if parity then if the next adjacent is less than 
// the first one then swap  

// so like 45320  
// 45 , then 5 and 3  
// char currentNum = 0; 
// char nextNum = 0; - shouldn't initalize outside or else always will pass the conditons  



---

# Java Solution

## Attempt 1

This was done for LC's weekly contest. Took me a while to figure out, in fact was still solving this one after the contest time ran out, however I learned so much from debugging!

Here are some of my scratch notes as I worked through failed tests and improved my code from there

// currentNum = 4  
// nextNum = 5  
// s.charAt(currentNumNewIndexPositon)= currentNum; unexpected type ccurs when  
// there's an attempt to assign a value to a specific index position in a string   
//

// i go tanother failed case where s= 10 and it produced 01

// i failed another returned "" for result instead of 10 and that's because we
// need a else to return s

// i had another fail that kept saying 11 - return "" and that was bc i
// initialize value of result to "" instead of s
// that fixed it
// 00545950 failed because 00 at front - only if 001 or 3 char so limit that one
// to 3 char oherwisd if 008, 009, no need to swap
// the other ones

```
class Solution {
    public String getSmallestString(String s) {

        String result = s;

        if (s.charAt(0) == '0' && s.charAt(1) == '0' && s.length() == 3) {
            return s;
        }

        // i got out of bounds error - and that's because we don't want to iterate up to
        // the last i in order to compare two ints
        for (int i = 0; i < s.length() - 1; i++) {
            char currentNum = s.charAt(i); // use char data type
            char nextNum = s.charAt(i + 1);
            // however because it's a char have to convert to integer to see if even or odd

            int intCurrentNum = Character.getNumericValue(currentNum);
            int intNextNum = Character.getNumericValue(nextNum);
            // now that we using nuumber value we don't have ot use the singel quotes
            if ((intCurrentNum % 2 == 0 && intNextNum % 2 == 0) || (intCurrentNum % 2 != 0 && intNextNum % 2 != 0)) {
                if (intNextNum < intCurrentNum) {

                    StringBuilder newStringB = new StringBuilder(s); // we only want to string build if there was parity
                    newStringB.setCharAt(i + 1, currentNum);
                    newStringB.setCharAt(i, nextNum);
                    // Convert StringBuilder to String
                    result = newStringB.toString();
                    break;

                }
            }

        }
        return result;
    }
}

```
To explain the solution, we declare a variable called result that represents our resulting "updated" lexicographically smallest string. It starts with teh value of `s` or the current string we have. This means that if it's already the smallest and no changes have to be done to it, when we return `result`, we are simply returning the original string `s`. 

The first if statement says that for when we have 2 0's as the first two chars, and there's 3 characters total, then we return `s`. I only thought of this because Example 2 states if we have 001, then return 001. There's no need to swap anything. I added the 3 character limit, because I had a failed test case with 00545950, which because it had two 0's, it passed the initial conditon of this if statement and returned `s` or the original string. Hence, I realized we have to limit the char length.

Then, we iterate through the string of numbers, assigning the variable `currentNum` and `nextNum` to the current char we are iterating over and the adjacent one respectively. 

One thing I learned is that in order to check if they are even or odd (for parity purposes as that is a constraint for swapping), then we need to have them as integers. We can't check if characters are even or odd. We use the `getNumericValue` to convert those two char values to integers, making sure we initalize the new variables to represent that with data type `int`. I realized that was needed, when Mr. Leetcode complained that .class needed. 

With those variables set, then we can check for parity: are both the `intCurrentNum` and `intNextNum` either both odd or even, and the nested if statement (thanks to Sal for showing that in a prior example last week) also checks if the `intNextNum` is less than `intCurrentNum` which means we should swap. If both of those conditoins are true, we can swap. Therein, I learned something else... strings are immutable in Java and so we can't use charAt(i) to reset the value of a character. Thus, we use `stringbuilder` to set the character with `.setCharAt(index, value)`- allowing us to swap their index positions. 
Finally, with `stringbuilder`, we convert it back to a string with `toString()`- and the value of `result` is updated to that. 

Lastly, since the constraint is only one swap, we have to `break` after the first swap happens.

Finally, if none of those conditons are met, i.e. (no pairs of character that are swappable), the if statements are skipped- and we go to "return result" and result would still have the value of that original string. 