# 2주차-스터디

# WEB에 대한 전반적 이해 ( REST API, HTTP/HTTP2.0/ HTTP3.0/ HTTPS )와 스프링 프레임워크를 사용하는 이유 (다른 프레임워크와의 차이점)

# WEB에 대한 전반적 이해

## REST API

### REST

자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든것.

HTTP URI를 통해 자원을 명시하고 HTTP Method(POST,GET,PUT,DELETE)를 통해 해당 자원에 대한 CRUD 를 수행하는 것.

### REST API

REST기반으로 서비스 API를 구현.

## HTTP/HTTP2.0/ HTTP3.0/ HTTPS

- HTTP 메시지에 모든 것을 전송

### HTTP

1. 클라이언트-서버 구조
    1. Request-Response 구조
    2. 클라이언트는 서버에 요청을 보내고, 응답을 대기
    3. 서버가 요청에 대한 결과를 응답
2. 무상태 프로토콜(Stateless)
    1. 서버가 클라이언트의 상태를 보존하지 않음.
    2. 모든 것을 무상태로 설계할 수 있는 경우도 있고, 없는 경우도 있다.
        1. 상태 유지해야하는 경우 : 로그인
        2. 무상태 : 로그인 필요 없는 단순 서비스 소개
        3. 상태 유지는 최소한만 사용
3. HTTP 메시지
    1. 거의 모든 형태의 데이터 전송 가능
4. 단순함, 확장 가능

### HTTPS

https보다 ㅂ

### 웹 서버 (Web Server)

- HTTP 기반 동작
- 정적 리소스 제공
    - 정적 HTML,CSS,JSS
- ex ) NGINX,APACHE

### 웹 애플리케이션 서버(WAS : Web Application Server)

- HTTP 기반으로 동작
- 웹 서버 기능 포함 == 정적 리소스 제공 가능
- 애플리케이션 로직 수행
    - **WAS는 애플리케이션 코드 실행에 특화되어있음**
    - 동적 HTML,HTTP API(JSON)
    - Servlet, JSP,Spring MVC
        - 자바는 Servlet 컨테이너 기능 제공하면 WAS
- ex ) Tomcat, Jetty,

### 웹 시스템 구성 (WEB-WAS-DB)

- 정적 리소스는 Web 서버가 처리
- 동적 리소스는 WAS에 요청 위임
- WAS는 애플리케이션 로직 전담
- 효율적 리소스 관리
    - 정적 리소스 많이 쓰면 Web 서버 증설
    - 애플리케이션 리소스 많이 쓰면 WAS 서버 증설

# 스프링 프레임워크를 사용하는 이유

### Spring

- 자바 언어 기반의 프레임워크
- 자바의 가장 큰 특징 객체 지향 언어
- 객체 지향 언어가 가진 특징을 살려내는 프레임 워크
    
    <aside>
    💡 객체지향은 프로그램을 유연하고 변경이 용이하게 만들기때문에 좋다.
    
    </aside>
    
    - 컴퓨터 부품 갈아 끼우듯이, 컴포넌트를 쉽고 유연하게 변경하며 개발

### 객체지향 4요소 (추상화,캡슐화,상속,다형성)

- 다형성
    - 역할과 구현으로 구분, 자동차가 역할. 아반떼가 구현
    - 역할과 구현을 구분하면 단순해지고 유연해지며 변경이 용이하다.
    - `DI 의존관계 주입`, `IoC 스프링 제어 역전` 은 다형성을 활용해 역할과 구현을 편하게 다룰 수 있도록한다.

### DI 의존관계 주입, IoC 스프링 제어 역전

- 제어의 역전 IoC가 일어나는 것을 전제로 스프링 내부 객체들 간 관계를 관리할 때 사용한다.
- 특정 개체에 필요한 객체를 외부에서 주입하는 것을 말하며, 여기서 외부는 객체 기준 외부.(Config File) 을 말한다.
- 실무에서는 정형화된 Service,Controller,Repository 같은 코드는 Component Scan 을 사용한다. 정형화 되지 않거나 상황에 따라 구현 클래스를 사용하는 경우 설정으로 Spring Bean을 등록한다.

## 컴포넌트 스캔, 자동 의존관계 설정

### 컴포넌트스캔

- 스프링빈이 수백개가 되면, 일일히 @Bean으로 등록하기 어려워진다.
- 그래서 스프링은 설정정보 없이 자동으로 스프링을 등록하는 `컴포넌트 스캔`을 제공한다.

### 의존관계 주입

의존관계 주입은 크게 4가지가있다.

- 생성자 주입
    - 최근에는 스프링을 포함한 DI 프레임워크 대부분이 생성자 주입을 권장한다.
        - 대부분 의존관계 주입은 한번 일어나면, 의존관계를 변경할 일이없다. 변경되면 안된다.
        - 생성자 주입은 객체 생성시 딱 1번만 호출되므로 이후 호출되는 일이 없고 따라서 불변하게 설계 가능하다.
    - 생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다. 오로지 생성자 주입방식만 final 키워드를 사용할 수 있으며, final 키워드를 사용하고 값이 설정되지 않는 오류가 발생한다면 컴파일 시전에 막아준다.
    - Lombok 라이브러리가 제공하는 @RequiredArgsConstructor를 사용하면 final이 붙은 필드들의 생성자를 자동으로 만들어준다. 따라서 생성자 주입 의존관계주입 코드는 다음과 같다.
    
    ```java
     @Component
      @RequiredArgsConstructor
      public class OrderServiceImpl implements OrderService {
    			private final MemberRepository memberRepository;
          private final DiscountPolicy discountPolicy;
    }
    ```
    
- 수정자 주입 (setter)
    - setXX 메소드를 public 으로 열어두어야하기때문에 좋은 설계방법이 아니다.
- **필드 주입**
    
    ```java
    @Component
        public class OrderServiceImpl implements OrderService {
    
            @Autowired
            private MemberRepository memberRepository;
            @Autowired
            private DiscountPolicy discountPolicy;
    }
    ```
    
    - DI(Dependency Injection)가 없으면 아무것도 할 수없다.
- 일반 메서드 주입

출처 : 김영한의 스프링핵심원리-기본편,김영한의 모든 개발자를위한 HTTP 웹 기본지식