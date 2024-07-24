# 20. Valid Parentheses

---


# Problem 


## Tags: 
#while-true #stack

**Link:** https://leetcode.com/problems/valid-parentheses/description/

**Problem Text:**   

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:  
Input: s = "()"  
Output: true  

Example 2:  
Input: s = "()[]{}"  
Output: true  

Example 3:  
Input: s = "(]"  
Output: false  
 
---

# Scratch Pad/ Pseudocode

// has to contain '(', ')', '{', '}', '[' and ']'   
// must be pairs of same type  
// s consist of parenthesis only  


## Ask what are the constraints:
1 <= s.length <= 104  
s consists of parentheses only '()[]{}'  
Side note: pretty interesting how after I watched a video on this, I realized how this can be applied to how our IDE checks our brackets. 

## What clarifying questions do I have:
I want to clarify why you are so tough Mr. Leetcode. ;) 

## Edge Case(s)
This problem really shows the importance of thinking of edge cases because while it wasn't explicity stated it wasn't until a few attempts in that I saw I had a failed edge case of ([]), which means the parens can also be balanced and pass. :)


## Initial Approach
This was my initial thoughts, and I was like hmmm doesn't seem like it would be that easy, especially with how Mr. Leetcode likes to be tricky. 
Yup, I was wrong. This approach below will only check if one is contained- and if one is contained, even if the rest of the characters don't pass, it returns true. And I oop. 
This n00b will go back to the drawing board. >_<

```
public boolean isValid(String s) {

    if (s.contains("()") || s.contains("{}") || s.contains("[]")) { 
            return true; 
        }

        return false;
    }
}
```

---

# Java Solution

## Attempt 1
My actual first attempt was like what if we use "sliding window" technique and peek ahead. 
Well, unfortunately this one does not take into account what if first char is say `}` so it never enters for loop

```
class Solution {
    public boolean isValid(String s) {

        // another idea to check every 2 characters and see if they are one of the
        // following and if so true
        boolean found = false;

        for (int i = 0; i < s.length(); i = i + 2) {
            char sCurrentChar = s.charAt(i);
            char sNextChar = s.charAt(i + 1);
            if (sCurrentChar == '{' && sNextChar != '}' || sCurrentChar == '(' && sNextChar != ')'
                    || sCurrentChar == '[' && sNextChar != ']') {
                return false;

            }

        }
        return true;
    }
}
```

## Attempt 2

```
// Code block correction
...
 if (sCurrentChar == '{' && sNextChar != '}' || sCurrentChar == '(' && sNextChar != ')'
                    || sCurrentChar == '[' && sNextChar != ']' || sCurrentChar == '}' || sCurrentChar == ')' || sCurrentChar == ']') 
...
```
Well, so I thought to fix that issue from Attempt 1 where it wasn't entering the loop if the first character is a closed parens, I did it as follows -- but I failed a test case.
I had failed to consider the case where what if our parens aren't necessarily in paired order of say `[]{}` but instead like `{[]}`. 

## Attempt 3 

```
public class Solution {
    public static boolean isValid(String s) {
        while (true) { // means keep going until you reach a return statement 
            if (s.contains("()")) {
                s = s.replace("()", "");
            } else if (s.contains("{}")) {
                s = s.replace("{}", "");
            } else if (s.contains("[]")) {
                s = s.replace("[]", "");
            } else {
                // If the string becomes empty, it indicates all brackets are matched.
                return s.isEmpty();
            }
        }
    }
}
```
The while (true) statement in this code means that the loop will run indefinitely unless it encounters a return statement. If it contains that parens pair, we remove the pair. For example `{[]}` would become `{}` which would become an empty string. Once the string is empty, that means all pairs were found- just like if all guys and girls on a dance floor partnered up and left for the night- the club would be empty.

This traverses through the string so it's linear time complexity. As the string becomes longer, it will take more time.


## Attempt 4
I got to shoutout this Leetcoder Vikas for his awesome [solution](https://leetcode.com/problems/valid-parentheses/solutions/3399077/easy-solutions-in-java-python-and-c-look-at-once-with-exaplanation). Learned how we can apply a stack to solve this! 

Basically, we know that if we see an opening parens, then it should have a closing parens to be a legit pair. For each open parens, we add its closing parens to the stack. Like say we have `{[)` --> we would add to the stack: `}, ]..` and then the next char `)`is a closing char so we use the final `else if` to check that closing char and we see "oh, it's not equal to the last character in the stack which is `]` so we know it's not a match. Alrighty- bye felicia- exit this loop by the return statement- return false and we out. 
Oh, and that last `else if` would also go "bye felicia" if the stack were empty which means that well we never even started with an opening character. You know if you start with a closing parens, no way it's going to be a match so bye and we out! 

```
public class Solution {
    public static boolean isValid(String s) {
        // for the opening bracket- we know if balanced- should have its respective closing bracket so add closing to stack 

        Stack <Character> stack = new Stack <> ();
        for (char c : s.toCharArray()) {
            if (c == '(') {
                stack.push (')');
            }
            else if (c == '[') {
                stack.push (']');
            }

            else if (c == '{') {
                stack.push ('}');
            }
            // if stack empty means did not start with opening bracket
            // or if char parens does not match the opening one then it's stack.pop != c 
            else if (stack.isEmpty() || stack.pop() != c) {
                return false;
            }
        }
        return stack.isEmpty();
    }
}
```
