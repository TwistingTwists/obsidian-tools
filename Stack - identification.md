# How to Identify Stack Problems

Great question! Recognizing when to use a stack is a crucial pattern recognition skill. Let me break this down.

## Key Indicators That Suggest Using a Stack

### 1. **Looking for Previous/Next Greater or Smaller Elements**
If the problem asks you to find:
- The nearest element greater/smaller than current
- The first element that satisfies some condition before/after current position
- Range queries based on relative ordering

**Keywords to watch for:** "previous greater," "next smaller," "nearest," "consecutive"

### 2. **Processing Elements in Reverse Order**
When you need to:
- Process nested structures
- Match pairs (like parentheses)
- Undo operations or backtrack

**Keywords:** "matching," "balanced," "valid," "nested," "reverse"

### 3. **Maintaining Order with Elimination**
When elements can be eliminated based on future elements:
- Monotonic sequences
- Removing redundant elements
- Maintaining extrema (min/max)

**Keywords:** "consecutive," "span," "visible," "maximum/minimum in range"

### 4. **LIFO (Last In, First Out) Nature**
The problem has an inherent LIFO structure:
- Function call simulation
- Undo/redo mechanisms
- Expression evaluation

## Classic Stack Problems by Category

### **Category 1: Parentheses/Bracket Matching**

**1. Valid Parentheses**
```
Input: "({[]})"
Output: true
```
Check if brackets are properly balanced.

**2. Longest Valid Parentheses**
Find the length of the longest valid parentheses substring.

**3. Minimum Add to Make Valid Parentheses**
Count minimum insertions needed to balance.

**4. Remove Invalid Parentheses**
Remove minimum parentheses to make valid.

---

### **Category 2: Next/Previous Greater/Smaller Element**

**5. Next Greater Element I & II**
```
Input: [4, 5, 2, 25]
Output: [5, 25, 25, -1]
```

**6. Previous Smaller Element**
Find the previous smaller element for each element.

**7. Daily Temperatures**
```
Input: [73, 74, 75, 71, 69, 72, 76, 73]
Output: [1, 1, 4, 2, 1, 1, 0, 0]
```
How many days until warmer temperature?

**8. Stock Span Problem** (our earlier discussion!)

---

### **Category 3: Monotonic Stack Applications**

**9. Largest Rectangle in Histogram**
```
Input: heights = [2, 1, 5, 6, 2, 3]
Output: 10
```
Classic monotonic stack problem.

**10. Maximal Rectangle**
Extension to 2D matrices using histogram approach.

**11. Trapping Rain Water**
```
Input: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6
```
Calculate water trapped between bars.

**12. Remove K Digits**
```
Input: num = "1432219", k = 3
Output: "1219"
```
Create smallest number by removing k digits.

**13. Create Maximum Number**
Merge arrays to create lexicographically maximum number.

---

### **Category 4: Expression Evaluation**

**14. Basic Calculator I, II, III**
Evaluate arithmetic expressions with +, -, *, /, parentheses.

**15. Evaluate Reverse Polish Notation**
```
Input: ["2", "1", "+", "3", "*"]
Output: 9  // ((2 + 1) * 3)
```

**16. Decode String**
```
Input: "3[a2[c]]"
Output: "accaccacc"
```

**17. Asteroid Collision**
```
Input: [5, 10, -5]
Output: [5, 10]
```
Simulate asteroid collisions.

---

### **Category 5: String Manipulation**

**18. Remove Duplicate Letters**
```
Input: "bcabc"
Output: "abc"
```
Remove duplicates maintaining lexicographic order.

**19. Simplify Path**
```
Input: "/a/./b/../../c/"
Output: "/c"
```
Simplify Unix-style file paths.

**20. Backspace String Compare**
```
Input: s = "ab#c", t = "ad#c"
Output: true
```

---

### **Category 6: Monotonic Stack (Advanced)**

**21. Sum of Subarray Minimums**
Find sum of minimum elements in all subarrays.

**22. Sum of Subarray Ranges**
Sum of (max - min) for all subarrays.

**23. Number of Visible People in Queue**
```
Input: heights = [10, 6, 8, 5, 11, 9]
Output: [3, 1, 2, 1, 1, 0]
```

**24. Online Stock Span** (real-time version)
Implement with streaming data.

---

### **Category 7: Recursion Simulation**

**25. Binary Tree Inorder Traversal (Iterative)**
Simulate recursion using explicit stack.

**26. Flatten Nested List Iterator**
```
Input: [[1, 1], 2, [1, 1]]
Output: [1, 1, 2, 1, 1]
```

**27. Mini Parser**
Parse nested integer lists.

---

### **Category 8: Design Problems**

**28. Min Stack**
Design stack supporting push, pop, top, and getMin in O(1).

**29. Max Stack**
Similar to min stack but with maximum.

**30. Implement Queue using Stacks**
Use two stacks to simulate queue behavior.

---

## Recognition Patterns - Quick Checklist

When you see a problem, ask yourself:

✅ **Do I need to look back at previous elements?** → Likely stack

✅ **Are there pairs/matches to find?** → Stack for matching

✅ **Do later elements affect earlier ones?** → Monotonic stack

✅ **Is there nested structure?** → Stack for tracking levels

✅ **Do I need to maintain order while removing elements?** → Stack

✅ **Is there a "cancel out" or "collapse" pattern?** → Stack

✅ **Am I simulating recursion iteratively?** → Explicit stack

## Common Mistakes to Avoid

1. **Using stack when array indexing suffices** — Not every "previous element" problem needs a stack
2. **Not considering monotonic stack optimization** — Can reduce O(n²) to O(n)
3. **Confusing stack with queue** — Remember LIFO vs FIFO
4. **Not handling edge cases** — Empty stack, single element, all same values

## Pro Tips

- If you find yourself writing nested loops to look backward/forward, consider a stack
- Monotonic stacks are powerful for "contribution" counting problems
- Stack + HashMap is common for "Next Greater Element" variants
- Practice recognizing the monotonic increasing/decreasing patterns

Would you like me to dive deeper into any specific category, or work through a particular problem in detail?

----- 

