---
title: nodejs 1일차
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-07-28
categories: [Node.js, basis]
tags: [nodejs, "백엔드", "기초"]

---

![Node.js](/assets/img/nodejs/1/nodejs.png)

#### **information**
주변에서 말하길 배워두면 써먹을 곳이 많은 언어중 하나이다. 남들이 다 추천하면서 꼭 배워두는것이 좋다고 말하여 나도 한번 공부해보자고 생각해서 시작한다.

## **Node.js**
`Node.js`는 내 컴퓨터 혹은 서버에서 돌리는 백엔드이다. `자바스크립트`를 이용하여 만들고, `npm`이라는 언어를 사용하여 모듈을 가져와서 조합해서 사용한다고 한다.

기존의 자바스크립트는 브라우저를 통해 실행하였는데 nodejs를 사용하면 따로 브라우저 없이도 실행이 가능하다.

## **Install Node.js**
[Node.js](https://nodejs.org/ko)에 들어가 `안정적, 신뢰도 높은`이 적혀있는 버전을 다운받자.

설치가 완료되면 `Visual Studio Code`를 열어 자신이 원하는 위치에 폴더 하나를 생성하자.

`index.js`파일을 만들자.

![index.js](/assets/img/nodejs/1/install.png)


### **Hello World**
 출력 연습을 해보자. 만들어진 index.js 파일에 아래 코드를 입력하자.
 ```js
 console.log("hello world")
 ```

 Visual studio code에서 새 터미널을 열고 Node 명령어를 사용해서 index.js 파일을 실행하자.

 ```console
 node index.js
 ```
 ![Node.js](/assets/img/nodejs/1/result_1.png)


## **npm**
 [npm](https://www.npmjs.com/)는 `Node package manager`의 약자로 모든 코드를 직접짜지 않고, 여기서 필요한 모듈을 검색해서 다운받아서 사용하는 것이다.

 window에서 exe 파일을 통해 설치하는 것처럼 node.js에서 모듈을 설치할때는 터미널을 이용하여 아래 명령어를 통해 필요 모듈을 설치한다.
 ```console
 npm install [모듈이름]
 ```

 > -g 라는 속성을 주게 되면 해당폴더말고도 다른 모든 폴더에서도 다운받은 모듈을 사용할 수 있다.
 {: .prompt-tip}
### **figlet**
 npm 홈페이지에 `figlet`을 검색해서 모듈을 다운받자. 
 > figlet은 아스키 art를 만들어주는 모듈.
 {: .prompt-info}

 터미널에 `npm install figlet`을 입력해 figlet모듈을 다운받자.

 ```js
 var figlet = require("figlet");

 figlet("Hello World!!", function (err, data) {
  if (err) {
    console.log("Something went wrong...");
    console.dir(err);
    return;
  }
  console.log(data);
});
 ```
 위 코드는 figlet 모듈 배포사이트에 있는 예시문으로 `hello world`를 아스키 art로 만들어준다.

  ![Node.js](/assets/img/nodejs/1/result_2.png)
 
 >package-lock: 생성된 package의 정보를 더 구체적으로 알려주는 파일.
 {: .prompt-info}


## **Express 모듈**
 node.js를 이용해서 `웹 프레임워크`를 만드는 것이다. 
 > 웹 프레임워크 : 웹의 프론트엔드에서 무언가 클릭했을때 백엔드로 요청을 보내 백엔드에서 요청에 대한 응답을 보내주는  역할.
 {: .prompt-info}

### 설치
```console
npm i express
```
위 명령어로 express 설치.

```js
const express = require('express') //
const app = express()
const port = 3000

// get : http 메소드
// '/' : 라우팅
// ()=>{} : 콜백 함수
app.get('/', (req, res) => {
  res.send('Hello World!')
  //res가 응답으로 "send('Hello World!')"을 보내줌.
})

// port : 들어오는 입구
// ()는 함수
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
simple 코드를 입력하고 `index.js`파일을 실행하면 
[localhost:3000](http://localhost:3000/)에 접속할 수 있다.


