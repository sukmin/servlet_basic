# servlet_basic

## HTTP
- HTTP란?    프로토콜
- 프로토콜이란?    규약
- 규약이란?    약속
- 그럼 HTTP란?    사전에 협의한대로 요청을 보내면, 사전에 협의한대로 응답을 주겠다는 약속
```
public static void main(String[] args) {

		String host = "www.naver.com";
		int port = 80;

		try {
			Socket socket = new Socket(host, port);
			OutputStream out = socket.getOutputStream();
			PrintStream printer = new PrintStream(out);

			StringBuilder builder = new StringBuilder();

			builder.append("GET / HTTP/1.1\r\n");
			builder.append("Host: www.naver.com\r\n");
			builder.append("Connection: keep-alive\r\n");
			builder.append("Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8\r\n");

			printer.println(builder.toString());

			printer.flush();

			BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			String line = in.readLine();
			while (line != null) {
				System.out.println(line);
				line = in.readLine();
			}

			printer.close();
			in.close();
			socket.close();

		} catch (UnknownHostException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}
```

- 참고 : https://ko.wikipedia.org/wiki/HTTP


## 서블릿이란?
- 1997년 Sun사에서 제안한, 자바 웹서비스를 위한 기본인터페이스.
- javax.servlet.Servlet 인터페이스를 구현한 것이 서블릿.
- 일반적인 자바 프로그램은 public static void main(String [] args){...} 메소드로 실행되지만, 서블릿은 서블릿 컨테이너에 등록된 다음 서블릿 컨테이너에 의해 생성, 호출, 소멸됨.

## 서블릿 컨테이너란?
- 개발자가 구현한 서블릿을 실행시키는 프로그램.
- 네트워크 및 쓰레드 프로그래밍은 까다롭다. 까다로운 네트워크, 쓰레드 처리를 서블릿 컨테이너가 알아서 처리해주고 개발자는 비즈니스 개발에 전념할 수 있음.
- 하나의 요청마다 하나의 쓰레드를 쓰레드풀에서 꺼내와 할당해줌.
- 서블릿 컨테이너로 톰캣, 제티, 글래스피시 등이 있음. 가장 대중적인 것은 톰캣.

## GenericServlet이란?
- 서블릿을 구현하면서 항상 구현하는 뻔한 것들을 미리 어느정도 구현해놓은 것.
- 개발자는 service(...)메소드만 구현하면 됨.

## HttpServlet이란?
- HTTP 메소드에 대응하도록 GenericServlet을 좀 더 추상화 시킨 것

## web.xml
- 어떤 서블릿이 있는지 서블릿 컨테이너에게 알려주는 설명서.
- 서블릿, 리스너, 필터 등을 설정한다.

## 정적자원 디폴트서블릿에서 처리하도록 설정
- 서블릿 컨테이너는 정적자원을 처리하기 위해 디폴트서블릿을 가지고 있다.
- web.xml에 설정
```
<servlet-mapping>
	<servlet-name>default</servlet-name>
	<url-pattern>*.css</url-pattern>
	<url-pattern>*.html</url-pattern>
	<url-pattern>*.js</url-pattern>
	<url-pattern>*.ico</url-pattern>
</servlet-mapping>
```

## 서블릿 생명주기
- init() : 서블릿 컨테이너가 서블릿을 생성한 후 초기화 작업을 수행하기 위해 호출
- service() : 클라이언트의 요청이 있을때마다 호출
- destory() : 서블릿 컨테이너가 종료되거나 해당 서블릿을 비활성화 시킬때 호출

## 리다이렉트
- 리다이렉트는 사용자의 응답주소를 다른 URL로 돌릴 때 사용한다.
- HttpServletResponse 의 sendRedirect 메소드
```
response.sendRedirect(리다이렉트보낼주소);
```

