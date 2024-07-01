2418. Sort the People
---

# Problem

## Tags: 
#sorting, #hashmap, #collections-sort, #object-class, #constructor, #java-new-array 


**Link:** https://leetcode.com/problems/sort-the-people/description/    

**Problem Text:**    
You are given an array of strings names, and an array heights that consists of distinct positive integers. Both arrays are of length n.  

For each index i, names[i] and heights[i] denote the name and height of the ith person.  

Return names sorted in descending order by the people's heights.  

 

Example 1:  
Input: names = ["Mary","John","Emma"], heights = [180,165,170]  
Output: ["Mary","Emma","John"]   
Explanation: Mary is the tallest, followed by Emma and John.  

Example 2:  
Input: names = ["Alice","Bob","Bob"], heights = [155,185,150]  
Output: ["Bob","Alice","Bob"]  
Explanation: The first Bob is the tallest, followed by Alice and the second Bob.  

---

# Scratch Pad/ Pseudocode

// My first thought is there has to be some way to combine the key value pairs of height and the name, so they're associated with each other.   
//... thoughts continued below in initial solution...


## Initial Solution
I'm not too confident yet with hashmaps as they were not taught in bootcamp, but I am aware hashmaps allow for key, value pairs, so here's the inital solution I started to type out:
My thought was creating a hashmap with te peoples names and heights that would iterate throught all the the names and for each name, put the name and its respective height. 

```
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {

Map <Integer, String> peopleHeightMap = new HashMap <> ()
    
       for (int i = 0; i < names.length; i++) {
            map.put(names[i], heights[i]);
        }

...
    }
```

However, what I learned from my super knowledgeable walking encyclopedia mentor is that hashmaps aren't really meant for sorting. Hashmaps are kind of like buckets that catch the rain. When you add to to a hashmap, the thing aren't added in order- just like how rain would randomly fall into buckets. 

Hashmaps are just a way to tie a value to a key, like imagine people at a dance- and they are all standing seperately- and each girl has a colored bracelet that matches her guy (a key-value pair).

Technically though we would make a list of heights, sort through that, and then since heights are already tied to the names with hashmap's key-value feature, then we could just tie the names back. 

## Edge Case(s)
I was going to say duplicate heights could be an edge case but then I see the constraints say the heights are distinct. 

--- 

# Java Solution

## Attempt 1: Hashmap
Note: this solution was not right as it did not account for edge case of duplicate heights, and additionally the error I got in Leetcode's IDE was - "incompatible types: Integer cannot be converted to String," which was because I was trying to assign an Integer value (retrieved from sortedHeightsList) to a String array (sortedNames). Yes, a silly n00b attempt, however I was also able to learn as I attempted it; learnings are shared after this code block.

```
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        // Step 1: Create a HashMap to associate names with heights
        Map<String, Integer> nameHeightMap = new HashMap<>();
        for (int i = 0; i < names.length; i++) {
            nameHeightMap.put(names[i], heights[i]);
        }

        
         // Step 2: Convert the heights array to a List and sort it in descending order
        List<Integer> sortedHeightsList = new ArrayList<>();
        for (int height : heights) {
            sortedHeightsList.add(height);
        }
        Collections.sort(sortedHeightsList, Collections.reverseOrder());

        // Step 3: Use the sorted heights to fetch names from the HashMap
        String[] sortedNames = new String[names.length];
        for (int i = 0; i < sortedHeightsList.size(); i++) {
            sortedNames[i] = nameHeightMap.get(sortedHeightsList.get(i));
        }

        return sortedNames;
    }
}
```
So step 1 was already something I had in mind but...    

### Learnings
What I learned is you can actually get a List out of a hashmap.  We can do that by initializing a new array list called `sortedHeightsList`. Then, we iterate through the heights array and add each height to that List. We can then sort this list and use that sorted list to tie back the list values with the key-value pairs.   

Also, what's really neat is that Java has built in sorting methods.  `Collections.sort` is a method that sorts a List, and you then provide it with how you want it sorted. `Collections.reverseOrder` sorts a List in descending order.  (Note: that is why we initialized our heights as a List, not an array).

I also got a refresher on creating new arrays in Java. You have to declare the array type, and then in brackets, the length of the array.  
```
String[] sortedNames = new String[names.length];
```

Finally, we iterate over the `sortedHeightsList` and then we use direct index access to get the update the value of sortedNames. We have to do it this way because arrays are immutable 
We call the `.get` on the hashmap to get the key that matches the height on that `sortedHeights`.   

```
for (int i = 0; i < sortedHeightsList.size(); i++) {
            sortedNames[i] = heightNameMap.get(sortedHeightsList.get(i));
        }
```

But again, as aforementioned, there is the error of "incompatible types" and hashmaps just aren't meant for sorting. Let's move on to another attempt! 

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

Then, we use `Collections.sort` to sort by the height attribute with dot notation.
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
