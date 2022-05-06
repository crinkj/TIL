> # JWT(JSON Web Token)
- JWT를 알기전에 나오게 된 배경을 이해하기 위해선 간략하게 SESSION/COOKIE 의 기본 개념을 알 필요가 있다.

- HTTP의 큰 특징중 두개가 `stateless(무상태)`와 `connectionless(비연결성)` 이다. 
    * `connectionless(비연결성)`이란 클라이언트가 서버에 요청을 하고 서버가 클라이언트에게 응답을 보내면 접속을 끊는다. 끊음으로써 서버에 부담도 줄이며 자원을 더 효율적이게 사용할 수 있다. 
    * `stateless(무상태)` 란 서버가 클라이언트의 이전 상태를 보전하지 않는다는 것이다. 밑에 보기와 같이 CASE 2 에 해당한다. stateful로 하는경우 이전 상태들까지 보전하므로 서버에 부담이 간다. 그래서 인증된 사용자인경우에는 stateless 상태로 인증된 유저인지 클라이언트와 서버간 인증된 사용자라는 내용만 주고받으면 된다.
<br>

- JWT 와 SESSION/COOKIE는 클라이언트가 APPLICATION을 사용하기위해 http 통신을 통해 서버와 인증(Authorized)된 클라이언트인지 확인하기 위해 통신을 하기 위한 개념으로 필요하다.

```
- 배경 롯데리아

CASE 1) STATEFUL
* 점원: 어떤 햄버거 준비 해드릴까요?
* 고객: 불고기버거 주세요.
* 점원: 세트로 드릴까요? 
* 고객: 네
* 점원: 불고기버거 세트로 준비하겠습니다.

CASE 2) STATELESS
* 점원: 어떤 햄버거 준비해드릴까요?
* 고객: 불고기버거 주세요.
* 점원: 세트로 드릴까요?
* 고객: 네 
* 점원: 어떤 햄버거 준비해드릴까요?
```

 > SESSION(서버측)

 * 세션이란? 
    * 클라이언트가 접속 후 사용끝날때까지의 시간이다. 접속을 하고 인증(로그인)이 되었을시 `SESSION ID`가 발급된다 이 `SESSION ID`는 어플리케이션을 이용할 때마다(e.g., clicking a link) HTTP requests를 통해 전달되며 인증을 확인한다.
   <p align="center">
  <img src="https://hazelcast.com/wp-content/uploads/2021/12/diagram-Web-Sessions.png" alt="DispatcherServlet image"/>
  </p>
 <em> * 세션 동작 과정 </em>
 
 1. 클라이언트가 로그인을 한다.
 2. 서버에서 인증이 되었을시 세션을 생성하고 `SESSION ID`를 클라이언트한테 반환한다.
 3. 클라이언트가 request를 서버에 요청할때마다 항상 `SESSION ID`도 같이 전달한다.
 4. `SESSION ID`를 서버에 전달한다. 
 5. 서버에서 사용자의 전달받은 `SESSION ID`에 맞는 response를 클라이언트에게 응답한다.

- 장점: 
   * 보안성이 좋다
   * 큰 용량이 허용
   * SESSION ID만 보내서, 세션의 크기가 커도 네트워크 부하가 없다.
- 단점: 
   *  클라이언트가 많아질수록 서버에 세션을 저장하기 때문에 서버에 부담이 간다.
 
- 사용 예시:
   * 로그인 같이 보안이 중요하며 외부에 사용자의 정보가 노출이 되야하지 않을때 사용한다.
    
 > COOKIE(클라이언트측)
 * 쿠키란?
 
    * 쿠키란 서버에 전달받은 작은 단위에 데이터 파일이며 클라이언트측(사용자 컴퓨터)에 저장되어있다. 쿠키는 사용자가 따로 요청하지 않아도 브라우저가 Request시에 Request Header를 넣어서 자동으로 서버에 전송을하면



> references <br>
 * https://hazelcast.com/glossary/web-session/ 
* https://server-engineer.tistory.com/152