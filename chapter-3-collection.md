# Chapter 3. Collection

## Array Rotate

```java
package com.example.sample0array;

public class ArrayRotate {
    private static final int ROTATE_LEFT = -1;
    private static final int ROTATE_RIGHT = 1;
    private int[][] nums;
    private int[][] temp;

    public static void main(String[] args) {
        ArrayRotate rotater = new ArrayRotate();

        rotater.createArray(3);
        rotater.printArray();

        System.out.println("  90도 좌회전");
        rotater.rotate(ROTATE_LEFT);
        rotater.printArray();

        System.out.println("  90도 좌회전");
        rotater.rotate(ROTATE_LEFT);
        rotater.printArray();

        System.out.println("  90도 좌회전");
        rotater.rotate(ROTATE_LEFT);
        rotater.printArray();

        System.out.println("  90도 좌회전");
        rotater.rotate(ROTATE_LEFT);
        rotater.printArray();

        System.out.println("  90도 우회전");
        rotater.rotate(ROTATE_RIGHT);
        rotater.printArray();

        System.out.println("  90도 우회전");
        rotater.rotate(ROTATE_RIGHT);
        rotater.printArray();

        System.out.println("  90도 우회전");
        rotater.rotate(ROTATE_RIGHT);
        rotater.printArray();

        System.out.println("  90도 우회전");
        rotater.rotate(ROTATE_RIGHT);
        rotater.printArray();
    }

    private void rotate(int direction) {
        if (direction == -1) {
            rotateLeft();
        } else {
            rotateRight();
        }
    }

    /*
     *     0.0        0.1        0.2        ==>        2.0        1.0        0.0
     */
    private void rotateRight() {
        temp = new int[nums.length][nums.length];

        for (int i = 0; i < nums.length; i++) {
            int x = nums.length-1;
            for (int j = 0; j < nums.length; j++) {
                temp[i][j] = nums[x][i];
                x--;
            }
        }

        nums = temp;
    }

    /*
     *     0.0        0.1        0.2        ==>        0.2        1.2        2.2
     */
    private void rotateLeft() {
        temp = new int[nums.length][nums.length];

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                temp[i][j] = nums[j][nums.length-1-i];
            }
        }

        nums = temp;
    }

    private void printArray() {
        if (nums != null) {
            for (int i = 0; i < nums.length; i++) {
                for (int j = 0; j < nums.length; j++) {
                    System.out.printf("%3d", nums[i][j]);
                }
                System.out.println();
            }
            System.out.println("  -------   ");
        }
    }

    private void createArray(int size) {
        int count = 1;
        nums = new int[size][size];

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                nums[i][j] = count;
                count++;
            }
        }
    }

}
```

```
  1  2  3
  4  5  6
  7  8  9
  -------   
  90도 좌회전
  3  6  9
  2  5  8
  1  4  7
  -------   
  90도 좌회전
  9  8  7
  6  5  4
  3  2  1
  -------   
  90도 좌회전
  7  4  1
  8  5  2
  9  6  3
  -------   
  90도 좌회전
  1  2  3
  4  5  6
  7  8  9
  -------   
  90도 우회전
  7  4  1
  8  5  2
  9  6  3
  -------   
  90도 우회전
  9  8  7
  6  5  4
  3  2  1
  -------   
  90도 우회전
  3  6  9
  2  5  8
  1  4  7
  -------   
  90도 우회전
  1  2  3
  4  5  6
  7  8  9
  -------
```

## Array Diagonal

