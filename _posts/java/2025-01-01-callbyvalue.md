---
layout: single
title:  "[JAVA] Pass By Value와 Pass By Reference"
categories: java
tag: java
---
## Pass by value
복사된 데이터를 전달하여 값을 수정해도 원본 데이터에는 영향을 주지 않도록 하는 방식이다. 

예를 들면, 책의 복사본을 친구에게 주고 그 친구가 복사본을 아무리 수정해도 책 원본의 내용은 그대로인 것과 같다.

## Pass by reference
주소 값을 전달하여 실제 값에 대한 별칭(alias)을 구성함으로써 값을 수정하면 원본 데이터도 수정되는 방식이다. 

예를 들어, Google Docs와 같은 클라우드 문서를 친구와 공유한다고 가정해보자. 

친구가 문서를 열어 내용을 수정하면 문서가 즉시 업데이트되어 내가 볼 때도 바뀐 상태가 된다. 문서를 참조한 상태이므로, 원본 문서 자체에 영향을 준다.

## Primitive type
아래는 `modifyValue`라는 메서드로 `value`를 수정하는 코드이다. 

메서드를 실행하면 `value`의 값은 어떻게 바뀔까?
```java
public class Test {
    public static void main(String[] args) {
        int value = 10;

        modifyValue(value);
        System.out.println("value = " + value);
    }

    static void modifyValue(int value) {
        value = 100;
    }
}
```
코드를 실행해보면 `value`를 출력하면 100이 아닌 10이 결과로 나온다.

이유는 복사된 Primitive 값, 즉 10이 전달되기 때문에 `modifyValue` 메서드로 값을 변경을 아무리 해도 `value`에는 여전히 10이 들어있다.

## Reference type
다음은 사람(Person)이라는 클래스이다. 

이름(name)과 나이(age)라는 멤버 변수가 선언되어 있다.
```java
class Person {
    private String name; // 이름
    private int age; // 나이

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 이름 변경 메서드
    public void modifyName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```
사람(Person) 클래스를 사용하여 객체를 생성하고 `modifyName` 메서드로 이름을 변경해 보자. 

과연 이름이 바뀔까?
```java
public class Test {
    public static void main(String[] args) {
        Person dudu = new Person("dudu", 24); // 객체를 생성

        dudu.modifyName("sun"); // 이름을 변경
        System.out.println("Updated name: " + dudu.getName()); // 이름이 sun으로 변경됨을 확인
    }
}
```
결론을 말하자면 실제로 이름이 바뀐다. `new Person()`을 호출하면 객체가 생성된다. 

즉, 힙에 메모리를 할당하고 참조 값을 리턴한다.

객체의 `modifyName` 메서드를 호출하면 참조를 통해 name을 변경하기 때문에 생성된 객체의 이름이 변경된다.

### 자바는 pass by value만?
구글링하다 보면 가끔 "자바는 pass by reference는 없고 pass by value만 있다"라고 알려준다.

→ 객체가 전달되었을 때 필드에 접근하여 값을 수정하는 것은 가능하지만 그 객체 자체를 변경하는 것은 불가능하다는 의미이다.


아래 코드는 객체의 참조값을 전달해서 객체를 변경하고자 한다. 결론을 먼저 말하면 객체 자체를 변경하는 것은 불가능하다.

```java
public class Test {
    public static void main(String[] args) {
        Person dudu = new Person("dudu", 24); 
        // dudu 객체를 생성, Reference Type 변수인 dudu에는 객체의 참조값이 저장된다.

        changePerson(dudu);
        // 메서드 인자로 dudu의 값(주소값)을 전달한다

        System.out.println("Updated name: " + dudu.getName()); // 여전히 dudu
    }

    static void changePerson(Person person) {
        person = new Person("sun", 22);    
    }
}
```
`changePerson` 메서드에서 넘어온 인자 `person`은 `dudu`를 가리키고 있지만, 

`person`은 `new Person()`을 통해 새로운 객체를 생성하여 힙에 메모리를 할당하고 참조 값을 리턴한다. 

`changePerson` 메서드가 종료되면 `person` 변수가 스택에서 사라지고 

이름이 `sun`이고 나이가 `22`인 사람을 가리키는 변수가 없으므로 GC에 의해 처리된다. 

그러므로 여전히 `dudu`에는 이름이 `dudu`이고 나이가 `24`인 Person 객체의 참조가 담겨있다.

## 요약
- 자바에서는 기본적으로 모든 값을 전달할 때 pass by value 방식을 사용한다.
- Primitive type(기본형 변수)은 값이 복사되어 전달되므로 메서드 내에서 값을 변경해도 원본에는 영향을 주지 않는다.
- Reference type(참조형 변수)은 객체의 참조값이 복사되어 전달되므로 메서드 내에서 객체의 필드를 변경하면 원본 객체에 영향을 준다. 
하지만 객체 자체를 새로운 객체로 변경하는 것은 불가능하다.
