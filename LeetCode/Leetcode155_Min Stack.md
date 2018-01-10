Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
Example:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.

```
/* 使用两个栈
   一个正常push,pop,top;  mystack1
   另一个用来保持最小元素 mystack2
*/
class MinStack {
private:
        stack<int> mystack1;
        stack<int> mystack2;
public:
    /** initialize your data structure here. */
    MinStack() 
    {
        
    }
    
    void push(int x) 
    {
        mystack1.push(x);
        if(mystack2.empty() || x <= getMin())
            mystack2.push(x);
    }
    
    void pop() 
    {
        if(top() == getMin())
            mystack2.pop();
        mystack1.pop();
    }
    
    int top()
    {
        return mystack1.top();
    }
    
    int getMin() 
    {
        return mystack2.top();
    }
};
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```