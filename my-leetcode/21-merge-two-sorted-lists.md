# 21. Merge Two Sorted Lists

---

# Problem 

## Tags: 
#linked-list 

**Link:**  https://leetcode.com/problems/merge-two-sorted-lists/description/

**Problem Text:**   
You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

 

Example 1:  
Input: list1 = [1,2,4], list2 = [1,3,4]  
Output: [1,1,2,3,4,4]  

Example 2:  
Input: list1 = [], list2 = []  
Output: []  

Example 3: 
Input: list1 = [], list2 = [0]  
Output: [0]  




---

# Scratch Pad/ Pseudocode

// we need to create another list to contain the output from merging these two lists
// to merge: we would need to iterate over both lists 
// the node we are on is the `current1` and we can compare with `current2` from the next list 
// we would initialize those with the head which are `list1` and `list2`    
// if current1 <= current2; then let's put current1 first; else current2
// and we need to track where we are putting this on the mergedList becaused linked list don't have indices
// to track- we can have a "fake" head on the merged list so we can then call fakeHead.next to add the other stuff on


## Ask what are the constraints:

The number of nodes in both lists is in the range [0, 50].
-100 <= Node.val <= 100
Both list1 and list2 are sorted in non-decreasing order.

// This means the lists are already sorted, and also there can be empty lists so we have to take into account where the head is null 
// Also, from Example 3, it seems if one list is empty, then we start from the other list 

## Edge Case(s)
One thing I did not take into account initially is the fact that there can be a different amount of nodes in each list. That means we have to account for sorting the remainder of nodes. 


---

# Java Solution

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

// Return the head of the merged linked list.
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // current node we are iterating over; initialize with value of the head 
        ListNode current1 = list1;
        ListNode current2 = list2;

        // create a fake head to help us add the new nodes to the head of the merged list 
        ListNode mergedListResultDummyStart = new ListNode(0); 
        ListNode mergedListCurrentNode = mergedListResultDummyStart;
      // Check if either or both lists are null
        if (list1 == null) return list2;
        if (list2  == null) return list1;
        // go over both lists, compare values at current node 
        while (current1 != null && current2 != null) {
            if (current1.val <= current2.val) {
                mergedListCurrentNode.next = current1;
                current1 = current1.next;
                // so this means if the current1.value < current2.val the next node which we point to on merged list is assigned value of current 1 . once we done assigning it, we point to the next node?

            } else {
                mergedListCurrentNode.next = current2;
                current2 = current2.next;
            }
            mergedListCurrentNode = mergedListCurrentNode.next; // Move to the next node in merged list
        }
        
        // Append remaining nodes from either list
        // outside of while loop because that's when one of the list has reached its end and and the other hasn't 
        if (current1 != null) {
            mergedListCurrentNode.next = current1;
        }
        
        if (current2 != null) {
            mergedListCurrentNode.next = current2;
        }

        return mergedListResultDummyStart.next; // Return the head of the merged list 
    }
}
```
1. We initialize the variables, firstly to represent the current node we are on in both lists. This is so we can compare their values to determine the order we want to add them to the list. 

2. We also need a way to add these numbers to the "merged list" and to do that- we need to give it a "fake head" to "glue" on the rest of the numbers. This fake head is called `mergedListResultDummyStart` and we give it a value of 0. 
`ListNode mergedListResultDummyStart = new ListNode(0);`
Then we want to also have a variable to represent the current "node" of the "merged list" that we are on- this is so we can keep track of where we are putting our sorted numbers. This is initialized as the "fake head"
`ListNode mergedListCurrentNode = mergedListResultDummyStart;`

3. Now that are our variables are initialized, we want to make the check for empty lists. Keep in mind if a list is empty, its head is null. 
Therefore, we write: `if (list1 == null)` and ditto for list2, then we would return the other list. For example, by returning `list2`- this would return `list2` (its head) and since each node has a built in reference to the next list, this would mean it would render the whole linked list. In other words, if a list is empty, we would return the other list (as there would only be one list to sort) 

4. Next, we iterate through both lists up until the last node -- 
Initially I had put it as `while (current1.next != null && current2.next != null)`. However this one skips the last node. This is saying while the next node is not null (which is when we are at the last node), meaning we would never reach the last node.
Therefore we have to instead do `while (current1 != null && current2 != null)`

5. Within our while loop, we compare the current node we are on in both lists. Based on the comparison, we then have the next node in the "merged list" point to the current node (that is smaller in list1 vs list2). 
`mergedListCurrentNode.next = current1;`
We use .next so that we can append it to the "fake head" and then subsequently- the .next represents the next node (or empty space) to reference the next number to add in this sorted merged list.

6. One thing I had forgotten to do at first is say let's say the current node of list 1 was referenced in merged list-- well then, we no longer need to "keep" that number to compare again so we want to move one node up by updating the value of `current1` with `current1.next`.  


7. Further, similar to the above - regardless of whether we add something from the first list or second list, we want to move past the current node of merged list where we added the value. Remember earlier in the if/else loops, we have a line that states: "mergedListCurrentNode.next = current1"?  Hence, we update the value of `mergedListCurrentNode` to point to `mergedListCurrentNode.next`.  
Because of that, when we iterate again and in the if/else loop and we call `mergedListCurrentNode.next = ...` then we can have the next empty node to add the value in. 
For example, let's say the first iteration - we start on the `mergedListResultDummyStart` (`mergedListCurrentNode`) and we set its `.next` to `current1`. Well, then we set the next bucket already- similar to how we moved a rider into the next seat of a Ferris Wheel. So now- we update the `mergedListCurrentNode` to the seat that has the rider we just added. This way in our next iteration, we can say `mergedListCurrentNode.next` and that next seat would be filled with our next sorted value. 


8. Finally, we have code that handles cases where the lists are different sizes. This means that let's say we are done iterating through the first list, but there's still numbers in list 2. Since everything is already ordered and we were already ordering things, then we can just if the current node of the 2nd list is not null, so we can reference mergedListCurrentNode.next to current2 - which will get that current node which references the next and references the next, hence adding the rest of the list onwards from the current node. 

9. Lastly, we do  `return mergedListResultDummyStart.next;` to return the merged list from the its actual head and onwards. 


## Time Complexity 
It is linear since we have to iterate through each linked list to sort them. From ChatGPT "The time complexity of your solution is linear, O(n + m), where n is the length of list1 and m is the length of list2."
However, this is more efficient versus initializing an array where we would have to swap elements to sort and then write it back into an linked list. 

Also, a quick side note:  
From ChatGPT, I realized perhaps I don't need to include this block. 

```
if (list1 == null) return list2;
if (list2  == null) return list1;
```

My code already has built in functionality to check:  
To see if either list is null:  

```
if (current1 != null) {
            mergedListCurrentNode.next = current1;
        }
        
if (current2 != null) {
            mergedListCurrentNode.next = current2;
        }

```
This means that if one list was null, we would render the other list. 

If both lists are null, none of the conditions in the while loop or the if statements would pass and we would `return mergedListResultDummyStart.next`. 
And that would be null since there would be nothing else added after the "fake head".

