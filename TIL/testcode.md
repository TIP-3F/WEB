# Overview
테스트 코드는 개인적으로 생각하기에 크게 세 가지로 구성되어 있는 듯 하다. Actual Data, Expected Data와 Logic이 그것이다. 그리고 Actual Data와 Expected Data를 이용하여 Logic이 잘 돌아가는지 테스트 해볼 것이다.

우리는 Logic(Function or Method)를 이용하여 Actual Data를 만든 뒤에, 이를 Expected Data와 비교할 것이다. 즉, 해당 Logic이 예상대로 잘 작동해서 정답에 해당하는 Expected Data가 나왔는지 확인해보는 것이다. 이때 사실 Given Data를 활용한다. Given Data를 Logic에 대입하여 Actual Data를 만들어내는 것이다.

# Mock Object
> In object-oriented programming, mock objects are simulated objects that mimic the behavior of real objects in controlled ways, most often as part of a software testing initiative. 
(wikipedia - Mock object)

공학은 문제를 마주할 때 발전해 나간다. Mock Object도 어떤 문제를 해결하기 위한 해결책으로 제시된 것이다. 

테스트 코드를 작성할 때, 실제 서버와 같은 환경에서 테스트를 진행할 수 없다는 **문제점**이 있다. 실제 서버를 대상으로 바로 테스트 하고자 해도 DB에 접근할 수 없는 문제가 발생하고, 서버를 복사해서 똑같은 환경을 만들어서 테스트 하고자 해도 해당 서버를 빌드하는데만 30분 이상이 걸리는 경우가 발생할 수 있다. 그러면 기능 하나 테스트할 때마다 30분씩 걸릴 수도 있다는 뜻이다.

따라서 우리는 이러한 문제점을 **해결**하고자 Mock Object란 것을 사용한다. Mock Object는 간단하게 설명하자면, 가짜 객체이다. 여기서 우리는 가짜 객체와 진짜 객체에 대해서 논할 필요가 있다. 진짜 객체란 실제 class 코드에 서술된 대로 모든게 완벽하게 구현되어 메모리에 올라간 객체이다. 가짜 객체는 겉모습만 class 코드에서 서술된 대로 구현되어 있지 속은 텅 비어 있는 객체를 말한다. 그래서 클래스 멤버 변수, 객체 멤버 변수, 메소드 등이 하나도 구현되어 있지 않다.

가짜 객체는 진짜 객체에서 테스트에 필요한 부분만을 가짜 객체에 구현해줄 수 있다. 따라서 빌드하는데 시간을 적게 잡아 먹으며 신속하게 테스트할 수 있다. 또한 시스템 환경 특성상 접근이 어려워 테스트하기 힘든 것들도 임의적으로 접근 가능한 환경에 구현하여 사용할 수도 있다. 이러한 장점 때문에 Mock Object는 유용하게 사용되며, 테스트 코드 작성 시에 아주 중요한 개념이다.

# Given-When-Then Style
> Given-When-Then is a style of representing tests - or as its advocates would say - specifying a system's behavior using [SpecificationByExample](https://martinfowler.com/bliki/SpecificationByExample.html).
(martinfowler.com - GivenWhenThen)

테스트 코드를 작성하는데도 스타일이 존재한다. 그래서 이 스타일을 따르면서 코드를 작성하면 보다 더 수월하게 작성할 수 있다. 대표적인 스타일이 바로 Given When Then 스타일이다. (SpecificationByExample이란 제목으로 쓴 에세이를 마틴 파울러가 링크를 걸어두었다.)

* The **given** part describes the state of the world before you begin the behavior you're specifying in this scenario. You can think of it as the pre-conditions to the test.
* The **when** section is that behavior that you're specifying.

* Finally the **then** section describes the changes you expect due to the specified behavior.

마틴 파울러의 설명은 위와 같다. 개인적인 설명을 덧붙이자면, given 부분에서는 Logic에 대입할 데이터들을 준비하는 곳이다. 그 다음에는 이 데이터들을 테스트 해 볼 Logic에 대입해야 하는데, 이를 when부분에 한다. 이때 Actual Data를 얻게 되는 것이다. 그 다음으로 then 부분에서 Actual Data를 Expected Data와 비교해보면서 최종적으로 검증을 해본다.

# 추가적으로 알아야 할 것들

