### Config http

Http 기능의 구성, http, server, location 3개의 논리 블록을 제공한다.

###### http

 구성 파일의 최상위에 삽입된다. 여러번 추가될 수 있다.

###### server

 호스팅하는 도메인이나 하위 도메인을 정의한다.

 http 블록 안에서만 사용할 수 있다.

###### location

 웹 사이트의 특정 위치에만 적용되는 설정을 정의하는 데 쓰는 블록

 server 블록 안이나 다른 location 블록 안에 중첩해서 사용할 수 있다.

```
http {
	gzip on;									<- gzip 압축 활성화
	
	server {
		server_name localhost;	
		listen 80;
		
		# 하위 블록에서도 활성화된 gzip은 유효하다.
		
		location /downloads/ {	
			gzip off;							<- gzip 압출 비활성화, /downloads/ 안의 문서에서만 적용된다.
		}
	}
}
```

```
= 조정 부호 : 요청된 문서 URI는 반드시 지정된 패턴과 정확하게 일치해야 한다. 정규식은 사용할 수 없다.
적용 대상
 /admin 적용
 /ADMIN (운영체제가 대소문자를 구분하는 파일 시스템을 사용할 경우)
 /admin/ 적용되지 않음.
location = /admin {
	...
}

조정 부호 생략 : 요청된 문서 URI가 지정된 패턴으로 시작해야 한다. 정규식은 사용할 수 없다.
적용 대상
 /admin 적용
 /ADMIN (운영체제가 대소문자를 구분하는 파일 시스템을 사용할 경우)
 /admin/ 적용
 /administrator 적용
location /admin {
	...
}

~ 조정 부호 : 요청된 URI가 지정된 정규식에 일치하는지 비교하면서 대소문자를 구분
location ~ ^/admin$ {
	...
}
적용 대상
 정규식에 따른다.

~* 조정 부호 : 요청된 URI가 지정된 정규식에 일치하는지 비교하면서 대소문자를 구분하지 않는다. 
location ~ ^/admin$ {
	...
}
적용 대상
 정규식에 따른다.
 
 
조정 부호 우선 순위
서로 다른 패턴으로 다수의 location 블록이 정의 되어 있을 경우 엔진엑스는 아래의 우선 순위로 location 블록을 결정한다.
 1. = 조정 부호가 사용된 블록
 2. 조정 부호가 생략되었지만 요청과 정확하게 일치하는 블록
 3. ^~ 조정 부호가 사용된 블록
 4. ~또는 ~* 조정 부호가 사용된 블록
 5. 조정 부호가 생략된 블록
```



#### 모듈 지시어

웹 어플리케이션 구성을 위해 지시어를 http, server, location 수준에 각각 삽입할 수 있다.

###### listen [주소]\[포트] [추가옵션];  - server

 웹 사이트를 제공하는 소켓을 여는 데 사용되는 IP 주소나 포트를 지정한다.

 추가 옵션 : ssl(웹 사이트가 SSL을 통해 제공되도록 지정), http2(http_v2 모듈이 있을경우 HTTP/2 프로토콜을 지원하도록 활성화)

###### server_name [호스트이름] [호스트이름..];  - server

 블록에 하나 이상의 호스트 이름을 할당, 호스트 이름은 정규식, 와일드 카드도 허용된다.
 엔진엑스는 HTTP 요청을 받을 때 요청의 Host 헤더를 server 블록 모두와 비교하여 이름과 맞는 첫 번째 server 블록이 선택된다. (일치하는 블록이 없다면 포트 기반으로 선택된다.) 

```
server_name www.website.com;
server_name *.website.com;
server_name ~^(www)\.website\.com$;
server_name _ ""; 			<- 빈 문자열을 사용해 Host 헤더 없이 들어오는 모든 요청을 받게 할 수도 있다.
```

######  server_name_in_redirect [on|off];  - http, server, location

 defalut : on

 내부적인 경로 재설정 여부를 설정한다. 활성화시 엔진엑스는 server_name 지시어에 지정된 첫 호스트 이름을 사용한다. 비활성화시 HTTP 요청의 Host 헤더 값을 사용

###### server_names_hash_max_size [number];  - http

 default : 512

 엔진엑스는 요청 처리 속도를 향상을 위해 해시 테으블을 여러 데이터 컬렉션에 사용한다. 해당 지시어는 서버 이름 해시 테이블의 최대 크기를 정의한다.

###### port_in_redirect [on|off];  - http, server, location

 경로 재설정 상황에 엔진엑스가 포트 번호를 재설정되는 URL에 추가할지 여부를 지정한다.

###### tcp_nodelay [on|off];  - http, server, location

 default : on

 Keep-alive 상황에서 TCP_NODELAY 소켓 옵션을 활성화하거나 비활성화 한다.

