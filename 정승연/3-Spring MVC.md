# servlet

Spring 에서 HTTP 통신 기반 클래스를 살펴보면 모두 Servlet 클래스가 활용되는 것을 볼 수 있다. 

### Servlet이란

웹 프로그래밍에서 클라이언트 요청을 처리하고 처리 결과를 클라이언트에 전송하는 기술이다. 

### 서블릿 객체는 싱글톤으로 관리한다.

- 고객의 요청이 올 때마다 계속 객체를 사용하는 것은 비효율.
- 최초 로딩 시점에 서블릿 객체 미리 만들어두고 활용.
- 모든 고객 요청은 동일한 서블릿 객체 인스턴스에 접근

### 서블릿은 Java Thread를 통해 동작한다.

요청마다 스레드를 생성해 다중 요청에 다중 스레드 사용

- 장점
    - 동시 요청 처리 가능
    - 리소스 (CPU,메모리)가 허용 할 때까지 처리 가능
    - 하나의 스레드가 지연되어도 나머지는 정상 동작
- 단점
    - 스레드 생성 비용이 비쌈
        - 매번 스레드 생성하면 응답속도가 느려짐
    - 스레드 context switching 비용이 발생
    - 스레드 생성에 제한이 없다.
        - 요청이 많이 오면 cpu,memory 임계점을 넘어 서버가 죽을 수도 있음

### → 이를 보완하기 위한 Thread Pool

### Thread Pool

필요한 스레드를 스레드 풀에 보관하고 관리한다. 스레드 풀에 생성 가능한 스레드의 최대치를 관리한다.

스레드가 필요하면 이미 생성되어있는 스레드를 스레드 풀에서 꺼내 사용한다. 사용을 종료하면 해당 스레드 풀에 스레드를 반납한다. 스레드 풀에 스레드가 없으면 기다리는 요청은 거절하거나 특정 숫자만큼 대기하도록 한다.

스레드가 미리 생성되어있으므로 스레드를 생성하고 종류하는 비용이 절약되고 응답시간이 빠르다. 생성가능한 스레드의 최대치가 있으므로 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리 가능하다.

- 스레드 풀을 너무 낮게 설정하면
    - 동시 요청이 많을 때 서버 리소스는 여유롭지만 클라이언트는 응답 지연
- 스레드 풀을 너무 높게 설정하면
    - 동시 요청이 많을 때 CPU,메모리 리소스 임계점 초과로 서버 다운
- 장애 발생 시
    - 클라우드면 일단 서버 늘리고 이후 튜닝
    - 클라우드가 아니면 열심히 튜닝

### WAS-서블릿컨테이너 의 멀티스레드 지원

톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고 한다. 

멀티 스레드에대한 부분은 WAS가 처리, 개발자가 스레드 관련 코드를 신경쓰지 않아도 됨. 멀티 스레드 환경이므로 싱글톤 객체(서블릿, 스프링 빈)은 주의하여 사용

사용자가 URL을 입력하면 요청이 서블릿 컨테이너로 전송된다.

요청을 전송 받은 서블릿 컨테이너는 HttpServletRequest, HttpServletResponse 객체를 생성한다.

### HttpServletRequest의 역할

Http 요청 메시지를 편리하게 사용할 수 있도록 파싱해준다. 그리고 그결과를 HttpServletRequest 객체에 담아서 제공한다.

- 임시저장소 기능
    
    해당 HTTP 요청이 시작부터 끝날 때까지 유지되는 임시 저장소
    
    저장 : request.setAttribute(name,value)
    
    조회 : request.getAttribute(name)
    
- 세션 관리 기능
    
    request.getSession(create:true)
    

### HttpServletRequest-기본 사용법

HTTP 요청 데이터 - POST HTML Form

- content-type : application/x-www-form-urlencoded
- message body : username=hello&age=20

```java
public class RequestBodyJsonServlet extends HttpServlet{
	private ObjectMapper objectMapper = new ObjectMapper();

@Override
protected void service(HttpServletRequest request, HttpServlet
	throws ServletException, IOException{

	ServletInputSteam inputSteam = request.getInputStream();
	String messageBody = SteamUtils.copyToString(inputStream,StandardCharsets.UTF_8);

	HelloData hellodata = objectMapper.readValue(messageBody,HelloData.class);
	}
}
```

```java
public class ResponseJsonServlet extends HttpServlet {
	private ObjectMapper objectMapper = new ObjectMapper();

@Override
protected void service(HttpServletRequest request, HttpServletResponse response)
	throws ServletException, IOException{

	response.setHeader("content-type","application/json");
	response.seCharacterEncoding("utf-8");

	HelloData data = new HelloData();
	data.setUsername("kim");
	data.setAge(20);

	String result = objectMapper.writeValueAsString(data);

	response.getWriter().write(result);
	}
}
```

### Model, View, Controller

- Controller : HTTP 요청을 받아 파라미터를 검증하고 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 결과 데이터를 조회해 모델에 담는다.
    - Service : 비즈니스 로직은 서비스 계층을 별도로 만들어 처리한다.
- Model : 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아 전달하기때문에 화면 렌더링 하는 일에 집중할 수 있다.
- View : 여기서는 HTML

