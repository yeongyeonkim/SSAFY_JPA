# JPA

https://gangnam-americano.tistory.com/53
gradle & buildship 생성

https://cpdev.tistory.com/23?category=621940.
Spring Starter Project를 생성하는데, Type을 설치된 Buildship 버전에 맞춰주고
Web을 체크해줌.
### compile과 implemenation
- gradle 3.0이 나오면서 compile은 더이상 사용되지 않고, implementation 또는 api를 써야 한다.
- api는 consumer에게 library가 노출이 되어, consumer의 classpath에 포함이 된다.
- implementation은 consumer에게 노출되지 않아, compile classpath에 들어가지 않는다.
- build.gradle에 tomcat과 jstl을 implementation 작성.
	implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
	implementation 'javax.servlet:jstl:1.2'
★이후 프로젝트를 우클릭하여 Gradle -> Refresh Gradle Project를 해준다.
application.properties와 WebController를 설정한다.
그리고 jsp가 있을 경로들에 대한 폴더들을 생성해 주고 jsp파일 작성 후 실행
경로 : localhost:8080

https://cpdev.tistory.com/24?category=621940
- lombok 라이브러리 추가 (domain getter, setter 등을 쉽게 작성할 수 있도록 해주는 라이브러리)
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.projectlombok:lombok:1.16.6'
- 추후 MySql로 DB를 변경하기 위해 관련 패키지도 추가한다.
	runtime('org.hsqldb:hsqldb')
	runtime('mysql:mysql-connector-java')
Gradle -> Refresh Gradle Project

