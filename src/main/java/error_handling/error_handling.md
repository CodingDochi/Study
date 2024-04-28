# Error와 Exception

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBNb5E%2Fbtrtajk25fA%2FZv1TvZrEdCUHluk0s9bRkK%2Fimg.png)

오류는 처리할 수 없는 심각한 오류를 나타내는 클래스입니다. 실행 중에 throw되었지만 catch되지 않은 오류에 대해 Error 또는 해당 하위 클래스에 대해 throw 절을 선언할 필요가 없습니다. 오류는 개발자가 처리 할 수 없는 경우입니다.

예외는 프로그램이 복구할 수 있고 응용 프로그램에서 개발자가 처리해야 하는 문제를 나타냅니다. 예외에는 두 가지 하위 유형이 있습니다.
컴파일 타임에 확인되는 이러한 예외를 Checked Exception, 그렇지 않은 에러를 Unchecked Exception 또는 RunTimeException이라고 합니다.

# 스프링에서 Request의 처리과정과 Exception 발생 위치
![](https://velog.velcdn.com/images/dabeen-jung/post/f9f75139-9108-4825-ba7b-3c51001d5e3f/image.png)
![](https://junhyunny.github.io/images/filter-interceptor-and-aop-1.JPG)
![](https://blog.kakaocdn.net/dn/94JVG/btrBauLFxVM/owFnRzkmcD670314lYJKL0/img.png)
![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fgithub.com%2Fbinghe819%2FTIL%2Fraw%2Fmaster%2FSpring%2F%25EA%25B8%25B0%25ED%2583%2580%2Fimage%2Fexception-handling-description-target.png&blockId=d5635617-9be0-4a56-966c-0d99c1657127****)

요청이 들어오게 되면 request > Filter > Servlet > Interceptor > AOP > Controller순으로 실행됩니다.
DisptacherServlet, Interceptor, AOP, Controller 는 Spring Context 에 속하고, Filter 의 경우에는 Web Context 에 속합니다.

스프링의 처리과정을 보면 예외가 발생하는 부분은 크게 두가지로 나눌 수 있다.
1. Dispatcher Servlet내에서 발생하는 예외 (Controller, Service, Repository등등)
2. Dispatcher Servlet전의 서블릿 (Filter)에서 발생하는 예외

## Filter (Dispatcher Servlet 이전)
- web.xml 에서 error-page 등록, 사용자에게 에러 응답
- Filter 내부 예외 처리를 위한 Filter 생성, try-catch문을 사용하여 예외 처리
- Filter 내부 try-catch문에서 발생한 예외를 HandlerExceptionResolver를 빈으로 주입받아 @ExceptionHandler에서 처리

## Dispatcher Servlet 내부에서 HandlerExceptionResolver의 예외 처리
- Controller Level: @ExceptionHandler
  @ExceptionHandler애노테이션을 통해 Controller의 메서드에서 throw된 Exception에 대한 공통적인 처리를 할 수 있다.

- Global Level: @ControllerAdvice
  @ControllerAdvice는 모든 Controller에서 발생하는 예외를 처리할 수 있게 해주는 애노테이션이다. DispatcherServlet에서 발생하는 예외를 전역적으로 처리해준다. DispatcherServlet에서 발생하는 예외만 처리할 수 있고 Filter에서 발생하는 예외는 따로 처리를 하지 않으면 처리가 불가능하다.
  - Q. Controller의 @ExceptionHandler와 ControllerAdvice의 @ExceptionHandler중 높은 우선순위는?
  - A. Controller의 @ExceptionHandler가 먼저다.

- Method Level: try/catch


### Dispatcher Servlet
디스패처 서블릿은 서블릿 컨테이너 제일 앞단에서, 클라이언트에서 서버로 오는 모든 요청을 받아 처리하는 프론트 컨트롤러입니다. 디스패치 서블릿은 공통적인 작업을 먼저 처리하고, 해당 요청을 처리해야 하는 컨트롤러를 찾아서 작업을 위임합니다.
 
HandlerMapping 의 구현체 중 하나인 RequestMappingHandlerMapping 은 모든 컨트롤러 빈을 파싱하여 HashMap 으로 관리합니다. 그래서 요청이 오게 되면 HandlerMapping 은 Http Method, URI 등을 사용해 요청 정보를 객체로 만들고, key 로 사용해 요청을 처리할 **HandlerMethod** 를 찾고, HandlerMethodExecutionChain 으로 감싸서 반환합니다.

### Interceptor
인터셉터는 스프링 프레임워크에서 제공하는 기능으로, 스프링 MVC에서 주로 사용됩니다.
스프링의 HandlerInterceptor 인터페이스를 구현하여 작성됩니다.

### AOP

### Controller



