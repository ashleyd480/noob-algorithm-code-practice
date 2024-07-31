# 83. Remove Duplicates from Sorted List

---


# Problem 

## Tags: 
#linked-list

**Link:** https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/

**Problem Text:**   
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

 
Example 1:  
Input: head = [1,1,2]  
Output: [1,2]  

Example 2:  
Input: head = [1,1,2,3,3]  
Output: [1,2,3]  



---

# Scratch Pad/ Pseudocode

// we want to iterate throug the linked list which means using a while loop  
// checking that next is not null  
// list is guaranteed to be sorted in ascending order    
// so it's already sorted so we would just check if the next val is a dupe  


## Ask what are the constraints:
The number of nodes in the list is in the range [0, 300].  
// ^ this means we can have an empty list so we have to account for that

-100 <= Node.val <= 100  
The list is guaranteed to be sorted in ascending order.  

## What clarifying questions do I have:
// looks like we are also passed the head value as the parameter in this Leetcode problem  


---

# Java Solution

```
## Attempt 1
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        // head
        // delete duplicates
        // sort the linked list
        // if we seen it - liek if node.next != 1, skip it

        if (head == null) {
            return null;
        }

        ListNode currentNode = new ListNode();
        ListNode nextNode = new ListNode();
    currentNode = head;
       nextNode = head.next;
        
        while (currentNode != null && nextNode !=null) {
            // changing the pointer
                System.out.println("current node is " + currentNode.val);
            System.out.println("next node is " + nextNode.val);

            while (nextNode != null && currentNode.val == nextNode.val) {
                nextNode = nextNode.next;
            }
            // if ( currentNode.val != nextNode.val) { // fixed it to compare the .val
        //    else if (currentNode.val != nextNode.val ) {
currentNode.next = nextNode;// current node still at current postion but pointint to next one 
       
  currentNode = nextNode; // moving to where current node is pointing to or nextNode 
if (nextNode != null) {
    nextNode = nextNode.next;
}
             } 
    
          
            // System.out.println("next node is " + nextNode.val);

          
        

        return head;
    }

}
```

This attempt was from my mock interview on 7/30. I was a bit rusty on linked lists--- my b... and in total n00b fashion approached it with a naive strategy.
There were some bugs which during the evening I debugged which is how I got the final code above.

- I realized when we had 1, 1, 2, 3, 3 and it output 1, 2, 3, 3-- this means that we were entering in the else statement where current != next which is not true- which means values were not correctly updating. The next needed a way to update. 
- I was able to resolve the null pointer issues through first doing the null check of the head before initializing the variable nextNode as head.next- otherwise we can get the null pointer. Also checking for if node.next= null before moving the node.next 
- Further used the nested while loop (yes I know - pretty yucky as not very good for time complexity- can make worse than linear)- however it was necessary in order to finish implementing the strategy of peeking ahead- where we stay on the currentNode and just keep peeking ahead until nextNode is not a dupe and point to that. 

## Attempt 2

(tl;dr 
We can also just use one while loop - and compare the current and next at each iteration - if it's a duplicate: skip the duplicate and have the current point to the next next node. Otherwise if it's not a duplicate- we can move to the next node. In a sense, with this single while loop- we are still iteratively peeking ahead if it's a duplicate and only moving ahead if current does not equal to next (not duplicates). 
)
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
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
       
    ListNode current = head; // initalize the current node we are on as head
  // If the list is empty, return null - this is because list size can be 0
        if (head == null) {
            return null;
        }

        while (current. next != null) { // keep iterating up to second to last node 
            if (current.val == current.next.val) {
                current.next = current.next.next; // if it's a duplicate we just set the current next value to the next next value
                                                  
            } else {
                current = current.next;
            }
        }

        return head; // return modified list starting from head 
    }
}
```

We start off by initializing a ListNote variable called `current` to represent the current node we are on.
The inital value would be the  `head`. (Most likely outside of this function, `head` is defined by something like this: ` ListNode head = new ListNode(1);`- where its value is 1.)

We keep iterating until we get to the last node. When get to the last node, there is nothing to the right of it so its `current.next= null` so we want to iterate up until the point that `current.next` does not equal to null. We want to do that because we need to compare two values to see if there is a duplicate!  

We are comparing the  `current.val` and the `current.next.val` (next value). If they are equal, then it's a duplicate pair. In that case, we want to set the next value value to the subsequent value (`current.next.next`)

If it's not a duplicate pair, then we can set the `current` to point to the next node. Let's say we have [1, 2, 3, 4] and we are currently on 2 and we check the next number 3. They're not a duplicate- we move our pointer to the next node. 

Finally, to return our modified list (or also in the case where there's only one element in the linked list) - we return `head`. In a linked list, when we return the `head` node (in this case `head` was that variable we got from the function parameter)- then this return the head and the rest of the linked list. This is because in a linked list- each node has a pointer that references the next node. 
(From ChatGPT: "when you return head from a method that operates on a linked list, you're leveraging the fact that each ListNode in the list has a next attribute. This attribute allows you to traverse through the list starting from head and access each subsequent node until you reach the end (where next is null)")

## Time Complexity 
The time complexity is linear which means that as the amount of nodes grow, it proportionally takes longer. However, this is more efficient than initializing an array and comparing each element and swapping.

We prefer linked list here because linked list are better for insertions and deletions; whereas with array- inserting or deleting elements requires shifting elements.

