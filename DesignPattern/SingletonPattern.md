## Singleton Pattern
대상 클래스는 오직 하나의 객체만을 갖도록 보장하고 이에 대한 전역적인 접근점을 제공한다.

장점
- 재사용이 가능하므로 메모리 낭비를 방지할 수 있다.
- 전역적인 성질이 있으므로 다른 객체와 공유가 용이하다.

단점
- 복잡한 역할을 맡을 경우 다른 객체간 결합도가 높아진다.
- 싱글톤 객체 수정시 사용하는 다른 곳에서 사이드 이펙트가 발생할 가능성이 높다.

## 구현 순서
1. 외부 호출을 막기위해 private의 생성자를 만든다.
    ```java
    private Singleton() {
       System.out.println("인스턴스 생성");
    }
    ```
1. 인스턴스를 얻을 메소드로서 getInstance를 생성.
    ```java
    public static Singleton getInstance() {
       if (singleton == null) {
           singleton = new Singleton();
       }
       return singleton;
    }
    ```
1. 테스트
    ```java
    @Test
    public void whenGetTwoSingletonThenIsEquals() {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();

        assertNotNull(s1);
        assertNotNull(s2);
        assertTrue(s1 == s2);
    }
    ```
   
### Thread safe 문제
위에서 만든 방식은 Thread-Safe 하지 않다. 이를 해결하기 위해 getInstance에 synchronized 키워드를 사용하여 문제를 해결할 수 있지만 cost가 높으므로 성능 저하가 일어 날 수 있다.  
문제 해결을 위해 클래스의 로드 시점을 이용한 `initialization-on-demand holder` 기법을 통해 구현한다.
```java
public class SingletonThreadSafe {
    private SingletonThreadSafe() {
        System.out.println("인스턴스 생성");
    }

    private static class LazyHolder {
        private static final SingletonThreadSafe INSTANCE = new SingletonThreadSafe();
    }

    public static SingletonThreadSafe getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```
   
   

### Repository
https://github.com/kangjoshi/DesignPattern/tree/master/src/main/java/singleton

---
### Reference
유키 히로시. _Java 언어로 배우는 디자인 패턴 입문_. 영진닷컴  