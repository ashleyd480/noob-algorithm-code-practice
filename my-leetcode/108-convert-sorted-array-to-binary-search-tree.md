# 108. Convert Sorted Array to Binary Search Tree


---


# Problem 

## Tags: 
#binary-search-tree

**Link:** https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/

**Problem Text:**   

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.  

 

Example 1:  
Input: nums = [-10,-3,0,5,9]  
Output: [0,-3,9,-10,null,5]  
Explanation: [0,-10,5,null,-3,null,9] is also accepted: 

Example 2: 
Input: nums = [1,3]  
Output: [3,1]  
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.  


---

# Scratch Pad/ Pseudocode

// note: we see it's alreayd sorted array 
// The middle index is 2, which is our first node or the root.
// From there, similar to a binary search, we split into two subarrays, one to the left of the middle, and one to the right. We recursivvely split down the middle.

We first start building the root.left with buildBST, and then move on to the right:

## Ask what are the constraints:
1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums is sorted in a strictly increasing order.


---

# Java Solution

```
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildBST(nums, 0, nums.length - 1);
    }
    
    private TreeNode buildBST(int[] nums, int left, int right) {
      
         if (left > right) {
            return null;  // Base case: no elements to construct the tree (for - we need a way to stop recursion )
        }
        
        
        int mid = (left + right) / 2;  // Find the middle index
             // Create the root node with the middle element
        TreeNode root = new TreeNode(nums[mid]);
         // Recursively build the left and right subtrees
        root.left = buildBST(nums, left, mid - 1); 
        root.right = buildBST(nums, mid + 1, right);
  
    
        return root;
    }
}

```

If we look at this array [-10,-3,0,5,9], we know its sorted already and we want to recursively split down the middle, looking at its two subarrays, and then subarrays of those two subarrays. 
Note: with a binary search tree, all values in left subtree must be less than nodes value and all values in right subtree must be greater than node's value. 

1. We first get the root note by calculating the middle. The middle `0` is the root.
Now, we have to two sub-arrays on either side of the middle root node.
With recursions, we also know that we must have a condition to allow the recursion to exit. As such, similar to a binary search, if left > right, then we've already completed our searchable area of a subarray- which means no more sub-children nodes. 


In the case of [-10,-3,0,5,9], the indices are: 0, 1, 2, 3, 4.



2. Next, we recursively build the left and right subtrees for the root node 0. Let's sart by building the left subtree of root node 0 .


3. Our root.left after getting the the root node, would call `buildBST` on nums, with values: left= 0 and right = 1. That function calls adds -10 (the new middle) as the next `new Treenode`. 

- Now, we recursively build the left and right subtrees for `-10`. 
    - We start with the left subtree for -10. We call `buildBST` on nums with values: left = 0 and right = -1. In this case since left > right, which means we have no more current elements in the subarrays to process, which mean no more subchild. The if statement returns null. 

    - Now, we do the right subtree for -10. We call `buildBST` on nums with values of 1 and 1. (We use the 1 and 1, as those are the derived from the values that the parent node -10 subarray range had of 0 and 1.)  
    Thus, we add the `-3` as the right root node of -10. 
    - When we check the root.left and root.right for -3, both would return null. 

- The function finishes processing all the nodes for -10, so we return to the root node 0.


4. Now, we can go ahead and build the right subtree using similar logic. [3,4] would be the indices of the right subarray. Remember we've already processing the `buildBST` left for `0` - so now we finish the `buildBST` right statement, and that is how we get `5` as the middle and we add that as the right sub-node of `0`.

- Now, we recursively build the left and right subtree of 5. 
    - For the left subtree of 5 (root.left), we'd have the range be 3 and 2 - this means there are no elements to check or add- we return null and exit out. 
    - Now, we try the right subtree (root.right), we'd have the range 4 and 4- so our middle would be 9, and that would be our new node. Root.right and root.left would be null for 9.
    - This means, we are done processing the left and right functions for `5` and we've also finished ditto for the `0`


5. Finally, we return the root, which returns the root and its linked nodes to create the BST. 
