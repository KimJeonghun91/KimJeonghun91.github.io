---
layout: post
title: 'Node - express-generator 구조'
subtitle: 'Node - express-generator 구조'
category: devlog
tags: node
comments: true
# image: 
#   path: /assets/img/default_code.png
---

### Node - express-generator 구조

# Node - express-generator 구조

> bin/www

```jsx
var app = require('../app');  
var debug = require('debug')('learn-express:server');  
var http = require('http');
```

- **app** : express() 인스턴스. `app.set(키,값)` 형태로 데이터를 저장할 수 있으며 `app.get(키)`로 가져올 수 있다.
- **debug** : 콘솔에 로그를 남길 수 있는 모듈
- **http** : 서버 생성 모듈

 

```jsx
 var port = normalizePort(process.env.PORT || '3000');
```

- process.env 객체에 PORT 값이 있으면 사용하고 없으면 3000을 기본 포트로 사용

 

```jsx
 var server = http.createServer(app);  server.listen(port);
```

- 지정된 포트로 서버 실행

 

> app.js

```jsx
var app = express();  
app.set('views', path.join(__dirname, 'views'));  
app.set('view engine', 'pug');
```

- `app.set` 을 사용하여 express 환경 설정 가능

 

```jsx
app.use(logger('dev'));  
app.use(express.json());  
app.use(express.urlencoded({ extended: false }));  
app.use(cookieParser());  
app.use(express.static(path.join(__dirname, 'public')));
```

- `app.use` 를 사용하여 미들웨어 추가

 

```jsx
...  
module.exports = app;
```

- 모듈로 내보내고 `www` 에서 사용

   

# express 미들웨어

### 미들웨어란?

> 요청과 응답의 중간에 위치하여 기능을 추가하거나 필터링함.

- 미들웨어 형태

    ```jsx
     app.use(function(req, res, next) {
         ...
         next();
     })
    ```

- **next()** : 요청의 흐름을 제어함
    - `next()` : 다음 미들웨어로 이동
    - `next('route')` : 다음 라우터로 이동
    - `next(error내용)` : 에러 핸들러 이동

 

### 미들웨어 종류 및 기능

- **morgan**

    콘솔 기록 미들웨어

- **body-parser**

    본문을 해석해주는 미들웨어. json, raw, text등의 본문 데이터들을 해석해 req.body에 추가함

    (ex) URL-encoded 형태의 `name=righthot&age=30` 데이터를 `{name: 'righthot', age:30}` 으로 변형

- **cookie-parser**

    쿠키를 해석해주는 미들웨어

- **static**

    정적인 파일 제공, 정적 파일을 불러올 수 있으며, 경로를 다르게 지정 가능

    ```jsx
    app.use('/pd', express.static(path.join(__dirname, 'images/pd')));

    // http://localhost:3000/images/pd
    // http://localhost:3000/pd
    ```

- **express-session**

    세션 관리 미들웨어