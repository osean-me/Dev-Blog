---
title: '[IntelliJ] Create Spring MVC + Maven Project'
date: '2020-07-26'
tags: ['IntelliJ', 'Spring MVC', 'Maven', 'Oracle']
draft: false
summary: '스프링 MVC + Maven 프로젝트를 만들었는지 기록 할 겸, 나 같이 완전 초보자가 정보 유목민 생활을 청산할 수 있게 아주 작게나마 도움이 될 겸 해서 글을 적어보려고 한다.'
---

![IntelliJ IDEA logo](https://resources.jetbrains.com/storage/products/company/brand/logos/IntelliJ_IDEA_icon.png)

# 들어가기에 앞서

> 현재 국비 지원 학원에서 자바를 공부하고 있고, 세미 프로젝트를 막 마친 상황이다.
> 이제 스프링 프레임 워크를 배우는 중인데, STS 말고 다른 IDE 로도 공부하면 좋을 것 같아 도전하게 되었다.
>
> 처음 IntelliJ 를 접했을 때 Eclipse 와는 UI 라던지 처음 프로젝트를 만들어 내는 과정이 달라 많이 헤맸고, 사람마다 사용하는 방법이 달라 정보 유목민 생활이 조금 길어졌다.
>
> 그래서 내가 어떻게 스프링 MVC + Maven 프로젝트를 만들었는지 기록 할 겸, 나 같이 완전 초보자가 정보 유목민 생활을 청산할 수 있게 아주 작게나마 도움이 될 겸 해서 글을 적어보려고 한다.

---

# 시작하기

다들 IntelliJ 는 잘 설치했을거라는 가정 하에 진행하려고 한다.
처음에 IntellJ 를 실행하면 다음과 같은 화면이 나올 것이다.

![그림 1-1 시작하기](/static/images/programming/ide/image-1.png)

여기서 새로운 프로젝트를 생성하기 전에 확인하고 넘어가야 할 것이 있다.
나는 처음에 Spring MVC 플러그인이 보이지 않아 이틀 정도는 별에 별 짓을 다 해봤던 것 같다.

## 1. 내가 사용할 플러그인이 있는지 확인하기

우리가 사용해야 할 플러그인은

1. Spring MVC
2. Maven

이다. 이 두 개의 플러그인을 사용할 것이라고 체크를 했는지 먼저 확인해야한다.

- 위의 화면에서 우측 하단에 **Configure** 라는 메뉴를 클릭한다.
- Configure 메뉴에서 **Plugins** 메뉴를 클릭한다.

![그림 1-2 Plugins](/static/images/programming/ide/image-2.png)

- 검색창에 **Spring MVC / Maven** 를 검색하고 해당 플러그인이 체크가 되어있는지 확인한다.

![그림 1-3 Spring MVC](/static/images/programming/ide/image-3.png)
![그림 1-3 Maven](/static/images/programming/ide/image-4.png)

위의 두 사항에 체크가 되었는지 확인이 되었다면 이제 본격적으로 사용할 준비가 되었다는 뜻이다.

## 2. 프로젝트 생성하기

- 처음 화면의 프로젝트 생성 메뉴를 클린단다.
- 좌측의 플러그인 선택 메뉴에서 Maven 을 선택한다.
- 상단에서 사용하고자 하는 Java 의 JDK 버전을 선택한다. (저는 11로 사용하겠습니다.)
- Next 를 누른 후 프로젝트명을 설정한다. (여기서 프로젝트명은 artifactId 와 동일하게 설정된다.)
- 프로젝트명 설정 페이지에서 **Artifact Coordinates** 를 클릭하여 GroupId 도 자신이 원하는대로 설정한다.
  - GroupId 는 도메인의 역순으로 작성한다.

![그림 2-1 Maven Plugin 선택하기](/static/images/programming/ide/image-5.png)
![그림 2-2 프로젝트명과 GroupId 설정](/static/images/programming/ide/image-6.png)

일단 Maven 기반의 프로젝트는 만들어졌다. 이제 우리는 이 프로젝트에 Spring MVC 를 덮어 씌울 예정이다.

## 3. Spring MVC + TomCat Server 추가하기

### 3-1. Spring MVC 프레임워크 추가하기

- 좌측 상단에 해당 프로젝트를 오른쪽 클릭하고 (맥의 경우 ctrl + 트랙패드) **Add Framework Support...** 를 클릭한다.

![그림 3-1 프로젝트 클릭 후 프레임워크 추가](/static/images/programming/ide/image-7.png)

- 새로운 창이 뜨면, 좌측 메뉴에서 **Spring > Spring MVC** 를 선택한다.
  - IntellJ 에서는 Spring 최신 버전인 **5.2.3 RELEASE** 를 지원하고 있고, 우리도 이 버전을 사용하기로 한다.

![그림 3-2 Spring MVC 선택](/static/images/programming/ide/image-8.png)

- 선택을 했다면 해당 프로젝트에 Spring 을 다운 받을 것이다.

![그림 3-3 Spring MVC 라이브러리 다운로드](/static/images/programming/ide/image-9.png)

### 3-2. TomCat Server 추가하기

- 상단 중앙에서 망치 모양 옆에 **Add Configuration...** 을 선택한다.

![그림 3-4 Add Configuration..](/static/images/programming/ide/image-10.png)

- 좌측 상단에서 **+** 을 클릭하고 **TomCat Server > Local** 을 더블클릭하여 선택한다.
- **Port 번호를 9090** 으로 바꿔준다.
  - 맥의 경우 도커와 포트가 겹치는 걸 예방하기 위해서 이렇게 설정한다. 저의 경우에는 도커 데이터 베이스 포트가 8080이라 톰캣을 9090으로 설정해줬습니다.
- 하단에 Fix 를 클릭하고, 경로를 자신의 프로젝트명과 똑같이 입력 후 1)Apply 2)Ok 를 눌러준다.
  - 많은 게시글에서 경로를 **/** 로 설정해주라고 하는데, 주소 입력할 때 헷갈리기도 하고 무엇보다 저는 **/** 로 했을 때 아무리 맞는 주소를 입력해도 404 에러가 발생해서 해당 프로젝트 명을 입력했습니다.

