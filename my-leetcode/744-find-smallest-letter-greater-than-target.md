# 744. Find Smallest Letter Greater Than Target


---
# Problem 


## Tags: 
#binary-search, #ternary

**Link:** https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/

**Problem Text:** 
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
So... ok I admit my n00b self got stuck on this here. It was my 3rd Leetcode problem ever and well my wizard mentor in shining armor gave me an idea to defeat this quagmire that Lord Leetcode had me trapped in.    

// We can use a closestSoFar to track our potential candidate for the smallest letter greater than the target.    
// `[ a, g, i, o, u] and b is our target`  

1. 1st iteration:  `middle` is 2 (letter i), `middle` > `target` so the middle could be a potential
We update closestsoFar to middle. Just like imagine how some women want the first guy over 6 ft. So any guy over 6 ft could be a potential. 
--> if letters [middle] > target { closestsoFar = letters [middle]}

2. 2nd iteration: Because `middle` was > `target`, then we should search the lower half. We don't want a guy thats too tall, am I right ladies? 
So now, we are looking at `left: 0` and `right: 1`
// The middle is 0 and that is the letter a. If a was our target, the next letter would be g.  
Or with dating analogy, if the guy in the middle exactly was 6 ft, then our next guy in line would be our dream guy.     
--> if letters [middle] == target { return letters [middle + 1]}  

3. ... but our target is b... and middle is less than b currently so increase the left by 1   
// Now, we are at `left: 1` and `right: 1`. The `middle` is 1 which is g which is greater than the target, so our answer would be g, or index of 1. Also, since `middle` > `target`, right would update to `right: 0` which would exit our loop.  
Back to dating analogy: when left = right, then we are staring at our last guy. And if he is taller than our target of 6 ft, then thats our guy.  




---
# Java

## Binary Search 

### Attempt 1 
So I attempted to put all of that in code

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        char closestSoFar = letters[0];

        while (left <= right) {
            int middle = left + (right - left) / 2;
            if (letters[middle] < target) {
                left = middle + 1;
        
            } 
            else if (letters[middle] == target) {
                closestSoFar = letters[middle + 1];
                return closestSoFar;
            }
            
            else if (letters[middle] > target) {
                closestSoFar = letters[middle]; // potential candidate 
                right = middle - 1;
            }
        }

        // After exiting the loop, 
        if (left == right) {
            return closestSoFar;
        } else {
            return letters[0];
        }
    }
}
```
3 test casees passed, then one failed.
`["c","f","j"] with target of d`. Mine output c, whereas it was expected to output f.

So mine is saying: 
1. 1st iteration: closestSoFar= c, middle = f, target = d; middle > target  
--> closestSoFar = middle
left= 0, new right= 1
2. 2nd iteration: closestSoFar= middle   
--> new middle is 0 (c), middle < target so:
new left = 1, new right = 1


3. 3rd iteration: middle = 1; middle > target: closestSoFar = middle, and right is updated to 0

4. ... and I oop! Now loop is exited but right is not equal to left so it goes to the second else and we get the letters[0]

### Attempt 2
Now we cookin`! 
This second attempt removes my n00by if/else statement and instead uses a ternary to check if closestSoFar > target and if so returns closestSoFar. Remember we initlaize with value of letters[0]. Otherwise, if whatever value of closestSoFar is less than the target, we'll just stick with letters[0].

Dating analogy- if Ms. Betsy doesn't find the first guy over 6 ft, she'll just stick with her current guy who is first in line. 

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        char closestSoFar = letters[0];

        while (left <= right) {
            int middle = left + (right - left) / 2;
            if (letters[middle] <= target) {
                left = middle + 1;

            }

            else if (letters[middle] > target) {
                closestSoFar = letters[middle]; // potential candidate
                right = middle - 1;
            }
        }

        // After exiting the loop,
        // If no character greater than target is found, return the first character
        return closestSoFar > target ? closestSoFar : letters[0];
    }
}
```

### Attempt 3
On 9/17, I revisited:
Essentially, similar logic to Attempt 2 however, I initialize the `result` with the "sad path" solution of `letters[0]`. 
`potentialSmallestGreateest` represents the 
Then, within the while loop, we check the whole searchable breadth of array, up to where left and right are on the the same letter. Then, we check to see if the middle letter is less than or equal to the the target, and if so, we know that can't be the `potentialSmallestGreateest`. Even if the middle is equal to the target, we would still need to keep searching because we are not searching for the target but rather `potentialSmallestGreatest`.

Only if the middle letter is greater than our target could that be a ``potentialSmallestGreateest` in which case we would update the value of `potentialSmallestGreateest` to the value of that middle. However, we want to find the smallest greatest, so we would continue narrowing down the search area with binary search until we can find the smallest greatest one. 

