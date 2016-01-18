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


## 서블릿 생명주기


## 리다이렉트

## 서블릿 초기화 매개변수

## 컨텍스트 초기화 매개변수

## 필터

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