![그림 3-5 Tomcat Server > Local](/static/images/programming/ide/image-11.png)

이제 프레임워크와 서버 설정을 끝났다. 다음 단계에서는 pom / web / dispatcher-servlet.xml 문서들을 손봐줄 예정이다.

## 4. xml 문서들 혼내주기

위의 과정들이 잘 마무리가 되었다면 3개의 xml 문서에 해당 프로젝트를 어떻게 운영할건지 선언(?) 해주는 작업을 거쳐야 한다.

### 4-1. pom.xml

- 위의 과정들이 끝나면 자동으로 pom.xml 파일이 열려있을 것이다.

![그림 3-6 pom.xml](/static/images/programming/ide/image-12.png)

- 해당 xml 문서에 아래의 코드를 복사해서 붙혀넣자. 그 전에 각자의 환경이 다르니 확인해야 할 것이 있다.
  1. 사용할 **JDK** 버전
  2. 사용할 **Spring** 버전
  3. 사용할 **데이터 베이스** + 버전
- 자신이 어떤 버전들을 사용할 것인지 확인 후에 아래의 코드를 복사해서 pom.xml에 복사 / 붙혀넣기를 하자.
- 이 코드에는 오라클 데이터베이스의 **OJDBC 8 / DBCP** 에 대한 코드가 있으니 확인하고 사용해야 한다.   
   **자신에게 맞는 데이터 베이스의 JDBC 와 DBCP 를 Maven Repository 에서 찾아 수정하는 것이 좋다.**
