## Discard Object
_유효기간이 지난 객체 참조는 폐기하라._

만기참조(다시 이용되지 않을 참조)를 계속해서 유지하는 경우 가비지 컬렉터 대상에서 제외되고 해당 객체를 통해 참조되는 다른 객체들고 가비지 컬렉터의 대상에서 제외되어 메모리 누수가 발생한다.

```java
// 메모리 누수가 발생하는 스택의 pop 메소드
public Object pop() {
    if(size == 0)
        throw new EmptyStackException();
    return elements[--size]
}
// 만기 참조를 제거해주는 코드 추가
public Object pop() {
    if(size == 0)
        throw new EmptyStackException();
    
    Object result = elements[--size];
    elements[size] = null; // 참조를 제거
    return result;
}
```

예시로든 스택과 같이 자체적으로 관리하는 메모리가 있는 클래스를 만들 때는 메모리 누수가 발생하지 않도록 주의 해야한다.

---
### Reference
조슈아블로크. _이펙티브자바2판_. 인사이트  
