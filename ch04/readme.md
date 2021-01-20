http 모듈로 서버 만들기
=======================
# 1. 요청과 응답 이해하기
## 1-1. 서버와 클라이언트
* 노드는 자바스크립트 실행기이고 서버가 아니다.
* 자바스크립트로 서버로 돌릴수있는 코드를 작성하면 서버를 실행해준다.
* 서버와 클라이언트의 관계
    * 클라이언트가 서버로 요청을 보낸다.
    * 서버는 요청을 처리한다.
    * 처리 후에 클라이언트로 응답을 보낸다.
## 1-2 노드로 http 서버 만들기
* http 요청에 응답하는 노드 서버
    * createServer로 요청 이벤트에 대기한다
    * req 객체는 요청에 관한 정보가, res 객체는 응답에 관한 정보가 담겨 있다.
* 서버 코드 수정 후에는 서버를 껏다 키자.
* 서버는 동시에  실행이 가능하다.
* server1.js 참고
```js
const http = require('http');

http.createServer((req, res) => {
    // 여기에 어떻게 응답할지 적는다.
});
```
# 2. REST와 라우팅 사용하기
## 2-1. REST API
* 서버에 요청을 보낼 때는 주소를 통해 요청의 내용을 표현
    * /index.html이면 index.html을 보내달라는 뜻이다.
    * 항상 html을 요구할 필요는 없다
    * 서버가 이해하기 쉬운 주소가 좋다.
* REST API(Representational State Transfer)
    * 서버의 자원을 정의하고 자원에 대한 주소를 지정하는 방법
    * /user이면 사용자 정보에 관한 정보를 요청하는 것
    * /post면 게시글에 관련된 자원을 요청하는 것
* HTTP 요청 메서드
    * GET : 서버 자원을 가져오려고 할 때 사용한다.
    * POST : 서버에 자원을 새로 등록하고자 할 때 사용한다.
    * PUT : 서버의 자원을 요청에 들어있는 자원으로 치환하고자할 때 사용한다. (전체수정)
    * PATCH : 서버 자원의 일부만 수정하고자 할 떄 사용 (부분수정)
    * DELETE : 서버의 자원을 삭제하고자할 때 사용

## 2-2. HTTP 프로토콜
* 클라이언트가 누구든 서버와 HTTP 프로토콜로 소통 가능
    * iOS, 안드로이드, 웹이 모두 같은 주소로 요청을 보낼 수 있다.
    * 서버와 클라이언트의 분리
* RESTful
    * REST API를 사용한 주소 체계를 이용하는 서버이다.

## 2-3. REST API 서버 만들기
## 2-4. POST, PUT, DELETE 요청 보내기
*  restServer.js 참조
* f12(개발자도구) Network 탭에서 요청 내용을 실시간으로 확인 할 수 있다.

# 3. 쿠키와 세션 이해하기
## 3-1. 쿠키의 필요성
* 요청에는 한가지 단점이 있다.
    * 누가 요청을 보냈는지 모른다(IP주소와 브라우저 정보 정도만 안다.)
    * 로그인을 구현하면 된다.
    * 쿠키와 세션이 필요하다.
* 쿠키 : 키=값의 쌍
    * ex) name=SY
    * 매 요청마다 서버에 동봉해서 보낸다.
    * 서버는 쿠키를 읽어 누구인지 파악한다.
```js
const http = require('http');

http.createServer((req, res) => {
    console.log(req.url, req.headers.cookie);
    res.writeHead(200, {'Set-Cookie' : 'mycookie=test'});
    res.end('Hello Cookie');
})
    .listen(8083, () => {
        console.log('8083번 포트에서 서버 대기 중입니다');
    });
```
* f12 -> Application 탭 -> Cokkies 에서 쿠키들 확인 가능
* 브라우저 끈 다음에는 쿠키가 사라진다.
## 3-2. 헤더와 본문
* http 요청과 응답은 헤더와 본문을 가진다.
    * 헤더는 요청 또는 응답에 대한 정보를 가진다.
    * 본문은 주고받는 실제 데이터이다.
    * 쿠키는 부가적인 정보이므로 헤더에 저장된다.
## 3-3. 쿠키로 식별하기
* 쿠키에 정보를 입력
    * parseCookies: 쿠키 문자열을 객체로 변환한다.
    * 주소가 /login인 경우와 /인 경우로 나뉜다.
    * /login인 경우 쿼리스트링으로 온 이름을 쿠키로 저장한다.
    * 그 외의 경우 쿠키가 있는지 없는지 판단한다.
