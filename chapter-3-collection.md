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
	 * 	1	2	4
	 * 	3	5	7
	 * 	6	8	9
	 * 
	 * 	회전		처리좌표				
	 * 	1		0.0	
	 * 	2		0.1 1.0	
	 * 	3		0.2	1.1	2.0	
	 * 	4		1.2	2.1	
	 * 	5		2.2	
	 * 
	 *  i		j		k=i-j		k>=0		k<3		nums[j][k]
	 *  -------------------------------------------------------------------------------------
	 *  0		0		0-0						nums[0][0]=1
	 *  		1		0-1		x
	 *  -------------------------------------------------------------------------------------
	 *  1		0		1-0						nums[0][1]=2
	 *  		1		1-1						nums[1][0]=3
	 *  		2		1-2		x
	 *  -------------------------------------------------------------------------------------
	 *  2		0		2-0						nums[0][2]=4
	 *  		1		2-1						nums[1][1]=5
	 *  		2		2-2						nums[2][0]=6
	 *  -------------------------------------------------------------------------------------
	 *  3		0		3-0				x		
	 *  		1		3-1						nums[1][2]=7
	 *  		2		3-2						nums[2][1]=8
	 *  -------------------------------------------------------------------------------------
	 *  4		0		4-0				x
	 *  		1		4-1				x
	 *  		2		4-2						nums[2][2]=9
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
	 * 	1	3	6
	 * 	2	5	8
	 * 	4	7	9
	 * 
	 * 	회전		처리좌표
	 * 	1		0.0
	 * 	2		1.0 0.1
	 * 	3		2.0 1.1 0.2
	 * 	4		2.1 1.2
	 * 	5		2.2
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