## 테스트의 종류
테스트에도 종류가 존재한다. 이는 소프트웨어 공학의 소프트웨어 프로세스 모델인 V 모델에서 제시된 개념이기도 하다. 간략하게 설명하자면, V 모델은 원래의 폭포수 모델에 테스트 개념이 추가된 모델이다. 그래서 구현 단계를 테스트할 때는 Unit Test를 해보고, 아키텍쳐 설계 단계를 테스트할 때는 Integration Test를 한다. 사실 더 있지만, 생략한다.

## JUnit
단위 테스트를 도와주는 라이브러리이다. 그래서 우리는 Then 부분에서 검증을 할 때 이 라이브러리에서 제공하는 Assertion이란 클래스를 활용한다. 이는 간단하게 Expected Data와 Actual Data를 서로 같은지 비교해주는 역할을 한다고 생각하면 된다.

## Naming Convention
테스트 메소드의 이름을 정할 때는 카멜 케이스와 표준 코딩 정의서를 준수해서 하도록 하자.
(메서드의 이름은 소문자로 시작해야 하며, 동사 단어로 시작한다. : 표준 코딩 정의서)

# Repository Test

```java
@DataJpaTest
public class RepositoryTest {
	
    @Autowired
    private Repository testRepository;
    
    private Data expectedData;
    
    @BeforeEach
    void setUp(){
    	expectedData = Data.builder()
        	.id(1234)
            .build();
    }
    
    @Test
    @DisplayName("Test Repository findById")
    void testFindById() {
        // given
        Data givenData = Data.builder()
        	.id(1234)
            .build();
        
        // when
        Data actualData = testRepository.findById(givenData.getId);
        

        // then
        assertAll(
                () -> assertEquals(expectedData, actualData)
        );
        
    }

}
```

Repository 테스트 코드에서는 JpaRepository에 선언된 모든 메서드들을 테스트해봐야 한다. 즉, given data를 JpaRepository를 통해서 데이터베이스에 CRUD를 해본 다음에 예상한 결과인 Expected Data와 같은지 비교해보는 것이다. 이때 Assertions 클래스를 활용한다. 

이때 **@Autowired**를 살펴볼 필요가 있다. 우린 이미 Mock에 대해서 논했지만, Repository는 @Autowired 어노테이션을 이용해서 진짜 객체를 만들어야 한다. 사실 테스트하려는 대상 그 자체는 진짜 객체일 필요가 있다. 

**@DataJpaTest** 또한 살펴볼 필요가 있다. 이 어노테이션은 기본적으로 JPA에 필요한 Bean들만 가져온다. 또한 기본적으로 @Transaction이 내장되어 있어서 자동으로 테스트 결과를 roll back 해준다. 덧붙여 기본적으로 테스트용 인메모리 DB에 연결시킨다. 이러한 장점에 때문에 @SpringBootTest 어노테이션 대신에 Repository를 테스트할 때 사용한다.

**@BeforeEach**는 테스트 이전에 매번 실행시키도록 하는 어노테이션이다. **@Test**는 테스트 메소드임을 스프링부트에게 인식하여 테스트 실행시에 메소드를 테스트 하도록 만든다. 그래서 setUp 부분은 테스트 해야 할 메서드가 여러 개 있을 시에 공통적으로 필요한 처리를 하도록 코딩한다. (그러나 @BeforeEach는 현업에서 잘 쓰이지 않는다고 한다.)

**assertAll**은 해당 메소드에서 검증해야 할 것이 여러 개일 때 사용한다. 즉 ssertEquals을 여러 개 사용해야 되는 상황에서 사용한다.

**@DisplayName**은 IDE에서 테스트 코드를 실행하면 어떤 테스트인지 이름을 명시해주는 어노테이션이다.

# Service Test
```java
@ExtendWith(SpringExtension.class)
class ServiceTest {
	@InjectMocks
    private Service testService;

    @Mock
    private Repository usedRepository;
    
    private Data expectedData;
    
    @BeforeEach
    void setUp(){
    	expectedData = Data.builder()
        	.id(1234)
            .build();
    }
    
    @Test
    @DisplayName("Test Service findById")
    void testService() {
        // given
        Data givenData = Data.builder()
        	.id(1234)
            .build();
        
        // when
        when(usedRepository.findById(any())).thenReturn(givenData);
        Data actualData = testService.process(usedRepository.findById(givenData.getId()));

        // then
        assertAll(
                () -> assertEquals(expectedData, actualData)
        );
        
    }

}
```