```
% TCP_NODELAY
Nagle 알고리즘 (조금씩 자주 보내지 말고 한번에 많이 보내라.)
즉 비활성화시 즉시 전송, 활성화시 지연 전송을 한다.

```

###### sendfile [on|off];  - http, server, location

 default : off

 활성화시 엔진엑스는 sendfile 커널 호출을 사용하여 파일을 전송한다. 비활성화시 엔진엑스 스스로 파일 전송

###### sendfile_max_chunk [number];  - http, server

 default : 0

 sendfile 호출마다 사용될 데이터 최대 크기를 정의한다.

###### root [directory path];  - http, server, location, if

 방문자에게 제공하고자 하는 파일을 담고 있는 최상위 문서 위치를 정의

###### alias [directory path|file path];  - location

 엔진엑스가 특정 요청에서 별도 경로의 문서를 읽도록 할당

 location 블록 안에서만 쓸 수 있다.

```
http {
	server {
		server_name localhost;
		root /home/user/docs;
		location /admin/ {
			alias /home/admin/docs;
		}
	}
}
```

###### error_page [code] [code...];  - http, server, location, if

 HTTP 응답 코드에 맞춰 URL을 조작하거나 다른 코드로 대체한다.

```
error_page 404 /not_found.html;
error_page 500 501 502 503 /server_error.html
error_page 403 =200 /index.html 			<- 응답 코드를 200 ok로 바꾸고 index.html으로 경로를 변경
```

###### index [file] [file...];  - http, server, location

 요청에 파일명이 지정되지 않았을 경우 엔진엑스가 기본으로 제공할 페이지를 정의한다. 

###### recursive_error_pages [on|off];  - http, server, location

 default : off

 재귀적인 오류 페이지 진입을 허용하거나 허용하지 않는다. 

###### try_files [file path] [file path..] [url];  - server, location

 첫번째 부터 마지막에서 두번째 까지 인자로 지정된 파일을 제공하려고 시도한다. 이 파일들이 모두 존재하지 않는다면 마지막 인자에 지정된 이름의 location 블록이나 URI를 제공한다. 

```
location / {
	try_files $uri $uri.html $uri.xml @proxy;	<- 	$uri -> $uri.html -> $uri.xml을 제공하려고 시도하고 없다면 @proxy를 제공한다. 
}

location @proxy {
	proxy_pass 127.0.0.1:8080
}
```

######  keepalive_requests [number];  - http, server, location

 연결을 닫지 않고 유지하면서 제공할 최대 요청 횟수를 지정

###### keepalive_timeout [time] [time2];  - http, server, location

 서버가 유지되는 연결을 끊기 전 몇 초를 기다릴지 정의하는 지시어, time2는 Keep-Alive: timeout=$time2 Http 응답 헤더의 값으로 전달된다. 클라이언트는 이 시간이 지난 후 스스로 연결을 끊는다. (특정 브라우저는 이 정보를 무시하고 고유의 시간을 유지하다 끊긴다.)

###### keepalive_disable [brower name];  -  http, server, location

 연결 유지 기능을 비활성화할 브라우저는 선택할 수 있다.

###### send_timeout [second];  - http, server, location

 default : 60

 지정된 시간 후 엔진엑스가 비활성 상태의 연결을 닫는다. 클라이언트가 데이터 전송을 중단하는 순간부터 연결은 비활성 상태가 된다.

###### client_body_in_file_only [off|clean|on];  - http, server, location

 default : off 

 활성화가 되면 http 요청의 body가 디스크에 파일로 저장된다. clean 설정은 요청이 들어올때 저장되고 요청이 처리되면 지운다.

###### client_max_body_size [size];  - http, server, location

 default : 1m

 request body의 최대 크기 값, 초과가 되면 413 요청 내용 용량 초과 오류를 반환한다.

###### ignore_invalid_headers [on|off];  - http, server

 default : on

 비활성화시 잘못된 요청이 들어오면 400 잘못된 요청 오류를 반환한다.

###### max_ranges [size]

 클라이언트가 파일의 일부 내용을 요청할 때 허용할 바이트 수의 범위를 지정한다. 

###### types;  - http, server, location

 MIME 타입과 파일 확장자의 상관관계를 맺는데 쓰인다. 엔진엑스는 파일 확장자를 확인하고 매칭된 MIME 타입을 결정하여 Content_Type 헤더 값으로 보낸다. 기본 세트를 mime.types라는 파일로 제공하므로 include 하여 사용할 수 있다.

```
types {
	MIME type1 확장자1;
	MIME type2 확장자2;
	MIME type3 확장자3;
}

include mime.types;
```

######  default_type [MIME type];  - http, server, location

 default : text/plain

 기본 MIME 타입을 정의한다. 정의된 types중 어느 것도 일치하지 않는다면 default_type의 값이 사용된다.

###### limit_except [method] [method...];  - location

 허용하는 Http Method를 제외하고 모든 Http Method를 막게 해준다.

