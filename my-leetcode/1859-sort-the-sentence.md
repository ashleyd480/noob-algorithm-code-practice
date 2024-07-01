1859. Sorting the Sentence
---

# Problem

## Tags:
#sorting, #hashmap, #collections-sort


**Link:** https://leetcode.com/problems/sorting-the-sentence/

**Problem Text:**    
A sentence is a list of words that are separated by a single space with no leading or trailing spaces. Each word consists of lowercase and uppercase English letters.  

A sentence can be shuffled by appending the 1-indexed word position to each word then rearranging the words in the sentence.  

For example, the sentence "This is a sentence" can be shuffled as "sentence4 a3 is2 This1" or "is2 sentence4 This1 a3".  
Given a shuffled sentence s containing no more than 9 words, reconstruct and return the original sentence.  

 
Example 1:
Input: s = "is2 sentence4 This1 a3"
Output: "This is a sentence"
Explanation: Sort the words in s to their original positions "This1 is2 a3 sentence4", then remove the numbers.  

Example 2:  
Input: s = "Myself2 Me1 I4 and3"  
Output: "Me Myself and I"  
Explanation: Sort the words in s to their original positions "Me1 Myself2 and3 I4", then remove the numbers.  

---

# Scratch Pad/ Pseudocode

// So each word's last char is the index position
// and the word is everything up to last character
// so we can create a new array list and we can make a new word class
// we can add the words and the word's index position (last char) as key-value pairs;



## Initial Thoughts
Here's a general pseudocode:

```
  // Step 1: Create a list of Word objects
    String words =
    List<Word> words = new ArrayList<>();for(
    int i = 0;i<words.length;i++)
    {
            words.add(new Word characters[i], numbers[i]));
        }

[word2, word1, word3]
as we add a word- it will have its respective characters and numbers attribute
so we would iterate through the sentence and for each word like word2, we would break that into its characters and number - we would have 2 arrays- one for the characters and one for the number

    // Step 2: Sort the list of Word objects based on word number
    Collections.sort(people,(a,b)->a.number-b.number);

    // Step 3: Remove the words last character- for each word by calling trim last
    // character
    return words
}

// Word class definition
class Word {
    String characters,
    int numbers

    // Constructor to initialize
    public Word(String characters, int numbers) {
        this.characters = characters;
        this.numbers = numbers;
```

## Edge Case(s)
I was going to say maybe extra spaces in front of word are edge case, but Leetcode says it's all single space. 

--- 

---

# Java Solution

## Attempt 1: 



---

# Javascript Solution

