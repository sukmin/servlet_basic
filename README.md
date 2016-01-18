# servlet_basic

## HTTP
- HTTP란? 프로토콜
- 프로토콜이란? 규약
- 규약이란? 약속
- 그럼 HTTP란? 정해진 약속대로 요청을 보내면 정해진 약속대로 응답을 주겠다.
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

| 메소드 | 역할 |
| --- | --- |
| --- | --- || --- | --- |

## 서블릿이란?

## 웹 프로젝트 폴더 구조

## web.xml

## 제네릭서블릿

## 서블릿 생명주기

## HTTP 서블릿

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