* cookie2.js 참조
## 3-4. 쿠키 옵션
* 쿠키명=쿠키값 : 기본적인 쿠키의 값이다.
* Expires=날짜 : 만료 기한이다. 기본값은 클라이언트가 종료될  때까지이다.
* Max-age-초 : Expires와 비슷하지만 날짜 대신 초를 입력할 수 있다. 해당 초가 지나면 쿠키가 제거된다.
* Domain=도메인명 : 쿠키가 전송될 도메인을 특정할 수 있다.
* Path=URL : 쿠키가 전송될 URL을 특정할 수 있다.
* Secure: HTTPS일 경우에만 쿠키가 전송된다.
* HTTPONLY : 설정 시 자바스크립트에서 쿠키에 접근할 수 없다. 쿠키 조작 방지를 위해서 설정한다.

## 3-5. 세션 사용하기
* 쿠키의 정보는 노출되고 수정되는 위험이 있다.
    * 중요한 정보는 서버에서 관리하고 클라이언트에는 세션 키만 제공한다.
    * 서버에 세션 객체(session) 생성 후, uniqueInt(키)를 만들어 속성명으로 사용
    * 속성 값에 정보를 저장하고 uniqueInt를 클라이언트에 보낸다.
    * session.js 참조

# 4. https와 http2
## 4-1. https
* 웹 서버에 SSL 암호화를 추가하는 모듈
    * 오고 가는 데이터를 암호화해서 중간에 다른 사람이 요청을 가로채더라도 내용을 확인할 수 없다.
    * 요즘에는 https 적용이 필수이다.
    * Let's Encrypt에서 무료로 발급 가능
```js
const https = require('https');
const fs = require('fs');

https.createServer({
    cert: fs.readFileSync('도메인 인증서 경로'),
    key: fs.readFileSync('도메인 비밀키 경로'),
    ca: [
        fs.readFileSync('상위 인증서 경로'),
        fs.readFileSync('상위 인증서 경로'),
    ],
}, (req, res) => {
    res.writeHead(200, {'Content-Type' : 'text/html; charset=utf-8'})
    res.write('<h1>Hello Node!</h1>');
    res.end('<p>Hello Server!</p>');
})
.listen(443, () => {
    console.log('443번 포트에서 서버 대기 중입니다!');
});
```

## 4-2. http2
* SSL 암호화와 더불어 최신 HTTP 프로토콜인 http/2를 사용하는 모듈
    * 요청 및 응답 방식이 기존 http/1.1보다 개선됐다.
    * 웹의 속도도 개선되었다.
```js
const http2 = require('http2');
const fs = require('fs');

http2.createSecureServer({
    cert: fs.readFileSync('도메인 인증서 경로'),
    key: fs.readFileSync('도메인 비밀키 경로'),
    ca: [
        fs.readFileSync('상위 인증서 경로'),
        fs.readFileSync('상위 인증서 경로'),
    ],
}, (req, res) => {
    res.writeHead(200, {'Content-Type' : 'text/html; charset=utf-8'})
    res.write('<h1>Hello Node!</h1>');
    res.end('<p>Hello Server!</p>');
})
.listen(443, () => {
    console.log('443번 포트에서 서버 대기 중입니다!');
});
```
# 5. cluster
## 5-1. cluster
* 기본적으로 싱글 스레드인 노드가 CPU 코어를 모두 사용할 수 있게 해주는 모듈이다.
    * 포트를 공유하는 노드 프로세스를 여러 개 둘 수 있다.
    * 요청이 많이 들어왔을 때 병렬로 실행된 서버의 개수만큼 요총이 분산된다.
    * 서버에 무리가 덜 간다.
    * 코어가 8개인 서버가 있을 때 : 보통은 코어 하나만 활용한다.
    * cluster로 코어 하나당 노드 프로세스 하나를 배정할 수 있다.
    * 성능이 8배가 되는것은 아니지만 개선이된다.
    * 단점: 컴퓨터 자원(메모리, 세션 등)을 공유할 수 없다.
    * Redis 등 별도 서버로 해결한다.

## 5-2. 서버 클러스터링
* 마스터 프로세스와 워커 프로세스
    * 마스터 프로세스는 CPU 개수만큼 워커 프로세스를 만든다(worker_threads랑 구조가 비슷하다.)
    * 요청이 들어오면 워커 프로세스에 고르게 분배한다.
