# 657. Robot Return to Origin

---


# Problem 

## Tags: 
#pair-class

**Link:** https://leetcode.com/problems/robot-return-to-origin/description/

**Problem Text:**   
There is a robot starting at the position (0, 0), the origin, on a 2D plane. Given a sequence of its moves, judge if this robot ends up at (0, 0) after it completes its moves.  

You are given a string moves that represents the move sequence of the robot where moves[i] represents its ith move. Valid moves are 'R' (right), 'L' (left), 'U' (up), and 'D' (down).  

Return true if the robot returns to the origin after it finishes all of its moves, or false otherwise.  

Note: The way that the robot is "facing" is irrelevant. 'R' will always make the robot move to the right once, 'L' will always make it move left, etc. Also, assume that the magnitude of the robot's movement is the same for each move. 

 

Example 1:  
Input: moves = "UD"  
Output: true  
Explanation: The robot moves up once, and then down once. All moves have the same magnitude, so it ended up at the origin where it started. Therefore, we return true. 

Example 2:  
Input: moves = "LL"  
Output: false  
Explanation: The robot moves left twice. It ends up two "moves" to the left of the origin. We return false because it is not at the origin at the end of its moves.  



---

# Scratch Pad/ Pseudocode

// robot starts at 0,0 --> want to see if it ends at 00  
// moves[i] - ith move  
// R, L, U, D 
// (L/R , U,D) - (x,y)  
// iterate through moves and if U-    
// Initialize the Pair object with the starting position (0, 0)  

## Ask what are the constraints:
1 <= moves.length <= 2 * 104  
moves only contains the characters 'U', 'D', 'L' and 'R'.  



---

# Java Solution


```
class Solution {
    public boolean judgeCircle(String moves) {
      
        Pair robotPair = new Pair(0, 0);

        for (int i = 0; i < moves.length(); i++) {
            char movesLetter = moves.charAt(i);
            if (movesLetter == 'U') {
                robotPair.verticalValue++;
            } else if (movesLetter == 'D') {
                robotPair.verticalValue--;
            } else if (movesLetter == 'L') {
                robotPair.horizontalValue--;
            } else if (movesLetter == 'R') {
                robotPair.horizontalValue++;
            }
        }

        // Check if the robot is back at the origin
        return robotPair.horizontalValue == 0 && robotPair.verticalValue == 0;
    }

    public static class Pair {
        int horizontalValue;
        int verticalValue;

        public Pair(int horizontalValue, int verticalValue) {
            this.horizontalValue = horizontalValue;
            this.verticalValue = verticalValue;
        }
    }
}
```
Here is the approach:

1. We create a `Pair` class that has attributes of  `horizantalValue` and `verticalValue`
Within it, we create a constructor so that when a `Pair` instance is initialized with the 2 parameters above, we can read its' values. 
```
  public Pair(int horizontalValue, int verticalValue) {
            this.horizontalValue = horizontalValue;
            this.verticalValue = verticalValue;
        }
```

2. In our main code block - we initialize a  `Pair` instance called `robotPair` to represent the coordinates where the robot is at - currently starting at (0,0).

3. Then we iterate through the `moves` string and go through each character- checking if the character is equal to `U`, `D`, `L`, and `R`. If it's `U` or `R` we respectively increment `verticalValue` and `horizantalValue` by one. If it's `D` or `L` we decrement by one. 

4. Finally, we use `return robotPair.horizontalValue == 0 && robotPair.verticalValue == 0;` to check if robotPair is back at the origin. At first I was checking if robotPair == (0,0). We can't do that since an object variables holds the reference address to that object so you can't compare it with the "key-value" pair of 0,0. You have to compare the attributes. 
