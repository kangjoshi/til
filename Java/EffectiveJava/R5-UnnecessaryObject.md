## Unnecessary Object
_불필요한 객체는 만들지 말라._

immutable 객체나 기능적으로 동일한 객체는 새로 생성하는 것 보다 재사용하는 편이 빠르고 더 우아하다.
```java
// 실행될 때 마다 immutable 클래스인 String 객체를 새로 생성하는 코드
String s = new String("A hole new world");
// 실행될 때 마다 같은 JVM에서 실행되는 모든 코드가 동일한 String 객체를 사용한다.
String s = "A hole new world";
```

생성자와 정적 팩터리 메서드를 함께 제공하는 immutable 클래스의 경우  
생성자 대신 정적 팩터리 메서드를 이용하면 불필요한 객체 생성을 피할 수 있을 때 가 많다.
```java
//아래쪽이 코드를 사용하자.
new Boolean("true)
Boolean.valueOf("true"); 
```

기본 자료형을 사용할 수 있을때는 기본 자료형을 사용 해야한다.
```java
// sum에 더해질때마다 Long 객체가 하나씩 만들어진다.
public static void main(String[] args) {
    Long sum = 0L;  // ==> long sum이 되어야 한다.
    for (long i=0; i<Integer.MAX_VALUE; i++)
        sum += i;
    
    System.out.println(sum);    
}
```

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
