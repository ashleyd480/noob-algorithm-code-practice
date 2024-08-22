# 1859. Sorting the Sentence
---

# Problem

## Tags:
#sorting, #hashmap, #collections-sort, #substring, #split, #parseInt


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
    Collections.sort(word,(a,b)->a.number-b.number);

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

## Attempt 1
We worked on this in one of our mentor's office hours. Ty Leo for helping me whiteboard and code this out together with you during your office hour!

```
class Solution {
    public String sortSentence(String s) {
        // break apart by white spaces to make collection of strings

        String[] words = s.split("\\s+");
        System.out.println("I have parsed the " + words.length);
        // last character to seperate the number from word

        String[] mySentenceArray = new String[words.length];
        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            String wordPart = word.substring(0, word.length() - 1);
            String numberPart = word.substring(word.length() - 1, word.length());
            Integer numberPartInteger = Integer.parseInt(numberPart);
            System.out.println("This is a wordpart " + wordPart);
            System.out.println("This is a numberpart " + numberPart);
            mySentenceArray[numberPartInteger - 1] = wordPart;
        }
        String completedSentence = "";
        for (int i = 0; i < mySentenceArray.length; i++) {
            if (i < mySentenceArray.length - 1) { // looking to see if not last word 
                completedSentence += mySentenceArray[i] + " ";
            } else { // this is the word 
                completedSentence += mySentenceArray[i];
            }
        }
        return completedSentence;
    }
}
```

While solving this, we learned from Leo how to use Google and specficially StackOverflow and Baeldung to research syntax. 
We also used some s.out statements to compile code as we went along to ensure the code compiles, and that our code is correctly producing the right output. This allows us to find the error sooner than later, and this also helps make the error easier to identify since we would have less lines of code to debug.
For example, we did s.out to check the wordPart and numberPart were correctly splicing with `substring`. 

I learned that for the `substring` method: when you specify word.substring(startIndex, endIndex), it will extract characters starting from startIndex up to (but not including) endIndex. Also, I realized after submitting my solution above-- that the reason I had to use `parseInt` was because I declared my `numberPart` as a type `String`. Had I just declared it as an Intger, it would not have been an issue. 

Another thing we learned in this exercise is initializing variables. In this case, we initalized a variable to represent the array to hold the sorted sentence outside of the for loop. This way, as we iterate through, we could assign each `wordPart` to its respective index in that sorted sentence array. That index is represented as `numberPartInteger-1` because the numbers start with `1` in the sentence. (By the way, we found that fix out when we got an out of bounds index error when we tried to compiled our code).

Then, we wrote that sorted setnence array back to a sentence by using string concatentation. 
That is we did ` completedSentence += mySentenceArray[i] + " ";`. 
Written out that is: `completedSentence = completedSentence + " " + mySentenceArray[i];`.

We noticed a space after the last word in a failed test case. Subsequently, I used an if/else statement that would only add a space if we were not at the last word.


## Attempt 2

```
class Solution {
    public String sortSentence(String s) {

        String[] words = s.split("\\s+");

        String [] sortedString = new String [words.length];
        int sortedStringIndex = 0;
       
        for (String word : words) {
          // Extract index and convert char to an int
            int index = word.charAt(word.length() - 1) - '0'; // Convert char to int
            String actualWord = word.substring(0, word.length() - 1);

            // Place the word in the correct position in the array
            sortedString[index - 1] = actualWord;

        }


        // for (int i = 0; i < sortedString.length; i++) {
        //     if (i < sortedString.length - 1) {
        //         sortedStringResult = sortedStringResult + sortedString[i] + " ";

        //     } else {
        //         sortedStringResult = sortedStringResult + sortedString[i];
        //     }

        // }

        //stringbuilder better
         StringBuilder sortedStringResult = new StringBuilder();
        for (int i = 0; i < sortedString.length; i++) {
            if (i < sortedString.length - 1) {
                sortedStringResult.append(sortedString[i]).append(" ");
            } else {
                sortedStringResult.append(sortedString[i]);
            }
        }

        return sortedStringResult.toString();
```

1. In this second attempt, we have a similar start where split the sentence by whitespace into an array of words, called `words`.

2. Then, we initialize a new sortedString array that has the same length as `words`.

3. After that, we iterate through each word in `words`.  The `index` is the last character of each word. The `actualWord` is the actual word with the last number scrubbed off.

4. We can simply just write `sortedString[index]` and that will assign the `index` position of that result array the corresponding `actualWord`.

5. Then, we can either use string concatenation or stringbuilder to append the words from `sortedString`. 

6. We know each word up until the last one has a space after it, hence the first if statement. Then, the final word, we simply appen the word. 



---


# What I Learned

I learned in this problem how we can use the index value to write a value to an array. In this case, I don't have to go in order and add the values- but rather say I can just call arrays[i]= value;. This would assign value at that index position. Yes, I was today's years old when that clicked. 