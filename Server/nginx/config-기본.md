### Config 파일 구조

기본적으로 엔진엑스는 하나의 기본 구성 파일을 사용하며 설정 값을 지정하여 프로그램의 동작을 정의한다.

구성 파일의 구조는 아래와 같다.

```text
/usr/local/nginx/conf/nginx.conf			<- 기본적인 파일 경로

#user		nobody;					<- #은 주석, 설정 값은 항상 ;(세미콜론)으로 끝나야한다.
work_processes	1; 					<- 엔진엑스가 단일 작업자 프로세스로 동작하도록 설정

includ mime.types;					<- 지정된 파일을 인클루드시킨다. mime.types : 파일 확장자와 연관된 MIME 타입의 목록
							   *로 패턴에 맞는 파일명을 지정하는 방식도 지정한다. include sites/*.conf;

event {						        <- event 모듈의 블록
	worker_connections	1024;			<- work_connections는 events 모듈에서만 영향을 미친다. 서버 전반에 영향을 미치는 
	                                		   지시어는 반드시 최상위(Main 블록)에 위치해야 한다.
}

http {							<- http 모듈의 블록, 여러개의 server 블록과 다양한 지시어를 선언할 수 있다.
	server {					<- server 모듈의 블록, 가장 호스트를 구성한다.
		listen	80;				<- server의 port 지정
		server_name	example.com;		<- server의 name 지정, 즉 해당 server 블록은 호스트명이 정확히 example.com과 일치 하는 Http 요청에 적용된다.
		access_log	/var/log/nginx/example.com.log;	<- 로그 설정
		location ^~ /admin/ {			<- location 블록, 지정된 경로와 일치할 때만 설정이 적용된다.
			index index.html;
			access_log	off;		<- location 블록에 포함된 요청에 대해서는 access_log 지시어를 비활성화
		}
	}
}

```



### 기반 모듈 설정

기반 모듈은 엔진 엑스의 기본적인 기능을 가진 모듈로 아래 3가지로 구분할 수 있다.

*와일드카드(\*)가 붙은 지시자는 중요 설정*

#### Core 모듈

프로세스 관리, 보안 같은 필수 기능을 구성한다.

###### daemon [on|off]

 default : on

 데몬 모드를 켜거나 끈다.

###### debug_points [stop|abort]

 default : none

 엔진엑스 내부의 디버그 지점을 활성화, stop은 디버그 지점에 다다랐을 때 프로그램을 중지하고 abort는 디버그 지점에서 프로그램을 중단시키고 코어 덤프 파일을 생성

###### env

 env VARIABLE=hello;

 환경 변수 정의

###### error_log [file path] [level]

 오류 로그 설정

 level은 info, notice, warn, error, critics, alet, emerg에서 선택 가능

###### load_module [module path]

 동적 모듈을 읽어 활성화

###### lock_file [file path]

 상호 배제를 위한 락 파일 지정

###### master_process [on|off]

 default : on

 다중 프로세스(주 프로세스와 작업자 프로세스) 여부 선택, 비활성화시 엔진엔스는 단 하나의 프로세스로 동작(테스트 용도로만 사용)

###### pcre_jit [on|off]

 펄 호환 정규식을 실행 시점에 컴파일 여부 선택, 기능이 동작하려면 부가적인 조건(pcre 라이브러리, 엔진엑스 빌드 옵션)이 있다.

###### pid [file path]

 엔진엑스 데몬의 pid 파일 경로, 기본값은 컴파일할 때 결정

###### ssl_engine [engine name]

 시스템에서 사용 가능한 하드웨어 SSL 가속 명령어를 선택

```
% SSL 가속기
서버에서 수행했던 SSL 처리를 부하 분산 장치가 대신하는 기능, 부하분산 장치가 클라이언트의 요청을 받아서 비밀키를 사용해 복원한 후 평문 상태인 HTTP로 서버에 전달, 클라이언트에게 응답할 때는 반대로 부하분산 장치가 암호화 처리를 한다.
- 서버 부하 경감
- SSL 인증서 일괄 관리
```

###### thread_pool [name] thread=[number] [max_queue=number]

 대용량 파일을 비동기로 처리하기 위한 aio 지시어와 함께 사용될 수 있는 스레드 풀 참조를 정의

