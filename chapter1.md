# 1. Recursive



```java
package com.example.recursive;

public class Rec1 {

	static void p() throws InterruptedException {
		System.out.println("hello");
		Thread.sleep(500);
		
		// 메소드 자신을 재 호출하는 것은 무한루프에 빠뜨린다.
		// 메소드 호출 시 마다 메소드 로직 처리를 위한 스택이 매번 생성된다.
		// 스택의 개수는 한계가 있으므로 결국 StackOverflowError가 발생한다.
		p();
	}

	public static void main(String[] args) throws InterruptedException {
		// 촉발(trigger)
		p();
	}

}
```



```
package com.example.recursive;

public class Rec2 {
	// 스택이 몇개 만들어지는지 알려주는 도구는 없다. 대신 직접 메소드 호출 횟수를 카운트한다.
	static int count = 0;

	static void p() throws InterruptedException {
		// 재귀호출은 무한루프에 빠지지 않기 위한 제어가 필요하다.
		if (count < 5) {
			count++;
			System.out.println("hello " + count);
			Thread.sleep(500);
			p();
		}
	}

	public static void main(String[] args) throws InterruptedException {
		p();
	}
}
```



