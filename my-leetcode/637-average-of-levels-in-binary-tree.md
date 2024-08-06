# [Problem number]. [Problem Title Here]

---


# Problem 

## Tags: 
#binary-tree, #breadth-first-search

**Link:** [insert link here]

**Problem Text:**   

Given the root of a binary tree, return the average value of the nodes on each level in the form of an array. Answers within 10-5 of the actual answer will be accepted.  
 

Example 1: 
Input: root = [3,9,20,null,null,15,7]  
Output: [3.00000,14.50000,11.00000]  
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.  
Hence return [3, 14.5, 11].  

Example 2:  
Input: root = [3,9,20,15,7]  
Output: [3.00000,14.50000,11.00000]  
 

---

# Scratch Pad/ Pseudocode

// traverse through it  
// first level; root.val  
// second: root.left.val + root.right.val / 2 ; edge case is if one is null - if left is null- it would just be right  
// fill a list with the node values, every 2^n- we can add those values and if nulls we can ignore them  

## Ask what are the constraints:
The number of nodes in the tree is in the range [1, 104].  
-231 <= Node.val <= 231 - 1  


---

# Java Solution

## Attempt 1

Note: this approach only accounts for balanced trees. I had failed to initally realize that binary trees can be unbalanced. 

```
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> averageOfLevels = new ArrayList<>();
        Queue<TreeNode> treeQueue = new LinkedList<>();
        treeQueue.add(root);
        int count = 1; // total count at that level including null to determine level
        int level = 0;
        int levelCount = 1; // only count leaves with values in that level
        double runningSum = root.val;

        while (!treeQueue.isEmpty()) {

            TreeNode tempNode = treeQueue.poll();
            // first iteration at the root
    
            System.out.println("curent level at start of iteration is " + level);
            System.out.println("curent node at start of iteration is " + tempNode.val);
            System.out.println("runningSum " + runningSum);
            System.out.println("count is " + count);

          
              if (count == (int) Math.pow(2, level)) {

                averageOfLevels.add(runningSum / levelCount);
                System.out.println("level count iscount " + levelCount);
                count = 0; // reset count
                level++;
                runningSum = 0; // reset running sum after level

                System.out.println("next level is " + level);
                levelCount = 0; // because level count specific to level we reset it

            }
   

            if (tempNode.left != null) {
                treeQueue.add(tempNode.left);
                runningSum = runningSum + tempNode.left.val;
                levelCount++;
                count++; // even null ones are nodes and this way keep track for the 2^n

            }
            if (tempNode.right != null) {
                treeQueue.add(tempNode.right);
                runningSum = runningSum + tempNode.right.val;
                levelCount++;
                count++; // even null ones are nodes and this way keep track for the 2^n

            }
    
            if (tempNode.left == null || tempNode.right == null) {

                count++;
            }
        
        }

        return averageOfLevels;
    }
}
```

Here is the approach; again note- this only works in a balanced tree situation. 

1. We start off by initialzing some variables:
-  `averageOfLevels` is used to keep track of the average across each level.
- `treeQueue` is a linked list-  we add the `root` which is received as the parameter.
- We also keep track of the `count` - which is total count of nodes at that level including null nodes- as we need the count of nodes to determine the level. We start with a count of 1 as we already added the `root`
- `level` is the level we're on as we're doing breadth-first-search.
- `levelCount` counts the leaves with values in that node which helps when we are doing the average per level as we only want to divide by number of nodes with value. This is reset after each level.
- `runningSum` is the sum of that level which we divide by `levelCount` to get the average. This is reset after each level. 

2. We know we're at the next level, because with a balanced binary tree, our first level has 2^0, second level has 2^1, and third level 2^2 amount of nodes. 
Thus, we know we're at the next level ` if (count == (int) Math.pow(2, level))`. 


3. We know we want to iterate through each node: using `while !treeQueue.isEmpty()` as with each iteration we `.poll` a node. The node we are removing is called `tempNode`


4.  At each iteration, the subproblem is twofold:
- At each level (within that while statement), we basically add the average for that level to the `averageOfLevels`. Then, we increment the `level`. We also make sure to reset the `count`,`levelCount`, and `runningSum` (as those values are specific to each level so we must reset them before moving on).
- We also want to iterate through the left and right nodes of `tempNode`  and check if they're not null. If not null then we can add their value to the `runningSum` and also increment the `count` and `levelCount` (for count of nodes for that level).


## Attempt 2

Here is a refined approach to handle both balanced and unbalanced binary trees.

```
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> averageOfLevels = new ArrayList<>();

        Queue<TreeNode> treeQueue = new LinkedList<>();
        treeQueue.add(root);

        while (!treeQueue.isEmpty()) {
            int levelSize = treeQueue.size();
            int levelSum = 0;

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = treeQueue.poll();
                levelSum += node.val;

                if (node.left != null) treeQueue.add(node.left);
                if (node.right != null) treeQueue.add(node.right);
            }

             //cast levelSum to double for accurate division

            double averageForLevel = (double) levelSum / levelSize;

            averageOfLevels.add(averageForLevel);
        }

        return averageOfLevels;
    }
}
```

Here is the refined approach:

1. We initalize `averageOfLevels` arrayList to hold the average of each level.

2. We also initialize a `treeQuue` to contain the nodes we're procesing it and we `.add` the root to initialize the start of the queue.

3. We want to continue iterating through the whole tree so ` while (!treeQueue.isEmpty())`.

4. We intialize a `levelSize` to represent how many nodes we have to go through in that level, as well as the `levelSum` which is the sum for that level.  (Note these variables are initialized at the start of each iteration in the while loop). For example, in the first iteration,  the `levelSize` would be 1 to represent the one node. 

5. Then, we iterate through the node(s) of that level. We do this by using a for-loop to ensure we only do that level- where the amount of iterations (`i` starting from count of 0) we do is up to the `levelSize`.

6. In the for-loop, `node` represents the node we process.  That node is removed from the queue with `.poll` and its value is added to the `levelSum`. 

7. The next part of the for-loop is checking whether the left and right of each node is not null, aka do they exist. (This also ensures that we don't increment count of nodes for null nodes)
With each iteration if there exists a left or right, we add that node to our queue. This way when we loop again in the while loop, we have a new `treeQueue.size` which excludes the processed nodes which we already took care of with `.poll` and will only include new nodes on the level that were added with `.add`. 

8. The last part of the for-loop is getting the average for that level represented as `averageForLevel`. Then, we add this value to our arraylist `averageOfLevels`.

9. Finally, when the queue of nodes is all done processing, we return the completed arraylist `averageOfLevels`.


Side note: At first, I was really confuzzled about why does `levelSum` have to be a double. However, `averageForLevel` is a double since averages can have decimal points from division. If you divide two integers in Java, even if you later cast it to a double, the result would still be an integer. This is because dividing two integers is "integer arithmetic" which truncates the result to an integer.