### DispatchServlet == FrontController

- 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
- 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출
- 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨

FrontController와 SubController를 살펴보면, 각 SubController에  다음과 같은 공통된 부분이 존재한다.

```java
String viewPath = "web-inf/view/new-form.jsp";
RequestDispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request,response);
```

이 부분은 컨트롤러에서 뷰로 이동하는 부분으로서 forward 로직을 수행하면 JSP 가 실행된다. 이를 MyView 의 render로 치환하여 사용하고, 이는 각 컨트롤러가 MyView 객체를 생성만해서 반환하면 되는 로직으로 변하게 해준다.

여기서 더 나아가, 컨트롤러는 서블릿 기술을 몰라도 동작할 수 있다. 

![스크린샷 2023-03-27 오후 10.27.09.png](servlet%2076e248e733a04ee9a35b3b4e3fe97f74/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_10.27.09.png)

### Adapter

어댑터 패턴을 사용해서 프론트 컨트롤러가 다양한 방식의 컨트롤러를 처리할 수 있도록 변경해보자.

![스크린샷 2023-03-28 오후 12.21.46.png](servlet%2076e248e733a04ee9a35b3b4e3fe97f74/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-28_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_12.21.46.png)

- 핸들러 어댑터 덕분에 다양한 종류의 컨트롤러를 호출할 수 있다.
- 핸들러(컨트롤러) 컨트롤러보다 넓은 범위인 핸들러로 변경했다. 어댑터가 있기 때문에 컨트롤러의 개념뿐만 아니라 어떠한 것이든 다 처리 가능하기 때문이다.

핸들러 어댑터는 다음과 같이 구성된다. 

```java
public interface MyHandlerAdapter {
      boolean supports(Object handler);
      ModelView handle(HttpServletRequest request, HttpServletResponse response,
  Object handler) throws ServletException, IOException;
  }
```

- supports(Object handler)
    
    handler는 컨트롤러를 말하며, 어댑터가 해당 컨트롤러를 처리할 수 있는지 판단한다.
    
    instance of ControllerV?를 통해
    
- ModelView handle(request,response,handler)
    - 어댑터는 컨트롤러를 호출하고 그 결과로 ModelView를 반환해야한다. 실제 컨트롤러가 ModelView를 반환하지 못하면 어댑터가 대신 생성해서라도 반환해야한다.

### DispatcherServlet

스프링 MVC도 FrontController 구조로 되어있다. 스프링의 FrontController가 DispatcherServlet이다. ~~그리고 이 DispatcherServlet이 스프링 MVC의 핵심이다.~~

DispatcherServlet도 부모 클래스에서 HttpServle을 상속받아 사용하고 Servlet으로 동작한다. 

- DispatcherServlet → FrameworkServlet → HttpServletBean → HttpServlet

스프링부트가 DispatcherServlet을 Servlet을 자동으로 등록하면서 모든 경로에 대해 매핑한다. (urlPatterns=”/”)

### Request Flow

1. Servlet 호출되면 HttpServlet이 제공하는 service()가 호출된다.
2. 스프링 MVC는 DispatcherServlet의 부모인 FrameworkServlet에서 service()를 오버라이드 하고, DispatcherServlet.doDispatch()가 호출된다.
3. DispatcherServlet.`doDispatch()`에서는
    1. getHandler(processedRequest) // 핸들러 조회
    2. ha = getHanderAdapter(mappedHandler.getHandler()) // 핸들러 어댑터 조회
    3. mv = ha.handle(request,response,handler.gethandler()) 
    // 핸들러 어댑터 실행 → 어댑터로 핸들러 실행 → modelAndView 리턴
    4. render(mv,request,response) // 뷰 렌더링 호출
    5. resolveViewName(viewName,mv.getModelInternal(),…) 
    // 뷰 리졸버로 뷰 찾기 → View 반환
- **동작 순서**
    1. **핸들러 조회**: 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
    2. **핸들러 어댑터 조회**: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
    3. **핸들러 어댑터 실행**: 핸들러 어댑터를 실행한다.
    4. **핸들러 실행**: 핸들러 어댑터가 실제 핸들러를 실행한다.
    5. **ModelAndView 반환** : 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 **변환**해서반환한다.
    6. **viewResolver 호출** : 뷰 리졸버를 찾고 실행한다.
    JSP의 경우: InternalResourceViewResolver 가 자동 등록되고, 사용된다.
    7. **View반환** : 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
    JSP의 경우 InternalResourceView(JstlView) 를 반환하는데, 내부에 forward() 로직이 있다.
    8. **뷰렌더링** : 뷰를 통해서 뷰를 렌더링한다.

그래도 이렇게 핵심 동작방식을 알아두어야 향후 문제가 발생했을 때 어떤 부분에서 문제가 발생했는지 쉽게 파악하고, 문제를 해결할 수 있다. 그리고 확장 포인트가 필요할 때, 어떤 부분을 확장해야 할지 감을 잡을 수 있다.

**핸들러 매핑으로 핸들러 조회 → 핸들러 어댑터 조회 → 핸들러 어댑터 실행 → 핸들러 실행**