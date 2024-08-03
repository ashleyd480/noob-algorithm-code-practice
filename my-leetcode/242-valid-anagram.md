# 242. Valid Anagram
---


# Problem 

## Tags: 
#array #hashmap

**Link:** https://leetcode.com/problems/valid-anagram/

**Problem Text:**   
Given two strings s and t, return true if t is an anagram of s, and false otherwise.  

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.  

 

Example 1:  
Input: s = "anagram", t = "nagaram"  
Output: true  

Example 2:   

Input: s = "rat", t = "car"  
Output: false  
 




---

# Scratch Pad/ Pseudocode
// anagram- can rearrange and use all the origianl letters exactly once  
// check count of each letter- using variable to keep track of count of letters  
// 26 variables for string S and 26 for string T- huge space complexity  
// was thinking at first use the same 26 variables and then reset count before string T  
// but we need to maintain count seperately to compare counts  
// and the compare which variables have count > 0 
// see is counts are similar  
// later did decide to go with hashmap approach



## Ask what are the constraints:
1 <= s.length, t.length <= 5 * 104  
s and t consist of lowercase English letters.  


## What clarifying questions do I have:


## Edge Case(s)
Edge case is if the strings are different length
```
if (s.length() != t.length()) {
    return false;
```

## Initial Approach

Here was my initial naive approach.


```    
        int countAs = 0;
        int countBs = 0;
        ...
        int countZs = 0;

        // edge case different length
        if (s.length() != t.length()) {
            return false;
        }
        for (i = 0; i < s.length(); i++) {
            char letter = s.charAt(i);
            if (letter == A ) {
                countA ++
            }
            ...
             if (letter == Z ) {
                countZ ++
            }

        }

        int countRs = 1
         int countAs = 1
         int countAt= 1

        
        int countAt = 0;
        int countBt = 0;
        ...
        int countZt = 0;
        for (i = 0; i < t.length(); i++) {
            char letter = t.charAt(i);
            if (letter == A ) {
                countA ++
            }
            ...
             if (letter == Z ) {
                countZ ++
            }

        }

/// for count varialbes we have for string S- we would see which one are >0 
        if (countAs == countAt = )
            return true;
        }
        else return false;
        
    }

```


---

# Java Solution
```
class Solution {
    // anagram- can rearrange and use all the origianl letters exactly once
    public boolean isAnagram(String s, String t) {
          if (s.length() != t.length()) {
            return false;
        }
         Map<Character, Integer> stringCountMapS = new HashMap<>();
         for (int i = 0; i < s.length (); i++) {
            char letter = s.charAt(i);
              if (stringCountMapS.containsKey(letter)) {
    
                stringCountMapS.put(letter, stringCountMapS.get (letter) + 1);
            }
            else  { stringCountMapS.put(letter, 1 ); 
            }// we added a letter 
   
        }

         Map<Character, Integer> stringCountMapT = new HashMap<>();

              for (int i = 0; i < t.length (); i++) {
            char letter = t.charAt(i);
         
            if (stringCountMapT.containsKey(letter)) {
                stringCountMapT.put(letter, stringCountMapT.get (letter) + 1);
            }
              else  { stringCountMapT.put(letter, 1 ); 
              }

        }

        for (Map.Entry<Character, Integer> entry : stringCountMapS.entrySet()) {
    Character key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println("key is " + key + "and value is " + value);
   
    if (!stringCountMapT.containsKey(key) || !stringCountMapT.get(key).equals(value)) {
                return false;
            }
        }
    
return true;
    
}
}
```

The approach is as follows:
1. Firstly, we check to see the string lengths. If they aren't equal, that immediately disqualifies it as an anagram.

2. Then we initialize a hashmap for both string S and string T: `stringCountMapS` for String S and `stringCountMapT` for String T

3. For each hashmap, we iterate through them- keeping track of the count. We use an if-else statement to correctly increment the count. If we've seen the letter, then we use `.put` and `.get(letter` to get the current value count and then increment by 1. ... Else if we haven't seen the letter, we just set the value to 1.

4. Then, we iterate through the key value pairs in the `stringCountMapS`. Each iteration prints the key and the value from `stringCountMapS` - or in other words the unique character and its count.

5. As we print a key and value in each iteration, we want to see whether `stringCountMapT` contains the key or if the value associated with that key in `stringCountMapT` is equal to the value we get. 
If either is false, that means it's false and not an anagram.

In other words, if the hashamp for String T doesn't contain that unique character from String S hashmap, it's not an anagram.
Also, if the hashmap for String T has the letter but the count is not right, then it's not an anagram either. (For that reason we the `||` (or) operand.)

We do the `!` check so this way as we iterate through and there's a condition that fails for an iteratiion, we don't have to keep iterating through. 

```
   if (!stringCountMapT.containsKey(key) || !stringCountMapT.get(key).equals(value)) {
                return false;
            }
```

Note also for the above when comparing if the count value in `stringCountMapT` does not equals to the `value` we get from `stringCountMapS`- note since in this case we are dealing with non-primitve `Integer` - we need to use `.equals` vs the `=` sign. This is because with non-primitives, `.equals` allows us to compare the actual value, vs `=` means we are comparing their reference address. 

## Time Complexity 
Since it's using hashmaps, we can use the .contains which helps make it more efficient in that sense. However, then iterating through each element in the hashmap makes it linear complexity.
Also, initializing the new hashmaps add space complexity in terms of memory.


## Any Better Algo?
Yes! My dear friend and algo genius Prakash and I were talking about this. I had just finished my mock interview with this question and he was like "how about you use map.compute and you can use one map."
(By the way, he was able to figure that out in less than a minute so I think he has a built in GPT in his brain ;)

So here's what I found:

```
  for (char letter : s.toCharArray()) {
            stringCountMap.compute(letter, (k, v) -> (v == null) ? 1 : v + 1);
        }
```

`compute` is a function that is called on the `map`. `compute` is a way to update the value of a key in the map. In the code above, we are passing the 2 parameters- the `letter` and an anonymous function that takes the key and value as parameters. This function checks if the `value` for each key is null (which means we didn't see the letter). If it's null, the letter's value (or coun) would be 1. If we've seen it, increment the letter count by 1. 

Regarding using one map, we could initialize a map with count of characters in String s using `map.compute` shown above. Then, as we iterate through String t, then we would decrement the count using `map.compute` as we go through string T- by checking if the map of letters we have in String S contains the key (letter). If they have the equal amount of characters, when we iterate through the values, they should be equal to 0. 

```
        for (char letter : t.toCharArray()) {
            if (!charCountMap.containsKey(letter)) {
                return false;
            }
            
        }
        ...
         for (int count : charCountMap.values()) {
            if (count != 0) {
                return false;
            }
        }
```