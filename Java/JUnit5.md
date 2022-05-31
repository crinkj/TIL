> ### 개요
* JUnit은 자바에서 대표적인 `unit-testing` 프레임 워크이다. JUnit 5 버전은 또한 Java 8 에 등장한 새로운 기능들도 지원해주면서 Java 8 버전과 호환성이 좋다.
  * `unit-test(단위 테스트) 사용이유`: 전체적으로 말했을 때 유닛테스트는 버그를 줄이고 코드 퀄리티를 높이기위해 만든다. 또한 어플리케이션이
  무거울 경우에 테스트하기 위해 매번 서버를 껐다 키면 시간적인 비용과 소모되는 자원들이 많다. 하지만 단위 테스트를 함으로써 단위만 실행시키면되서 
  더욱 빠르게 정상적으로 작동하는지 확인할 수 있다.

JUnit5 구성요소 
  * JUnit Platform: 테스팅 프레임워크를 구동하기 위한 launcher와 테스트 엔진을 위한 API를 제공한다.
  * JUnit Jupiter: JUnit 5를 위한 테스트 API와 실행 엔진을 제공한다.
  * JUnit Vintage: JUnit 3과 4로 작성된 테스트를 JUnit 5 Platform에서 실행하기 위한 모듈을 제공한다.
```
@Test
public void test(){
  assertEquals(5 + 2, 7)
}
```
* 간단하게 단위테스트란 위처럼 원하는 단위 함수에 `@Test` 어노테이션을 붙혀준 후 `assertEquals(예상값,실제 값)` 이런식으로 예상값과 실제값을 넣은후 테스트를 실행하는게 단위테스트이다. 실행을 하면 실패 아니면 성공이 출력된다.

> ### JUnit 5 추가된 Annotations
* `@TestFactory` – 다이나믹(동적) 테스트를 진행할때 팩토리 메소드로 생성해야할 때 사용하는 어노테이션 
* `@DisplayName` – 테스트할 메소드나 클래스 위에 붙혀주면 테스트 이름을 커스텀할 수 있게 지원하는 어노테이션 `(ex:@DisplayName("개인 테스트"))`
* `@Nested` – non-statc 클래스이며 중첩 클래스일때 사용하는 어노테이션
* `@Tag` – 태그 어노테이션을 사용하면 테스트를 할때 해당 어노테이션이 달린 메소드를 제외하고 실행한다. Run/debug configuration에서 설정한 tag 이름 선택해줘야 필터 작동한다. `(ex:@Tag("exclude"))`
* `@ExtendWith` – @RunWith 대체되서 나온 어노테이션
  * `@RunWith` - 테스트에 필요한 @Autowired,@MockBean에 해당되는 것들만 application context에 로딩한후 테스트를 진행하게 도와주는 어노테이션
* `@BeforeEach` – 매번 테스트가 실행되기전에 이 어노테이션이 달려있는 함수가 작동되게 도와주는 어노테이션(보통 테스트 케이스 초기화할때 사용, 전에는 @Before로 사용)
* `@AfterEach` – 매번 테스트가 실행된 후 이 어노테이션이 달려있는 함수가 작동되게 도와주는 어노테이션(전에는 @After로 사용)
* `@BeforeAll` –  모든 테스트가 실행되기전에 이 어노테이션이 달려있는 함수가 작동된다.(전에는 @BeforeClass로 사용)
* `@AfterAll` – 모든 테스트가 실행된후 이 어노테이션이 달려있는 함수가 작동된다. (전에는 @AfterClass로 사용)
* `@Disable` – 이 어노테이션이 달려있으면 테스트가 동작을 안한다 (전에는 @Ignore로 사용)

> ### JUnit and Java 8
#### Assertions
* `Assertions`: Assertion이 한글 뜻으로 주장이라는 뜻인데 테스트가 원하는 결과를 제대로 리턴하는지 에러는 발생하지 않는지 확인할 때 사용하는 메소드를 말합니다.
  * ex) "A"를 넣으면 "B"가 나온다.
```
@Test
void lambdaExpressions() {
    List numbers = Arrays.asList(1, 2, 3);
    assertTrue(numbers.stream()
      .mapToInt(Integer::intValue)
      .sum() > 5, () -> "Sum should be greater than 5");
}
```
* 위와같이 람다 표현식으로 얻을 수 있는 장점들이 있다.
  * 함수에 메세지를 달면서 가독성있게 테스트 코드를 짤 수 있다. 이렇게 테스트 코드에 메세지를 담음으로써 많은 시간과 자원을 절약할 수 있다.
```
 @Test
 void groupAssertions() {
     int[] numbers = {0, 1, 2, 3, 4};
     assertAll("numbers",
         () -> assertEquals(numbers[0], 1),
         () -> assertEquals(numbers[3], 3),
         () -> assertEquals(numbers[4], 1)
     );
 }
```
* 위처럼 `assertAll()`을 사용해서 여러개의 테스트 케이스를 한 함수로 한번에 테스트를 할 수 있다. 
하나라도 실패하면 테스트 실패로 뜬다.

#### Assumptions
* `Assumptions`: assumption은 한글로 추정이라는 뜻으로 메소드별 조건을 만족할 경우 진행시키고 아닌 경우 스킵하는 메소드입니다. `assumeTrue()`,`assumeFalse()`,`assumingThat()`가 대표적으로 사용된다.
  * ex) 만약 if인경우 테스트를 진행해라.

```
@Test
void trueAssumption() {
    assumeTrue(5 > 1);
    assertEquals(5 + 2, 7);
}

@Test
void falseAssumption() {
    assumeFalse(5 < 1);
    assertEquals(5 + 2, 7);
}

@Test
void assumptionThat() {
    String someString = "Just a string";
    assumingThat(
        someString.equals("Just a string"),
        () -> assertEquals(2 + 2, 4)
    );
}
```
* assumptionThat() 테스트같은 경우 만약 someString가 "Just a String" 이면 assertEquals(2+2, 4)를 진행해라. 이런식으로 조건문 처럼 사용할 수 있다.

> ### 결론
  
* `JUnit 5`의 아키텍쳐 변경도 기존 JUnit 라이브러리와 다르게 변경이 많이 되었다. 또한 `JUnit5`은 `Java 8`과 호환성이 띄어나며 특히 Stream 과 Lambda 컨셉같은 표현식도 JUnit에서 사용이 가능하다.
  
> ### 참조
* https://www.baeldung.com/junit-5
* https://ssons.tistory.com/63?category=959869
* https://effortguy.tistory.com/123

  