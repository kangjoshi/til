## Hashtable
효율적인 탐색을 위한 키 & 값 쌍의 자료구조  
탐색 시간은 O(1), 최악의 경우 O(N)이다.

값을 넣을때
1. 키의 해시 코드 계산
1. 계산된 해시 코드와 배열의 길이로 모듈러 연산(hash(key) % array.length)하여 배열의 인덱스 값을 계산
1. 키와 값를 계산된 인덱스 위치에 저장, 충돌에 대비하여 반드시 연결리스트를 이용해야 한다.(해시 충돌 방지)

값을 찾을때
1. 주어진 키로 부터 해시 코드 계산
1. 해시 코드를 이용해 인덱스 계산
1. 인덱스에 저장되어 있는 배열에서 해당 키에 상응 하는 값을 탐색

## 해시 함수
키를 배열의 인덱스 값으로 변환하는 함수  
이상적인 함수는 정수범위 (0 ~ N-1)를 변환 범위로 하면서, 계산 하기 쉽고, 인덱스가 균일하게 분포 시켜주는 함수이다.  

정수를 해싱할 때 가장 흔하게 사용되는 방법은 모듈러 해싱 (key % array.length)이다. (주의사항 array.length는 균일한 분포를 위해 소수를 선택하는 것이 좋다.)

```java
public int hash(K key) {
    return (key.hashCode() & 0x7fffffff) % this.capacity;
}
```

`& 0x7fffffff` Index가 음수가 나오는것을 방지하기 위해 연산


## 해시 충돌
서로 다른 키라면 서로 다른 인덱스로 매핑 되는것이 이상적이지만 서로 다른 키가 같은 배열 인덱스에 매핑되는 경우가 발생한다.
충돌에 따른 충돌 처리는 아래 두가지 방법이 있다.

개별 체이닝
- 해시를 통한 배열 인덱스 값이 같은 복수의 항목을 연결리스트로 관리
- 어떤 크기(해시 테이블의 크기)를 지정해도 탄력적이다. 즉 기대했던것 보다 좀 더 많은 키가 삽입되면 탐색 시간이 조금 더 느려질 뿐이고 더 적은 키가 삽입되고 약간의 공간만 낭비 된다는 문제만 가지고 있다.
- [Code](https://github.com/kangjoshi/data-structure/blob/master/src/main/java/SeparateChainingHashtable.java), [Test](https://github.com/kangjoshi/data-structure/blob/master/src/test/java/SeparateChainingHashtableTest.java)
      
선형 탐지를 이용한 해싱
- 테이블의 크기가 저장할 키의 수보다 큰 경우 해시 값이 충돌 날 때 존재하는 빈 공간에 의존하여 충돌을 해소(개방형 주소 지정 해싱)
- 메모리 공간을 연결 리스트에 허비하는 대신 해시 테이블의 키 저장 공간에 사용 하는것.
- [Code](https://github.com/kangjoshi/data-structure/blob/master/src/main/java/hashtable/LinearProbingHashtable.java), [Test](https://github.com/kangjoshi/data-structure/blob/master/src/test/java/hashtable/LinearProbingHashtableTest.java)

### Repository
https://github.com/kangjoshi/data-structure.git  

---
### Reference
로버트세지윅, 케빈웨인. _Algorithms Fourth Edition_. 길벗  