- **붙여넣을 때 주의사항**

  - pom.xml 문서 내에 처음에 작성된 코드 다음에 붙여넣기를 해야한다.

  ```xml
    <!-- 이 문서에서 사용할 변수 -->
    <properties>
        <java-version>11</java-version>
        <org.springframework-version>5.2.3.RELEASE</org.springframework-version>
        <org.aspectj-version>1.6.10</org.aspectj-version>
        <org.slf4j-version>1.6.6</org.slf4j-version>
    </properties>

    <!-- 의존성 라이브러리 - 프로젝트에서 사용할 라이브러리의 정보 - groupId : 회사정보 - artifactId : 프로그램정보
        - version : 버전정보 -->
    <dependencies>
        <!-- Spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${org.springframework-version}</version>
            <exclusions>
                <!-- Exclude Commons Logging in favor of SLF4j -->
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${org.springframework-version}</version>
        </dependency>

        <!-- AspectJ -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>${org.aspectj-version}</version>
        </dependency>

        <!-- Logging -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${org.slf4j-version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>${org.slf4j-version}</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${org.slf4j-version}</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.15</version>
            <exclusions>
                <exclusion>
                    <groupId>javax.mail</groupId>
                    <artifactId>mail</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>javax.jms</groupId>
                    <artifactId>jms</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>com.sun.jdmk</groupId>
                    <artifactId>jmxtools</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>com.sun.jmx</groupId>
                    <artifactId>jmxri</artifactId>
                </exclusion>
            </exclusions>
            <scope>runtime</scope>
        </dependency>

        <!-- @Inject -->
        <dependency>
            <groupId>javax.inject</groupId>
            <artifactId>javax.inject</artifactId>
            <version>1</version>
        </dependency>

        <!-- Servlet -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.3.3</version>
            <scope>provided</scope>
        </dependency>

        <!-- Test -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.7</version>
            <scope>test</scope>
        </dependency>

        <!-- OJDBC8 -->
        <dependency>
            <groupId>com.oracle.database.jdbc</groupId>
            <artifactId>ojdbc8</artifactId>
            <version>19.7.0.0</version>
        </dependency>
        <!-- 의존성 설정 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${org.springframework-version}</version>
        </dependency>

        <!-- DBCP -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-dbcp2</artifactId>
            <version>2.7.0</version>
        </dependency>

    </dependencies>

    <!-- 실제 명령 수행 시 필요한 설정 -->
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-eclipse-plugin</artifactId>
                <version>2.9</version>
                <configuration>
                    <additionalProjectnatures>
                        <projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
                    </additionalProjectnatures>
                    <additionalBuildcommands>
                        <buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
                    </additionalBuildcommands>
                    <downloadSources>true</downloadSources>
                    <downloadJavadocs>true</downloadJavadocs>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <configuration>
                    <source>${java-version}</source>
                    <target>${java-version}</target>
                    <compilerArgument>-Xlint:all</compilerArgument>
                    <showWarnings>true</showWarnings>
                    <showDeprecation>true</showDeprecation>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>org.test.int1.Main</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
  ```

- 아래의 코드를 넣은 다음 문서에서 나타난 Maven 아이콘을 클릭한다. 그러면 해당 문서에 기입된 dependency 를 다운받아 온다.

![그림 3-7 Maven Reload](/static/images/programming/ide/image-13.png)

### 4-2. web.xml

web.xml 에서는 인코딩 방식, dispatcher 서블릿에 대한 설정을 진행한다.

- WEB-INF > web.xml 파일을 찾는다.

![그림 4-3 web.xml](/static/images/programming/ide/image-14.png)

- web-app 태그 내에 있는 코드들을 지우고 아래의 코드를 복사 후 붙여넣기 한다.

