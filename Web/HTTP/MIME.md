## MIME(Multipurpose Internet Mail Extensions)이란?

클라이언트에게 전송된 문서의 타입이 무엇인지, 브라우저가 리소스를 내려 받았을 때 해야할 기본 동작이 무엇인지 결정하기 위해 사용한다.

이전에는 텍스트 파일을 주고 받는데 공통 표준인 ASCII 코드를 따르기만 하면 문제가 되지 않았다. 네트워크를 통해 음원파일, 영상파일 등 바이너리 파일을 기존의 시스템에서 전달하기 위해서는 텍스트 파일로 변환이 필요하게 되었다.



 - Encoding : 바이너리 파일 -> 텍스트파일로 변환
 - Decoding : 텍스트 파일 -> 바이너리 파일로 변환

Encoding <-> Decoding을 통해 여러 타입의 바이너리 파일들을 자유롭게 주고 받을수 있게 되었고 MIME으로 Encoding한 파일은 Content-type 정보를 통해 파일의 타입을 명시한다.

```
MIME Type (Content-type)
- 타입/서브타입의 구조를 가지고 있다. 대소문자를 구분하지 않지만 전통적으로 소문자로 쓰여진다.
1. text
  - 지정된 문자셋의 text 타입
  - text/plain, text/html, text/javascript, text/css 등
2. image
	- 이미지 타입
	- image/gif, image/png, image/jpeg
3. audio
	- 오디오 타입
	- audio/mpeg, audio/webm, audio/wav
4. vidio
	- 비디오 타입
	- vidio/webm, vidio/ogg
5. application
	- 바이너리 데이터 타입
	- application/xml, application/json, application/octet-stream
6. multipart
	- 다른 MIME 타입을 지닌 개별적인 파트들로 합성된 문서를 나타내는 타입
	- multipart/form-data, multipart/mixed
```



### 회고

웹 개발자로서 MIME type이 왜 필요한지 생각 해본적이 없었다. 진행하는 프로젝트에서 각 파일들이 어떤 MIME type을 가지는지 그에 따른 브라우저에서 기본동작이 어떻게 이루어지는지 확인 해봐야겠다.