If we exhaust the search at the end of the while loop, in which case we're on the last letter and there's still no sign of a letter > target, left would = right on that last letter, and middle would be that letter. That letter would be less than target, so left would increment by 1. In that case, left would beceome equal to letters.length, in which case we'd just return the default value of `result` or `letters[0]`.  Otherwise, if we found our `potentialSmallestGreateest` at the end of the while loop, then we would just return that value. 

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        // sorted so can use binary        // could we use hashmap and iterate through hashmap values but binary more efficient since array have indices
        char result = letters[0];
        int left = 0;
        int right = letters.length - 1;
        int middle = (left + right) / 2;
        char potentialSmallestGreatest = letters[middle];
        // doesn't matter if we find the target or not
        // we check if middle > target
        // middle would be potentialSmallestGreater - like c f j- f is potential
        // as c (new mew middle) > target - our old middle potentialSmallestGreatest would be answer
        // but if there is another letter that is smaller but also greater than middle then we want that one
        while (left <= right) {
            middle = (left + right) / 2;

            if (letters[middle] <= target) {
                left = middle + 1;
                // in this case we are binary searching for the next largest which is why if
                // middle is less than or equal to target- we search upper half
            } else if (letters[middle] > target) {
                right = middle - 1;
                potentialSmallestGreatest = letters[middle];
                System.out.println(potentialSmallestGreatest);
            }
        }
        // we exit loop having iterated throuh all possibilities and target not found--> then check if at end of array (and not found - which would mean last execution
        return (left == letters.length) ?result : potentialSmallestGreatest; 
    }
}
```

### Attempt 4
This one is thanks to ChatGPT- a cleaner approach:
Notice how in this case, we just have the `result` variable. 
Result would be updated to the current middle, however we would continue iterating similar to attempt above until we 've narrowed it down.

Then, if we've exited the loop, we return `result`- which either is the updated value with the `middle` that is greater than target, or `result` was never updated.  With latter, tat means we've searched all searchable area and have ended up at the last letter (as no letter found to be greater than target), in which case that last letter (middle) would be less than target - and we would return initial value of `result` or `letters[0]`

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0;
        int right = letters.length - 1;
        char result = letters[0]; // Initialize with the smallest possible result

        while (left <= right) {
            int middle = (left + right) / 2;

            if (letters[middle] <= target) {
                left = middle + 1;
            } else {
                result = letters[middle]; // Update result to the current letter
                right = middle - 1;
            }
        }

        // After the loop, `result` will be the smallest letter greater than `target`
        // If `left` is within bounds, it means we have a valid `result`
        return result;
    }
}
```

---
# Javascript

```
const nextGreatestLetter = (letters, target) => {
    let left = 0;
    let right = letters.length - 1;
    let closestSoFar = letters[0];

    while (left <= right) {
        let middle = Math.floor(left + (right - left) / 2);
        if (letters[middle] <= target) {
            left = middle + 1;
        } else {
            closestSoFar = letters[middle]; // potential candidate
            right = middle - 1;
        }
    }

    // After exiting the loop,
    // If no character greater than target is found, return the first character
    return closestSoFar > target ? closestSoFar : letters[0];
};
```

Oh, and another cool thing I learned while doing this. I was wondering how come Javascript is slower than Java.
Here's the answer from ChatGpt "Java runs on the Java Virtual Machine (JVM), which is highly optimized for performance and has Just-In-Time (JIT) compilation, garbage collection, and other advanced optimization techniques." Also since Java is strictly typed, vs Javascript is dynamically typed which is why Javascript can have overhead. 