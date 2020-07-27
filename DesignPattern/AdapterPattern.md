## Adapter Pattern = 래퍼(Wrapper)
이미 제공되어 있는 것을 그대로 사용할 수 없는 경우 필요한 형태로 바꿔서 사용  
서로 일치하지 않는 인터페이스를 갖는 클래스들을 함께 동작시킨다.   
상속을 통한 방법과 위임을 통한 방법이 있다. 

### 위임을 통한 구현 예제
```java
// Adaptee (이미 구현되어 있는 메소드)
public class Banner {
    private String title;
    private StringBuilder sb;

    public Banner(String title) {
        this.title = title;
        this.sb = new StringBuilder();
    }

    private void clear() {
        sb.delete(0, sb.length());
    }

    public void showWithBrackets() {
        sb.append("(").append(title).append(")");
        System.out.println(sb.toString());
        clear();
    }

    public void showWithAsters() {
        sb.append("*").append(title).append("*");
        System.out.println(sb.toString());
        clear();
    }
}
```
```java
// Target (Client로 부터 호출되는 메소드를 제공)
public interface Print {
    void printWeak();
    void printStrong();
}
```
```java
// Adapter (Target 인터페이스에 Adaptee의 인터페이스를 적응시키는 클래스)
public class PrintBanner implements Print {

    private Banner banner;

    public PrintBanner(String title) {
        this.banner = new Banner(title);
    }

    @Override
    public void printWeak() {
        banner.showWithBrackets();
    }

    @Override
    public void printStrong() {
        banner.showWithAsters();
    }
}
```
```java
// Client (Target 메서드 호출)
@Test
    public void printTest() {
        Print print = new PrintBanner("Good Life");

        print.printStrong();
        print.printWeak();
    }
```

### Repository
https://github.com/kangjoshi/DesignPattern/tree/master/src/main/java/adapter

---
### Reference
유키 히로시. _Java 언어로 배우는 디자인 패턴 입문_. 영진닷컴  