```xml
    <!-- 스프링에서 제공해주는 인코딩 필터를 등록 -org.springframework.web.filter.CharacterEncodingFilter -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <!-- 1. 인코딩 방식을 UTF-8 방식으로 설정 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <!-- 2. 충돌 시 강제 인코딩 설정 -->
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- The definition of the Root Spring Container shared by all Servlets
        and Filters -->
    <!-- 모든 서블릿 및 필터가 공유하는 루트 스프링 컨테이너의 정의 (최상위 설정 파일) -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </context-param>

    <!-- Creates the Spring Container shared by all Servlets and Filters -->
    <!-- 모든 서블릿 및 필터가 공유하는 스프링 컨테이너를 만듭니다. (설정을 연결해주는 도구를 등록) -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener
        </listener-class>
    </listener>

    <!-- Processes application requests -->
    <!-- 응용 프로그램 요청 처리 (요청 처리 메인 서블릿 등록 > DispatcherServlet) -->
    <servlet>
        <servlet-name>appServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <!-- 추가 설정에 관한 내용을 등록 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/dispatcher-servlet.xml
            </param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>appServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

### 4-3. dispatcher-servlet.xml

해당 xml 에서는 pom.xml 에서 등록했던 JDBC / DBCP 를 등록하는 과정이다

- WEB-INF > dispatcher-servlet.xml 을 찾아 파일을 연다.

![그림 4-4 dispatcher-servlet.xml](/static/images/programming/ide/image-15.png)

- xml 내에 모든 코드를 지우고 아래의 코드를 복사 후 붙여 넣는다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans
        xmlns="http://www.springframework.org/schema/mvc"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:beans="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context"
        xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- DispatcherServlet Context: defines this servlet's request-processing
        infrastructure -->

    <!-- Enables the Spring MVC @Controller programming model -->
    <annotation-driven />

    <!-- Handles HTTP GET requests for /resources/** by efficiently serving
        up static resources in the ${webappRoot}/resources directory -->
    <resources mapping="/resources/**" location="/resources/" />

    <!-- Resolves views selected for rendering by @Controllers to .jsp resources
        in the /WEB-INF/views directory -->
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <beans:property name="prefix" value="/WEB-INF/views/" />
        <beans:property name="suffix" value=".jsp" />
    </beans:bean>

    <!--JDBC 등록 -->
    <beans:bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <beans:property name="driverClassName" value="oracle.jdbc.OracleDriver"></beans:property>
        <beans:property name="url" value="jdbc:oracle:thin:@localhost:1521:XE"></beans:property>
        <beans:property name="username" value="C##DO"></beans:property>
        <beans:property name="password" value="C##DO"></beans:property>
    </beans:bean>

    <!-- DBCP 등록 -->
    <beans:bean id="dbcpSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <beans:property name="driverClassName" value="oracle.jdbc.OracleDriver"></beans:property>
        <beans:property name="url" value="jdbc:oracle:thin:@localhost:1521:XE"></beans:property>
        <beans:property name="username" value="C##DO"></beans:property>
        <beans:property name="password" value="C##DO"></beans:property>

        <beans:property name="maxTotal" value="20"></beans:property>
        <beans:property name="maxIdle" value="10"></beans:property>
        <beans:property name="maxWaitMillis" value="3000"></beans:property>
    </beans:bean>

    <beans:bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!-- Dependency Injection > 필요한 도구를 주입한다는 의미 -->
        <beans:property name="dataSource" ref="dataSource"></beans:property>
    </beans:bean>
    <context:component-scan base-package="GroupId.ArtifactId" />
</beans:beans>
```

- 아래에서 `<context:component-scan base-package\="자신이 설정한 GroupId.ArtifactId" />` 를 찾아 수정한다.
  - 현재에 자신이 설정한 대로 작성하면 빨갛게 변할텐데, 이 부분은 나중에 해결할 것이다.

xml 에 대한 부분은 모두 끝났다. 추후에 자신이 더 필요한게 있다면 입맛에 맞게 수정하면 된다.
이제 설정한 xml 을 해당 프로젝트 라이브러리에 추가하는 작업이 있다.

## 5. xml 에 대한 라이브러리 Artifacts에 추가하기

위에서 설정한 모든 것들을 해당 프로젝트의 라이브러리에 추가하는 작업이 필요하다.

- `command + ;` 혹은 우측 상단에 아이콘을 눌러 Project Structure 창을 연다.

![그림 5-1 Project Structure](/static/images/programming/ide/image-16.png)

- 좌측 메뉴에서 **Artifacts** 메뉴 클릭한다.

![그림 5-2 Artifacts](/static/images/programming/ide/image-17.png)

- 화면의 우측(**Available Elements**)에서 자신의 프로젝트를 더블 클릭하면 자신의 프로젝트 라이브러리에 넣어야 할 목록이 뜬다.

  - 이 라이브러리들을 더블클릭하여 **WEB-INF > lib** 에 넣어주도록 하자.

  ![그림 5-3 Available Elements](/static/images/programming/ide/image-18.png)

- 다 넣어준 후, 1) apply 2)ok 순으로 클릭한다.

이제 대부분의 설정은 거의 끝이 났다.
앞으로 main > java 폴더에 패키지를 만들고 Servlet + JSP 를 만들어 정상적으로 실행이 되는지 테스트 할 것이다.

## 6. Package + Servlet + JSP 만들기

---

### 6-1. package + servlet 만들기

앞에서 진행한 dispatcher-servlet.xml 에서 `<context:component-scan base-package\="자신이 설정한 GroupId.ArtifactId" />` 태그가 오류가 난 것을 확인할 수 있는데, java 폴더 내에 위의 경로대로 package 를 만들면 해결이 된다.

