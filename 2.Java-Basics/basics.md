## Array vs Arraylist

| Feature       | Array                                      | ArrayList                                  | When to Use                                                                 |
|--------------|-------------------------------------------|-------------------------------------------|-----------------------------------------------------------------------------   |
| **Size**     | Fixed at creation                        | Dynamic (can grow/shrink)                | Use Array when size is known and constant; ArrayList when size changes           |
| **Type**     | Can store primitives & objects           | Stores objects only                      | Use Array for primitives; ArrayList for object collections                       |
| **Performance** | Faster for direct access              | Slight overhead due to resizing          | Use Array for performance-critical tasks                                         |
| **Memory**   | Less memory overhead                     | More memory (capacity + resizing)        | Use Array when memory is tight                                                   |
| **Methods**  | No built-in methods for manipulation     | Many utility methods (`add()`, `remove()`, etc.) | Use ArrayList for frequent insertions/removals                           |
| **Resizing** | Not possible                             | Automatic                                 | Use ArrayList when flexibility is needed                                        |
| **Part of**  | Core Java                                | Java Collections Framework               | Use ArrayList for modern collection handling                                     |
| **Example**  | `int[] arr = new int[5];`               | `ArrayList<Integer> list = new ArrayList<>();` | Depends on use case                                                       

## to count elements
Array uses **.length** method

Arraylist uses **.size** method

### for loop

 **For Loop** when you need index control or modify elements.

 Syntax:

 for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

### Enhanced for loop

Use **Enhanced For Loop** for clean, readable iteration when index is not needed.

syntax:

for (int num : arr) {
    System.out.println(num);
}
