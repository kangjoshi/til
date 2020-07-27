## Static Factory Method
_생성자 대신 정적 팩토리 메서드를 사용할 수 없는지 생각해 보라._

장점
1. 이름을 가질수 있다.
    - 객체를 특성에 따라 생성할 때 생성자 방식은 파라미터의 수를 구분하여 사용한다. 하지만 전달되는 파라미터는 객체의 특성에 대해 설명하지 못하지만 정적 팩토리 메소드를 통한 생성은 이름을 통해 객체의 특성을 쉽게 묘사할 수 있다. 
    ```java
    // 생성자 이용
    User jack = new User("jack@gmail.com", "정회원", "Jack");
    User kate = new User("kate@gmail.com", "준회원");   
    ---
    // 정적 팩토리 메서드 이용
    User jack = User.ofRegular("jack@gmail.com", "Jack");
    User kate = User.ofAssociate("kate@gmail.com");
    ```
1. 생성자와 달리 호출때 마다 새로운 객체를 생성할 필요는 없다.
    - immutable 클래스라면 이미 만들어 둔 객체를 활용할 수 있다.
    ```java
    // 새로운 객체를 생성하지 않고 만들어진 객체를 리턴
    Boolean isDone = Boolean.valueOf(true);
    ```
1. 생성자와 달리 반환되는 객체의 클래스를 유연하게 결정할 수 있다.
1. 형인자 자료형 객체를 만들 때 편하다.

단점
1. 정적 팩터리 메서드만 있는 클래스를 만들면 접근 가능한 생성자가 없으므로 하위 클래스를 만들 수 없다.
    - 계승보다는 구성을 지향하는 설계의 경우 장점이 된다.
1. 정적 팩터리 메서드가 다른 정적 메서드와 확연히 구분되진 않는다.
    - valueOf, of, getInstance와 같이 이름 규칙을 정한다.

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
