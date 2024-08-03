# 844. Backspace String Compare

---


# Problem 

## Tags: 
#stringbuilder

**Link:** https://leetcode.com/problems/backspace-string-compare/description/

**Problem Text:**   

Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

 

Example 1:  
Input: s = "ab#c", t = "ad#c"  
Output: true 
Explanation: Both s and t become "ac".  

Example 2:  
Input: s = "ab##", t = "c#d#"  
Output: true  
Explanation: Both s and t become "".  

Example 3: 
Input: s = "a#c", t = "b"  
Output: false   
Explanation: s becomes "c" while t becomes "b". 



---

# Scratch Pad/ Pseudocode

// 2 strings s and t 
// #. means backspace the last char we have seen before the #  
// String are immutable  
// this is out of bounds since index changes in SB   
// the first time i is correct 2 - 1--> i - 0 --> i - countPound*2
// the second i is larger by 2 vs the sbindex (because we saw #) 2--> i-  
// countPound*2  
// and if we had 3 - i would be larger by 4 3--> i -4--> i- countPound*2  

## Ask what are the constraints:

1 <= s.length, t.length <= 200 
s and t only contain lowercase letters and '#' characters.  

^ That is good to know as that means we don't have to worry about converting to lower-case to compare for equality. 


---

# Java Solution
## Attempt 1

```
class Solution {
    public boolean backspaceCompare(String s, String t) {
        StringBuilder newStringBuildS = new StringBuilder(s);
        int sbIndex = 0;
        StringBuilder newStringBuildT = new StringBuilder(t);
        int tbIndex = 0;
       

        if (s.equals(t)) { // because maybe already equal
            return true;
        }
        int countDeletedLetter = 0;

        for (int i = 0; i < s.length(); i++) {
            // char stringLetter = s.charAt(i)
            if (s.charAt(i) == '#') {

                sbIndex = i - countDeletedLetter;
                newStringBuildS.deleteCharAt(sbIndex); // delete the backspace
                countDeletedLetter++; // after we see the pound sign increment at the end

                if (sbIndex !=0)
                {
                     newStringBuildS.deleteCharAt(sbIndex - 1);
                     countDeletedLetter++;
                }

               
            }
        }

        countDeletedLetter = 0; // have to reset before the second loop- since we already declare we can use without data type to just update

        for (int i = 0; i < t.length(); i++) {
            if (t.charAt(i) == '#') {
                tbIndex = i - countDeletedLetter;

                System.out.println(tbIndex);
                newStringBuildT.deleteCharAt(tbIndex); // delete the backspace
                 countDeletedLetter++; 

                    if (tbIndex !=0)
                {

                newStringBuildT.deleteCharAt(tbIndex - 1);
                countDeletedLetter++;
                }
                
            }

        }

        String resultS = newStringBuildS.toString(); // want to compare the updated result 
        String resultT = newStringBuildT.toString();
        return resultS.equals(resultT);
    }
}
```
The main idea is we are initializing two StringBuilders to hold the content of `s` and `t`. This is because strings are immutable.
We also initialize a `sbIndex` and  `tbIndex` to allow us to find the index of the character to delete through the StringBuilder

For each string `s` and `t`, we iterate through them and we know if the `charAt(i)` is a `#`, then we can delete the backspace by: ` newStringBuildS.deleteCharAt(sbIndex);`

Also, we can delete the character that precedes the backspace:
`newStringBuildS.deleteCharAt(sbIndex - 1);`

One issue I got was the array out of bounds which is because as we remove characters from stringbuilder, the index changes. To represent that, we want to ensure that as characters are removed, that we also trak that. This is where `countDeletedLetter` comes into play. It starts out at 0. So the first time, we delete a `#`, the stringbuilder index matches the string index.
However after that, when we delete a backspace, the stringbuilder index is off by 1 (one character less)- so we do `countDeletedLetter++` to reflect that. And when we delete the preceding character, the stringbuilder index is off by 1 more (one more character less)- so we do `countDeletedLetter++` to reflect that.

Finally, we only want to delete the preceding character before the `#` sign if we are not at the 0th index. That is say I have `#ab`. We can delete the #, but there is no character we can delete before the `#` as we are already at the 0th index. 

## Attempt 2
The `newStringBuildS` and `newStringBuildT` method are duplicate blocks of code and we use two for loops.
Instead, we can simply use a helper method to further simplify.
`resultS` and `resultT` equal to the return of the helper method which we call `procesBackspaces`. 

```
class Solution {
    public boolean backspaceCompare(String s, String t) {
        String resultS = processBackspaces(s);
        String resultT = processBackspaces(t);
        return resultS.equals(resultT);
    }

    private String processBackspaces(String str) {
        StringBuilder newStringBuilder = new StringBuilder(str);
        int countDeletedLetter = 0;

        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == '#') {
                int indexToRemove = i - countDeletedLetter;

                newStringBuilder.deleteCharAt(indexToRemove); // delete the backspace
                countDeletedLetter++; // count the backspace

                if (indexToRemove != 0) {
                    newStringBuilder.deleteCharAt(indexToRemove - 1); // delete the preceding character
                    countDeletedLetter++;
                }
            }
        }

        return newStringBuilder.toString(); // return the processed string
    }
}
```

