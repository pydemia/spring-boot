# spring-boot

https://goddaehee.tistory.com/212
https://goddaehee.tistory.com/211?category=367461

---
단위 테스트

▶ @WebMvcTest
 - MVC를 위한 테스트, 컨트롤러가 예상대로 동작하는지 테스트하는데 사용된다.
(To test whether Spring MVC controllers are working as expected, use the @WebMvcTest annotation.)

 - @WebMvcTest 어노테이션을 사용시 다음 내용만 스캔 하도록 제한한다. (보다 가벼운 테스팅이 가능하다.)
@Controller, @ControllerAdvice, @JsonComponent, Converter, GenericConverter, Filter, HandlerInterceptor, WebMvcConfigurer, HandlerMethodArgumentResolver

※ 자동 구성 설정정보 리스트는 다음 공식 문서에서 확인하며 더 최신화되고, 정확할 것이다.

https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-test-auto-configuration.html#test-auto-configuration

 - MockBean, MockMVC를 자동 구성하여 테스트 가능하도록 한다.
 - Spring Security의 테스트도 지원 한다.

https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-use-test-with-spring-security

 

 - @WebMvcTest를 사용하기 위해 테스트할 특정 컨트롤러 클래스를 명시 하도록 한다.

 

● 장점

 - WebApplication 관련된 Bean들만 등록하기 때문에 통합 테스트보다 빠르다.

 - 통합 테스트를 진행하기 어려운 테스트를 진행 가능하다.

ex) 결제 모듈 API를 콜하며 안되는 상황에서 Mock을 통해 가짜 객체를 만들어 테스트 가능.

 

● 단점

 - 요청부터 응답까지 모든 테스트를 Mock 기반으로 테스트하기 때문에 실제 환경에서는 제대로 동작하지 않을 수 있다.

 ▶ @WebFluxTest
 - 스프링 웹플럭스에 대한 이해도가 너무 낮아서 이부분은 그냥 공식 문서에 있는 내용 그대로만 발췌해서 남겨놓고 추후 자세히 포스팅 해보려 한다.
 - 비동기-논블록킹 리액티브 개발에 사용되며 서비스간 호출이 많은 마이크로 아키텍처에 적합 하다.

ex) @WebFluxTest예제 (공식 문서의 테스트 예제)

▶ @DataJpaTest
 - JPA 관련된 설정만 로드합니다.
 - 설정이 정상적인지, JPA를 사용하서 데이터를 올바르게 조회, 생성, 수정, 삭제 하는지 등의 테스트가 가능하다.
 - @Entity 클래스를 스캔하여 스프링 데이터 JPA 저장소를 구성한다. ( 다른 컴포넌트를 스캔하지 않음 )
 - @Transactional 어노테이션을 포함하고 있기 때문에 따로 선언하지 않아도 됨
   @Transactional 기능이 필요하지 않으면 @Transactional(propagation = Propagation.NOT_SUPPORTED) 설정
 - 기본적으로 in-memory embedded database에 대한 테스트를 진행한다.
 - real database를 사용하고자 하는 경우@AutoConfigureTestDatabase 사용하면 된다.
   @AutoConfigureTestDatabase Default 설정 값은 Any이다. (기본적으로 내장된 데이터소스를 사용한다).
   @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)을 지정하면 실제 디비도 사용 가능하며,
   @ActiveProfiles("test") 등의 프로파일이 설정도 가능하다.

@JdbcTest는 JPA를 사용하지 않고 기본 데이터베이스의 테스트 할 때 사용한다.

▶ @RestClientTest
참고) 공식 doc 
You can use the @RestClientTest annotation to test REST clients.
By default, it auto-configures Jackson, GSON, and Jsonb support, configures a RestTemplateBuilder, and adds support for MockRestServiceServer.
Regular @Component beans are not loaded into the ApplicationContext.

@RestClientTest 을 사용하여 REST 클라이언트 테스트가 가능하다.
REST 통신의 JSON 형식이 예상대로 응답을 반환하는지 등을 테스트 합니다.
예를 들면, Apache HttpClient나 Spring의 RestTemplate을 사용하여 외부 서버에 웹 요청을 보내는 경우에 이에 응답할 Mock서버를 만드는 것이라고 생각하면 된다.

▶ @JsonTest
 - @JsonTest는 JSON serialization과 deserialization 테스트를 편하게 할 수 있다.
 - JSON의 직렬화, 역직렬화를 수행하는 라이브러인 Gson과 Jackson의 테스트를 제공한다.
(JacksonTester, GsonTest, BasicJsonTester)



---
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
