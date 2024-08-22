# 933. Number of Recent Calls

---


# Problem 

## Tags: 
#queue

**Link:** https://leetcode.com/problems/number-of-recent-calls/description/?envType=study-plan-v2&envId=leetcode-75

**Problem Text:**   

You have a RecentCounter class which counts the number of recent requests within a certain time frame.  

Implement the RecentCounter class:  

RecentCounter() Initializes the counter with zero recent requests.  
int ping(int t) Adds a new request at time t, where t represents some time in milliseconds, and returns the number of requests that has happened in the past 3000 milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range [t - 3000, t].  
It is guaranteed that every call to ping uses a strictly larger value of t than the previous call.  

 

Example 1:  
Input  
["RecentCounter", "ping", "ping", "ping", "ping"]  
[[], [1], [100], [3001], [3002]]  
Output  
[null, 1, 2, 3, 3]  

Explanation  
RecentCounter recentCounter = new RecentCounter();  
recentCounter.ping(1);     // requests = [1], range is [-2999,1], return 1  
recentCounter.ping(100);   // requests = [1, 100], range is [-2900,100], return 2  
recentCounter.ping(3001);  // requests = [1, 100, 3001], range is [1,3001], return 3  
recentCounter.ping(3002);  // requests = [1, 100, 3001, 3002], range is [2,3002], return 3  



---

# Scratch Pad/ Pseudocode

// grab first element of the queue and compare it to lowerNumberOfRange  
// while lowerNumberOfRange > first element in our queue .peek to see the first element  
// we could remove those elements .poll  
// numberOfRequests = updated size and we can manually keep track of that  subtracting as we remove  
// how to calculate ping to return number of requests that have happened in past 3 seconds  


## Ask what are the constraints:
1 <= t <= 109
Each test case will call ping with strictly increasing values of t.
At most 104 calls will be made to ping.

^It was especially good to note that its already sorted in ascending order.
This means that this will ensure that our while loop comparison can work. 




---

# Java Solution

```
class RecentCounter {

    private int numberOfRequests;
    private Queue<Integer> callQueue;

    public RecentCounter() {
        // counts the number of recent requests within a certain time frame
        numberOfRequests = 0;
        callQueue = new LinkedList<>();

    }

    // t is parameter which represent time in milliseconds

    public int ping(int t) {

        callQueue.add(t);
     
        numberOfRequests = numberOfRequests + 1;

        int lowerNumberOfRange = t - 3000;

        int firstElementInQueue = callQueue.peek();
    

        // requests = [1, 100, 3001, 3002], range is [2,3002]
        // Remove requests that are outside the 3000ms range

        while (lowerNumberOfRange > firstElementInQueue) {
            callQueue.poll(); // allow
            numberOfRequests--;
            firstElementInQueue = callQueue.peek();

        }

        
        return numberOfRequests; // the past 3000 milliseconds
    }
}

```

Here are the steps:

## Attributes and Constructor  

Firstly, we want to define the `RecentCounter` class. This class counts the number of recent requests, so we know it should have an attribute of `int numberOfRequests`. 

We also know that this class has a method that adds a new request at time `t`- and when we see thoes words about "adding request"- that means we will be using a Queue data structure. We also know that when we call `RecentCounter()`, that means that we are initilaizing a counter with 0 requests. 

As we can see below, this initializes a queue, and then when we call the method `ping`, then the number `t` in parameter of that method is added to the queue, representng a new request at time `t`. 

```
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);  // requests = [1], range is [-2999,1], return 1  
recentCounter.ping(100);   // requests = [1, 100], range is [-2900,100], return 2  
```

The above tells us that our class needs a `Queue<Integer> callQueue` attribute as well, as that will be used when we initialize a new RecentCounter, a new `callQueue` has to be initialized as well to hold those requests. 

Our constructor ends up looking like this:

```
   public RecentCounter() {
        // counts the number of recent requests within a certain time frame
        numberOfRequests = 0;
        callQueue = new LinkedList<>();

    }
```


## Method 

The `RecentCounter` has a method that takes the time `t` and adds a request at that time- and it returns the `numberOfRequests` that take place in the past 3000 milliseconds. 

1. Firstly, we want to add the `t` to the queue so we do `callQueue.add(t)`. We also know we need to get a range, where the lower part of the range `lowerNumberOfRange` is  `t-3000` which would get us the starting time of the past 3000 milliseconds. Say `t` is at 4000, then your startng time of the past 3000 milliseconds is 1000. 

2. Then, since `t` is added, that means a request is added too, so we increment `numberOfRequests` by one.

3. However, we do see from the provided example, that up to a certain `t`, the request count doesn't keep incrementing. This is because the request count would only be the requests in the last 3000 milliseconds. This gives us a hint that anything that happens before the last 3000 milliseconds isn't counted.


```
Input
["RecentCounter", "ping", "ping", "ping", "ping"]  
[[], [1], [100], [3001], [3002]]  
Output  
[null, 1, 2, 3, 3]  
```

4. Above,  we can see once we reach 3002, the request that happened at `t=1` is no longer in range - so it's not included in the `numberOfRequests`. This means that we need to be able to have some way to check what's in range. 
Since we know the `t` are added in ascending time order, we can rest assured it's in chronological time order. As such, remember `lowerNumberInRange` which represents the starting point of our time range? If `lowerNumberInRange` is greater than the first `t` time element in queue (think of a cell phone tower and when you drive out of range) - then we wouldn't include that request because it's no longer in our range. 

Hence, we can use ` while (lowerNumberOfRange > firstElementInQueue)`, where `firstElementInQueue` is represented as `callQueue.peek();`. 


5. As mentioned in step 4, if the request is no longer within the time range, we would want to remove it. As such, we do `callQueue.poll();`. Additionally, we also decrement `numberOfRequests` by 1. We also then want to update the `firstElementInQueue` to the new first `t` in line by using `.peek()` again. 

6. Finally, we can return `numberOfRequests`.


---


# What I Learned
- One thing I learned is thinking about what can we compare this information against to help us make the decision of when to say remove a request. 
- Also, another concept refresher I got is say if we initialize a variable outside of the while loop, and that variable is also used to determine the continual running of the while loop, then we should ensure we update the variable within the loop, i.e. say if it changes- otherwise we will be using the original value of that variable with every iteration leading to incorrect checks in the while loop.
- Another interesting thing I learned. "You cannot instantiate an interface like Queue<> directly. Instead, you should use a class that implements the Queue interface, such as LinkedList<>." This concept also applies to ArrayLists too, i.e.  List<String> list = new ArrayList<>();. Notice how we can't directly instatiate an interface of List. 
- I additionally got a refresher on the difference between static and non-static attributes or methods in Java. Static attributes or methods belong to the class itself, meaning they remain the same across all instances of the class. That's why we call them directly with the class name, like ClassName.methodName(). On the other hand, non-static attributes or methods are tied to a specific instance of the class and can vary between instances, so we call them using the instance, like instanceName.methodName().