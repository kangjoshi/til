## Private Constructor
_객체 생성이 필요 없을 때는 private 생성자를 이용하여 생성을 막아라._

유틸리티 클래스는 대게 static 메서드로 기능을 제공한다.  
생성자를 생략하면 컴파일러는 기본 public 생성자를 만든다. 그러므로 명시적으로 private 생성자 두어 객체 생성을 하지 못하게 해야한다.
  
```java
public class CustomUtil {
    private CustomUtil() {
        throw new AssertionError("객체 생성이 허용 되지 않은 클래스");
    }
    ...    
}
```

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
