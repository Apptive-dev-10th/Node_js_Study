# 첫째주 스터디

첫째주 스터디는 Node.js에 대한 간략한 이해 및 실습을 진행합니다.

## Node.js 설치하기

https://nodejs.org/ko/download/package-manager/

## Node.js 란?

* node.js는 google의 V8 JavaScript 엔진에 기반한 서버 측 기술이다.
* 쓰레드나 별도 프로세스 대신 ```비동기 이벤트 위주 I/O```를 사용함
* 가장 쉽게 서버를 구성할 수 있는 언어

### 장점  
단일 쓰레드를 실행하면 [Overhead][overhead]가 적어서 어플리케이션의 확장성을 쉽게 얻을 수 있다.

멀티 쓰레드로 개발하지 않고도 리소스 사용량을 최소화 할 수 있다. 즉 [Thread Safe][thread_safe] 하게 만들지 않아도 됨


## Asynchronous I/O ?? vs Synchronous I/O??

가장 간략하게 설명 - 
http://twinbraid.blogspot.kr/2015/02/blog-post_2.html

전공지식을 포함한 설명 - 
http://djkeh.github.io/articles/Boost-application-performance-using-asynchronous-IO-kor/

## Emit 

``` js
// emit(eventName) method는 이벤트를 강제로 발생시킬때 사용한다

process.on('exit', (code) => {
    console.log('안녕히 가세요 ^^7');
});

process.emit('exit');
process.emit('exit');
process.emit('exit');
process.emit('exit');

console.log('프로그램 실행중');
// 이벤트를 강제로 호출하면, 이벤트 리스너만 실행이 된다
```

## Event

```js
// method on(eventName or eventType, eventHandler or eventListener), 이벤트를 연결할때 사용
// process 객체에 exit 이벤트를 연결합니다
process.on('exit', (code) => {
  console.log("안녕히 가거라 ^^");
});

// process 객체에 uncaughtException 이벤트를 연결합니다
// uncaughtException 이벤트는 예외가 발생할 때 실행되는 이벤트 입니다.
process.on('uncaughtException', (error) => {
  console.log("예외가 발생했네...ㄷㄷ");
});

var count = 0;
var test = () => {
  // 탈출 코드
  count = count + 1;
  if (count > 3) {return;}

  setTimeout(test, 2000);
  // 예외를 강제로 발생시킵니다
  error.error.error();
};
setTimeout(test, 2000);

// setTimeout(function, milliseconds) 는 milliseconds를 기다린 이후에 function을 실행

```
event의 연결 개수는 기본적으로 10개 이하이다. 이를 초과할 경우 개발자 실수로 간주한다. 그래서 setMaxListeners(limit) method를 통해서 event의 개수를 늘릴 수 있다.

## Server Event

```js
var http = require('http');

// server 객체 생성
var server = http.createServer();

// server 객체에 event 연결
server.on('request', (code) => {
    console.log('Request On');
});

server.on('connection', (code) => {
    console.log('Connection On');
});

server.on('close', (code) => {
    console.log('Close on');
});

server.listen(8124);
```

## HTTP
```js
// http module
var http = require('http');
// file system module 
var fs = require('fs');

// create http server at local
// req == ServerRequest, res == ServerResponse
http.createServer(function (req, res){
    
    // read hello.js file using uft8
    // third parameter is anonymous and call-back function
    fs.readFile('hello.js', 'utf8', function(err, data) {
        
        // write with plain text type
        res.writeHead(200, {'content-type': 'text/plain'});        
        
        // occur error
        if(err){
            res.write('Could not find or open file for reading\n');
        } else {
            res.write(data);
        }
        // end of response
        res.end();
    });
}).listen(8124, function() { console.log('bound to port 8124'); });
// second parameter at listen method is call-back function

console.log('server running on 8124');
```

1. listen method 는 http 서버 개체에 지정된 포트에 대해 연결을 대기 하기 시작하라고 알려준다. node는 연결이 맺어지는 것을 대기하는 동안 차단되지 않으므로, 연결이 맺어지고 나면, call back 함수를 제공하기만 하면 된다.
2. 연결이 맺어지면, listening event가 발생하여서 call back 함수가 호출이 되고 콘솔에 log 가 찍힌다.

**필요한 개념**
1. 요청 : 웹 페이지에 접속하려고 하는 어떤 요청을 지칭
2. 응답 : 요청을 받아 이를 처리하는 작업을 지칭
3. http 모듈 : HTTP 웹 서버와 관련된 모든 기능을 담은 모듈
4. server 객체 : 웹 서버를 생성하는 데 꼭 필요한 객체
5. response 객체 : 응답 메세지를 작성할 때 request 이벤트 리스너의 두번쨰 매개변수로 전달되는 객체
6. request 객체 : 응답 메세지를 작성할 때 request 이벤트 리스너의 첫번쨰 매개변수로 전달되는 객체
7. 포트 : 컴퓨터와 컴퓨터를 연결하는 정보의 출입구 역할을 하는 곳이다.





[overhead]: https://ko.wikipedia.org/wiki/%EC%98%A4%EB%B2%84%ED%97%A4%EB%93%9C
[thread_safe]: https://kldp.org/node/36904