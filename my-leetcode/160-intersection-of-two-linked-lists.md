# 160. Intersection of Two Linked Lists

---

# Problem 

## Tags: 
#linked-list, #set

**Link:** https://leetcode.com/problems/intersection-of-two-linked-lists/description/

**Problem Text:**   

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null. 
 
Note that the linked lists must retain their original structure after the function returns.   

Example 1:  
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.  

Example 2:  
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'  
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.  


Example 3: 
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2  
Output: No intersection  
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values. 
Explanation: The two lists do not intersect, so return null.  

---

# Scratch Pad/ Pseudocode

// initially i assumed that the intersection is where their values equal  
// however- with listA = [1,9,1,2,4], listB = [3,2,4] - this shows that they intersect at 2
// so this is where their values equal and the .next also equals.  
// as we traverse we want to check the .next value
// to check did see that number and the exact reference (exact list node)  


---

# Java Solution

```
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
      
            ListNode currentA = headA;
            ListNode currentB = headB;

            //hashset of type nodes (and nodes have reference aka pointer to the next and val)
            HashSet<ListNode> seenNodes= new HashSet<ListNode>();
            
            while (currentA !=null)
            {
                seenNodes.add(currentA);
                currentA = currentA.next;

            }

            while (currentB !=null) {
                // checks to see if the B's node (it's pointer and val in list A- both have to match)

                if (seenNodes.contains(currentB)) {
                    return currentB;
                }
                currentB = currentB.next;
               
            }
            return null;
            
    }
}
```
Here are the steps:
1. We know we want to traverse both lists. Thus, we need a variable to represent the node we are traversing over. We initialize `currentA` and `currentB` with the current `headA` and `headB` which are parameters we are given.

```
ListNode currentA = headA;
ListNode currentB = headB;
```

2. We know that we want to compare and check if the current node in List A - if its value and reference point (`.next`) matches the current one in List B. 
While we iterate through List A- we can write its nodes to a set. Each node contains the value and reference pointer (`.next`) of that current node in List A. 

```
while (currentA !=null)
    {
    seenNodes.add(currentA);
    currentA = currentA.next;
    }
```

Note: we want to make sure we are calling `.next` to ensure we continue moving on to the next node (helping us traverse List A).

3. If a node in list B matches that node in the set (i.e. has to match both the value and the `.next`)- then we have found the intersection.

```
while (currentB !=null) {
                // checks to see if the B's node (it's pointer and val in list A- both have to match)

    if (seenNodes.contains(currentB)) {
        return currentB;
    }
    currentB = currentB.next;

```

Note, we want to make sure we are calling `.next` to ensure we continue moving on to the next node (helping us traverse List B).

4. If no match was found after iterating through all of List B, we want to `return null`.


## Time Complexity 

Hashsets have a constant time complexity to see if something is contained. Meanwhile, traversing through both linked lists is linear. 


