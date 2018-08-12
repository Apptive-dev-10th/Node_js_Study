## Week2 Study

### redirect
```js
let http = require('http');

http.createServer((req, res) => {
    res.writeHead(302, { 'Location': 'http://www.pusan.ac.kr'});
    res.end();
}).listen(8124, () => {
    console.log('Server Running at localhost:8124');
});
```

### url path parsing
```js
let http = require('http');
let fs = require('fs');
let url = require('url');

http.createServer((req, res) => {
    let pathname = url.parse(req.url).pathname;

    if(pathname =='/'){
        // index.html 파일 읽음
        fs.readFile('index.html', (err, data) => {
            // 응답
            res.writeHead(200, { 'ContentType' : 'text/html'});
            res.end(data);
        });
    } else if (pathname == '/OtherPage') {
        // OtherPage.html 읽음
        fs.readFile('OtherPage.html', (err, data) => {
            // 응답
            res.writeHead(200, { 'ContentType' : 'text/html'});
            res.end(data);
        });
    }
}).listen(8124, () => {
    console.log('server running at localhost:8124');
});
```

### http method
```js
let http = require('http');

http.createServer((req, res) => {
    if(req.method == 'GET'){
        console.log('GET 요청입니다');
    } else if(req.method == "POST") {
        console.log('POST 요청입니다');
    }
}).listen(8124, () => {
    console.log('server is running at localhost:8124');
});
```

## 과제

1. HTTP status code 에 대해서 자세히 알아오기
2. 암호화 방식 종류 알아오기. + bcrypt 가 무엇인지 알아오기.
3. middleware 에 대해서 알아오기