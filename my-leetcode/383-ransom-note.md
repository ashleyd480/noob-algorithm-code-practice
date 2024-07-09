# 383. Ransom Note

---

# Problem 

## Tags: 
#found-flag, #hashmap

**Link:** https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target/description/

**Problem Text:**   

GGiven two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.

Each letter in magazine can only be used once in ransomNote.


Example 1:  
Input: ransomNote = "a", magazine = "b"  
Output: false  
Example 2:  

Input: ransomNote = "aa", magazine = "ab"  
Output: false  

Example 3:  
Input: ransomNote = "aa", magazine = "aab"  
Output: true  


---

# Scratch Pad/ Pseudocode

// at first I'm thinking of iterating through each string - ransomNote and magazine and writing them to an array of letters  

after researching, I see that would look something like this:
```
  for (int i = 0; i < ransomNote.length(); i++) {
            ransomArray[i] = ransomNote.charAt(i);
        }
        
```

// then, we would iterate through the ransomNote array - and in a nested for loop check to see if that first char in that ransomArray is present in the magazine array 
```
  for (int i = 0; i < ransomArray.length; i++) {
            boolean found = false;
            for (int j = 0; j < magazineArray.length; j++) {
                if (ransomArray[i] == magazineArray[j]) {// compare character
                    // if match, we can say found is true 
                    found = true;
        
                    break; // then we can break out of that iteration with that char that we just checked from ransomNote which did exist, then we can move on to the next ransomNote char
                }
            }
            // If character not found in magazine, return false
            if (!found) {
                return false;
            }
        }
```
However, we would need some way to keep track if the exact combo of letters exist... 

## Ask what are the constraints:
^Further note magazine.length <= 10^5; so imagine iterating through all those letters- that could be a lot and inefficient 

## What clarifying questions do I have:
Do the chars have to be in the exact order to count? i.e. if ransomNote = "aa"- would I need that exact order of letters in magazine? Answer: no, as long as the letters are present. 

## Further rumination
So with the above - we firstly want something more efficient than a nested for loop due to the max size that the magazine array can get up to, and also we need to ensure that not only does say the letter "a" from ransomNote exist in magazine, that exact - count- of letters needs to exist. Emphasis on the word "count".

Input: ransomNote = "aa", magazine = "aab"

// newHashmap <Char, Integer> we start with empty hashamp - this is our magazine hashamap  
// ransomNote.toCharArray and also write magazineNote to charArray?
// for each- (iterate through magazineNote) to charArray then call .put to put it into a hashmap 
// and kudos to my Indeedian friend Sal who talked this through with me today (giving credit where it's due)- he hinted why not in our array, we keep track of each char and its count 

---

# Java Solution

```

class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {

        HashMap<Character, Integer> magazineHashmap = new HashMap<>();

        for (int i = 0; i < magazine.length(); i++) {
            char magazineLetter = magazine.charAt(i); // the variable magazineLetter equal to the value charAt(i); No, you don't need to convert the string to an array first. The charAt(i) method allows you to access each character in the string directly. 
            if (magazineHashmap.containsKey(magazineLetter)) {
                magazineHashmap.put(magazineLetter, magazineHashmap.get(magazineLetter) + 1); // increment the count by 1 because that letter already exists ( and we need to do this because as we iterate through the magazine list- there can be dupe characters and we need to increase count)
            } else {
                magazineHashmap.put(magazineLetter, 1); // if letter did not exist yet in our blank hashmap
            }
        } 
        // now let's check to see if the magazineHashmap contains ransomNoteletter + associated count
        for (int i = 0; i < ransomNote.length(); i++) {
            char ransomNoteLetter = ransomNote.charAt(i);
            if (magazineHashmap.containsKey(ransomNoteLetter)) {
                magazineHashmap.put(ransomNoteLetter, magazineHashmap.get(ransomNoteLetter) - 1); // if letter is found, decrement the count-  and say for ransom note aa- if its found twice in magazine, then the count would decrease to 0 
           

                if (magazineHashmap.get(ransomNoteLetter) < 0) {
                    return false; // nested if statement means say we have 3 letter a's in ransom note and only 2 in magazine hashmap, then we would decrease magazineHashmap by 3 but it would now be negative: 2-3 which means magazine does have enough fo that count of char to do it 
                }
            } else {
                return false; // if ransom note letter not in magazine, return false
            }
        }

        return true; // if ransom note letter in magazine, and also there is sufficient count of each letter, then we good and it's a amtch
    }
}
```

**Step 1:** Initialize the Magazine Hashmap to Hold Character and Associated Count
We initialize a magazine hashmap that includes a key-value pair of the character and the integer count of each character. We make the character the key because hashmaps let us search for the key. We want to see if the magazine- which holds the greater amount of characters has the characters in ransomNote and the exact count of each character. 

In our first for-loop, we are iterating through the magazine string of characters, and then accessing the character at each index position in that string with `charAt(i)`. This loop is to put the each character of magazine and its count into a hashamp like: 
{(a,2), (b,1)}
- To do this, we check to see if this hashmap already contains that character from magazine `magazineCharacter`. Sure, the first time we pass by a letter- it will be its first occurence- so it's count is one- hence the "else" says `magazineHashmap.put(magazineLetter, 1)`.
- However if we already saw the letter (`if (magazineHashmap.containsKey(magazineLetter)`), then we `.put` to update the value associated with that character key. 
- `.put` can be used in hashmaps to both update a value tied to an existing key, or add a new key-value pair.  
When updating- in the value, we use the key.get(valueWeWantUpdatedTo) and in the if-statement- since we're saying we saw that letter already in magazine, then we'll just increase its count by 1 if we saw it again.

**Step 2:** Check if Magazine Hashmap Key Contain RansomNote's Char and Associated Count
Now, we want to iterate through the ransomNote, accessing its character at each iteration with `charAt(i)`.
- We want to check if the hashmap `containsKey` and pass it the `ransomNoteLetter` to check and also we want to see if they have the correct count. If ransomNote has 2 A's and so does magazine, then their difference should be 0. OR if magazine has more of the letter than ransomNote, that works too- so their difference should be positive. 
- To do that, we want to decrement the count of the value tied to the key (that letter) in magazine hashmap- and if it equals to 0 then we're good. However, say ransomNote has 3 A's and magazine only has 2 A's. Well, then 2-3 = -1. This means if ransomNote has more occurences of that letter than magazine, then magazine won't be able to spell out the ransomNote-- which means return false (hence our nested if to check if value < 0).   
- Remmber too it's okay if the value > 0. This just means say ransomNote has 3 A's and magazine has 4 A's, then we would decrement 4 by 3 to get 1, which is still fine- magazine has enough A's to cover the 
We use nested if when we want to check if the first if statement is true (letter is contained) and that the second if statement is also true (value < 0).  
- The "else" outside of the main if statement is saying if the letter is not in magazine, then our ransomNote can't be from magazine. So return false.
- However, if the main if statement passes - meaning the letter is contained and we didn't go into the second nested if (where count is not < 0)
