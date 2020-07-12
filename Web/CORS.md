## Cross-origin resource sharing (CORS)
교차 출처 리소스 공유  
웹 애플리케이션은 리소스가 자신의 출처(Domain, Port, Protocol)와 다를 때 Cross-origin Http Request 실행한다. 

## 시나리오
1. https://foo.example에서 ajax를 사용하여 https://bar.other/data.json을 요청
1. Request Header에 `Origin: https://foo.example` 포함
1. 서버는 이에대한 응답으로 `Access-Control-Allow-Origin` 헤더의 내용으로 접근 정책을 포함하여 전송

## Preflight
`OPTIONS` 메서드를 통해 다른 도메인의 리소스르 Http Request를 보내기전 실제 요청이 전송하기에 안전한지 확인하는 절차

## Http Request Header
`Origin` : Request가 시작된 서버(출처)를 나타낸다  
`Access-Control-Request-Method` : Preflight에서 실제 요청의 Method를 서버에 알려주기 위해 사용
`Access-Control-Request-Headers` : Preflight에서 실제 요청의 Header를 서버에 알려주기 위해 사용

## Http Response Header
`Access-Control-Allow-Origin` : 리소스 접근에 허용된 출처를 지정
```text
// 모든 출처 허용
Access-Control-Allow-Origin: *
//해당 출처만 허용
Access-Control-Allow-Origin: https://mozilla.org
```
`Access-Control-Expose-Headers` : 접근할 수 있는 Header를 서버의 화이트리스트에 추가  
`Access-Control-Max-Age` : Preflight 결과를 캐시할 수 있는 시간을 나타냄  
`Access-Control-Allow-Methods` : 허용 Method 지정  
`Access-Control-Allow-Headers` : 허용 Header 지정  

---
### Reference
_교차 출처 리소스 공유 (CORS)_.MDN web docs(https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)  
_Cross-origin resource sharing_.Wikipedia(https://en.wikipedia.org/wiki/Cross-origin_resource_sharing#:~:text=Cross%2Dorigin%20resource%20sharing%20(CORS,scripts%2C%20iframes%2C%20and%20videos.)