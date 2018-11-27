# Queue
Operation 
- throw exception: add, remove, element
- return null if empty: offer, poll, peek

# Stack
Operation
- push, pop, top

## Implement queue by two stacks
把新的元素放在stack2中，当pop或者peek的时候，如果stack1为空，就把stack2倒在stack1中，如果stack1不为空，就返回stack1的pop/peek

Time complexity: push - O(1), pop amortized - O(1)

https://leetcode.com/problems/implement-queue-using-stacks
```java
class MyQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;

    /** Initialize your data structure here. */
    public MyQueue() {
        stack1 = new Stack<Integer>();
        stack2 = new Stack<Integer>();
        
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stack2.push(x);
        
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (stack1.empty()) {
            while(!stack2.empty()) {
                stack1.push(stack2.pop());
            }
        }
        return stack1.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if (stack1.empty()) {
            while(!stack2.empty()) {
                stack1.push(stack2.pop());
            }
        }
        return stack1.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack1.empty() && stack2.empty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```
## Implement min stack

Using stack2 to maintain min. Only if stack2 is empty or stack2.peek() > new element, stack2 add new elmement.

```java
class MinStack {
    
    Stack<Integer> stack1, stack2;

    /** initialize your data structure here. */
    public MinStack() {
        stack1 = new Stack<Integer>();
        stack2 = new Stack<Integer>();
    }
    
    public void push(int x) {
        stack1.push(x);
        if (stack2.empty() || x <= stack2.peek()) {
            stack2.push(x);
        }
    }
    
    public void pop() {
        int x = stack1.pop();
        if (!stack2.empty() && x <= stack2.peek()) {
            stack2.pop();
        }
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int getMin() {
        return stack2.peek();
    }
}
```
