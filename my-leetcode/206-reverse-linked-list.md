# 206. Reverse linked List 

---

# Problem 

## Tags: 
#linked-list

**Link:** https://leetcode.com/problems/reverse-linked-list/

**Problem Text:**   

Given the head of a singly linked list, reverse the list, and return the reversed list.  

 

Example 1:  
Input: head = [1,2,3,4,5]  
Output: [5,4,3,2,1]  

Example 2:  
Input: head = [1,2]  
Output: [2,1]  

Example 3: 
Input: head = [] 
Output: []  



---

# Scratch Pad/ Pseudocode

// initialize a linked list called ReverseList  
// a fake head to add the reversed stuff on  
// 0 - null  - since number of nodes can be null --> have to account for empty lists 
// head of list is passed along as a parameter 
// was thinking of traversing the whole linked list until I get to the last node and then having that last node be the first of the reversed list, but this would required multiple traversals, not very time efficient

## Ask what are the constraints:
The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000



---

# Java Solution

```
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode currentNode = head;
        ListNode reverseListCurrentReference = new ListNode(0);

       // for empty list 
        if (head == null) {
            return null;
        }
        while (currentNode != null) {
           
            // so reverseListCurrentNode is used to make a space for variable 
            ListNode reverseListCurrentNode = new ListNode();
            if (currentNode == head) {
                reverseListCurrentNode.next = null;
            } else {
                reverseListCurrentNode.next = reverseListCurrentReference;
            }
            reverseListCurrentNode.val = currentNode.val;
         
            reverseListCurrentReference = reverseListCurrentNode;

            currentNode = currentNode.next;

        }

        return reverseListCurrentReference;
    }
}
```
Here are the steps:

1. We know we want to traverse the original list to "write" its nodes to the reversed list. To represent the current node we are on in the original list, we initialize the `currentNode` with a value of `head` since we will start from the head. 

2. We also know that to build this reversed linked list- as we iterate each node on the original list, we want to add that node to the reversed list. Thus, with each iteration - we want to create a node. 
`reverseListCurrentNode` is a new node that is made in each iteration to make "space" to hold the "value" we "write" over from the `currentNode` in the original list.

This is where we come up with the lines: 

```
  while (currentNode != null) {
    ListNode reverseListCurrentNode = new ListNode();
...

 reverseListCurrentNode.val = currentNode.val;
```

To visualize this approach, we are iterating from the head to the end of the current list- and building the reversed list from right to left (using of the `.next` property.)

3. As we start with the `head` of the original list, in the reversed list, that would be the end. 
We know that the last node of a linked list has a `.next` of null. So to tell the system that the head should go to the end, we write the following line:

```
if (currentNode == head) {
    reverseListCurrentNode.next = null;
```
4. Keep in mind, we also need a way to advance the `currentNode`. Thus, with each iteration- after the `currentNode` is added to the reversed list, we call `currentNode = currentNode.next;`

5. So now with that `.next` we've moved past the head of the original list onto the 2nd node, etc. 
In the reversed list, the 2nd node points to the node to its right. 
The one to the right was the `reverseListCurrentNode` that we created in that first iteration... but that value and reference only lives and dies within the context of that specific iteration. 
This means... we need a way to track it. 

That is why outside of the `while` loop, we initialize a variable to track that:
```
ListNode reverseListCurrentReference = new ListNode(0);
```
And before each iteration ends, we want to update the value of that `reverseListCurrentReference` to the `reverseListCurrentNode` before it "dies" off. 

```
reverseListCurrentReference = reverseListCurrentNode;
```
Note: we do this before the next iteration, so that is why that line is placed before the `currentNode= currentNode.next`

6. Now that we have that value of the node from prior iteration saved (sort of like you remembering the last place you went to so you can trace your steps back), then in the `else` statement of the next iterations (excluding when we are on the original's `head`), we can set the `reverseCurrentNode` to point to that prior iteration's node. 

```
 else {
    reverseListCurrentNode.next = reverseListCurrentReference;
```

7. The last iteration is when we are on the last node the original list- say 5. 
We would enter the `else` statement of the if/else within the while loop where `reverseListCurrentNode.next = reverseListCurrentReference;` 
The last node points to the prior node that contains 4. 

Then, we set the values with the: `reverseListCurrentNode.val = currentNode.val;`
Next, we have this line `reverseListCurrentReference = reverseListCurrentNode;` to store that node we are on.
Finally, with ` currentNode = currentNode.next;`- that results in null so the loop is exited.

We return `reverseListCurrentReference` which stores that last node we were on which is the head of the new reversed linked list. 


Note:
I also added the following code block at the top to handle cases where the linked list is empty.

```
if (head == null) {
    return null;
```