여기선 Repository 클래스는 가짜 객체(Mock)을 사용하였다. 왜냐하면 이미 Repository 클래스는 테스트를 완료했기에 진짜 객체를 사용할 필요가 없다. 그리고 이 Repository 클래스는 Service 클래스 코드 내부에서 final로 선언되어 있다고 가정하였다. 그래서 Repository를 Service 내부에 주입해줘야 한다. 따라서 이를 구현하기 위해 전용 어노테이션을 붙여 주었다.

**@ExtendWith(SpringExtension.class)**을 붙이면 Spring TestContext Framework와 Junit5와 통합하여 사용하게 된다. (개인적으로는 Mockito 관련 테스트를 진행할 때 필요한 옵션인 듯 하다.) 또한 다른 .class를 넣으면 그 기능을 해당 클래스에 확장해서 사용할 수 있다.

**@Mock**은 가짜 객체를 만들어 주는 어노테이션이다.

**@InjectMocks**은 @Mock으로 만든 가짜 객체를 주입할 객체를 명시해준다. 또한 이 어노테이션이 붙은 객체는 진짜 객체로서 생성된다. 따라서 테스트할 객체에 붙여서 진짜 객체로서 테스트 하게끔 만든다.

# Controller Test
```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = Controller.class)
class ControllerTest {

	@MockBean
    protected Service usedService;
    
    private ObjectMapper objectMapper;

    private MockMvc mockMvc;
    
    private Data expectedData;
    
    @BeforeEach
    void setUp(){
    	expectedData = Data.builder()
        	.id(1234)
            .build();
    }
    
    @Test
    @DisplayName("Test Controller")
    void testService() {
        // given
        Data givenData = Data.builder()
        	.id(1234)
            .build();
        
        // when
        when(usedService.process(any())).thenReturn(givenData);
        Data actualData = testService.process(usedService.process(givenData.getId()));

        // then
        mockMvc.perform(get("/url/{id}",UUID.randomUUID())
                        .param("ID", actualData.getId()))
                .andExpect(status().isAccepted())
                .andExpect(content().json(objectMapper.writeValueAsString(expectedData)))
                .andDo(print());
       
       mockMvc.perform(post("/url")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(actualData)))
                .andExpect(status().isOk())
                .andDo(print());
                
        mockMvc.perform(put("/url",UUID.randomUUID())
                        .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(actualData)))
                .andExpect(status().isOk())
                .andDo(print());
        
        mockMvc.perform(delete("/url/{id}", givenData.getId()))
                .andExpect(status().isOk())
                .andDo(print());
      
        
    }
}

```

이제는 컨트롤러를 테스트할 차례이다. 이전까지 서비스, 레포지토리 테스트는 단위 테스트였다. 그러나 컨트롤러 테스트는 해당 모듈들을 통합해서 사용하는 테스트이므로 통합 테스트이다.

**@WebMvcTest(controllers = Controller.class)** 어노테이션은 controllers 인수에 어떤 콘트롤러를 테스트할 지 명시하여 컨트롤러 테스트를 할 수 있도록 도와준다. 그리고 컨트롤러 테스트를 진행하려면 스프링 컨테이너가 진짜로 메모리 상에 존재해야 한다. 위의 어노테이션은 그래서 ApplicationContext를 메모리에 Load 해준다.

**@MockBean**은 ApplicationContext에 등록되는 가짜 빈이다.

**mockMvc**는 스프링 컨테이너에 테스트를 위해 요청을 보낼 수 있도록 해주는 객체이다.

**objectMapper**는 mockMvc에서 스트링 또는 바이트만을 인자로 받기에 클래스를 해당 자료형으로 변형해주는 객체이다.

여기서 주의할 점은 사실 위의 코드에서 mockMvc가 4개나 있는데 이건 POST, GET, DELETE, PUT의 예시를 한번에 서술하기 위해서 그런 것이다. 진짜 테스트 코드에서는 HTTP 요청 하나에 대해서만 코드가 작성되어야 한다.

# 번외
## jacoco

자바 코드의 테스트 커버리지를 측정해주는 툴이다.

## @VisibleForTesting
private로 설정된 클래스나 메서드는 외부 파일인 테스트 코드 파일에서 접근이 불가능하다. 이런 경우 테스트할 때만 접근 가능하도록 만들기 위해 해당 어노테이션을 사용한다.
