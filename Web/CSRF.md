## Cross-site request forgery (CSRF)
사이트간 요청 위조

공격자는 사용자가 의도하지 않은 웹 요청을 수행하도록 유도하여 서버 데이터 유출, 세션 상태 변경, 사용자 계정 조작 등 공격

## 공격 방법
1. 해커는 많은 사용자에게 이메일등으로 악의적인 요청을 수행하는 프로그램 전송(하이퍼 링크, 임베디드 form 등)
1. 사이트에 로그인 되어 있는 사용자 공격 프로그램이 담긴 이메일 열람
1. 공격 프로그램 실행

## 대응 방법
1. 사용자 관점의 예방
    - 사용하지 않을때 로그아웃 하기
    - 브라우저에 비밀번호 저장하지 않기
    - 로그인한 상태에서 동시 탐색하지 않기
    
1. 애플리케이션 관점의 예방
    - 모든 요청에 대해 임의 토큰 생성
    - Http Header Referer 체크
     
---
### Reference
_Cross site request forgery (CSRF) attack_. https://www.imperva.com/learn/application-security/csrf-cross-site-request-forgery/