```java
package com.example.sample0array;

public class ArrayDiagonal {
    private int[][] nums;

    public static void main(String[] args) {
        ArrayDiagonal rotater = new ArrayDiagonal();

        rotater.createArrayUpDown(3);
        rotater.printArray();

        rotater.createArrayBottomUp(3);
        rotater.printArray();
    }

    private void printArray() {
        if (nums != null) {
            for (int i = 0; i < nums.length; i++) {
                for (int j = 0; j < nums.length; j++) {
                    System.out.printf("%3d", nums[i][j]);
                }
                System.out.println();
            }
            System.out.println("  -------   ");
        }
    }

    /*
     *     1    2    4
     *     3    5    7
     *     6    8    9
     * 
     *     회전        처리좌표                
     *     1        0.0    
     *     2        0.1 1.0    
     *     3        0.2    1.1    2.0    
     *     4        1.2    2.1    
     *     5        2.2    
     * 
     *  i        j        k=i-j        k>=0        k<3       nums[j][k]
     *  ---------------------------------------------------------------
     *  0        0        0-0                                nums[0][0]=1
     *           1        0-1            x
     *  ---------------------------------------------------------------
     *  1        0        1-0                                nums[0][1]=2
     *           1        1-1                                nums[1][0]=3
     *           2        1-2            x
     *  ---------------------------------------------------------------
     *  2        0        2-0                                nums[0][2]=4
     *           1        2-1                                nums[1][1]=5
     *           2        2-2                                nums[2][0]=6
     *  ---------------------------------------------------------------
     *  3        0        3-0                        x        
     *           1        3-1                                nums[1][2]=7
     *           2        3-2                                nums[2][1]=8
     *  ---------------------------------------------------------------
     *  4        0        4-0                        x
     *           1        4-1                        x
     *           2        4-2                                nums[2][2]=9
     */
    private void createArrayUpDown(int size) {
        int count = 1;
        nums = new int[size][size];
        int cycle = 2 * size - 1;

        for (int i = 0; i < cycle; i++) {
            for (int j = 0; j < nums.length; j++) {

                int k = i - j;
                if (k >= 0 && k < size) {
                    System.out.printf("nums[%d][%d] = %d\n", j, k, count);
                    nums[j][k] = count;
                    count++;
                }
            }
        }
    }

    /*
     *     1    3    6
     *     2    5    8
     *     4    7    9
     * 
     *     회전        처리좌표
     *     1        0.0
     *     2        1.0 0.1
     *     3        2.0 1.1 0.2
     *     4        2.1 1.2
     *     5        2.2
     */
    private void createArrayBottomUp(int size) {
        int count = 1;
        nums = new int[size][size];
        int cycle = 2 * size - 1;

        for (int i = 0; i < cycle; i++) {
            for (int j = 0; j < nums.length; j++) {

                int k = i - j;
                if (k >= 0 && k < size) {
                    System.out.printf("nums[%d][%d] = %d\n", k, j, count);
                    nums[k][j] = count;
                    count++;
                }
            }
        }
    }

}
```

```
nums[0][0] = 1
nums[0][1] = 2
nums[1][0] = 3
nums[0][2] = 4
nums[1][1] = 5
nums[2][0] = 6
nums[1][2] = 7
nums[2][1] = 8
nums[2][2] = 9
  1  2  4
  3  5  7
  6  8  9
  -------   
nums[0][0] = 1
nums[1][0] = 2
nums[0][1] = 3
nums[2][0] = 4
nums[1][1] = 5
nums[0][2] = 6
nums[2][1] = 7
nums[1][2] = 8
nums[2][2] = 9
  1  3  6
  2  5  8
  4  7  9
  -------
```

## Stack

```java
package com.example.sample1stack;

public interface Stack {
    boolean isEmpty();
    void push(Object item);
    Object pop();
    boolean delete();
    Object peek();
    int size();
}
```

## Array Stack

```java
package com.example.sample1stack;

public class ArrayStack implements Stack {
    private int top = -1;
    private Object[] items = new Object[10];

    private Object[] sizeUpAndCopy() {
        Object[] temp = new Object[items.length+10];
        for (int i = 0; i < items.length; i++) {
            temp[i] = items[i];
        }
        return temp;
    }

    @Override
    public boolean isEmpty() {
        return top == -1;
    }

    private boolean isFull() {
        return top == (items.length - 1);
    }

    @Override
    public void push(Object item) {
        if (isFull()) {
            items = sizeUpAndCopy();
            items[++top] = item;
        } else {
            items[++top] = item;
        }
    }

    @Override
    public Object pop() {
        if (isEmpty()) {
            return null;
        } else {
            return items[top--];
        }
    }

    @Override
    public boolean delete() {
        if (isEmpty()) {
            return false;
        } else {
            pop();
            return true;
        }
    }

    @Override
    public Object peek() {
        if (isEmpty()) {
            return null;
        } else {
            return items[top];
        }
    }

    @Override
    public int size() {
        return top + 1;
    }

    @Override
    public String toString() {
        if (isEmpty()) {
            return "[t --> null]";
        }
        StringBuilder sb = new StringBuilder();
        sb.append("[t --> ");

        for (int i = top; i >= 0; i--) {
            if (!(i == top)) {
                sb.append(",");
            }
            sb.append(items[i].toString());
        }

        sb.append("]");
        return sb.toString();
    }

    public static void main(String[] args) {
        basicTest();
        sizeUpTest();
    }

    private static void sizeUpTest() {
        System.out.println("------------");
        ArrayStack stack = new ArrayStack();
        System.out.println(stack.size());
        System.out.println(stack);

        for (int i = 0; i < 21; i++) {
            stack.push(i);
        }

        System.out.println(stack.size());
        System.out.println(stack);
    }

    private static void basicTest() {
        ArrayStack stack = new ArrayStack();
        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println(stack.peek());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.pop());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.delete());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.isEmpty());
        System.out.println(stack.pop());
        System.out.println(stack.isEmpty());
        System.out.println(stack);
    }
}
```

