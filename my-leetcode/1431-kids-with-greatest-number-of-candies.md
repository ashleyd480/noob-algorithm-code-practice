# 1431. Kids With the Greatest Number of Candies 

---


# Problem 

## Tags: 
#array

**Link:** https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/description/

**Problem Text:**   

There are n kids with candies. You are given an integer array candies, where each candies[i] represents the number of candies the ith kid has, and an integer extraCandies, denoting the number of extra candies that you have.

Return a boolean array result of length n, where result[i] is true if, after giving the ith kid all the extraCandies, they will have the greatest number of candies among all the kids, or false otherwise.

Note that multiple kids can have the greatest number of candies.


Example 1:

Input: candies = [2,3,5,1,3], extraCandies = 3  
Output: [true,true,true,false,true]   
Explanation: If you give all extraCandies to:  
- Kid 1, they will have 2 + 3 = 5 candies, which is the greatest among the kids.  
- Kid 2, they will have 3 + 3 = 6 candies, which is the greatest among the kids.  
- Kid 3, they will have 5 + 3 = 8 candies, which is the greatest among the kids. 
- Kid 4, they will have 1 + 3 = 4 candies, which is not the greatest among the kids. 
- Kid 5, they will have 3 + 3 = 6 candies, which is the greatest among the kids. 

Example 2:  
Input: candies = [4,2,1,1,2], extraCandies = 1  
Output: [true,false,false,false,false]   
Explanation: There is only 1 extra candy.  
Kid 1 will always have the greatest number of candies, even if a different kid is given the extra candy.

Example 3:  
Input: candies = [12,1,12], extraCandies = 10   
Output: [true,false,true]  

---

# Scratch Pad/ Pseudocode

// candies is array, candies[i]= num of candies the ith kid has  
// extraCandies= extra candies I have  
// subproblem:  so as I iterate through the array i will add extraCandies   
// then i can sort the array - and if the first number would be the max, then as we iterate through this sorted  array- return true in this resultArray if the num is equal to the max 
// doesn't seem this is sorted so we could either iterate through this loop once to add the candies, then iterate through it again to find the max but can we combine these two iterations?  
// then after that first loop, we can then loop through and add to the array list or...can we just put it under that one iteration? -- but that prob wouldn't work since we would have to find the currentMax first by getting through all candies  
// unless we sort the array first! then we'd know the max   
// BUT after a failed test case, this tells me that I need to sort after I add the candies!  
// BUT also I do see how if we sort the array, then it won't return the boolean in order - n00b sigh  
// OH but NOW I see- the "greatest" just means as a comparison against the current max   

## Ask what are the constraints:
n == candies.length 
2 <= n <= 100  
1 <= candies[i] <= 100  
1 <= extraCandies <= 50  




---

# Java Solution

```
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {

        int currentMax = 0; // to track the current max candies before new are added
        // initializing it with 0 as the problem does say only dealing with positive so
        // that's fine to have for baseline comparison

        for (int i = 0; i < candies.length; i++) {
            // find currentMax first

            // System.out.print("iteration" + i + " candy: " + candies[i]);
            if (candies[i] > currentMax) {
                currentMax = candies[i]; // assign currentMax to value of candy so we can find the max

            }

        }

        List<Boolean> candyTrackerList = new ArrayList<>(); // to keep track of boolean

        for (int i = 0; i < candies.length; i++) {
            candies[i] += extraCandies;
            System.out.print("iteration" + i + " candy: " + candies[i]);
            System.out.print("currentMax" + currentMax);
            if (candies[i] >= currentMax) {
                candyTrackerList.add(true);
            } else {
                candyTrackerList.add(false);
            }
        }

        return candyTrackerList;

    }
}
```

---


# What I Learned
1. First, I got a refresher on `Arrays.sort`. This method allows us to sort in ascending order, however this also "messes" up the order, because when we print the boolean array- it needs to be based on the original candies array. 

2. I made a n00b mistake. I was thinking let me use the  variable of "candy" to make my code easier to read in the for loop. Then, I was like wait what, why is my `currentMax` not updating? Then, I realized, the `currentMax` was stuck as the s.out showed it was the `currentMax` of the original array before candies were added. 
The reason is because candy is a local variable which exists in the loop and is assigned to `candies[i]` and we increment `candy` by the `extraCandies`. However, this does not actually update the value of the `candies[i]` so the array still holds its original values. 

```
for (int i = 0; i < candies.length; i++) {
int candy = candies[i]; // if I do it this way 
candy += extraCandies;
```

3. Another thing I learned from reading a solution on LC is this clearner boolean code.
I like this way of adding the boolean.

```
for(int i=0; i<n; i++){
    list.add(candies[i]+extraCandies >=max);
  }
```
