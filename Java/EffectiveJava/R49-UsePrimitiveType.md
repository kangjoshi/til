## use primitive class
_객체화된 기본 자료형 대신 기본 자료형을 이용하라._

모든 자료형에는 대응되는 참조 자료형이 있다. 이를 객체화된 기본 자료형(boxed primitive type)이라 한다.

Java 1.5 부터 추가된 자동 객체화(auto-boxing)와 자동 비객체화(auto-unboxing)로 코딩시 번거로움을 줄여 주지만 기본 자료형과 객체 표현형 간의 차이가 희미하게 되었다. 
하지만 실질적인 차이가 있으므로 어떤 것을 사용할지 신중하게 결정해야 한다.

차이점
1. 기본 자료형은 값만 가지지만 객체화된 기본 자료형은 값 외에도 신원(identity)를 가진다. 즉 기본 자료형이 두개가 있을때 값이 같더라도 신원은 다를 수 있다.
1. 기본 자료형에 저장되는 값은 전부 기능적으로 완전한 값 이지만 객체화된 기본 자료형에 저장되는 값은 null이 될 수도 있다.
1. 기본 자료형은 시간이나 공간 요구량 측면에서 일반적으로 객체 표현형보다 효율적이다.

문제점
1. 객체화된 기본 자료형에용 값을 비교할 목적으로 == 연산자를 사용하면 오류가 발생할 확률이 아주 높다.
    ```java
    public int compare(Integer number1, Integer number2) {
       return number1 < number2 ? -1 : (number1 == number2) ? 0 : 1;
    }
    // number1 < number2  <- number1과 number2가 auto-unboxing되어 값에 대한 비교가 된다.
    // number1 == number2 <- number1과 number2가 값에 대한 비교가 아닌 객체에 대한 비교를 한다.
    ``` 
2. 객체화된 기본 자료형과 기본 자료형을 한 연산 안에 엮어 놓으면 객체화된 기본 자료형은 auto-unboxing된다.
    ```java
    Integer num;
    if(num == 42)
       System.out.println("Unbelievable");
    // num의 값은 null이므로 NullPointerException 발생
    ```
3. 객체화된 기본 자료형을 sum과 같은 여러번 같은 종류의 연산을 수행하는 경우 성능 문제가 생길 수 있다.
    ```java
    Long sum = 0L;
    for(long i=0; i<Integer.MAX_VALUE; i++)
       sum += i;
   
    // 오류는 없지만 Long sum은 계속해서 boxing, unboxing을 반복하고 그 과정에서 많은 객체를 생성하므로 성능 문제 발생
    ```    

객체화된 기본 자료형의 사용
1. 컬렉션의 요소, 키로 사용
1. 리플렉션을 통해 메서드를 호출할 때도 객체화된 기본 자료형을 사용
   

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
