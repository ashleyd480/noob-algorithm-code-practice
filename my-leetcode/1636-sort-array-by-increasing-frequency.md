# 1636. Sort Array by Increasing Frequency

---


# Problem 

## Tags: 
#pair-class #hashmap, #comparator

**Link:** https://leetcode.com/problems/sort-array-by-increasing-frequency/description/ 

**Problem Text:**   

Given an array of integers nums, sort the array in increasing order based on the frequency of the values. If multiple values have the same frequency, sort them in decreasing order.

Return the sorted array.
 

Example 1:  
Input: nums = [1,1,2,2,2,3]  
Output: [3,1,1,2,2,2]  
Explanation: '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.  

Example 2:  
Input: nums = [2,3,1,3,2]  
Output: [1,3,3,2,2]  
Explanation: '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.  

Example 3:  
Input: nums = [-1,1,-6,4,5,-6,1,4,1]  
Output: [5,-1,4,4,-6,-6,1,1,1]  





---

# Scratch Pad/ Pseudocode

// sort based on frequency (smallest to largest)  
// if we brute force array, then we would take each num and traverse entire array to check for count    
// ^ that would involve many traversals which is not efficient - we would also  
// need to maintain several count variables  
// ... unless we use arrays.sort - but even if we sort still going to have to  
// maintain severl coutn variables  
// we can't use set because that won't allow duplicates  
// each num needs to have a count which is why i'm thinking hashamp  
// we can't use hashmap to sort- but we can use hashmap to keep track of count  
// and then write hashmap to an array  

## Ask what are the constraints:
1 <= nums.length <= 100  
-100 <= nums[i] <= 100  

## What clarifying questions do I have:
Is the array sorted? No 
nums can be negative   


---

# Java Solution

## Attempt 1

```
class Solution {

    private static class Pair {
        public int number;
        public int frequency;
    
        public Pair(int number, int frequency) {
            this.number = number;
            this.frequency = frequency;
        }
    }

    public int[] frequencySort(int[] nums) {
        HashMap <Integer, Integer> frequencyMap = new HashMap<>();

  

      for (int num : nums) {// iterating through array to get frequency
      frequencyMap.compute (num, (k,v) -> (v==null)? 1: v+ 1);

      } 
        List<Pair> trackingList = new ArrayList <> ();


        // Populate the tracking list with pairs from the frequency map

     for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
            trackingList.add(new Pair(entry.getKey(), entry.getValue()));
        }


        // Sort the tracking list first by frequency (ascending) and then by number (descending)

    Collections.sort(trackingList, (a, b) -> {
    if (a.frequency != b.frequency) {
        return a.frequency - b.frequency; // ascending (increasing order based on frequency)
    }
    else {
        return b.number - a.number; // sort by decreasing order of number
    }
    });
    
    
    // Write list back to array
    int[] result = new int [nums.length];
    int index = 0;

    for (Pair pair: trackingList) {// iterate through the pairs in trackingList
        for (int i = 0; i < pair.frequency; i++) { // now for each pair, we iterate up to the frequency of that pair to print out all the numbers 
            result [index++] = pair.number;
        }
    }

return result;
    }
}
```
The approach is as follows:

1. We initialize a `frequencyMap` hashmap to keep track of the count of values for each key (number).

2. Then, we iterate through the array `nums` and write each `num` as a key and get its associated count. This is down with the `.compute` which as we go through each `num`- we take the (k,v) key-value pair and check to see is the value null (meaning the number hasn't been seen yet). If so, set the count to 1, otherwise if it's been seen- update its count by 1 more.

3. Since we can't sort hashamps on value, we can instead write these values to a `trackingList` of `Pair` data type. You can see how at the start of the code, we initialize a Pair class with its `number` and `frequency` attributes. We also define the constructor so we know that when we initalize a new Pair with the `number` and `freqency` that it will be assigned those values.

4. We use entry-set to iterate through the `frequncyMap` and then with each iteration add the `key,value` as a new `Pair` instance on the `trackingList`. 

5. Then, we use `Collections.sort` to sort the `trackingList`. We define a custom comparator to sort the pairs, where `a` and `b` represent pairs.

If the frequencies are different in each number-frequncey pair- we'll sort in increasing order based on frequency. Else, if the frequencies are the same, then we sort by decreasing order for the number in the `a` and `b` pair.

```
if (a.frequency != b.frequency) {
        return a.frequency - b.frequency;
    }
    else {
        return b.number - a.number; 
    }
```

6. Next, we write this ArrayList back to an array `result` since that is what our function requires as a return type. Also, this will allow us to write out all the numbers again in sorted format.

- Essentially, we iterate through the pairs in   `trackingList`. For each pair, we iterate up to the `frequency` of that pair. Let's say we have <1, 4> - that means we need to print the number 1 four times into our `result` array. 

- To accomplish that, we initialize a variable called `index` to track the index of the `result` array to which we are adding the number. Let's say adding the number 1 four times... So we will go through result[0], result[1], result[2], result[3] to add the number 1. ... or in other words `result [index++] = pair.number;`

- Finally, we return the result. 


## Attempt 2
Here is a cleaner version of that code.
We had to to import the `Collection` in order to use the its methods. `Collection`  is the parent interface of other interfaces like List, Set, and Queue.

Instead of writing the hashmap to an ArrayList, we just write the values of the hashmap as a Pair class which includes the "key-value" pairs. This way, then we can write the values to a `Collection<Pair>` by calling `frequencyMap.values();`. We can then convert that Collection to an array [ ] of Pair data type.
 with `.toArray(new Pair [0])`. Side note: the 0 is just so that when that array is created from the Collection, it will auto-size to what the Collection size is. 

 Then, we can just iterate over each Pair instance in that array, and render it in our `result` array. 
 
```
import java.util.Collection;
class Solution {

    private static class Pair {
        public int number;
        public int frequency;

        public Pair(int number, int frequency) {
            this.number = number;
            this.frequency = frequency;
        }
    };

    public int[] frequencySort(int[] nums) {
        HashMap <Integer, Pair> frequencyMap = new HashMap<>(); // we changed value to Pair

      for (int num : nums) {// iterating through array to get frequency
      frequencyMap.compute (num, (k,v) ->{
        //  (v==null)? 1: v+ 1
        if (v == null) { // v is the pair 
            return new Pair (num, 1);
        } else {
            v.frequency++;
            return v; // return updated pair
        }
      });
      }
// frequencyMap.values() - collection of values from map 

Collection <Pair> trackingCollection = frequencyMap.values();
Pair [] trackingArray = trackingCollection.toArray(new Pair [0]); // to create array of pairs 
    Arrays.sort(trackingArray, (a, b) -> {
    if (a.frequency != b.frequency) {
        return a.frequency - b.frequency; // ascending (increasing order based on frequency)
    }
    else {
        return b.number - a.number; // sort by decreasing order of number
    }
    });
    
    
    // Write list back to array
    int[] result = new int [nums.length];
    int index = 0;

    for (Pair pair: trackingArray) {// iterate through the pairs in trackingList
        for (int i = 0; i < pair.frequency; i++) { // now for each pair, we iterate up to the frequency of that pair to print out all the numbers 
            result [index++] = pair.number;
        }
    }

return result;
    }

}
```

---


# What I Learned
- We can't sort a hashmap by value, though we can sort by the keys if we use a treemap. 
- With hashmap - we can convert the entries of a hashmap ot a list (i.e. List <Map.Entry<K,V> and then sort by value. )