# 744. Find Smallest Letter Greater Than Target


---
# Problem 

Link: https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/
Problem Text: 
You are given an array of characters letters that is sorted in non-decreasing order, and a character target. There are at least two different characters in letters.

Return the smallest character in letters that is lexicographically greater than target. If such a character does not exist, return the first character in letters.


Example 1:
Input: letters = ["c","f","j"], target = "a"
Output: "c"
Explanation: The smallest character that is lexicographically greater than 'a' in letters is 'c'.

Example 2:
Input: letters = ["c","f","j"], target = "c"
Output: "f"
Explanation: The smallest character that is lexicographically greater than 'c' in letters is 'f'.

Example 3:
Input: letters = ["x","x","y","y"], target = "z"
Output: "x"
Explanation: There are no characters in letters that is lexicographically greater than 'z' so we return letters[0].


---
# Scratch Pad/ Pseudocode

## Edge Case(s)
Looks like example 3 is an edge case. If we have the letter z, no letters follow it- so we return letters[0]. 
Also, example 3 shows that some letters could repeat.
Another edge case would be what if our target letter is say y? But... our array ends with n?
Per the rule decreed by this high and lofty Lord Leetcode, we just: "If such a character does not exist, return the first character in letters." (That's because there would be no letter greater than y in our array)
-->
So if the target is z OR if no letter is gretater than target, we return letters[0].


## Initial Approach
// Let's start with a simple loop. `["c","f","j"], target = "a"`
// So is c > a? Yes, then we exit the loop. 
// So as we iterate- as soon as letters[i] > target --> return letters[i] and we break out the loop
// Else if no match after looping through it, return letters [0]
// But we know that this may not be efficient esp with larger data sets. Would you want to open 10,000 drawers to find your socks?
--> if letters[i] > target --> return letters[i]

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        for (int i = 0; i < letters.length; i++) {
            if (letters[i] > target) {
                return letters[i]; 
            }
        }
        return letters[0]; // This would also account for the target of z since no letter would be greater than z 
    }
}
```


## Further Rumination 
// Yes, yes it's binary search time again. Notice how Leetcode tells us the array is sorted. 
So... ok I admit my n00b self got stuck on this here. It was my 3rd Leetcode problem ever and well my wizard in shining armor gave me an idea to defeat this quagmire that Lord Leetcode had me trapped in.

// We can use a closestSoFar to track our potential candidate for the smallest letter greater than the target.
// `[ a, g, i, o, u] and b is our target`
// Middle is 2 (letter i), target < middle so we do right - 1 
// Now, we are looking at `left: 0` and `right: 1`
// The middle is 0 and that is the letter a. If a was our target, the next letter would be g
--> if letters [middle] == target { return letters [middle + 1]}

// but our target is b... and middle is less than b currently so increase the left by 1
// Now, we are at `left: 1` and `right: 1`. The middle is 1 which is g which is greater than the target, so our answer would be g, or index of 1. Also, since middle > target, right would update to `right: 0`

Again 
-->
So if the target is z OR if the target is not found we return letters[0].

---
# Java


## Binary Search 

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        char closestSoFar = letters[0];

        while (left <= right) {
            int middle = left + (right - left) / 2;

            if (letters[middle] <= target) {
                closestSoFar = letters[middle]; // Update closestSoFar if current letter is <= target
                left = middle + 1;
            } else if (letters[middle] > target) {
                closestSoFar = letters[middle]; // Update closestSoFar with current letter
                right = middle - 1;
            }
        }

        // After exiting the loop, 'left' is the insertion point
        if (left >= letters.length) {
            return letters[0]; // Wrap around to the first letter
        } else {
            return letters[left];
        }
    }
}

```

---
# Javascript
