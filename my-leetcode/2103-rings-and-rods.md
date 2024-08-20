# 2103. Rings and Rods

---


# Problem 

## Tags: 
#hashmap #hashset

**Link:** https://leetcode.com/problems/rings-and-rods/description/

**Problem Text:**   

There are n rings and each ring is either red, green, or blue. The rings are distributed across ten rods labeled from 0 to 9.  

You are given a string rings of length 2n that describes the n rings that are placed onto the rods. Every two characters in rings forms a color-position pair that is used to describe each ring where:  

The first character of the ith pair denotes the ith ring's color ('R', 'G', 'B').  
The second character of the ith pair denotes the rod that the ith ring is placed on ('0' to '9'). 
For example, "R3G2B1" describes n == 3 rings: a red ring placed onto the rod labeled 3, a green ring placed onto the rod labeled 2, and a blue ring placed onto the rod labeled 1. 

Return the number of rods that have all three colors of rings on them. 

 
Example 1: 
Input: rings = "B0B6G0R6R0R6G9"  
Output: 1  
Explanation:  
- The rod labeled 0 holds 3 rings with all colors: red, green, and blue.
- The rod labeled 6 holds 3 rings, but it only has red and blue.
- The rod labeled 9 holds only a green ring.
Thus, the number of rods with all three colors is 1.

Example 2: 
Input: rings = "B0R0G0R9R0B0G0"  
Output: 1  
Explanation:   
- The rod labeled 0 holds 6 rings with all colors: red, green, and blue.
- The rod labeled 9 holds only a red ring.
Thus, the number of rods with all three colors is 1.

Example 3: 
Input: rings = "G4" 
Output: 0  
Explanation:   
Only one ring is given. Thus, no rods have all three colors. 


---

# Scratch Pad/ Pseudocode

// n rings  
// each ring is red, green, or blue  
// 10 rods 0-9  
// rings is a string  
// color-posiiton pairs - first of pair : color; second of pair: rod positon 0-9  

## Ask what are the constraints:
rings.length == 2 * n
1 <= n <= 100
rings[i] where i is even is either 'R', 'G', or 'B' (0-indexed).
rings[i] where i is odd is a digit from '0' to '9' (0-indexed).




---

# Java Solution

```
class Solution {
    public int countPoints(String rings) {

        HashMap<Character, Set> ringMap = new HashMap<>();
        for (int i = 0; i < rings.length(); i++) {
             if (i % 2 != 0 && i >= 0) {// to avoid out of bounds
                char rodPosition = rings.charAt(i);
                char ringColor = rings.charAt(i - 1);

                if (!ringMap.containsKey(rodPosition)) {
                    HashSet<Character> colorSet = new HashSet();
                    colorSet.add(ringColor);
                    ringMap.put(rodPosition, colorSet);
                }

                else {

                    // ringMap.put(rodPosition, ringMap.get(rodPosition).add(ringColor));
                    Set<Character> updatingColorSet = ringMap.get(rodPosition);
                    updatingColorSet.add(ringColor); // Add the ringColor to the existing set
                }
            }
        }

        int numberOfRodsThreeColors = 0;
        // at least 1 red, 1 green, 1 blue

        for (Set rodSet : ringMap.values()) {
            if (rodSet.size() == 3) {
                numberOfRodsThreeColors++;
            }

        }

        return numberOfRodsThreeColors;

    }

}
```

Here's the approach, and a quick hint I learned from my amazing wizard mentor Chad - "whilst a problem asks you if you've seen something at least once but count doesn't matter, then thou shalt use thy hashset". He is much wizard.

Steps:
1. We start off by initializing a new HashMap `ringMap` where the rod character is the key, and the colors on that rod are the value, represented by a `HashSet`. We will call this HashSet `colorSet` and it will represent the unique colors on the rod. This is because with hashset, even if there are multiple of each color on the ring, each color can be added only once. 

2. We iterate through the string of characters, and we see that the color and rod are in pairs. The rod is always on the odd indexes, so we check if `i % 2 != 0`. 

3. For each iteration, the `rodPosition` is  `rings.charAt(i);` and `ringColor` is `rings.charAt(i - 1);`. Because of this, we want to check that `i >=0` to avoid out of bounds with the (i-1) index of `ringColor`.

4. Then, with each iteration, we check to see if the key `rodPosition` already exists. If not, then that means we need both add that new rod to the map as a key, and also initialize a new `colorSet` and add that color in the pair of that ieration that we just saw.

5. Else, if we've see the rod already, we can simply fetch the hashset for that rod with `ringMap.get(rodPosition)`, and we can add the `ringColor` of that pair.

6. Once we are done iterating through that string, we can then see how many rods have 3 colors - we initialize a variable called `numberOfRodsThreeColors`. 

7. ^ We can do that by iterating through the values of the  `rodSet`, and if the `rodSet` size is 3- meaning that 3 unique colors were able to be added, then we can increment the count of `numberOfRodsThreeColors` by one. We would return the final value of `numberOfRodsThreeColors`.


---


# What I Learned
I was today's years old when I learned that hashmap can have other data structures for value. For example, in this case we can use a hashset for the value. Um, mindblown!

Oh, another way I could have done this with the pairs instead of checking if i is odd is to just step by 2 to process pairs! And I oop.

```
for (int i = 0; i < rings.length(); i += 2) { // Step by 2 to process pairs
        char ringColor = rings.charAt(i);
        char rodPosition = rings.charAt(i + 1);
```