## :heavy_check_mark: Spring MVC (Bean 객체와 싱글톤 디자인 패턴)

### MVC란?

* MVC 패턴은 애플리케이션을 개발할 때 사용하는 디자인 패턴이다. 개발 영역을 Model, View, Controller로 구분하여 각 역할에 맞는 코드를 작성한다. UI 영역과 business logic 영역을 구분하여 개발 및 유지보수의 효율성 향상되었다.
* 처음 요청이 들어오면 DispatcherServlet(Front Controller)이 적절한 Controller를 호출한다. 호출된 **Controller**는 적절한 Model 클래스를 선정해 위임한다. **Model**에서 business logic 및 data 저장 관리 (DB 접속 및 처리)를 수행하게 하고, 다시 **Controller**가 적절한 **View**로 최종 결과 화면을 forwarding한다.
    - Model : business logic 및 data 저장 관리 수행
    - View : presentation logic 구현
    - Controller : Model과 View 사이의 실행 흐름 제어

### Spring MVC란?

* MVC 패턴은 유지보수 하기에 좋은 패턴이지만 구조가 복잡해지는 한계가 있고, 이를 극복하게 해주는 것이 Spring MVC이다.
* 처음 요청이 들어오면 **DispatcherServlet**(Front Controller)이 HandlerMapping(Controller Mapping) 클래스를 통해 적절한 Controller를 결정한다. 그 결정에 따른 Controller가 호출되고, **Controller**는 DispatcherServlet가 전달한 HTTP 요청을 처리하고, 그 결과를 **Model**에 저장한다. 그리고 **ModelAndView**를 DispatcherServlet에 전달한다. DispatcherServlet은 **View Resolver**를 통해 Model에 저장된 데이터를 사용해 **View**를 확보하고 이를 클라이언트에 응답한다.

<br/>

### Spring Container란?

* Spring은 Spring Container를 통해 객체를 관리하고 이 객체를 **Bean**이라고 한다.
* BeanFactory
    - 객체를 생성하고, 객체 사이의 런타임 의존관계를 맺어주는 역할을 하는 Spring Container의 최상위 인터페이스이다.
* ApplicationContext
    - BeanFactory를 포함한 여러 인터페이스들을 상속받은 인터페이스로, Spring Container라고 하면 일반적으로 ApplicationContext를 의미한다.
* 개발자를 대신해 객체(Bean)를 생성, 관리, 제거하며 생명주기를 관리하며 생성된 인스턴스들에게 추가 기능을 제공한다.
* Container가 Bean을 관리해주기 때문에 개발자는 모듈 간 의존 및 결합으로 인해 발생하는 문제로부터 자유로워졌다. 독립적인 코드에 **Annotation**만 붙이면 개발자가 원하는 상황에 매개변수를 설정해 메소드를 호출한다.

### Bean이란?

* Spring IoC Container가 관리하는 자바 객체를 말한다.
    - IoC가 적용된 경우, 객체의 생성을 특별한 관리 위임 주체에게 맡긴다. 이 경우 사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 다른 주체가 된다. 사용자의 제어권을 다른 주체에게 넘기는 것이 IoC이다.
* 기존 Java Programming에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용했다. 하지만 Spring에서는 직접 new를 사용하여 생성한 객체가 아닌 Spring에 이해 관리당하는 객체를 사용한다. 이렇게 **Spring에 의해 생성되고 관리되는 자바 객체**를 **Bean**이라고 한다. Spring Framework에서는 Spring Bean을 얻기 위해 ApplicationContext.getBean()와 같은 메소드를 사용해 Spring에서 직접 자바 객체를 얻어서 사용한다.
* Spring Bean을 등록하기 위해 @Component Annotation을 사용하거나 @Bean과 @Configuration Annotation을 사용한다.

<br/>

### 싱글톤 디자인 패턴이란?

* **객체의 인스턴스가 오직 한 개만 생성**되는 패턴이다.
* 장점
    - 최초 한 번의 new 연산자를 통해서 고정된 메모리 영역을 사용하기 때문에 메모리 낭비가 방지되며 이미 생성된 인스턴스를 활용하니 속도 측면에서도 이점이 있다.
    - 다른 클래스 간 데이터 공유가 쉽다. 전역으로 사용되는 인스턴스이므로 다른 클래스의 인스턴스들이 접근하여 사용 가능하다. 하지만 동시성 문제가 발생할 수 있어 주의해야 한다.
* 단점
    - 싱글톤 패턴을 구현하는 코드 자체가 많이 필요하다.
    - 테스트하기가 어렵다. 전역에서 상태를 공유하고 있기에 완벽히 격리된 환경에서 테스트가 수행되려면 매번 인스턴스의 상태를 초기화시켜야 한다.
    - 클라이언트가 특정 클래스에 의존하게 된다. DIP를 위반하게 되고 OCP도 위반할 가능성이 높다.
        - DIP (Dependency Inversion Principle, 의존성 역전 원칙)
        - OCP (Open Closed Principle, 개방 폐쇄의 원칙)
    - 자식 클래스를 만들 수 없다.
    - 내부 상태를 변경하기 어렵다.
* 문제점들이 많아 싱글톤 패턴을 단독으로 사용한다면 객체 지향에 위반되는 사례가 많다. Spring Container의 도움을 받으면 싱글톤 패턴의 문제점들을 보완하며 이점을 누릴 수 있다.

### Spring 싱글톤 패턴

* 클래스 자체에 의해서가 아니라 **Spring Container에 의해 구현**된다. 특정 클래스에 대해 @Bean Annotation이 정의되면 Spring Container는 해당 클래스에 대해 단 한 개의 인스턴스를 만든다. 이 공유 인스턴스는 설정 정보에 의해 관리되고, Bean이 호출될 때마다 Spring은 생성된 공유 인스턴스를 반환한다.
* Spring Container가 구현하는 싱글톤 패턴은 Tread Safe가 자동으로 보장된다.
    - Tread Safe - 멀티 스레드 프로그래밍에서 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다.

### 싱글톤 패턴 사용 주의점

* 여러 클라이언트가 하나의 인스턴스를 공유하기 때문에 상태를 유지(stateful)하게 설계하면 안된다.
    - 무상태(stateless)로 설계해야 한다.
    - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
    - Spring Bean 필드에 공유 값을 설정하면 안된다. @Bean만 사용하면 Spring Bean으로 등록되지만 싱글톤 패턴을 보장하진 않는다. @Configuration을 사용하면 싱글톤 패턴을 보장하는 Spring Bean으로 등록된다.

<hr/>

### References
[https://code-lab1.tistory.com/258](https://code-lab1.tistory.com/258)

[https://dev-aiden.com/spring/Spring-Container/#21-beanfactory](https://dev-aiden.com/spring/Spring-Container/#21-beanfactory)

[https://ibocon.tistory.com/122](https://ibocon.tistory.com/122)

[https://melonicedlatte.com/2021/07/11/232800.html](https://melonicedlatte.com/2021/07/11/232800.html)

[https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)

[https://gem1n1.tistory.com/96](https://gem1n1.tistory.com/96)

[https://bangu4.tistory.com/286](https://bangu4.tistory.com/286)