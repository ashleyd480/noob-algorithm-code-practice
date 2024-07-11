# 1913. Maximum Product Difference Between Two Pairs

---


# Problem 

## Tags: 
#sort, #stream-api, #priority-queue, #heap, #initialization-point, #nested-if

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

Note: After looking at this, in the for loop (in my other solutions), I should have used two if/else statements- one to check for max and nextMax, and the other for min and nextMin to help the code stay clean. You can see this was fixed below.
Also note the extra else statements which fix how we update the `nextMin` and `nextMax`

```
class Solution {
    public int maxProductDifference(int[] nums) {
        int max = nums[0];
        int nextMax = nums[0];
        int min = nums[0];
        int nextMin = nums[0];

        for (int num : nums) {

            if (num > max) {
                nextMax = max; // we have a new max, so nextMax becomes the current value of max
                max = num; // max becomes the new large num
            } else if (num > nextMax) {
                nextMax = num; // if num is < max and this num > nextMax, then we should update our nextMax
            } 
            
            if (num < min) {
                nextMin = min;
                min = num;
            } else if (num < nextMin) {
                nextMin = num;
            }

        }

        int maxProductDifferenceResult = (max * nextMax) - (min * nextMin);
        return maxProductDifferenceResult;
    }

}
```

## Attempt 5

Ok, so that resolved the case of updating nextMax and nextMin but now we had another failed test where it read the min as 1, and then the nextMin as 1.

Example below:
array: [1,6,7,5,2,4,10,6,4]

num is 1 max is 1 and nextMax is 1 and min is 1 and nextMin is 1
num is 6 max is 6 and nextMax is 1 and min is 1 and nextMin is 1
...
num is 2 max is 7 and nextMax is 6 and min is 1 and nextMin is 1

When we reached the number 2, that should have updated nextMin but it didn't because num was not less than current value of nextMin which was 1. Therefore, we could not trigger the else if (num < nextMin). 

The problem is we can't have nextMin equal to min. If so, in this case min was equal the 0th index, and that number is the actual min. nextMin is also set as the 0th index which means ruh-roh- when we go to check if num < nextMin - which is still the value of that 0th array element, nothing can be less than the min. Hence, the value is not updated.

This means I have to initialize nextMin and nextMax to be value of index positon 1. However, after research, because we used our first two index values to initalize the values of max, nextMax, min, and nextMin-->  we have to start iterating from position 1. 

Key takeaway for myself: "The number of elements used for initialization directly affects where we start our main iteration. If we initialize with n elements, we typically start our main loop from index n."

```
class Solution {
    public int maxProductDifference(int[] nums) {
        int max = nums[0];
        int nextMax = nums[1];
        int min = nums[0];
        int nextMin = nums[1];

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > max) {
                nextMax = max;
                max = nums[i];
            } else if (nums[i] > nextMax) {
                nextMax = nums[i];
            }

            if (nums[i] < min) {
                nextMin = min;
                min = nums[i];
            } else if (nums[i] < nextMin) {
                nextMin = nums[i];
            }
           
        }

        return (max * nextMax) - (min * nextMin);
    }
}
```
---            

# Alternative Solution: Priority Queue

My mentor hinted I look up "priority queue". I looked it up and then I got wizard powers and I lived happily ever after.   
Just kidding! Still a n00b. >_<  But actually- so here's what I found. 


We initlize a priorityQueue called `maxHeap` which sorts the numbers in descending order. 
`poll()` retrieves and removes the head of the queue (the element with the highest priority in a min heap, or the lowest  in a max heap). In our max heap,  `poll()` removes and returns this largest element and second largest (both at the front of the "line")

We then initialize another priorityQueue called `minHeap` that has the numbers in default ascending order. 
`.poll()` retrieves the smallest numbers (also both at the front of that "line").

Note: even though this method appears to sort, in the context of a PriorityQueue in Java, the sorting process itself (done internally using a binary heap) is optimized to maintain efficient operations. In other words, the efficiency of a priority queue comes from its use of a heap data structure, not from fully sorting the elements. As elements are inserted, then the elements can be sorted to maintain the heap property as determined by the comparator. 
And what's nice and more efficient about this method is we can use `maxHeap` and `minHeap` to easily find the two largest and 2 smallest. 

## Attempt 1
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
## Attempt 2
Thanks to my awesome superstar mentor, he pointed out we could make this even more efficient because well Mr. Leet likes it fast and furious. 

Therefore, I thought= hey we can just run through the array of nums once and with each num iteration- add that num to both the maxHeap and the minHeap.

```
class Solution {
    public int maxProductDifference(int[] nums) {
        // Initialize a max heap (priority queue)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a); // provide comparator to sort descending 
         PriorityQueue<Integer> minHeap = new PriorityQueue<>(); // this sorts ascending

        // Insert elements into the max heap
        // Let's just iterate through the nums array once and for each num we use .offer to add to both the maxHeap and Min Heap 
        for (int num : nums) {
        maxHeap.offer(num);
         minHeap.offer(num);
        }

        // Extract the largest and second largest elements
        // Extract the smallest and second smallest elements
       

        int firstMin = minHeap.poll();
        int secondMin = minHeap.poll();

        int firstMax = maxHeap.poll();
        int secondMax = maxHeap.poll();

        // Calculate the maximum product difference
        int maxProductDiff = (firstMax * secondMax) - (firstMin * secondMin);

        return maxProductDiff;
    }
}
```

## Attempt 3
However, my mentor was like nuh uh- we can make this even speedier. I call him chad-gpt because he is seriously a code optimizing machine. 

So, he gave me a hint that anyways we only need 2 elements in the maxHeap and minHeap respectively. This means I don't have to write each num to those two heaps. 

This is where the nested if statement comes in- while we iterate through the array, we add each num to its respective heap. Then, once we are are over 2, we remove the one from the head. 
The maxHeap is ordered ascending a-b, so 1, 2, 3. So as soon the 3 is inserted (and the heap does it's magical heap stuff to maintain this ascending order), the 1 gets removed- helping us maintain 2-3 which would be the two max's. Similar logic for the minHeap. 


```
class Solution {
    public int maxProductDifference(int[] nums) {
        // limit the size to 2
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> a - b);
        // Min heap for the two smallest elements
        PriorityQueue<Integer> minHeap = new PriorityQueue<>((a, b) -> b - a);

        for (int num : nums) {
            //we only need 2 elements in each
            maxHeap.offer(num);
            if (maxHeap.size() > 2) {
                maxHeap.poll(); //and we can do this because maxHeap is sorted ascending so we're removing smaller ones at the head

            }

            // Update minHeap
            minHeap.offer(num);
            if (minHeap.size() > 2) {
                minHeap.poll(); // and we can do this because minHeap is sorted descending so we can remove da  big ones at da head
            }
        }

        // Extract the values
        int max2 = maxHeap.poll();
        int max1 = maxHeap.poll();
        int min2 = minHeap.poll();
        int min1 = minHeap.poll();

        // Calculate and return the maximum product difference
        return (max1 * max2) - (min1 * min2);
    }
}
```