- **src > main > java** 폴더에 위에 **작성한 경로대로 package 생성**한다.
- 테스트를 위해 마지막 package 에 TestController 를 만든다.

![그림 6-1 TestController](/static/images/programming/ide/image-19.png)

### 6-2. WEB-INF > views > test.jsp 만들기

현재 우리의 프로젝트는 dispatcher-servlet.xml 에서 view 에 대한 부분을 WEB-INF.views 부터 적용해놨다.
이 부분은 자신의 입맛에 따라 변경가능하지만, 우리는 STS 에서 자동으로 만들어진 태그 그대로 가져다 쓴 것이기 때문에 일단은 테스트 용으로 진행해보도록 하자.

- **WEB-INF 폴더 안에 views 폴더를 만든다.**
- views 폴더 안에 자신이 위에서 **리턴하고자 하는 문자열의 jsp 파일을 만든다.** (나는 test 를 리턴하니까 test jsp 를 만들 것이다.)

![그림 6-2 Create JSP](/static/images/programming/ide/image-20.png)

이제 모든 설정은 끝이 났다.
마무리로는 우리가 설정한 것이 잘 구동이 되는지 테스트 해보는 것이다.

## 7. 테스트 해보기

이제 우리의 과정이 잘 적용이 되었는지 테스트 해볼 일만 남았다. 긴장이 되는 순간이다.
우리는 테스트에서 jsp 파일을 직접 구동시키는게 아니라 tomcat 을 구동시킬 것이다.

- 맥 > **ctrl + option + r** 단축키를 눌러 **TomCat > Run**

![그림 7-1 Start Server](/static/images/programming/ide/image-21.png)

- 브라우저가 실행되면 index.jsp 파일이 열릴 것이다.
- **주소창에 자신이 설정한 컨트롤러 주소(RequestMapping) + GetMapping 에 지정한 주소를 넣고 이동한다.**
  - 저와 똑같이 하셨다면 아래의 주소로 이동하시면 됩니다.
  - http://localhost:9090/testspring/test/go
- 이동 후, 자신이 리턴하고자 하는 JSP 가 제대로 열리면 성공이다.

![그림 7-2 Success](/static/images/programming/ide/image-22.png)

---

# 마치며

> 나는 Spring MVC + Maven 프로젝트를 만드는 과정을 학원에서 윈도우로 배웠다.
> 그래서 윈도우에서 해당 프로젝트는 이젠 혼자서도 너무 잘 만든다.
>
> 하지만 IntelliJ 에서 프로젝트를 만드는 과정이 STS 와 정말 많이 달랐고, 많은 블로그에서 말하는 방법들이 (xml 이라던지) 제 각각이라 어떤게 맞는건지 알 수가 없었다. 그래서 결국 블로그를 따라하기 보단 많은 글들이 말하는 부분만을 제외하고 학원에서 배운대로 해보려고 하나 하나 과정을 이해하면서 나아가니 결국은 성공했다.
>
> 이 과정에서 나는 스프링을 배운 첫 날부터 지금까지 밤낮 없이 계속 도전해왔는데, 내가 그렇게 고생할 수 밖에 없었던 이유는!
>
> 프로젝트가 만들어지는 과정을 전혀 이해하지 못하고 그저 블로그에서 하라는대로 따라하기 급급했기 때문에 근본에 대한 이해없이 만들려고 하다보니 계속 실패했던 것 같다.
>
> 학원 동기들에게도 항상 '강사님 코드 무조건 복사 붙여넣기 하지 말라' 고 말하는 내가 그렇게 행동하다니 내 자신에게 참 부끄럽다.
>
> 이 글을 읽는 많은 분들은 나처럼 무작정 복사 붙여넣기는 하지 않았으면 좋겠다.
>
> 이해되지 않고 하는 것과 미세하게나마 이해하고 하는 것은 완전 하늘과 땅 차이라고 생각한다. 앞으로 나아가는 정도도 확연히 차이가 나고.
>
> 두서도 없고 자세하지도 않은 글을 읽어주신 모든 분들께 감사하고, 이번 사건(?) 을 계기로 차근차근 나아가려고 노력해야겠다.
