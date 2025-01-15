---
layout: single
title:  "[JAVA] Pass By Value와 Pass By Reference"
---

## Pass by value
복사된 데이터를 전달하여 값을 수정하여도 원본 데이터에는 영향을 주지 않도록 하는 방식이다.
예를 들면, 책의 복사본을 친구에게 주고 그 친구가 복사본을 아무리 수정한다고 해도 책 원본의 내용은 그대로인 것과 같다.

## Pass by reference
주소 값을 전달하여 실제 값에 대한 Alias를 구성함으로써 값을 수정하면 원본 데이터도 수정되는 방식이다.
예를 들어, Google Docs와 같은 클라우드 문서를 친구와 공유했다고 가정하자.
친구가 문서를 열어 내용을 수정하면 문서가 즉시 업데이트되어 내가 볼 때도 바뀐 상태가 된다.문서를 참조한 상태이므로, 원본 문서 자체에 영향을 준다.

```java
public class Test {
    public static void main(String[] args) {
        int value = 10;

        modifyValue(value);
    }

    static void modifyValue(int value) {
        value = 100;
    }
}
```


```java
class Person{
		private String name; // 이름
		private int age; // 나이
		
		public	Person(String name, int age){
			this.name = name;
			this.age = age;
		}
		
		// 이름 변경 메서드
		public void modifyName(String name){
			this.name = name;
		}
}
```


```java
public class Test{
	public static void main(String[] args){

		Person dudu = new Person("dudu", 24); // 객체를 생성
	
		dudu.modifyName("sun"); // 이름을 변경
	}
}
```

```java
public class Test{
	public static void main(String[] args){

		Person dudu = new Person("dudu", 24); 
		// dudu 객체를 생성, Reference Type변수인 dudu에는 객체의 레퍼런스 값을 저장한다.
		
		Person sun = changePerson(dudu);
		// 메서드 인자로 dudu의 값(주소값)을 전달한다


	}

		static void changePerson(Person afterPerson){
			afterPerson = new Person("sun", 22);

			// 넘어온 인자, afterPerson는 dudu를 가르키고 있지만
			// afterPerson은 새로운 객체를 생성하여 힙에 메모리를 할당하고 레퍼런스 값을 리턴한다.
			// 메서드를 나가면 스택에서 사라지고 sun을 가르키는 변수가 없으므로 GC에 의해 처리가 된다.
			// 그러므로 여전히 sun변수에는 여전히 dudu Person객체 레퍼런스가 담겨있다.
			
			
		}
}
```



