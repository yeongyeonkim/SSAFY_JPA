**@SpringBootApplication**

- 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성을 모두 자동으로 설정.
- 특히 이 어노테이션 위치부터 설정을 읽어가기 때문에 이 클래스는 항상 프로젝트의 최상단 위치.



**SpringApplication.run** : 

run으로 내장 WAS를 실행한다.

내장 WAS를 사용해야 '언제 어디서나 같은 환경에서 스프링 부트를 배포'할 수 있다.



**@RestController**

- 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 줍니다.
- @Controller 와 @ResponseBody 어노테이션이 합쳐진 것으로 리턴값에 자동으로
  HTTP 응답데이터(body)에 자바 객체가 매핑되어 전달 된다.



**@RunWith**

- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킵니다.
  @RunWith(SpringRunner.class)



**@WebMvcTest**

- 스프링 테스트 어노테이션 중 Web(Spring MVC)에 집중할 수 있는 어노테이션.



**@Autowired**

- 스프링 컨테이너에서 만들어지는 객체 = 스프링 빈(bean). Autowired는 이 bean을 주입 받는다.



**MockMvc mvc** 

웹 API를 테스트할 때 사용, 스프링 MVC 테스트의 시작점.

mvc.perform(get(urlTemplate: "/hello")) // MockMvc를 통해 /hello 주소로 HTTP GET 요청을 한다.

.andExpect(status().isOk()) // 200, 404, 500 상태를 검증한다. 여기선 Ok() 즉, 200인지를 검증.

.andExpect(content().string(hello)); // 응답 본문 내용을 검증한다.

**param** 

- API 테스트할 때 사용될 요청 파라미터를 설정합니다.
- 단, 값은 String만 허용되므로, 숫자/날짜 등의 데이터를 문자열로 변경해야한다.
  String.valueOf(변수)

**jsonPath**

- JSON 응답값을 필드별로 검증할 수 있는 메소드.
- $를 기준으로 필드명을 명시한다. 
  ex) $.name, $.amount



**@RequestParam**

- 외부에서 API로 넘긴 파라미터를 가져오는 어노테이션.
- 외부에서 name(@Requestparam("name"))이란 이름으로 넘긴 파라미터를
  메소드 파라미터 name(String name)에 저장하게 됩니다.



<hr/>

## 롬복(lombok)

롬복은 자바 개발할 때 자주 사용하는 코드 Getter, Setter, 기본생성자, toString 등을 어노테이션으로 자동 생성해 줍니다.

build.gradle에 compile('org.projectlombok:lombok') 이라고 코드를 작성해 의존성 추가.

혹은, 버전에 따라서 implementation 'org.projectlombok:lombok' 



**@Getter**

- 선언된 모든 필드의 get 메소드를 생성해 줍니다.

**@RequiredArgsConstructor**

- 선언된 모든 final 필드가 포함된 생성자를 생성해 줍니다.

**@NoArgsConstructor**

- 기본 생성자 자동 추가

**@Builder**

- 해당 클래스의 빌더 패턴 클래스 생성
  - **빌더 패턴**(Builder pattern)이란?
    복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게 하는 패턴이다.
- 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함

<hr/>

###### 관계형 데이터베이스가 계속해서 웹 서비스의 중심이 되면서 모든 코드는 SQL 중심이 되어간다.

###### 현업에서 수십, 수백 개의 테이블이 있는데, 단순 반복적인 SQL이 애플리케이션 코드보다 많아지는 현상이 발생되었다.

그래서 JPA가 등장.

#### * JPA는 객체를 매핑하는 자바 표준 ORM(Object Relational Mapping) 기술이다.

반면, MyBatis는 쿼리를 매핑하는 SQL Mapper로 ORM이 아니다.

**스프링 부트에서 JPA로 데이터베이스를 다뤄보자!**



## JPA

서로 지향하는 바가 다른 2개 영역(객체지향 프로그래밍 언어와 관계형 데이터베이스)을 중간에서 **패러다임 일치**를 시켜주기 위한 기술.