## 서블릿 초기화 매개변수
- 서블릿컨테이너가 init()시 서블릿에 전달하는 데이터
- 보통 외부요인으로 고정되어 있는 값을 넣을때 사용 (web.xml만 변경하면 되므로 재빌드가 필요없음)
- 변수설정(web.xml)
```
	<servlet>
		<servlet-name>test</servlet-name>
		<servlet-class>com.naver.school.TestServlet</servlet-class>
		<init-param>
			<param-name>dog</param-name>
			<param-value>멍멍</param-value>
		</init-param>
		<init-param>
			<param-name>duck</param-name>
			<param-value>꽥꽥</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>test</servlet-name>
		<url-pattern>/test</url-pattern>
	</servlet-mapping>
```

- 변수사용(Servlet)
```
public class TestServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		System.out.println(this.getInitParameter("dog")); // 멍멍
		System.out.println(this.getInitParameter("duck")); // 꽥꽥
		
	}

}
```

## 컨텍스트 초기화 매개변수
- 서블릿 초기화 매개변수처럼 변수인데, 서블릿 하나에서만 쓸수있는 것이 아니고 모든 서블릿에서 사용가능한 변수
- 변수설정(web.xml)
```
	<context-param>
		<param-name>good</param-name>
		<param-value>아르타니스</param-value>
	</context-param>
	
	<context-param>
		<param-name>bad</param-name>
		<param-value>아몬</param-value>
	</context-param>
```

- 변수사용(Servlet)
```
public class TestServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		ServletContext sc = this.getServletContext();
		System.out.println(sc.getInitParameter("good")); // 아르타니스
		System.out.println(sc.getInitParameter("bad")); // 아몬
		
	}

}
```

## 필터
- 서블릿의 요청 전, 응답 후 어떤 작업을 할 수 있는 것이 필터
- filterChain.doFilter() 호출을 기준으로 호출전에 작업하면 서블릿을 타기 전, 호출후에 작업하면 서블릿을 탄 후 해당 코드가 실행.
- 코드
```
public class TestFilter implements Filter {
	
	private FilterConfig config;

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// 서블릿의 init과 같은 역할
		// 필터 초기화 매개변수를 사용한다면 filterConfig를 저장하여 사용
		this.config = filterConfig;
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain)
			throws IOException, ServletException {
		
		// 들어올때 한번, 나갈때 한번 총 2번 호출된다.
		
		System.out.println("들어올때 " + config.getInitParameter("pig"));	// 서블릿 타기 전에 실행할 코드 작성
		filterChain.doFilter(request, response); // 다음번 필터 호출
		System.out.println("나갈때 " + config.getInitParameter("pig")); // 서블릿 탄 후 실행할 코드 작성
		
	}

	@Override
	public void destroy() {
		// 서블릿의 destroy과 같은 역할
	}

}
```

- 설정(web.xml)
```
	<filter>
		<filter-name>testFilter</filter-name>
		<filter-class>com.naver.school.TestFilter</filter-class>
		<init-param>
			<param-name>pig</param-name>
			<param-value>꿀꿀</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>testFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```
- 필터는 순서에 의존적임. 예를 들어 web.xml에 필터1, 필터2, 필터3 순으로 등록했다고 하면 아래와 같이 동작
```
요청 -> 필터1 -> 필터2 ->필터3 -> 서블릿실행 -> 필터3 -> 필터2 -> 필터1
```

## MVC 아키텍쳐

## 포워드&인클루드

## 서블릿컨텍스트

## 서블릿컨텍스트리스너

## 추천서적
| 제목 | 주소 |
| --- | --- |
| Head First Servlets & JSP | http://www.yes24.com/24/Goods/3301415 |
| 열혈강의 자바 웹 개발 워크북 | http://www.yes24.com/24/goods/13159413 |
| 자바 웹 개발 완벽 가이드 | http://www.yes24.com/24/goods/16519488 |
| HTTP 완벽 가이드 | http://www.yes24.com/24/goods/15381085 |
| 웹 프로그래머를 위한 서블릿 컨테이너의 이해 | http://www.hanbit.co.kr/ebook/look.html?isbn=9788979149685 |
