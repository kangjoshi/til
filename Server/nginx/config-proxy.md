### Config proxy

클라이언트에서 들어오는 HTTP 요청을 백엔드 서버로 전송한다.



주요 지시어

###### proxy_pass [url];  - location, if

백엔드 서버로 전달될 수 있도록 지정한다.

upstream 블록을 참조할 수도 있다.

```
proxy_pass http://localhost:8080

upstream backend {
	server localhost:8080;
	server localhost:8081;
}
proxy_pass http://backend;
```

###### proxy_method [method];  - http, server, location

전달될 요청의 HTTP 메서드를 재정의 한다. POST로 정의하면 모든 요청은 POST로 전송된다.

###### proxy_hide_header [header_name];  - http, server, location

전달하지 않고 숨길 헤더를 지정한다.

###### proxy_pass_header [header_name];  - http, server, location

무시되는 헤더를 강제로 클라리언트에 전달한다.

###### proxy_pass_request_bodyproxy_pass_request_headers [on|off];  - http, server, location

요청 body와 header를 전달할지 여부 선택한다.

###### proxy_redirect [url|off|default];  - http, server, location

백엔드 서버에서 정의된 Location HTTP 헤더 속 URL을 재작성한다.

off : 경로 재설정 그대로 전달된다.

default : proxy_pass 지시어의 값을 호스트로 사용하고 현재 경로의 문서를 추가한다.

URL : URL의 일부를 다른 값으로 대체한다.

###### proxy_connect_timeout [number];  - http, server, location

백엔드 서버에 연결하는 제한시간을 정한다.

###### proxy_read_timeout [number];  - http, server, location

###### proxy_send_timeout [number];  - http, server, location

뒷단 서버에서 데이터를 읽는, 보내는 제한시간을 정한다.

proxy_ignore_client_abort [on|off];  - http, server, location

on으로 설정되면 클라이언트가 요청을 취소 했더라도 엔진엑스는 프록시 요청을 계속 처리한다.