즉, 개발자는 객체지향적으로 프로그래밍을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성해서 실행합니다. 

객체 중심으로 개발을 하게 되니 생산성 향상은 물론 유지 보수가 편하다.

**저장소 교체의 용이성** : 단순히 **의존성** 교체로, 관계형 데이터베이스 외에 다른 저장소로 쉽게 교체할 수 있다.



**@Entity**

- JPA 어노테이션으로, 테이블과 링크될 클래스임을 나타낸다. 테이블 이름을 매칭한다.
- **Entity 클래스에서는 절대 Setter 메소드를 만들지 않는다.**

**@Id**

- 해당 테이블의 PK 필드를 나타냅니다.

**@GeneratedValue**

- PK의 생성 전략을 명시하는데 사용.
- 선택적 속성으로 generator와 strategy가 있다.
  - **generator**는 <u>SequenceGenerator</u>나 <u>TableGenerator</u> 어노테이션에 명시된 주키 생성자를 재사용할 때 쓰인다. 디폴트는 공백문자("").
  - **strategy**는 엔티티의 주키를 생성할 때 사용해야 하는 <u>주키생성 전략</u>을 의미한다. 디폴트는 **AUTO**
    - IDENTITY : 기본 키 생성을 데이터베이스에 위임하는 방법. (MySQL etc..)
    - SEQUENCE : 데이터베이스 시퀀스를 사용해서 기본 키를 할당하는 방법. (H2 etc..)
    - TABLE : 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만듬.
    - AUTO : 데이터베이스에 따라서 IDENTITY, SEQUENCE, TABLE 방법 중 하나를 자동으로 선택해주는 방법.
- 스프링 부트 2.0 에서는 GenerationType.IDENTITY 옵션을 추가해야만 **auto_increment**가 된다.



**@Column**

- 해당 클래스의 필드는 모두 칼럼이 됩니다.
  ex) @Column(length = 500, nullable = false)
  	  @Column(columnDefinition = "TEXT", nullable = false)



**toEntity()**

- toEntity() 메소드를 이용하여 domain을 호출.

<hr/>

#### JpaRepository

보통 MyBatis 등에서 Dao라고 불리는 DB Layer 접근자.

JPA에선 Repository라고 부르며 **인터페이스**로 생성한다.

단순히 인터페이스를 생성 후, JpaRepository<Entity 클래스, PK 타입>를 상속하면 기본적인 CRUD 메소드가 자동으로 생성된다.

**@Repository를 추가할 필요도 없다.**

* 주의할 점은 Entity 클래스와 기본 Entity Repository는 함께 위치해야 한다.
  그래서 같은 패키지에서 함께 관리한다.

  

**@After**

- Junit에서 단위 테스트가 끝날 때마다 수행되는 메소드를 지정.



postsRepository.save : 테이블 posts에 insert/update 쿼리를 실행합니다.
										 id 값이 있다면 update 쿼리가, 없다면 insert 쿼리가 실행됩니다.

postsRepository.findAll : 테이블 posts에 있는 모든 데이터를 조회하는 메소드.

postsRepository.save() : 파라미터안에는 이제 posts의 Entity를 입력해준다.



<hr/>

## DTO(Data Transfer Object)

VO와 비슷한 의미로 사용이 된다.

1. Data를 Object로 변환하는 객체이며, DTO에서 Object는 우리가 만드는 DTO class라고 생각
2. Data가 포함된 객체를 한 시스템에서 다른 시스템으로 전달하는 작업을 처리하는 객체
3. Database record의 data를 mapping 하기 위한 data object를 말한다.

즉, Database에서 data를 얻어 service나 controller 등으로 보낼 때 사용하는 객체를 말한다.

Key&Value로 존재하는 data를 자동화 처리된 DTO로 변환되어 손 쉽게 셋팅된 object를 받아서 사용하기 위함.



<hr/>

#### 등록/수정/조회 API 만들기

