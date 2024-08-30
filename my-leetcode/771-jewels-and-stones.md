# 771. Jewels and Stones

---

# Problem 

## Tags: 
#hashset

**Link:** https://leetcode.com/problems/jewels-and-stones/description/

**Problem Text:**  
You're given strings jewels representing the types of stones that are jewels, and stones representing the stones you have. Each character in stones is a type of stone you have. You want to know how many of the stones you have are also jewels.  

Letters are case sensitive, so "a" is considered a different type of stone from "A".  


Example 1:  
Input: jewels = "aA", stones = "aAAbbbb"  
Output: 3  

Example 2:  
Input: jewels = "z", stones = "ZZ"  
Output: 0  
  


---

# Scratch Pad/ Pseudocode

// each char in stones is a stone  
// so we can make a character array?  
// jewels are unique so we can write it as a set   
// as we iterate through stones we can see if it .contains in the set and if it  
// does increment the count  
// are stones sorted?  

## Ask what are the constraints:
1 <= jewels.length, stones.length <= 50
jewels and stones consist of only English letters.
All the characters of jewels are unique.




---

# Java Solution

```
class Solution {
    public int numJewelsInStones(String jewels, String stones) {

        HashSet<Character> jewelSet = new HashSet<>();
        char[] jewelsArray = jewels.toCharArray(); // Convert string to char array
        char[] stonesArray = stones.toCharArray(); // Convert string to char array

        for (char jewel : jewelsArray) {
            jewelSet.add(jewel);

        }
        int numStones = 0;
        for (char stone : stonesArray) {
            if (jewelSet.contains(stone)) {
                numStones++;

            }

        }
        return numStones;

    }
}


```
Here are the steps:

1. We know the characters of jewels are unique which allows to add them to a HashSet called `jewelSet`. 
2. To iterate through the characters of each string, we write our string to a character array using `.toCharArray()`. 
3. We want to see how many stones are also jewels. We use `numStones` to keep track.

4. We need to see if a character in `stones` also appears in `jewel`.  Thus, we could just iterate through each character in `stones` and then check if that character is `.contain` in `jewelSet`.  If so, then we increment `numStones` by 1.

5. Finally, we return `numStones`.