## Spring HATEOAS
HATEOAS 원칙을 따르는 REST API 작성에 도움이되는 API 제공

**H**ypermedia 
**A**s
**T**he
**E**ngine
**O**f
**A**pplication
**S**tate  
REST API 서버에서 클라이언트가 사용 가능한 자원에 대한 정보(Link)를 제공(제어)한다. 
HATEOAS에 의해 부과된 제어은 클라이언트와 서버를 분리(느슨한 결합)시켜 독립적인 개발을 가능하게 한다.

### HAL
REST API에서 리소스를 일관되게 하고 쉽게 Hyper Link 방식을 제공할 수 있도록 하는 형식

### API
`EntityModel` : Entity + Link (= RepresentationModel)를 모델링 하는데 사용되는 컨테이너이다.  
`CollectionModel` : Entity, Collection의 래퍼 클래스로 컬랙션 모델을 만드는데 사용된다.  
`PagedModel` : 페이징 기능이 있는 CollectionModel
`RepresentationModelAssembler` : Entity 객체를 RepresentationModel로 변환하는 메소드를 제공 (Assembler를 구현하지 않고 컨트롤러 메소드 단위에 직접 링크 관련 코드를 추가해도 된다.)  
`WebMvcLinkBuilder` : Link 객체를 만드는데 유용하게 사용된다.

### 작업 순서
1. RepresentationModelAssembler 작성
    ```JAVA
    @Component
    public class PersonResourceAssembler implements RepresentationModelAssembler<Person, EntityModel<Person>> {
    
        @Override
        public EntityModel<Person> toModel(Person personDto) {
            return EntityModel.of(personDto,
                    linkTo(methodOn(PersonController.class).getOnePerson(personDto.getId())).withSelfRel(),
                    linkTo(methodOn(PersonController.class).getAllPersons()).withRel("list"));
        }
    }
    ```
1. 컨트롤러 return type 변경
    ```
    // 리스트형
    @GetMapping
    public ResponseEntity<CollectionModel<EntityModel<Person>>> getAllPersons() {
        return ResponseEntity.ok(personResourceAssembler.toCollectionModel(personList));
    }
    
    // 단일객체형
    @GetMapping(value = "/{personId}")
    public ResponseEntity<EntityModel<Person>> getOnePerson(@PathVariable int personId) {
        return ResponseEntity.ok(personResourceAssembler.toModel(personList.get(personId)));
    }
    ```
1. 출력 확인
    ``` JSON
    MockHttpServletResponse:
        Status = 200
        Error message = null
        Headers = [Content-Type:"application/hal+json"]
        Content type = application/hal+json
        Body = {
                    "id":1,
                    "name":"Jack",
                    "age":37,
                    "_links":{
                        "self":{
                            "href":"http://localhost/persons/1"
                        },
                        "list":{
                            "href":"http://localhost/persons"
                        }
                    }
                }
        Forwarded URL = null
        Redirected URL = null
        Cookies = []
    ```
    
### 느낀점
기존 REST API 작성 시 객체 내 Link에 대한 변수로 정보를 표현했다. 그 결과 URL 변경시 일일이 변경을 해야 하는 불편, 객체 정보와 Link 정보가 혼재 되는 불편이 있었다. 
이 기술을 이용하면 위 두가지 문제가 해결되는 효과를 볼 수 있었다.

### Repository
_https://github.com/kangjoshi/spring-hateoas.git_

---
### Reference
_spring-hateoas-example_. gitHub(https://github.com/spring-projects/spring-hateoas-examples)
_HATEOAS_. Wikipedai(https://en.wikipedia.org/wiki/HATEOAS#:~:text=Hypermedia%20as%20the%20Engine%20of,provide%20information%20dynamically%20through%20hypermedia.)
