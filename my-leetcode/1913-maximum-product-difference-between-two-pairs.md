# 1913. Maximum Product Difference Between Two Pairs

---


# Problem 

## Tags: 
#sort, #stream-api, #priority-queue, #max-heap

**Link:** https://leetcode.com/problems/maximum-product-difference-between-two-pairs/description/

**Problem Text:**   
The product difference between two pairs (a, b) and (c, d) is defined as (a * b) - (c * d).

For example, the product difference between (5, 6) and (2, 7) is (5 * 6) - (2 * 7) = 16.
Given an integer array nums, choose four distinct indices w, x, y, and z such that the product difference between pairs (nums[w], nums[x]) and (nums[y], nums[z]) is maximized.

Return the maximum such product difference.

 
Example 1:  
Input: nums = [5,6,2,7,4]  
Output: 34  
Explanation: We can choose indices 1 and 3 for the first pair (6, 7) and indices 2 and 4 for the second pair (2, 4).
The product difference is (6 * 7) - (2 * 4) = 34. 

Example 2:  
Input: nums = [4,2,5,9,7,4,8]  
Output: 64  
Explanation: We can choose indices 3 and 6 for the first pair (9, 8) and indices 1 and 5 for the second pair (2, 4).
The product difference is (9 * 8) - (2 * 4) = 64.

---

# Scratch Pad/ Pseudocode

// this means we have to find the largest two numbers and the smallest two  
// we could use max and min but how to find the next max and next min?  
// ok, what if we sort it and then we could easily know it by the index positon  

## Ask what are the constraints:
While I was sweating bullets on this, my mentor also gave me a great idea to think about the constraints. For example, say if the samllest number could be negative and we multiply two negative numbers, that could be even greater than the two largest positive numbers. However, luckily Mr. Leet was merciful and had constraints that the smallest number would be 1. 



## Edge Case(s)
n/a

---

# Java Solution

## Attempt 1

So... based on my pseudocode above- the solution I came up was as follows. You can see how `Arrays.sort` is used to sort my nums array in ascending order.
Then, we find the two biggest numbers at the end (the last one and the second last one). Then, we find the two smallest ones (the first one and one after).

```
class Solution {
    public int maxProductDifference(int[] nums) {
        Arrays.sort(nums);
        int maxProductDifferenceResult= (nums[nums.length-1]*nums[nums.length-2]) -  (nums[0]*nums[1]);
        return maxProductDifferenceResult;
    }
}
```

However, sorting is not as efficient as iterating through the whole array. My idea behind the why is that well when you sort, you got to compare each item right- like imagine on the Bachelor show- going through a line of girls and sorting them (comparing each to the next)... vs just going through the line and looking at each girl. 
Iterating would be more time efficient, so on to attempt 2, and thank you my mentor Chad for being so patient as I blabbed and floundered through this on Friday. 

## Attempt 2
So at first I was thinking, yeah let's iterate through it twice, getting the max and then the min. And then, remove the max and min from the array, and iterate through again 2x to get the other max and min-- but that's 4 loops.  
In the words of Chad "what if we can iterate through it just once to get both?"   

Ok, so that got me thinking- yes, iterate through it once to get both the max and min would require an if/else statement. Say, if the number we are on is greater than the max, then we update the max-- and we if the number we are on is less than the min, then we update the min. This means, we have to also initialize those 2 variables before the start of the loop. 

Note: this is incomplete pseudocode below:

```
class Solution {
    public int maxProductDifference(int[] nums) {
        int min  = nums [0];
        int max = nums [0];
    for (int num: nums) {
        if (num > max) {
            max = num;
        }
        else if (num < min) {
            min = num;
        }
    System.out.println ("max is " + max + " and min is " + min); // used Leo's way to try to compile code and check values every so often to catch errors early on 
    }
    // remove the max and min and then run that loop up again to get the 2nd max and 2nd min 
    // then calculate the difference 
    // int maxProductDifferenceResult = 
    // return maxProductDifferenceResult;
    }
}
```
However, badumtis- as arrays are immutable in Java, removing the max and min would be another fun adventure. 
After I looked it up, I found the resource on GeeksForGeeks and it reccomended Java Stream API and that was an awesome refresher on a concept, and additionally, I learned how the Stream API can be used with `.toArray' as follows:

```
int[] newArray = Arrays.stream(nums)
                               .filter(num -> num != min && num != max)
                               .toArray();

