## WEB에 대한 전반적 이해 ( REST API, HTTP/HTTP2.0/ HTTP3.0/ HTTPS ) 
<hr/>

### API란?

* 두 소프트웨어 구성 요소가 서로 통신할 수 있게 하는 메커니즘이다.
* Application Programming Interface의 약자로, API의 맥락에서 Application은 고유한 기능을 가진 모든 소프트웨어를 말한다. Interface는 두 Application 간의 서비스 계약이다. 이 계약은 **요청과 응답을 사용하여 두 Application이 서로 통신하는 방법**을 정의한다. 
* API가 생성된 시기와 이유에 따라 네 가지 방식으로 작동할 수 있다. 
  - SOAP API
  - RPC API
  - Websocket API
  - **REST API**
<br/>
<br/>

### REST API란?
* 오늘날 웹에서 볼 수 있는 가장 많이 사용되고 유연한 API이다.
* 클라이언트가 서버에 요청을 데이터로 전송한다. 서버가 이 클라이언트 입력을 사용하여 내부 함수를 시작하고 출력 데이터를 다시 클라이언트에 반환한다.
* REST는 Representational State Transfer의 줄임말이며, REST는 클라이언트가 서버 데이터에 접근하는 데 사용할 수 있는 GET, PUT, DELETE 등의 함수 집합을 정의한다. 클라이언트와 서버는 **HTTP**를 사용하여 데이터를 교환한다.
* 서버에 대한 클라이언트 요청은 웹 사이트를 방문하기 위해 브라우저에 입력하는 **URL**과 유사하다. 서버의 응답은 일반 데이터이다.
* REST API의 주된 특징은 무상태이다.
    - **무상태**란? **서버가 요청 간에 클라이언트 데이터를 저장하지 않음**을 의미한다.
* REST API 설계 규칙 중 일부는 다음과 같다.
    - URI는 정보의 자원을 표현한다.
        - 동사보다는 명사, 대문자보다는 소문자, 도큐먼트(객체 인스턴스나 데이터베이스 레코드와 유사한 개념) 이름은 단수 명사, 컬렉션(서버에서 관리하는 리소스), 스토어(클라이언트에서 관리하는 리소스) 이름은 복수 명사를 사용한다.
    - 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.
        - URI에 HTTP Method, 행위에 대한 동사 표현이 들어가면 안된다. 즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.
    - 자원 간의 연관 관계가 있는 경우 둘 다 명시적으로 작성한다.

<br/>
<br/>

### HTTP (Hyper Text Transfer Protocol)
하이퍼 텍스트 전송 프로토콜로, 인터넷에서 하이퍼 텍스트를 교환하기 위한 통신 규약이다. 웹 서버와 클라이언트 간의 데이터 전송을 위한 응용계층 프로토콜이다. HTTP 서버가 80번 포트에서 요청을 기다리고 있으며 클라이언트는 80번 포트로 요청을 보내게 된다.

<br/>
<br/>

### HTTPS (Hyper Text Transfer Protocol Secure)
하이퍼 텍스트 전송 프로토콜 보안으로, 표준 HTTP와 동일한 방식으로 작동한다. 서버와 주고받는 데이터가 암호화되기 때문에 웹 사이트에 추가적인 보호를 제공한다. HTTPS는 443번 포트를 사용하며, 네트워크 상에서 전송 시 중간에 데이터를 볼 수 없도록 작동한다.

<br/>
<br/>

### HTTP 0.9
1991년 HTTP 초기 버전이다. 요청은 단일 라인으로 구성되고 자원에 대한 Method는 GET만 존재하며, 응답도 단순하게 파일 내용 자체로 구성되어 있다. HTTP 헤더도 없고 HTML 파일만 전송 가능했다.

<br/>
<br/>

### HTTP 1.0
1996년 HTTP 헤더 개념, 상태 코드 라인, Content-Type의 도입으로 개선되었다. 하지만 하나의 connection 당 요청 하나와 응답 하나만 처리 가능했다. 매우 비효율적이며 서버 부하 문제도 있다.

<br/>
<br/>

### HTTP 1.1
1997년 HTTP 1.0의 문제점을 보완하는 Persistent Connection과 Pipelining이 추가되었다. 지정한 timeout 동안 connection을 닫지 않고 connection의 사용성을 높였으며, 앞 요청의 응답을 기다리지 않고 요청을 연속적으로 보내고 그 순서에 맞게 응답을 받게 되었다. 하지만 동시에 처리되지는 않아 앞 요청의 응답이 오래 걸리면 뒤 요청이 blocking 되고, 연속된 요청의 헤더의 많은 중복이 발생하는 문제가 있다.