API를 만들기 위해 총 3개의 클래스가 필요하다.

- Request 데이터를 받을 Dto
- API 요청을 받을 Controller
- 트랜잭션, 도메인 기능 간의 순서를 보장하는 Service



**Spring 웹 계층**

1. Web Layer

   - 흔히 사용하는 컨트롤러(@Controller)와 JSP 등의 뷰 템플릿 영역.
   - 이외에도 필터(@Filter) 등 **외부 요청과 응답**에 대한 전반적인 영역을 말함.

2. Service Layer

   - @Service에 사용되는 서비스 영역.

   - 일반적으로 Controller와 Dao의 중간 영역에서 사용.

   - @Transactional이 사용되어야 하는 영역이기도 하다.

     - **@Transactional**

       : 선언적 트랜잭션은 설정 파일이나 어노테이션을 이용해서 트랜잭션의 범위, 롤백 규칙 등을 정의.

3. Repository Layer

   - **Database**와 같이 데이터 저장소에 접근하는 영역이다. (Dao 영역으로 이해하면 쉽다.)

4. Dtos

   - Dto(Data Transfer Object)는 **계층 간에 데이터 교환을 위한 객체**를 이야기하며 Dtos는 이들의 영역을 얘기합니다.

5. Domain Model

   - 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라고 한다.
   - 이를테면 택시 앱에서 배차, 탑승, 요금 등이 모두 도메인이 될 수 있다.
   - VO처럼 값 객체들도 이 영역에 해당하기 때문에 무조건 데이터베이스의 테이블과 관계가 있어야만 하는 것은 아니다.



스프링에선 Bean을 주입받는 방식들은 다음과 같다.

- @Autowired
- setter
- 생성자

이 중 가장 권장하는 방식은 **생성자로 주입**받는 방식이다.

그래서 @RequiredArgsConstructor을 사용해서 해당 클래스의 의존성 관계가 변경될 때마다 생성자 코드를 계속해서 수정하는 번거로움을 해결한다.



<hr/>

###### <조회 기능 실제로 톰캣을 실행해서 확인해보기>

main.resources의 application.properties에 spring.h2.console.enabled=true 옵션을 추가해준다.

그 후 Application의 main 메소드를 실행한다. 정상적으로 실행됐다면 톰캣이 8080 포트로 실행.

웹 브라우저에 http://localhost:8080/h2-console로 접속하고

JDBC URL에 jdbc:h2:mem:testdb로 작성하여 h2를 관리할 수 있는 페이지로 이동한다.

간단한 쿼리문으로(insert into ~ values ~) 데이터를 추가시키고

http://localhost:8080/api/v1/posts/1 을 입력해 API 조회 기능을 테스트한다.

##### [Result]

```
{"id":1,"title":"title","content":"content","author":"author"}
```



<hr/>


### JPA Auditing으로 생성시간/수정시간 자동화하기

언제 만들어졌는지, 언제 수정되었는지 등은 차후 유지보수에 있어 굉장히 중요한 정보이다.

매번 DB에 Insert, Update 하기 전에 날짜 데이터를 등록/수정하는 코드는 굉장히 귀찮고 지저분해 보인다. 그래서 JPA Auditing을 사용.

```
@Getter
@MappedSuperclass // JPA Entity 클래스들이 이 클래스(BaseTimeEntity)를 상속할 경우 필드들(createdDate, modifiedDate)도 칼럼으로 인식하도록 합니다.
@EntityListeners(AuditingEntityListener.class) // 이 클래스에 Auditing 기능을 포함.
public abstract class BaseTimeEntity {
    @CreatedDate // Entity가 생성되어 저장될 때 시간이 자동 저장.
    private LocalDateTime createdDate;

    @LastModifiedDate // 조회한 Entity의 값을 변경할 때 시간이 자동 저장.
    private LocalDateTime modifiedDate;
}
```

위와 같이 작성한 BaseTimeEntity 클래스를 Entity 클래스에서 상속(extends)한다.



