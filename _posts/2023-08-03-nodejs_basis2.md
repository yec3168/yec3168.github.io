---
title: nodejs GET, POST
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-03
categories: [Node.js, basis]
tags: [nodejs, "백엔드", "기초"]
math: true  # mathematical 기능
mermaid: true  # 표생성 도구
pin: true  # 홈페이지 메인화면에 게시물 고정
#image : /path/to/image-file 포스트 최상단에 이미지를 넣고 싶을때
---


## **Get 요청**

```js
app.get('/', (req, res) => {
  res.send('Hello World!')
})

```

위 코드처럼 주소창에서 데이터를 같이 넘기는 방식.(`localhost:3000`같은 주소창으로 접근 )

위와 같은 형태로 콜백함수에 전달된 req객체를 이용하면 get 방식으로 요청이 들어오면 데이터를 가져와서 html 파일에 적용할 수 있다.

> req : request<br>
> res : response


### **query, 파라미터 값으로 라우팅**

1. 파라미터 <br>

```js
app.get('/user/:id', (req, res) => {
  //파라미터
  const p = req.params;
  console.log(p)

  res.json({'userid': p.id})
  //res가 응답으로 "send('Hello World!')"을 보내줌.
})

```

req는 요청할때 넣는 정보를 가지고 있다. 사용하는 방식이 get방식이 때문에 주소창에다가 `ID`값을 입력해서 전달한다.

![para](/assets/img/nodejs/2/para.png)

`console.log`로  params를 출력하면 아래와 같은 결과가 나온다.

![result2](/assets/img/nodejs/2/result2.png)


2. query <br>

```js
app.get('/user/:id', (req, res) => {
  //파라미터
  const q = req.query;
  console.log(q)
  res.json(
    {
      'userQ': q.q,
      'userName' : q.name          
    } )
  //res가 응답으로 "send('Hello World!')"을 보내줌.
})
```

query를 보낼때 주소창에 `localhost:3000/user/IDSetting?q=ec&name=eungchan`처럼 보낸다. `?`뒤에 보낼 정보들을 입력하는데 요청으로 보낼때 알맞는 값들을 `&`문자로 구분하여 값을 넣어주며 request한다.

![query](/assets/img/nodejs/2/query.png)

위 사진처럼 id값과 q, name값을 입력하게 되면 결과값이 나온다.

![result](/assets/img/nodejs/2/result.png)

> url에 한글 즉 `/강아지`, `/고양이`같은 방식은 안된다. url은 아스키문자이기때문에 한글을 지원하지 않는데, 한글을 url에 쓰고 싶다면 따로 [URL/ Decoder/Encoder](https://meyerweb.com/eric/tools/dencoder/)에 접속해서 나오는 문자를 입력하면 된다.
{: .prompt-info}

## **Post 요청**

주소창이 아니라 내부적으로 body에 데이터를 전달한다.

 **나중에 더 자세히 알아보자**







 