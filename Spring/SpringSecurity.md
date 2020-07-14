## Spring Security 
스프링 프레임워크 기반의 어플리케이션의 보안(인증, 인가)을 담당하는 프레임워크  
Authentication(인증) : 자원에 접근하는 사용자에 대한 신원 확인  
Authorization(인가) : 자원에 대해서 사용자의 접근 권한 여부 관리

## Password Storage
단방향 암호화를 지원하는 다양한 `PasswordEncoder`를 제공한다.  

`DelegatingPasswordEncoder` : 여러개의 암호화 기법을 같이 사용 (레거시의 암호화를 지원하면서 미래에 권장 암호화가 변경될 경우를 대비)
```java
Map encoders = new HashMap<>();
encoders.put("bcrypt, new BCryptPasswordEncoder());
encoders.put("noop", NoOpPasswordEncoder.getInstance());
encoders.put("sha256", new StandardPasswordEncoder());

PasswordEncoder passwordEncoder = new DelegatingPasswordEncoder(idForEncode, encoders);
```
이외
`BCryptPasswordEncoder`, `Argon2PasswordEncoder`, `SCryptPasswordEncoder` 등 제공

## Secure Http Response Header
`Cache-Control`  
사용자가 중요 정보 열람한 뒤 악의 적인 사용자가 뒤로 가기로 다시 볼 수 없도록, 기본적으로 캐싱 설정은 비활성화 한다. 애플리케이션이 자체 캐시 제어 헤더를 제공하는 경우 무시된다.
```text
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
```
`Content Type Options`  
악의적인 XSS 공격을 실행할 수 있게 하는 Content Sniffing은 비활성화 한다.
```text
X-Content-Type-Options: nosniff
```
`HTTP Strict Transport Security (HSTS)`   
서버에서 HTTPS로 강제할때 서버는 클라이언트를 302 Redirect를 통해 이동 시킬수 있다. 하지만 이것은 취약점이 될수 있어 클라이언트에서 HTTPS를 강제 하도록 하는것이 권장되는데
이를 HSTS라 한다.
```text
// 브라우저가 도메인을 31536000초(1년) 동안 HSTS 호스트로 취급하도록 지시
Strict-Transport-Security: max-age=31536000 ; includeSubDomains ; preload
```

### Repository
https://github.com/kangjoshi/spring-security

---
### Reference
_Spring security Reference_ Spring.io(https://docs.spring.io/spring-security/site/docs/5.3.3.BUILD-SNAPSHOT/reference/html5/#preface)