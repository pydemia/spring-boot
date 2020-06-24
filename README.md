# spring-boot

https://goddaehee.tistory.com/212
https://goddaehee.tistory.com/211?category=367461

1. 통합 테스트
1) 개요
 - 실제 운영 환경에서 사용될 클래스들을 통합하여 테스트 한다.
 - 단위 테스트와 같이 기능 검증을 위한 것이 아니라 spring framework에서 전체적으로 플로우가 제대로 동작하는지 검증하기 위해 사용 한다.
 
2) 장점
 - 애플리케이션의 설정, 모든 Bean을 모두 로드하기 때문에 운영환경과 가장 유사한 테스트가 가능하다.
 - 전체적인 Flow를 쉽게 테스트 가능하다.

3) 단점
 - 애플리케이션의 설정, 모든 Bean을 모두 로드하기 때문에 시간이 오래걸리고 무겁다.
 - 테스트 단위가 크기 때문에 디버깅이 어려운 편이다.

▶ @SpringBootTest
 - 스프링 부트는 @SpringBootTest 어노테이션을 통해 스프링부트 어플리케이션 테스트에 필요한 거의 모든 의존성을 제공한다.

 - @SpringBootTest는 통합 테스트를 제공하는 기본적인 스프링 부트 테스트 어노테이션 이다.

 - 해당 어노테이션을 사용시 Junit 버전에 따라 유의할 사항이 있다. (공식문서 참고)

 

If you are using JUnit 4, don’t forget to also add @RunWith(SpringRunner.class) to your test, otherwise the annotations will be ignored.
☞ Junit4 사용시 @SpringBootTest 기능은 반드시 JUnit의 SpringJUnit4ClassRunner 클래스를 상속 받는 @RunWith(SpringRynnver.class)와 함께 사용해야 한다.
 
If you are using JUnit 5, there’s no need to add the equivalent @ExtendWith(SpringExtension.class) as @SpringBootTest 
and the other @…Test annotations are already annotated with it.
☞ Junit5 사용시에는 해당 어노테이션은 명시할 필요없다.

 

① properties

1.1) 프로퍼티를 {key=value} 형식으로 직접 추가할 수 있다.

1.2)  기본적으로 클래스 경로상의 application.properties(또는 application.yml)를 통해 애플리케이션 설정을 수행 한다.
 하지만 테스트를 위한 다른 설정이 필요하다면 다른 프로퍼티를 로드할 수 있다.

③ webEnvironment 

 - 웹 테스트 환경 구성이 가능하다.
 - webEnvironment 파라미터를 이용하여 손쉽게 웹 테스트 환경을 선택할 수 있다.


3.1) Mock
 - 실제 객체를 만들기엔 비용과 시간이 많이 들거나 의존성이 길게 걸쳐져 있어 제대로 구현하기 어려울 경우, 가짜 객체를 만들어 사용한다.
 - WebApplicationContext를 로드하며 내장된 서블릿 컨테이너가 아닌 Mock 서블릿을 제공한다.
 - 별도로 지정하지 않으면 기본값은 Mock 서블릿을 로드하여 구동하게 된다.
 - @AutoConfigureMockMvc 어노테이션을 함께 사용하면 별다른 설정 없이 간편하게 MockMvc를 사용한 테스트를 진행할 수 있다.
   MockMvc는 브라우저에서 요청과 응답을 의미하는 객체로서 Controller 테스테 사용을 용이하게 해주는 라이브러리이다.

※ @AutoConfigureMockMvc
 - Mock 테스트시 필요한 의존성을 제공해준다.
 - MockMvc 객체를 통해 실제 컨테이너가 실행되는 것은 아니지만 로직상으로 테스트를 진행할 수 있습니다. (DispatcherServlet을 로딩학여 Mockup으로 처리 가능)
 - print() 함수를 통해 좀 더 디테일한 테스트 결과를 볼 수 있다.

3.5) TestRestTemplate
 - webEnvironment설정시(NONE을 설정시 사용 불가) 그에 맞춰서 자동으로 설정되어 빈이 생성되며, RestTemplate의 테스트를 처리가 가능하다.

- Spring 4.x 이후부터 지원하는 Spring의 HTTP 통신 템플릿
- HTTP 요청 후 Json, xml, String 과 같은 응답을 받을 수 있는 템플릿
- Http request를 지원하는 HttpClient를 사용함
- ResponseEntity와 Server to Server 통신하는데 자주 쓰인다.
  (ResponseEntity는 응답 처리시 값 뿐만 아니라 상태코드, 응답메세지 등을 포함하여 리턴 가능하다. HttpEntity를 상속받기 떄문에 HttpHeader와 body를 가질수 있다.)
- Header, Content-Type등을 설정하여 외부 API 호출


▶ @MockBean
 - Mock 객체를 빈으로써 등록할 수 있다.
 - @MockBean은 Spring의 ApplicationContext는 Mock 객체를 빈으로 등록하며, 혹시 @MockBean으로 선언된 객체와 같은 이름과 타입으로 이미 빈이 등록되어있다면 해당빈은 선언한 @MockBean으로 대체된다.



출처: https://goddaehee.tistory.com/211?category=367461 [갓대희의 작은공간]
