## StringBuilder
JDK 1.5부터 등장
비동기화로 스레드로부터 안전하지 않다(Non Thread Safe).
Thread Safe 구현이 아니므로 StringBuffer에 비해 속도가 빠르다. 

## StringBuffer
동기화되어 스레드로부터 안전하다.(Thread Safe)
Thread Safe 구현이므로 StringBuilder에 비해 속도가 느리다.

## 느낀점
두 객체의 수행 속도의 차이가 있지만 보통 실무에서 문자열을 연결하는 횟수는 한정적이다. 그러므로 환경에 맞춰서 선택을 하자.

---
### Reference
_StringBuilder and StringBuffer in Java_. https://www.baeldung.com/java-string-builder-string-buffer