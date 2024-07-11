# 1598. Crawler Log Folder

---

# Problem 

## Tags: 
#string-manipulation 

**Link:** https://leetcode.com/problems/crawler-log-folder/description/

**Problem Text:**   

The Leetcode file system keeps a log each time some user performs a change folder operation.

The operations are described below:

"../" : Move to the parent folder of the current folder. (If you are already in the main folder, remain in the same folder).
"./" : Remain in the same folder.
"x/" : Move to the child folder named x (This folder is guaranteed to always exist).
You are given a list of strings logs where logs[i] is the operation performed by the user at the ith step.

The file system starts in the main folder, then the operations in logs are performed.

Return the minimum number of operations needed to go back to the main folder after the change folder operations.

 

Example 1:  
Input: logs = ["d1/","d2/","../","d21/","./"]  
Output: 2  
Explanation: Use this change folder operation "../" 2 times and go back to the main folder.  

Example 2:  
Input: logs = ["d1/","d2/","./","d3/","../","d31/"] 
Output: 3  

Example 3:  
Input: logs = ["d1/","../","../","../"]  
Output: 0 



---

# Scratch Pad/ Pseudocode

// logs is the list of string
// logs [i] is the operation performed by user
// assumed we start in main folder before list of operations performed
// return min # of operations to go back to the main folder
// we would have to parse each string to include everything before the / symbol
// and maintain a count variable
// - if the parsed part does not contain . (x/) then we move to child folder -
// so + 1
// - if the parsed is a ./ , we remain in that folder so do nothing to the count
// - if the parse contains a .. , we go up a folder so subtract one


---

# Java Solution

```
class Solution {
    public int minOperations(String[] logs) {

        int count = 0;

        for (String log : logs) {

            if (log.contains("..")) {
                if (count > 0) {
                    count = count - 1;
                } else if (count < 0) {
                    count++;
                }

            } else if (!log.equals("./")) {
                if (count >= 0) {
                    count++;
                } else {
                    count = count - 1;
                }
            }

        }
        return count;
    }
}


```

Below are some further notes I made while attempting this question. One thing I'm doing is as I compile and run into failed test cases, I try to understand why did the test case fail and see how I can modify my code.

// 1st failed test case got me thinking:
// ["d1/","../","../","../"]

// move to  
// child folder+1..  
// move back up-1  
// move to  
// back up-1-->-1  
// move back up-1-->-2  

// so this tells me hey, when we're at count 0, and we're moving up a folder to  
// parent, then we should be adding +1 and if we were moving down to a child -  
// we would subtract  

// 2nd failed test case 
// ["./","../","./"]  
// start at 0 in first iteration - count is 0  
// then the 2nd operation goes we go up a folder - but we wouldn't subtract one  
// because we're at 0 already  
// we would add one to the count  
// this makes me see that if we start with ./ main level folder that would mean  
// we would similarly reverse the operatoins similar to 1st test case findings  
 
// ^ actually I forgot this "constraint"--- (If you are already in the main
// folder, remain in the same folder) for "../" - which means i should change
// count to < 0 because if at 0, we don't want to do anything to count

