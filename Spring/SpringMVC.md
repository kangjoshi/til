## SpringMVC
Spring이 제공하는 Servlet 기반의 MVC 아키텍쳐 프레임워크

## MVC 아키텍쳐
아래 세 가지 요소가 협력하여 하나의 웹 요청을 처리하고 응답을 하는 구조  
Model : 어플리케이션 데이터 구조
View : 사용자 인터페이스 요소
Controller : 제어 작업

## 프론트 컨트롤러 패턴
중앙집중형 컨트롤러(Front Controller)가 서버로 들어오는 모든 요청 받아 먼저 처리
Spring MVC에서는 DispatcherServlet가 Front Controller 역할을 한다.

@ 동작순서
1. DispatcherServlet HTTP 요청 접수
1. 모든 요청에 대해 공통적으로 진행해야 하는 작업이 있다면 수행
1. 각 세부 컨트롤러에 작업 위임
    1. 사용자 요청 해석
    1. 서비스 계층에 작업 위임
    1. 결과를 받아 모델 생성
    1. View의 타입 결정
1. 클라이언트에게 결과물 생성
1. HTTP 응답 리턴

## DispatcherServlet의 기본 기능과 기능 확장
1. HandlerMapping
    - Url과 요청 정보 기준으로 알맞은 컨트롤러 결정
    - HandlerInterceptor 적용  
        Servlet Filter VS HandlerInterceptor  
            - Servlet Filter : 모든 요청에 적용, 스프링 빈이 아니다.  
            - HanlderInterceptor : HandlerMapping 단위로 적용, 스프링 빈으로 등록 가능하다.
1. HandlerAdapter
    - HandlerMapping을 통해 선택된 컨트롤러를 DispatcherServlet이 호출할 때 사용하는 Adapter
1. HnadlerExceptionResolver
    - 예외 발생시 DispatcherServlet로 부터 처리를 위임 받는다. 예외 처리는 개별 컨트롤러가 아닌 HandlerExceptionResolver가 해야 한다.
1. ViewResolver
    - 컨트롤러로 부터 결정된 뷰 오브젝트를 찾아주는 로직 을 가진다.
1. LocaleResolver
    - 지역 정보를 결정한다.
1. ThemeResolver
    - 테마(스킨) 정보를 결정한다.
1. RequestToViewNameTranslator
    - 컨트롤러에서 뷰에 대한 정보를 결정하지 않았을 경우 요청 정보를 바탕으로 뷰를 생성해주는 전략
    
## Spring @MVC
애노테이션을 중심으로 한 MVC 확장 기능  

1. @RequestMapping
    - 메소드 레벨로 세분화 된 HandlerMapping
    - consumes : Content-Type 지정
    - Accept : Media-Type 지정
1. @Controller
    - 컨트롤러 역할을 담당하는 메소드의 파라미터와 리턴등을 자유롭게 결정할 수 있게 한다.
    - 메소드 파라미터
        - HttpServletRequest, HttpServletResponse
        - HttpSession
        - Locale : Locale Resolver가 결정한 Locale 오브젝트
        - @PathVariable
        - @RequestParam
        - @CookieValue
        - @RequestHeader
        - Map, Model, ModelMap
        - @ModelAttribute
        - Errors, BindingResult
        - SessionStatus
        - @RequestBody
        - @Value
        - @Valid
    - 메소드 리턴
        - 자동 추가 모델 오브젝트와 자동생성 뷰 이름 : 메소드 리턴 타입 상관없이 조건만 맞으면 모델이 추가된다.
            - @ModelAttribute 모델 오브젝트
            - Map, Model, ModelMap 파라미터
            - BindingResult
        - ModelAndView
        - String : 뷰 이름으로 사용
        - void : RequestToViewNameResolver에 의해 URL에 따라 뷰 이름을 매칭
        - 모델 오브젝트 : RequestToViewNameResolver을 사용하고 뷰가 참조해야할 오브젝트가 하나라면 Model 파라미터를 쓰지 않고 모델 오브젝트를 바로 리턴 가능
        - @ResponseBody : 메시지 컨버터를 통해 Http Response의 본문으로 전환
1. Converter
    - 소스 타입 -> 타깃 타입의 단방향 타입 변환 지원
    - Enum와 같은 사용자 정의 타입의 일괄 바인딩을 원할때 사용 가능
1. Fomatter
    - 애노테이션 정보를 활용한 모델 필드 바인딩을 원할때 사용 가능
    - @NumberFormat
    - @DateTimeFormat
1. Message Converter
    - Http Request Body와 Http Response Body를 메세지로 다루는 방식
    - ByteArrayHttpMessageConverter
        - 오브젝트 : byte[]
        - 미디어 타입 : 전부 허용
        - 콘텐츠 타입 : application/octet-stream
    - StringHttpMessageConverter
        - 본문을 String으로 받아 직접 구현한 MessageConverter를 사용 하고 싶을때 사용
        - 오브젝트 : String
        - 미디어 타입 : 전부 허용
        - 콘텐츠 타입 : text/plain
    - FormHttpMessageConverter
        - 오브젝트 : MultiValueMap <-- 하나의 이름을 가진 여러 개의 파라미터가 사용될 수 있는 경우
        - 미디어 타입 : application/x-www-form-urlencoded
        - 콘텐츠 타입 : application/x-www-form-urlencoded
    - SourceHttpMessageConverter
        - XML 문서를 Source 타입의 오브젝트로 전환시 사용
        - 오브젝트 : DOMSource, SAXSource, StreamSource
        - 미디어 타입 : application/*+xml, text/xml
        - 콘텐츠 타입 : application/*+xml, text/xml
    - Jaxb2RootElementHttpMessageConverter
    - MarshallingHttpMessageConverter
        - XML와 오브젝트 사이의 메시지 변환
    - MappingJacksonHttpMessageConverter
        - Json와 오브젝트 사이의 메시지 변환
        - 미디어 타입 : application/*+json
1. SessionAttributeStore
    - SessionAttributeStore 인터페이스를 구현 함으로 세션 정보 저장에 대한 정책 변경
1. WebArgumentResolver
    - WebArgumentResolver 인터페이스를 구현 함으로 애플리케이션에 특화된 컨트롤러 메소드 파라미터 타입 정의 

---
### Reference
이일민. _토비의 스프링 3.1 vol.2_. 에이콘  