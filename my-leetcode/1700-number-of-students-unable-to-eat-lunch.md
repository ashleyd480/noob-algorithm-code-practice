# 1700. Number of Students Unable to Eat Lunch

---


# Problem 

## Tags: 
#stack, #queue

**Link:** https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/

**Problem Text:**   

The school cafeteria offers circular and square sandwiches at lunch break, referred to by numbers 0 and 1 respectively. All students stand in a queue. Each student either prefers square or circular sandwiches.

The number of sandwiches in the cafeteria is equal to the number of students. The sandwiches are placed in a stack. At each step:

If the student at the front of the queue prefers the sandwich on the top of the stack, they will take it and leave the queue.
Otherwise, they will leave it and go to the queue's end.
This continues until none of the queue students want to take the top sandwich and are thus unable to eat.

You are given two integer arrays students and sandwiches where sandwiches[i] is the type of the i​​​​​​th sandwich in the stack (i = 0 is the top of the stack) and students[j] is the preference of the j​​​​​​th student in the initial queue (j = 0 is the front of the queue). Return the number of students that are unable to eat.

 

Example 1:

Input: students = [1,1,0,0], sandwiches = [0,1,0,1]
Output: 0 
Explanation:
- Front student leaves the top sandwich and returns to the end of the line making students = [1,0,0,1].  
- Front student leaves the top sandwich and returns to the end of the line making students = [0,0,1,1]. 
- Front student takes the top sandwich and leaves the line making students = [0,1,1] and sandwiches = [1,0,1].  
- Front student leaves the top sandwich and returns to the end of the line making students = [1,1,0].  
- Front student takes the top sandwich and leaves the line making students = [1,0] and sandwiches = [0,1].  
- Front student leaves the top sandwich and returns to the end of the line making students = [0,1].  
- Front student takes the top sandwich and leaves the line making students = [1] and sandwiches = [1].  
- Front student takes the top sandwich and leaves the line making students = [] and sandwiches = [].  
Hence all students are able to eat.  

Example 2:  
Input: students = [1,1,1,0,0,1], sandwiches = [1,0,0,0,1,1]  
Output: 3  
 


---

# Scratch Pad/ Pseudocode


// we can add studentsDoneChecking and we make sure that studentsWeChecked is less than current queue size; this is becuase otherwise if we increment by 1 and we have = to queue size, it will be over by 1  
// and we can remove a student who passes from number we checked  



---

# Java Solution
```
class Solution {
    public int countStudents(int[] students, int[] sandwiches) {

        Stack<Integer> sandwichStack = new Stack<>();

        for (int i = sandwiches.length - 1; i >= 0; i--) { // notice for stack start at top of array
            sandwichStack.push(sandwiches[i]);
        }
        Queue<Integer> hungryStudentQueue = new LinkedList<>();

        for (int student : students) {
            hungryStudentQueue.add(student);
        }

        int numberStudentsChecked = 0;

        while (!hungryStudentQueue.isEmpty() && numberStudentsChecked < hungryStudentQueue.size()) {
            int studentWeChecking = hungryStudentQueue.poll();
            int sandwichWeChecking = sandwichStack.peek();

            if (studentWeChecking != sandwichWeChecking) {
                hungryStudentQueue.add(studentWeChecking);
                numberStudentsChecked++;

            } else {
                // The student wants the sandwich, serve the sandwich
                sandwichStack.pop();
                numberStudentsChecked = 0;

                // Reset unsuccessful attempts count and we want to do this since after a
                // student leaves the queue, line structure changes and we want to recheck
            }

            // numberStudentsChecked = 0;
            // Reset attempts since a student ate

            // if (numberStudentsChecked == hungryStudentQueue.size())
            //     break;

        }

        return hungryStudentQueue.size(); // students left in the queue

    }
}
```
The approach is as follows: 
1. We initialize a stack to represent the sandwiches. Since stack is last in first out, we iterate through the array in reverse order: 
`for (int i = sandwiches.length - 1; i >= 0; i--)`
For each sandwiches[i], we `push` it onto the the `sandwichStack`.

2. Then, we write the array of students into a queue called `hungryStudentQueue`. With queues, we have to initialize it with a class that implements the queue interface- in this case we are using a Linked List.
`Queue<Integer> hungryStudentQueue = new LinkedList<>();`
For each student, we `add`it onto the `hungryStudentQueue`.

3. We also keep track of `numberStudentsChecked` as we don't want to keep looping nonstop. We would have to know when we are done checking all of them in the line,  and when none of them are happy with the sandwich size. Hence I wrote `while (!hungryStudentQueue.isEmpty() && numberStudentsChecked < hungryStudentQueue.size())`

4. `numberStudentsChecked` would increment by 1 each time a student is checked - that is only when they don't like the sandwich option. When a student likes a sandwich, we reset that back to 0, just becuse now the "structure" changes when that student leaves the line as other behind him can possibly see the sandwiches in a different order. So as those students iterate through again the choices, we just keep increasing `numberStudentsChecked` again. 

5. Finally, when the `numberStudentsChecked` which are the picky ones who don't want to eat is equal to the students left, then we can exit the loop- as that while condition would no longer be true.


6. After we break out, we just return the number of students left in line - which would the "picky ones"

