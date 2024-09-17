# 1370. Increasing Decreasing String

---


# Problem 

## Tags: 
#array 

**Link:** https://leetcode.com/problems/increasing-decreasing-string/description/  

**Problem Text:**   
You are given a string s. Reorder the string using the following algorithm:  

Remove the smallest character from s and append it to the result.
Remove the smallest character from s that is greater than the last appended character, and append it to the result.
Repeat step 2 until no more characters can be removed.
Remove the largest character from s and append it to the result.
Remove the largest character from s that is smaller than the last appended character, and append it to the result.
Repeat step 5 until no more characters can be removed.
Repeat steps 1 through 6 until all characters from s have been removed.
If the smallest or largest character appears more than once, you may choose any occurrence to append to the result.

Return the resulting string after reordering s using this algorithm.

 

Example 1:   
Input: s = "aaaabbbbcccc"  
Output: "abccbaabccba"  
Explanation: After steps 1, 2 and 3 of the first iteration, result = "abc"  
After steps 4, 5 and 6 of the first iteration, result = "abccba"  
First iteration is done. Now s = "aabbcc" and we go back to step 1  
After steps 1, 2 and 3 of the second iteration, result = "abccbaabc"  
After steps 4, 5 and 6 of the second iteration, result = "abccbaabccba"  

Example 2:   
Input: s = "rat"  
Output: "art"  
Explanation: The word "rat" becomes "art" after re-ordering it with the mentioned algorithm.  

---

# Scratch Pad/ Pseudocode

// 1. append smallset character from s   
// 2. next largest after that 1st one and append   
// keep doing step 2 until can't pick any more characters - either we finished check all characters OR there are no other larger ones or ones left are equal to the current character we on   



## Ask what are the constraints:
1 <= s.length <= 500
s consists of only lowercase English letters.


## Initial Approach
We have a new String to hold the result, and a boolean to indicate the direction which we start as increasing = true. 
We konw we want to continue to iterate until we go through every letter of the string `s`, so we'll use a while (s.notEmpty). Based on the if its increasing or not, if we it's increasing- we want to return the largest character, and vice versa for decreasing. 
To determine the toggle direction, we know that if we have the largest character in the remaining string `s` that hasn't been added to the result in this sequence- then we are on the largest one and it's about time to switch direction, hence:

```
if (currentChar == returnLargestCharNotLast(s, currentChar) && increasing)
```


## Sub-Problem
We know we need to determine both the direction and also based on the toggle direction, we need to do something. 


---

# Java Solution

## Attempt 1

Here is an initial attempt which uses the initial described approach above along with helper functions. 

```
class Solution {
    public String sortString(String s) {
        // 
        // 1. append smallset character from s
        // 2. next largest after that 1st one and append
        // keep doing step 2 until can't pick any more characters - either we finished check all characters OR there are no other larger ones or ones left are equal to the current character we on 
        // 4. 

        String result = new String ();
    
    boolean increasing = true;
  char currentChar = returnLowestCharFromString(s)
 s = s.removeFirstChar(returnLowestCharFromString(s), s);
while (s.notEmpty) {
    while 

    // determining toggle direction
    if (currentChar == returnLargestCharNotLast(s,  currentChar ) && increasing) {
        increasing = false;
         currentChar = returnLargetCharFromString(s, currentChar);
    s = s.removeFirstChar(returnLargestCharFromString(s), s);
    result = result + currentChar;
    }
    else if (currentChar ==  returnLowestCharFromString (s, currentCar) && !increasing) {
        increasing = true; 
        currentChar = returnLowestCharFromString(s, currentChar);
    s = s.removeFirstChar(returnLowestCharFromString(s), s);
    result = result + currentChar;
    }

    // based on toggle direction we do something
    if (!increasing) {
  currentChar = returnLowestCharFromString(s, currentChar);
    s = s.removeFirstChar(returnLowestCharFromString(s), s);
    result = result + currentChar;
    }
    else if (increasing) {
  currentChar = returnLargestCharFromString(s, currentChar);
    s = s.removeFirstChar(returnLargestCharFromString(s), s);
    result = result + currentChar;
    }


     
       
    }

     public char returnLowestCharFromString (String s, char lastAddedChar) {

     }

     public String removeFirstChar (char c, String s) {
    
     }

     public char returnLargestCharNotLast (String s, char lastAddedChar) {

     }

  
}
```

## Attempt 2
I was able to find a cleaner solution that also my n00b brain could wrap itself around. ;)

1. We initialize an array with 26 positions called `charCount` to keep track  of all 26 chracters.

2. Then, we can go through `s` which we convert to a characterArray. As we iterate through the `s` characters and each time we see a character, we increment its count of that character by 1. Let's say we see a letter `b`, we would go the index position of that `b` and increase its count by 1. 

3. Now, we have the update count for each character in that string in the `charCount`.

4. Then, we use `result` to build the string with Stringbuilder, and we have a boolean toggle flag which we initally set as `inreasing= true`.

5. We want to continue until the result's length has the same length of the string `s`- so we use that while loop.

6. With each iteration- we have a sub-iteration for loop based on if it's increasing or decreasing.

- If it's increasing, then we would go through `charCount` in ascending order so we can append the smaller characters first.  We would append if its count is greater than 0. Once that character is appended, its count would decrease by one: `charCount[i] -- `

- Else, if we are decreeasing, then we would go through our `charArray` in reverse and append characters that way. Similarly, when a aracter is appended, its count would decrease by one: `charCount[i] -- `.

7.  When we are done with either the if for-loop or else for-loop, removing the first round of either large or small characters, we can make additional passes if there are still letters left in `s` and before we make a pass, we want to toggle the increasing flag with `increasing = !increasing;`
---


# What I Learned
`charCount[c - 'a']++` is used to keep track of character frequencies. `c - 'a'` calculates the index where `c` is the chracter and `a` is the character `a`. Subtracting `a` allows us to to have a zero-based index for each ltter of the alphabet. 
For example:
- If c is 'a', then c - 'a' is 0.
- If c is 'b', then c - 'a' is 1.