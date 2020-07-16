## SymbolTable
키/값 쌍에 대한 데이터 구조로 삽입과 탐색을 지원한다.  
삽입(put)은 새로운 쌍을 테이블에 저장하는 것이고 탐색(get)은 주어진 키에 연관된 값을 찾는 것이다.

## 제약 사항
1. 키 하나에는 하나의 값만 연관된다.
1. 이미 존재하는 키에 대해 키/값을 다시 입력할 경우 대체된다.
1. 키는 null 아니여야 한다.
1. 테이블에 존재하지 않는 키에대해 값으로 null을 리턴하므로 키에 연관된 값으로 null을 사용하지 않는다.
1. 삭제에는 두가지 타입이 있다. 
    - 느긋한 삭제 : 키를 null로 설정해 두었다가 나중에 삭제
    - 성급한 삭제 : 테이블에서 키를 즉시 삭제

## 순차 SymbolTable
키의 객체가 Comparable을 구현한 경우에는 키 간의 순서를 정렬하여 관리하고 이를 이용하여 여러가지 효율적인 작업을 정의 할 수 있다.
 
---
### Repository
https://github.com/kangjoshi/data-structure.git  
1. 비순차 [Code](https://github.com/kangjoshi/data-structure/blob/master/src/main/java/SequentialSymbolTable.java), [Test](https://github.com/kangjoshi/data-structure/blob/master/src/test/java/SequentialSymbolTableTest.java)  
1. 순차  
    추후 구현
    
---
### Reference
로버트세지윅, 케빈웨인. _Algorithms Fourth Edition_. 길벗  