```
30
3
[t --> 30,20,10]
------------
30
2
[t --> 20,10]
------------
true
1
[t --> 10]
------------
false
10
true
[t --> null]
------------
0
[t --> null]
21
[t --> 20,19,18,17,16,15,14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
```

## Linked Stack

```java
package com.example.sample1stack;

public class LinkedStack implements Stack {
    private StackNode top;

    @Override
    public boolean isEmpty() {
        return top == null;
    }

    @Override
    public void push(Object item) {
        StackNode node = new StackNode();
        node.data = item;
        node.link = top;

        top = node;
    }

    @Override
    public Object pop() {
        if (isEmpty()) {
            return null;
        }
        Object item = top.data;
        top = top.link;
        return item;
    }

    @Override
    public boolean delete() {
        if (isEmpty()) {
            return false;
        }
        top = top.link;
        return true;
    }

    @Override
    public Object peek() {
        if (isEmpty()) {
            return null;
        }
        return top.data;
    }

    @Override
    public int size() {
        if (isEmpty()) {
            return 0;
        }
        int count = 0;
        StackNode temp = top;
        while (temp != null) {
            count++;
            temp = temp.link;
        }
        return count;
    }

    @Override
    public String toString() {
        if (isEmpty()) {
            return "[t --> null]";
        }
        StringBuilder sb = new StringBuilder();
        sb.append("[t --> ");

        StackNode temp = top;
        while (temp != null) {
            sb.append(temp.data);
            temp = temp.link;

            if (temp != null) {
                sb.append(",");
            }
        }

        sb.append("]");
        return sb.toString();
    }

    public static void main(String[] args) {
        basicTest();
        noBoundTest();
    }

    private static void noBoundTest() {
        System.out.println("------------");
        LinkedStack stack = new LinkedStack();
        System.out.println(stack.size());
        System.out.println(stack);

        for (int i = 0; i < 21; i++) {
            stack.push(i);
        }

        System.out.println(stack.size());
        System.out.println(stack);
    }

    private static void basicTest() {
        LinkedStack stack = new LinkedStack();
        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println(stack.peek());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.pop());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.delete());
        System.out.println(stack.size());
        System.out.println(stack);
        System.out.println("------------");
        System.out.println(stack.isEmpty());
        System.out.println(stack.pop());
        System.out.println(stack.isEmpty());
        System.out.println(stack);
    }
}

class StackNode {
    Object data;
    StackNode link;
}
```

```
30
3
[t --> 30,20,10]
------------
30
2
[t --> 20,10]
------------
true
1
[t --> 10]
------------
false
10
true
[t --> null]
------------
0
[t --> null]
21
[t --> 20,19,18,17,16,15,14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
```

## Reverse Word

```java
package com.example.sample1stack;

public class ReverseWord {

    public static void main(String[] args) {
        String txt = "hello";

        ArrayStack v = new ArrayStack();

        for (int i = 0; i < txt.length(); i++) {
            v.push(txt.charAt(i));
        }

        char[] temp = new char[txt.length()];

        for (int i = 0; i < txt.length(); i++) {
            temp[i] = (char) v.pop();
        }

        String result = new String(temp);

        System.out.println("txt = "+txt);
        System.out.println("result = "+result);
    }

}
```

```
txt = hello
result = olleh
```



