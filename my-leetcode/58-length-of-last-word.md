# 58. Length of Last Word

---


# Problem 

## Tags: 
#splice #trim #array

**Link:**  https://leetcode.com/problems/length-of-last-word/description/?envType=study-plan-v2&envId=top-interview-150

**Problem Text:**   
Given a string s consisting of words and spaces, return the length of the last word in the string.  

A word is a maximal substring consisting of non-space characters only.  

 

Example 1:  
Input: s = "Hello World"  
Output: 5  
Explanation: The last word is "World" with length 5.  

Example 2:  
Input: s = "   fly me   to   the moon  "  
Output: 4  
Explanation: The last word is "moon" with length 4.  

Example 3:  
Input: s = "luffy is still joyboy"  
Output: 6  
Explanation: The last word is "joyboy" with length 6.  




## Checklist

- [ ] Formatting reminder: Ensure 2 spaces after each line for line breaks  
- [ ] Run the code periodically with `System.out.println` to check return values and ensure it compiles  
- [ ] Best practice: time-split 45/15-minute for go over coding/time-efficiency - i.e. sorting is less time efficient than iterate through whole dataset
- [ ] Ask what are the constraints 
- [ ] Think of clarifying questions 
- [ ] Any edge cases? 

---

# Scratch Pad/ Pseudocode
// my thought is iterate through the string reverse  
// does string include the " " - i guess not as constraint says its only spaces  
// so let's go in reverse  
// skip the first space at the end if there is one  
// and when we reach the space before last word, just break out of the loop  
// we need a way to make sure space we break on is the 2nd one, not the first so  
// we don't break too early  
// just realized too there can be multiple spaces at the end!!   
// so if we've seen a space but no letter on the next we don't count those  
// but if we see a letter first and then a space we know the stop   
// forgot another caes is what if just one word  
// like "a " or what if "asdsdf "   
// that would be like if no space ahead and just one word  
// if (letter == ' '  && i < s.length()- 1 && s.charAt(i+1) != ' ' cause out of bound  
// because last letter is space and i <   
// how about a count of spaces and until we reach a character we reset the count  
// and then if we see a space to the left of character or we reach 0th character and no space then we breka ou  






---

# Java Solution

## Attempt 1

```
class Solution {
public int lengthOfLastWord(String s) {
  int countLettersInWord = 0;
        // if (s.length ()== 2 && s.charAt()) {
        //     return 1;
        // }

        for (int i = s.length()- 1; i >0 ; i -- ) {
            char letter= s.charAt(i);
            System.out.println(i);
            System.out.println(letter);
     
        
            if (letter != ' ') {
            countLettersInWord ++;
             }
            if  (letter == ' '  && i < s.length()- 1 && s.charAt(i+1) != ' ') {
                 
                break;
            }
      

        }
        return countLettersInWord;
    }
}


```

The approach is to iterate through String s in reverse as we're basically looking for the last word's length. 
We keep track of `countLettersInWord` and we increment that count if the letter is not a space. 
For "a " - we don't count that first space and just count the a. 

Also, for longer strings of multiple words- if we see we space before a character that indicates the start of a word. Say for instance: "hello world". There is a space in front of `w`. 
This means if I see a space, then the letter to the right or `s.charAt(i+1)` should not be a space. 

To deal with the out of bounds issue, we add another check to ensure ` i < s.length() - 1` This is because there's no way to increment `i` by 1 if we're already at the end. 

With this, we have this conditional to break out the loop `if (letter == ' ' && i < s.length() - 1 && s.charAt(i + 1) != ' ')`. This means the letter should be a space but not a space at the very end and the character to the right can not be a space to exit out. This would mean we're done seeing the word. 

(^Note: since the above if statement goes after the first `if` statement - this bypasses the first space check and character count continue to increment until we hit the space again which precedes a character- at which point we break out. )

## Attempt 2

```
class Solution {
    public int lengthOfLastWord(String s) {
    
         // Trim the string to remove trailing spaces
        s = s.trim();
        
        // Split the string into words based on spaces
        String[] words = s.split("\\s+");
        
        // Get the last word
        String lastWord = words[words.length - 1];
        
        // Return the length of the last word
        return lastWord.length();
    }
}
```

This second approach is a bit simpler and easier to visualize. 
Essentially, we want to divide our string into an array of words.
First, we use `.trim` to remove trailing spaces. Then, we split the string with s.split("\\s+") to make it into an array of words.
Then, we get the last word in that array of words, and use `.length` to get its length. 



