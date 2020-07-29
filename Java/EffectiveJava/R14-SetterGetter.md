## Setter, Getter
_public 필드를 두지 말고 접근자 메서드를 사용하라._

```java
// 데이터 필드를 직접적으로 조작할 수 있으므로 정보은닉, 캡슐화 이점을 누릴 수 없다.
class Point {
    public double x;
}

// 필드는 private으로 직접적인 접근이 불가능하게 하고 public 접근 가능한 메서드를 제공 해야한다.
class Point {
    private double x;
    
    public double getX() {
        return this.x;
    }
    
    public void SetX(double x) {
        this.x = x;
    }
}
```

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