<br/>
<br/>

### HTTP 2.0
2015년 HTTP 1.1 표준의 확장 버전이다. 기존 일반 텍스트 형식으로 전송되었던 HTTP 메시지 전송 방식이 바이너리 형식 사용으로 전환되어 속도 향상 및 오류 발생 가능성이 낮아졌다. Multiplexed Streams, Stream Prioritization, Server Push, Header Compression을 도입하여 요청을 Stream으로 처리해 동시 처리가 가능해졌고, 헤더 오버헤드를 감소시켰다. 하지만 Stream으로 구분해 병렬 처리를 하더라도 TCP 고유의 HOL Blocking이 존재한다. 
  
<br/>
<br/>

### HTTP 3.0
QUIC을 기반으로 나온 새 HTTP 버전이다. 기존 TCP 고유의 문제점을 개선하기 위해 UDP 기반의 전송 프로토콜인 QUIC을 사용하여 속도를 높이고 HOL BLocking 문제를 해결한다.

<br/>
<br/>
<br/>

## 스프링 프레임워크를 사용하는 이유 (다른 프레임워크와의 차이점)

<hr/>

### POJO 프로그래밍을 지향
POJO는 순수 Java만을 통해서 생성한 객체를 말하고, 특정 기술이나 환경에 종속되지 않는다. 만약 외부 모듈에 의존하고 있는 객체를 사용하는데 해당 기술이 Deprecated되거나 신기술이 등장한다면 관련 코드를 전부 수정해야 한다. POJO를 사용해 비즈니스 로직을 구현하면 객체지향 설계를 제한없이 적용할 수 있고 코드가 단순해져 테스트와 디버깅도 쉬워진다.
POJO 프로그래밍을 지향하는 것이 스프링 프레임워크의 큰 특징이다. POJO 프로그래밍을 위해 스프링이 지원하는 기술로는 IoC/DI, AOP, PSA가 I있다.

<br/>
<br/>

### IoC/DI (Inversion of Control / Dependency Injection, 제어의 역전 / 의존성 주입)
스프링을 사용하면 애플리케이션 로직 외부에서 클래스 A가 사용할 객체를 별도로 설정할 수 있다. 개발자가 아닌 스프링이 A가 사용할 객체를 생성해 의존 관계를 맺어주는 것을 IoC(제어의 역전)이라고 하며, 그 과정에서 C를 A의 생성자를 통해 주입해주는 것을 DI(의존성 주입)이라고 한다.

<br/>
<br/>

### AOP (Aspect Oriented Programming, 관심 지향 프로그래밍)
애플리케이션 개발할 때 구현해야 하는 기능들은 크게 공통 관심 사항과 핵심 관심 사항으로 분류할 수 있다. 공통 관심 사항은 모든 핵심 관심 사항에 공통적으로 적용되는 관심 사항들을 의미하며, 두 코드가 모여 있으면 필연적으로 공통 관심 사항과 관련된 코드가 중복될 수밖에 없다. 공통 관심 사항을 수행하는 로직이 변경되면 모든 중복 코드를 수정해야 한다. 애플리케이션 전반에 걸쳐 적용되는 공통 기능을 비즈니스 로직으로부터 분리해내는 것을 AOP라고 한다.

<br/>
<br/>

### PSA (Portable Service Abstraction, 일관된 서비스 추상화)
동일한 사용 방법을 유지한 채로 데이터베이스를 바꿀 수 있다. 스프링이 데이터베이스 서비스를 추상화한 인터페이스를 제공해주기 때문에 가능하다. 스프링은 Java를 사용하여 데이터베이스에 접근하는 방법을 규정한 인터페이스를 제공하며 이를 JDBC(Java Database Connectivity)라고 한다. 
  
<br/>
<br/>

### 트랜잭션의 지원
데이터베이스 연동하여 사용할 때 신경써야 하는 부분이며 상황에 따라 여러 코드를 작성할 필요가 있는데, 스프링에서는 Annotation이나 XML로 설정할 수 있다.

<br/>
<br/>

### 단위 테스트
전체 애플리케이션을 실행하지 않아도  Annotation을 활용하여 기능별 단위 테스트가 용이하다.

<br/>

<hr/>

### References
[https://velog.io/@neity16/HTTP-HTTP-버전-별-특징](https://velog.io/@neity16/HTTP-HTTP-%EB%B2%84%EC%A0%84-%EB%B3%84-%ED%8A%B9%EC%A7%95)

[https://aws.amazon.com/ko/what-is/api/](https://aws.amazon.com/ko/what-is/api/)

[https://www.codestates.com/blog/content/스프링-스프링부트](https://www.codestates.com/blog/content/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)