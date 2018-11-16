# Selection sort
In each round find the min of remaining numbers and put it in left side of array.
[-1, -10, 5, 2 3]
* iteration 1 : [-10 | -1, 5, 2, 3]
* iteration 2 : [-10, -1 | 5, 2, 3]
* iteration 3 : [-10, -1, 2 | 5, 3]
* iteration 4 : [-10, -1, 2, 3 | 5]

time complexity: O(n^2) => 1 + 2 + 3 + ... + n(n - 1)/2 => n^2 
```java
for (int i = 0; i < n - 1; i++) {
   for (int j = i + 1; j < n; j++)
}
```

https://github.com/anim2019/2019
```java
public void sortIntegers(int[] A) {
     // selection sort
     int length = A.length;
     for (int i = 0; i < length - 1; i++) {
         int min = i;
         for (int j = i + 1; j < length; j++) {
             if (A[j] < A[min]) {
                 min = j;
             }
         }
         int temp = A[min];
         A[min] = A[i];
         A[i] = temp;
     }
 }
```

Selection sort by two stacks:
```java
    public void sortIntegers(int[] A) {
        // write your code here
        // selection sort with two Stacks
        
        if (A == null || A.length == 0) {
            return;
        }
        
        Stack<Integer> stack1 = new Stack();
        Stack<Integer> stack2 = new Stack();
        
        for (int i = 0; i < A.length; i++) {
            stack1.push(A[i]);
        }
        
        while(!stack1.isEmpty()) {
            int tempPeek = stack1.pop();
            
            // the number in stack2 should from the large to small, so if the temppeek is larget than stack2 peek, we should push all smaller number back to stack1 temporarily
            while (!stack2.isEmpty() && tempPeek > stack2.peek()) {
                stack1.push(stack2.pop());
            }
            
            stack2.push(tempPeek);
        }
        
        int i = 0;
        while(!stack2.isEmpty()) {
            A[i ++] = stack2.pop();
        }
        
    }
```
