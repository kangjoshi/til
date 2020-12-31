### Config rewrite

URL을 재작성한다.

클라이언트 요청이 수신된 후 파일을 제공하기 전 요청 URL을 재작성한다.

###### 정규식

rewrite 모듈은 정규식 패턴으로 작성되므로 정규식을 이해하는것이 중요

| 문자  | 설명                                                         |
| ----- | ------------------------------------------------------------ |
| ^     | ^ 문자뒤에 따르는 패턴이 시작 부분에 있어야한다.<br />^hel <br />O : hel, hello, hello world<br />X : he, hi |
| $     | $ 이전의 패턴이 끝 부분에 있어야한다.<br />e$<br />O : sample, e, file<br />X : extra, shell |
| .     | 모든 문자열과 일치한다.<br />hell.<br />O : hello, hellx, hell!<br />X : hell, helo |
| []    | 집합 내 문자 중 하나와 일치한다. <br />hell[a-z]<br />O : hello, hella, hellb<br />X : hell1, hell2, hell! |
| [^]   | 지정한 집합에 포함되지 않은 문자에 일치한다.<br />hell\[^a-z]<br />O : hell1, hell2, hell!<br />X : hello, hella, hellb |
| \|    | \| 문자 앞이나 뒤 요소 중 하나에 일치한다.<br />hello \| welcome<br />O : hello, welcome, hello world, welcom world<br />X : hi, hi world |
| ()    | 여러 패턴 요소를 한 단위로 묶는다.<br />^(hello\|hi) there$<br />O : hello there, hi there<br />X : hey there, why there |
| \     | 메타문자를 일반 문자로 인식하도록 한다.<br />hello\.<br />O : hello., hello....<br />X : hi.. |
| *     | * 앞에 오는 패턴은 요소가 없거나 1회 이상이다.<br />he*llo<br />O : hllo, heeeello |
| +     | + 앞에 오는 패턴은 요소가 1회 이상 있어야한다.<br />he+llo<br />O : hello, hello<br />X : hllo |
| ?     | ? 앞에 오는 패턴은 없거나 한 번 있어야한다.<br />he?llo<br />O : hello, hllo<br />X : heello |
| {x}   | {x} 앞에 오는 패턴은 요소가 x회 있어야 한다.<br />he{3}llo<br />O : heeello<br />X : hello |
| {x,}  | {x,} 앞에 오는 패턴은 요소가 x회 이상 있어야 한다.<br />he{3,}llo<br />O : heeello, heeeeello<br />X : hello |
| {x,y} | {x,} 앞에 오는 패턴은 요소가 x회 이상 y회 이하 있어야 한다.  |



###### 캡처

하위 정규식에 해당하는 문자를 캡처한다. 캡처된 문자는 $N 같은 변수 형태로 사용할 수 있다.

```
server {
	server_name website.com;
	location ~* ^/(download|files)/(.*)$ {
		add_header Capture1 $1;  // $1 = download 또는 files
		add_header Capture2 $2;  // $2 = /download/ 또는 /file/ 뒤에 따를 문자열
	}
}
```



###### rewrite 처리 순서

```text
server {
	server_name website.com;
	
	location /documents/ {
		rewrite ^/document/(.*)$ /storage/$1;
	}
}
```

1. /document/a.txt로 요청이 들어온다.
2. location /documents/ 블록에 일치하여 rewrite ^/document/(.*)$ /storage/$1가 수행된다.
3. /storage/a.txt로 변경된 요청은 처음부터 다시 시작하여, 처리할 location 블록을 새로 찾는다.