###### timer_resolution [time]

 내부 시계를 동기화하는 gettimeofday() 시스템 호출 간격을 조절, 값이 지정되지 않으면 커널 이벤트 알림마다 시계가 고쳐진다.

###### * user [username] [groupname]

 엔진엑스 작업자 프로세스를 시작시키는 사용자 계정과 그룹을 지정, 값이 지정되지 않으면 엔진엑스의 주 프로세스 사용자와 그룹이 사용된다.
 보안성을 높이려면 엔진엑스 전용 사용자와 그룹을 만들고 서비스되는 파일에 적절한 권한을 적용하는것이 좋다.

```
user root root;	   <- 작업자 프로세스가 root 사용자로 시작하도록 설정한다. root 사용자는 파일 시스템 전체 권한을 엔진엑스에 부여하므로 보안상 위험하다.
```

###### worker_cpu_affinity [auto] [CPU_마스크]

 worker_processes와 함께 사용되며 작업자 프로세스와 CPU 코어간의 선호도를 부여한다.

###### * worker_priority [number]

 default : 0

 일반적으로 작업자 프로세스는 일반적인 프로세스 우선 순위로 시작된다. 시스템이 다른 작업을 동시에 수행하는 경우 엔진엑스 작업자 프로세스에 높은 우선 순위를 부여할 수 있다. 작업자 프로세스의 우선 순위를 최고 -20 ~ 19 범위에서 지정한다. 하지만 커널 프로세스는 우선순위 -5에서 실행되기 때문에 -5 이하의 우선 순위는 설정하지 않는 것 이 좋다.

###### * worker_processes [number|auto]

 default : 1

 작업자 프로세스의 수를 지정, 듀얼 코어 이상이라면 기본값인 1보다 더 큰 값으로 지정하는 것이 좋다. 권장 값으로는 auto를 사용할 수 있는데 auto는 시스템에서 감지된 CPU 코어의 수가 된다.

###### worker_shutdown_timeout [time]

 graceful shutdown시 진행 중인 작업자 프로세스가 끝내도록 대기할 시한을 정한다. 시한이 지나면 모든 연결을 강제로 끊고 프로세스를 종료한다.

###### worker_rlimit_core [time]

 프로세스당 코어 파일의 크기를 지정

###### worker_rlimit_nofile [number]

 프로세스가 동시에 사용할 수 있는 파일의 개수를 지정

###### working_directory [file path]

 작업자 프로세스가 작성할 코어 파일의 위치를 지정, user 지시어로 지정되는 작업자 프로세스의 사용자 계정은 해당 디렉토리에 쓰기 권한을 가지고 있어야 한다.

###### worker_aio_requests [number]

 aio에 epoll 연결 처리 방식을 사용 중이라면 작업자 프로세스 하나의 최대 비동기 I/O 작업 수를 설정



#### Event 모듈

네트워크 매커니즘을 구성할 수 있는 지시어를 제공한다.

###### accept_mutex [on|off]

 default : off

 소켓 연결을 열고자 상호 배제를 사용할 지 여부를 결정.

###### accept_mutex_delay [number]

 default : 500ms

 자원을 다시 얻으려고 시도하기 전에 작업자 프로세스가 기다리는 시간을 지정

###### debug_connection [ip]

 지정된 ip와 일치하느 요청에 대해 상세 로그를 남긴다. 기능을 사용하려면 반드시 엔진엑스를 --debug 스위치와 함께 컴파일 해야한다.

###### multi_accept [on|off]

 엔진엑스가 수신 큐에서 들어와 있는 연결을 한 번에 다 받게 할지 여부를 선택

###### use [/dev/poll|epoll|kqueue|rtsig|select]

 사용할 이번트 모델을 선택, 엔진엑스가 자동으로 가장 적합한 모델을 선택하므로 사용자가 값을 바꿀 필요는 없다.

###### * worker_connections [number]

 작업자 프로세스가 동시에 처리할 수 있는 연결의 수를 지정. 예를 들어 1024개의 연결을 수용하는 작업자 프로세스가 4개라면 서버는 동시 연결을 최대 4096개까지 처리하게 된다. 고성능 서버일수록 더 많은 동시 연결을 수용하기 위해 이 설정 값을 올린다.



#### Configuration 모듈

외부 파일의 구성 정보를 가져와 포함시킨다.

###### include [file path]

 다른 파일을 포함 한다.

