1859. Sorting the Sentence
---

# Problem

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
// so we can create a new array list 
// we can add the words and the word's index position (last char) as key-value pairs;
we can make a new `Word` class 
// then we can say 


## Initial Solution


## Edge Case(s)
I was going to say maybe extra spaces in front of word are edge case, but Leetcode says it's all single space. 

--- 

---

# Java Solution

## Attempt 1: 


## Attempt 2: List

My awesome bestest mentor in da world also reccomended the idea of simply using a List and a `Person` class.
The Person class has the height and class attribute, and we us the .this to help us know that when we initalize a `Person`, what its attribute values are.

```
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        // Step 1: Create a list of Person objects
        List<Person> people = new ArrayList<>();
        for (int i = 0; i < names.length; i++) {
            people.add(new Person(names[i], heights[i]));
        }

        // Step 2: Sort the list of Person objects based on height in descending order
        Collections.sort(people, (a, b) -> b.height - a.height);

        // Step 3: Extract the sorted names into an array
        String[] sortedNames = new String[names.length];
        for (int i = 0; i < people.size(); i++) {
            sortedNames[i] = people.get(i).name;
        }

        return sortedNames;
    }

    // Person class definition
    class Person {
        String name;
        int height;

        // Constructor to initialize name and height
        public Person(String name, int height) {
            this.name = name;
            this.height = height;
        }
    }
}
```

In our code, we started by initializing a list of `Person` objects: `new ArrayList<>`
Then we iterate through it and call `.add` to add a new `Person` instance that has the attribute of that index value we are iteratng over in names and heights array. 
Note: we used a List because you can stuff to a List in Java, vs arrays are immutable. 

Then, we use `Collections.sort` to sort:  
```
Collections.sort(people, (a, b) -> b.height - a.height);
```
We sort it to make "negative" and if we have (a,b) in descending like (2,1), then b-a will result in negative. (Note: we could have also used the `Collections.reverseOrder()`.)  

Now that our list of `Person` objects are sorted... remember each `Person` object has a name and height attribute. This means that we need to now extract the list of names. 
We by initializing an empty array of `sortedNames` and then we for each `Person` object we iterate through, we get that person's (`people.get(i)`) name. 

```
String[] sortedNames = new String[names.length];
for (int i = 0; i < people.size(); i++) {
sortedNames[i] = people.get(i).name;
}
```

---

# Javascript Solution
```
  const sortPeople = (names, heights) => {
        // Step 1: Create an array of Person objects
        let people = [];
        for (let i = 0; i < names.length; i++) {
            people.push({ name: names[i], height: heights[i] });
        }

        // Step 2: Sort the array of Person objects based on height in descending order
        people.sort((a, b) => b.height - a.height);

        // Step 3: Extract the sorted names into an array
        let sortedNames = people.map(person => person.name);

        return sortedNames;
    }
```

With Javascript, we can initialize an array of `Person` objects, as arrays are mutable in Javascript and we can call `.add` to add the `Person` objects.  
We can directly add the `Person` object "instances" as object key-value pairs:

```
people.push({ name: names[i], height: heights[i] });
```

Then, we sort the array with `.sort` and then the comparator of `b.height- a.height`. 

Finally, we use `.map` to make a copy of our `people` array so we can "transform" it to just get the names. 
