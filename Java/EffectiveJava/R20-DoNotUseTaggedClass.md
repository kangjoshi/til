## Tagged Class
_태그 달린 클래스 대신 클래스 계층을 활용하라._

### 태그 달린 클래스 (Tagged Class)
```java
class Figure {
    enum Shape { RECTANGLE, CIRCLE }

    final Shape shape; // 사각형, 원형인지 태그
    
    // 사각형에서만 사용되는 length, width
    double length; 
    double width;
    
    // 원형에서만 사용되는 radius
    double radius;
        
    // 사각형을 만들기 위한 생성자
    public Figure(double length, double width) {
        this.length = length;
        this.width = width;
    }
    
    // 원형을 만들기 위한 생성자
    public Figure(double radius) {
        this.radius = radius;
    }   
}
```

##### 태그 달린 클래스의 문제점
1. enum 선언, 태그 필드, 구분을 위한 분기 처리등 코드가 반복되는 클래스가 만들어진다.
2. 서로 다른 기능을 처리하기 위한 코드가 한 클래스에 모여 있으니 가독성도 떨어짐.
3. 객체를 만들 때 마다 불필요한 필드도 함께 생성되므로 메모리 요구량이 늘어남.

### 하위 자료형으로 정의
```java
abstract class Figure {
    abstract double area();
}

class Circle extends Figure {
    final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
}

class Rectangle extends Figure {
    final double length;
    final double width;
    
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
}
```
- 사각형과 원형을 각각 클래스로 분리
- 특정한 기능에만 필요한 데이터, 메서드는 해당 기능의 클래스에만 넣는다.
- 태그 달린 클래스의 단점을 해결할 수 있다.


---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
