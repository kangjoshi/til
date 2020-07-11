## JWT (JSON Web Token)
정보를 안전하게 JSON 객체로 전송하기 위해 사용.  
JWT 토큰은 일반적으로 Request Header(Authorization에 Bearer 스키마)에 포함하여 전송한다.


## 구성
토큰은 .을 기준으로 Header.Payload.Signature 3 부분으로 구성되고 각 부분의 내용은 `Base64Url` 인코딩된다.  

`Header` : 토큰의 타입(JWT), 사용된 서명 알고리즘
```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```
`Payload` : 등록된 클레임(토큰에 대한 정보), 공개 클레임, 개인 클레임(정보)
```JSON
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
`Signature` : 인코딩 된 헤더, 페이로드, Secret, 헤더에 정의된 서명 알고리즘을 사용하여 서명한다. 


생성된 JWT
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```


---
### Reference
_Introduction to JSON Web Tokens_. https://jwt.io/introduction/