```
location /admin/ {
	limit_except GET {				<- 모든 방문자는 GET 메소드만 사용할 수 있다.
		allow 192.168.1.0/24;
		deny all;
	}
}
```

###### limit_rate [rate];  - http, server, location, if

 default : 무제한

 개별 클라이언트 연결의 전송률을 제한한다. 

###### satisfy [any|all];  - location

 default : all

 클라이언트가 명시된 모든 접근 조건(allow, deny)을 만족해야 하는지, 아니면 하나만 만족하면 되는지 정의

###### internal;  - location

 location 블록을 내부용으로 지정한다. 지정된 자원에 접근하는 방법은 내부 경로 재설정으로 가능

```
server {
	location /admin/ {
		internal;			<- /admin/에 외부 요청이 접근하지 못하도록 한다.
	}
}
```

###### log_not_found [on|off];  - http, server, location

 default : on

 404 오류를 로그에 남길지 말지 결정

###### log_subrequest [on|off];  - http, server, location

 내부 경로 재설정이나 SSL 요청에 의해 발생된 2차 요청을 요구에 남길지 결정

###### resolver [IP];  - http, server, location

 엔진엑스가 사용할 DNS 서버를 지정한다. 

###### server_tokens [on|off|build];  - http, server, location

 엔진엑스가 실행되는 버전 정보를 클라이언트에게 알릴지 여부를 정의.

###### underscores_in_headers [on|off];  - http, server

 default : off

 사용자 정의 Http Header에 \_ 부호를 허용할지 여부를 지정

###### variables_hash_max_size [number];  - http

 변수 해시테이블의 최대 크기를 설정, 서버 구성에서 사용되는 변수의 총합이 1024개 이상이라면 값을 올려야한다.

###### variables_hash_bucket_size [number];  - http

 변수 해시테이블의 버킷 크기를 조정

###### post_action [url];  - http, server, location, if

 요청 처리가 완료된 후 엔진엑스가 호출하는 URI를 정의

```
location /payment/ {
	post_action /script/done.php;
}
```



#### 모듈 지시어

##### request header

###### $http_host

 클라이언트의 호스트 이름을 나타낸다.

###### $http_user_agent

 클라이언트의 웹 브라우저를 나타낸다.

###### $http_referer

 클라이언트가 마지막으로 방문했던 이전 페이지의 URL

###### $http_via

 클라이언트가 사용했을 프록시에 대한 정보

###### $http_x_forwarded_for

 클라이언트가 프록시를 거쳐 접근할 경우 실제 클라이언트의 IP 주소

###### $http_cookie

 클라이언트가 전송한 쿠키 데이터

###### $http_...

 사용자 정의 헤더의 값을 얻을수 있다. (\_ 는 \- 로 치환된다.)



##### response header

###### $sent_http_content_type

 Content_type의  값

###### $sent_http_content_length

 Content_Length의 값

###### $sent_http_location

 Location의 값, 요구된 자원의 위치가 지정된 위치와 다를 때 그 위치를 나타낸다.

###### $sent_http_last_modified

 Last_Modified의 값, 요청된 자워느이 수정된 날짜를 나타낸다.

###### $sent_http_connection

 Connection의 값, 연결이 유지될 것 인지 닫힐 것인지 정의한다.

###### $sent_http_keep_alive

 Keep_Alive의 값, 연결이 유지되는 시간을 정의한다.

###### $sent_http_transfer_encoding

 Transfer-Encoding의 값, compress, gzip과 같은 응답 본문 인코딩 방법에 대한 정보를 제공

###### $sent_http_cache_controle

 Cache-Control의 값, 클라이언트 브라우저가 자원을 캐시해야 할지 여부를 알려준다.

###### $sent_http_...

 사용자 정의 헤더의 값을 얻을수 있다. (\_ 는 \- 로 치환된다.) 



##### 엔진엑스 생성 변수

###### $arg_xxx

 GET 메서드의 매개변수에 접근

###### $args

 모든 인자 값이 하나로 합쳐진 형태의 문자열

###### $binary_remote_addr

 이진 데이터 형태의 클라이언트 IP 주소

###### $connection

 연결을 식별하기 위한 일련번호

###### $connection_requests

 현재 사용되는 연결로 지금까지 처리된 요청 수

###### $cookie_xxx

 쿠키 값에 접근

###### $document_root

 현 요청의 root나 alias 지시어의 값

###### $document_uri, $uri

 현 요청의 URI

###### $host

 $http_host와 동일한 값 요청 헤더에 host 정보가 포함되지 않으면 이 변수로 값 제공

###### $https

 SSL 요청이라면 ON, 아니면 빈 값

###### $pid

 엔진엑스 프로세스의 ID

###### $remote_addr, $remote_port, $remote_user

 클라이언트 정보를 나타낸다.

###### $request_id, $request_method, $request_uri

 클라이언트 요청에 대한 정보를 나타낸다.



 

 