```

## Attempt 3
Ok, but you can see how in Attempt 2, I still have to make another pass to find the 2nd max and 2nd min, and my awesome mentor who is like the superest smartest wizard ever was like what if we do that in one pass. I know we looked at the "closestSoFar" idea in another Leetcode problem, so maybe we could track something similar here. I hemmed and hawed, my brain spinning like a dazed washing machine trying to think. Sunday, I mapped it out and got aha moment.

// nums = [5,6,2,7,4]
// i = 0, max = 5, and num is not greater than 5 or less than, move on
// i = 1, max = 5, but num is greater - so max= 5 and we update nextMax to the max 
// ok and then along those lines, if we update the min to num, then nextMin would be the value of min 

```
class Solution {
    public int maxProductDifference(int[] nums) {
        int min = nums[0];
        int max = nums[0];
        int nextMin= nums[0];
        int nextMax= nums[0];
        
        for (int num : nums) {
            if (num > max) {
                nextMax = max;
                max = num;

            } else if (num < min) {
                nextMin = min;
                min = num;
               

            }
            System.out.println("max is " + max + " and nextMax is " + nextMax + " and min is " + min
                    + " and nextMin is " + nextMin);

        }
        int maxProductDifferenceResult = (max * nextMax) - (min * nextMin);
        return maxProductDifferenceResult;
    }
}


```


## Attempt 4
When I ran Attempt 3, it failed. I added an s.out to check the values. I noticed it was not updating the value of min in the last iteration.
```
  System.out.println("max is " + max + " and nextMax is " + nextMax + " and min is " + min
                    + " and nextMin is " + nextMin);
```

Note how in the final iteration that happens:
array: [5,6,2,7,4]

```
// s.out returned  
...
max is 7 and nextMax is 6 and min is 2 and nextMin is 5  
max is 7 and nextMax is 6 and min is 2 and nextMin is 5  
```

We check if the 4 is less than the current min of 2, which it's not so we don't update the nextMin- but 4 should be the `nextMin`. Therefore, we need also compare `nextMin` with num too to see if it should be updated.
If `nextMin` < num then let's update `nextMin` to num. 
We should also then do that for nextMax too - checking if nextMax > num.  

Note: After looking at this, in the for loop- I could have broken that into two if/else statements- one to check for max and nextMax, and the other for min and nextMin to help the code stay clean.

```
class Solution {
    public int maxProductDifference(int[] nums) {
        int min = nums[0];
        int max = nums[0];
        int nextMin = nums[0];
        int nextMax = nums[0];

        for (int num : nums) {

            if (num > max) {
                nextMax = max;
                max = num;
            } else if (num > nextMax) {
                nextMax = num;
            } else if (num < min) {
                nextMin = min;
                min = num;
            } else if (num < nextMin) {
                nextMin = num;
            }

            System.out.println("max is " + max + " and nextMax is " + nextMax + " and min is " + min
                    + " and nextMin is " + nextMin);

        }

        int maxProductDifferenceResult = (max * nextMax) - (min * nextMin);
        return maxProductDifferenceResult;
    }

}
```

Ok, so that resolved the case of updating `nextMax` and `nextMin` but now we had another failed test where it read the min as 1, and then the nextMin as 1.  

Example below:  
array: [1,6,7,5,2,4,10,6,4]

 
max is 6 and nextMax is 1 and min is 1 and nextMin is 1 - 2nd iteration 
max is 7 and nextMax is 6 and min is 1 and nextMin is 1 - 3rd iteration

When we reached the number 2, that should have updated `nextMin` but it didn't because num was not less than current value of `nextMin` which was 1. Therefore, we could not trigger the `else if (num < nextMin)`. On top of that, another test failed because Mr. Leet slyly decided we can make an array of duplicate values. 

## Attempt 5

My mentor hinted I look up "priority queue". I looked it up and then I got wizard powers and I lived happily ever after.   
Just kidding! Still a n00b. >_<  But actually- so here's what I found. 

We initlize a priorityQueue called `maxHeap` which sorts the numbers in descending order. 
`poll()` retrieves and removes the head of the queue (the element with the highest priority in a min heap, or the lowest  in a max heap). In our max heap,  `poll()` removes and returns this largest element and second largest (both at the front of the "line")

We then initialize another priorityQueue called `minHeap` that has the numbers in default ascending order. 
`.poll()` retrieves the smallest numbers (also both at the front of that "line").

Note: even though this method sorts, in the context of a PriorityQueue in Java, the sorting process itself (done internally using a binary heap) is optimized to maintain efficient operations. Therefore, the comparator does not put big dent into time complexity in this case. 

Da code below:
```
class Solution {
    public int maxProductDifference(int[] nums) {
        // Initialize a max heap (priority queue)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a); // provide comparator to sort descending 

        // Insert elements into the max heap
        for (int num : nums) {
            maxHeap.offer(num);
        }

        // Extract the largest and second largest elements
        int firstMax = maxHeap.poll();
        int secondMax = maxHeap.poll();

        // Extract the smallest and second smallest elements
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        for (int num : nums) {
            minHeap.offer(num);
        }
        int firstMin = minHeap.poll();
        int secondMin = minHeap.poll();

        // Calculate the maximum product difference
        int maxProductDiff = (firstMax * secondMax) - (firstMin * secondMin);

        return maxProductDiff;
    }
}
```
