# 28. Find the Index of the First Occurrence in a String


---


# Problem 

## Tags: 
#substring

**Link:** https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/

**Problem Text:**   

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

 

Example 1:  
Input: haystack = "sadbutsad", needle = "sad"  
Output: 0  
Explanation: "sad" occurs at index 0 and 6.  
The first occurrence is at index 0, so we return 0. 

Example 2:  
Input: haystack = "leetcode", needle = "leeto"  
Output: -1  
Explanation: "leeto" did not occur in "leetcode", so we return -1. 


---

# Scratch Pad/ Pseudocode

// haystack: butsad
// needle: sad
// String needle
// String haystack
// first occurence of needle in haystack. b
// return where needle starts in haystack

// entire string of needle needs to be in haystack

// add 1 in case same length
// for each letter in haystack : we would check it against
// after we find match
// check each letter against first letter of needle
// peek ahead in both arrays
// and if so look one more chracter each array head
// i= 3
// j= 0 ; 1

## Ask what are the constraints:
1 <= haystack.length, needle.length <= 104
haystack and needle consist of only lowercase English characters.\


## Sub-Problem
// for each iteration, we want to check if the first char matches, and if so peek ahead 


---

# Java Solution

## Attempt 1

```
class Solution {
    public int strStr(String haystack, String needle) {

        for (int i = 0; i <= haystack.length() - needle.length(); i++) {
            int j = 0; // needle index
            for (j = 0; j < needle.length(); j++) {
                // inner for loop check

                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break; // if not match then we leave the inner loop
                }

            }

            // If the inner loop wasn't broken, a match was found
            // statement of outter for loop
            if (j == needle.length()) { // if needle index is at the length of the needle 
                return i; // return the i which is start of the string we start at 
            }
        }

        return -1;

    }
}
```

Here are the steps: 
1. We know we want to iterate over the haystack. Also, shoutout to my mentor's sharp eyes for saying that we don't need to traverse the entire haystack. Let's say you know you want to find 3 connected trains that are red, orange, and green. Well would it make sense going to the end of the train? No, because you know you need 3 spaces. 
So we can do `<= haystack.length() - needle.length()`
We use the equal since say if your 4 cars in a train and you need to find the series of 3, you would start from index 1, or 4-3

Another thing is what if you only have 3 trains and the train is only 3 cars long? That would mean starting to check at index 0, or 3-3

2. As we traverse each letter of the haystack, we compare this letter against the needle- specifically the first character to start with. If that matches, then we can peek ahead. 
`j` represents the index of needle- which will always start with 0, 1, 2, etc.
Let's pretend we start with an index of 1 for `i`.
If we want to peek ahead - the next index of `i` would be 2, 3, 4, etc.
This means that the `peek ahead` index would be `i + j` 

3. Thus, if any of the character don't match- either the first or subsequent ones, then we can just break out of the loop.
```
 if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break; // if not match then we leave the inner loop
    }

```

4. However, if we traversed that entire loop and didn't break out then that means it's a match. Imagine lasting a whole year with a guy without breaking up- then you're a match. 
So as `j` is the index of the needle- if we reach the length of the needle, then it's a match. 
This means say we're on the letter `s` in the haystack of the word `butsadbug` and the needle is `sad`. 
We've peeked ahead and all the letters match. We didn't break out the loop- so we're done with the inner loop. So j would be equal to `needle.length()`and we would return that letter `s` that we are currently on aka the index we are on or `i`. 

5. If it's not found, then we can just return -1. 

## Attempt 2
My inital attempt with the nested for loop has worse than linear time complexity. 
So how can we make just one pass? 

I visited my friend Chatgpt. He was like "duh, substring." 
And I was like "oh yes yes- write that down, write that down!" >_< 

By using a substring, we don't have to iterate through the needle to check against each of the needle's character and can just check against the whole needle substring.

```
class Solution {
    public int strStr(String haystack, String needle) {

    
        int h = haystack.length();
        int n = needle.length();
        for (int i = 0; i <= h - n; i++) {
            if (haystack.substring(i, i + n).equals(needle)) {
                return i;
            }
        }
        return -1;
    }

}
```

1. We start with two variables to represent the length of the haystack `h` and needle `n`.

2. Then, we iterate through the haystack. As we are on each letter of the haystack, we still "peek ahead" by looking at the substring - peeking ahead the amount of characters in needle `n`. 
In Java's substring method, when you specify `word.substring(startIndex, endIndex)`, it will extract characters starting from startIndex up to (but not including) endIndex.
So let's say `needle` is 3 characters long and we are on the 1st character of haystack. 
We would go from that first character `i` and up to but not including index position 4.

3. If it's a match, we return the character we are on - `i`.

4. If no match, then we return -1. 

---


