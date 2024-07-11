15# 1518. Water Bottles

---


# Problem 

## Tags: 
#while-loop

**Link:** https://leetcode.com/problems/water-bottles/description/?envType=daily-question&envId=2024-07-07

**Problem Text:**   

There are numBottles water bottles that are initially full of water. You can exchange numExchange empty water bottles from the market with one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Given the two integers numBottles and numExchange, return the maximum number of water bottles you can drink.

 

Example 1:  
Input: numBottles = 9, numExchange = 3  
Output: 13  
Explanation: You can exchange 3 empty bottles to get 1 full water bottle.  
Number of water bottles you can drink: 9 + 3 + 1 = 13.  

Example 2: 
Input: numBottles = 15, numExchange = 4  
Output: 19  
Explanation: You can exchange 4 empty bottles to get 1 full water bottle.   
Number of water bottles you can drink: 15 + 3 + 1 = 19.  
 

---

# Scratch Pad/ Pseudocode

// numExchange to get the one full water bottle 
// drinking a bottle turns it into empty bottle 
// numBottle is number of bottles you initially have - have to drink all of them  
// to exchange because they have to be empty  
// --> so we divide 9 by 3 in first example  
// numBottle/ numExchange= we get 3 new bottles that we can drink  
// then the 3/ 3 = 1 bottle that we exchange and we one more full one to drink  
// and because 1 < 3 (numExchange) - this means that we can't exchange  
// in the other example 15 bottles divided by 4 it's 3.75 and it appears we  
// round down  


---

# Java Solution


```
class Solution {
    public int numWaterBottles(int numBottles, int numExchange) {

        // numBottles current number of bottles for exchange
        int totalDrink = numBottles;
        int numNewBottlesFromExchange = 0;

        while (numBottles >= numExchange) {
            numNewBottlesFromExchange = numBottles / numExchange;
            numBottles = numNewBottlesFromExchange + (numBottles % numExchange);

            // we want to keep track of the remainder
            // we get 3 bottles from diviing 15/4 and then we have 3 left which is the new
            // numBottles we can exchange
            totalDrink = numNewBottlesFromExchange + totalDrink;
            System.out.println(numBottles);
            System.out.println(numNewBottlesFromExchange);
        }
        // we want to keep dividing those two and getting new value of numBottles until  
        // numBottles < numExchange  
        // we need to keep track of the numBottlesFromExchange - adding on the new value  
        // of numBottlesFromExchange to the new value  

        return totalDrink;
    }
}

```

We start by initializing a `totalDrink` - which represents the total number of bottles you can drink.
We also initalize`numNewBottlesFromExchange`  which is equal to how many bottles you can get from the exchange.
I figure as long as the `numBottles` we have - which is the current number of bottles we have for exchange is greater than equal to to the `numExchange` (or number needed for exchange), then we can make an exchange. 

One thing I noticed thorugh my test cases and you can see my notes below from failed test cases, how I modify my code- but the main thing is remembering about the remainder. Even if we say when we start with 15 bottles and you need 4 bottles to get 1. Sure, you would get 3 bottles back- but don't forget to take into account your remainder which is then added to the `newBottlesFromExchange` to become your new `numBottles`.

// 100- 50- 25- 12 - 6 - 3 - 2

// 50 /2
// learning from failed test case- start with 9 and 3 is exchange num
// i changed the while loop to >= because when I had 9 and 3, it output 12
// because 9/3 =3 and 3 is not greater than 3

// also i did int totalDrink = numBottles - when i was printing out values of
// num bottles- i got 1 and 3 to total 4, it did not add the 9

// then i failed test case regarding how 15 bottles and 4 to exchange; this
// means ok we still have the remainder leftover - 15 total and 4 exchange- even
// though we ended up with 3 i forgot to take into account the remander - so if there is remainder divide and then add 1 


// then test 3 failed 15, 4 - and so we get 3.75 which is round down to 3 

// now we fail again - 15, 4 we get 6 and 3 which represent bottles we can exchange but we want to get bottles to drink