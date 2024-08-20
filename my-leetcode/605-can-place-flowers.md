# 605. Can Place Flowers 

---

# Problem 

## Tags: 
#helper-function 

**Link:** [insert link here]

**Problem Text:**   
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.  

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return true if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and false otherwise.  

 
Example 1:  
Input: flowerbed = [1,0,0,0,1], n = 1  
Output: true  

Example 2:  
Input: flowerbed = [1,0,0,0,1], n = 2  
Output: false 




---

# Scratch Pad/ Pseudocode

// flowers cannot be plant in adjacent plot  
// flwoerbed iwth plots  
// flowerbed 0 - empty 1- no empty  
// n = nmumber of new flower  
// we need a way to track the 2 flowers  

## Ask what are the constraints:
1 <= flowerbed.length <= 2 * 104  
flowerbed[i] is 0 or 1. 
There are no two adjacent flowers in flowerbed.  
0 <= n <= flowerbed.length 


## Initial Approach

```
if (n == 0) {
    return true;
}
int flowersPlanted = 0;
for (int i = 0; i <flowerbed.length; i++) {
    if (flowerbed[i]==0 && flowerbed[i-1] !=1 && flowerbed [i+1] !=1) { // checking that flower in middle hole not adjacent to flowers
    flowerbed[i] = 1;
    flowersPlanted ++;

    }
}

if (flowersPlanted >= n) {
    return true;
}
else return false;
    }
}
```


---

# Java Solution

```
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {

        int flowersPlanted = 0;
        for (int i = 0; i < flowerbed.length; i++) {
            System.out.println(i + " " + isAlreadyPlanted(flowerbed, i));
            if (!isAlreadyPlanted(flowerbed, i - 1) && !isAlreadyPlanted(flowerbed, i)
                    && !isAlreadyPlanted(flowerbed, i + 1)) { // checking that flower in middle hole not adjacent to
                                                              // flowers
                flowerbed[i] = 1;
                flowersPlanted++;

            }
        }

        return (flowersPlanted >= n); 

    }

    private static boolean isAlreadyPlanted(final int[] flowerbed, final int i) {
        // if index in array and array value at index is 1 then return true, else false
        return (i >= 0 && i < flowerbed.length && flowerbed[i] == 1);
    }

}
```
Here is the approach:
1. We want to keep track of `flowersPlanted` which are flowers that we can plant and compare that against `n` which is the deired number of flowers we want to plant. Let's initialize `flowersPlanted` outside the loop with 0.

2. Then, we check if `isAlreadyPlanted`. This helper function checks to see if the index is in the array so we don't go out of bounds, and if so then to see if a flower is already planted there by seeing if `flowerbed[i] == 1`. 

3. We want to check `isAlreadyPlanted` for the index or flower hole we are on `i`, as well as for the hole to the left `i-1` and hole to the right `i+1`. We want to see that nothing is planted in the hole that we want to place the flower in, and that nothing to the left or right (as that would violate the adjacency rule).

4. Finally, we would return and check if `flowersPlanted` is greater than or equal to `n`. This would essentially check if we were able to plant at least `n` flowers. 

---


# What I Learned
I learned about the helper function and how we should be using helper function to ensure that we don't glob code together in one function. In this case, we can leave the boolean check to a helper function. 

Another thing is no need to seperately check for a one-off say in this case if "n=0" since we already check for that with our `return (flowersPlanted >= n)`. This means it does account for fact if there were 0 flowers, `n` would be 0, and no flowers can be planted so `flowersPlanted` would be 0 too. 

Lastly, I learned that we don't need to use an if/else where it checks `if (flowersPlanted >=n) return true; else ... return false`. In those cases, we can simply just return the `true` expression and it would evaluate true or false for use as follows: `return (flowersPlanted >= n);` 
