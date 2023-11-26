---
title: Spring Boot 5일차
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-11-06
categories: [SpringBoot, basis]
tags: [SpringBoot, learning, Intellij]
---

2학기 시작하고 팀 프로젝트와 개인사정이 생겨 못했던 블로그와 springboot공부를 다시 시작합니다.
기존에 블로그를 보고선 공부 하였지만 진행하다보니 오류가 많이 생겨 [인프런 SpringBoot강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)를 보고 처음부터 공부하려합니다.


# Intellij
Jetbrains사에서 제작한 자바 통합 개발 환경(IDE)이다. 물론 eclipse로도 Springboot개발이 가능하지만 요즘 회사에서 다들 intellij로 개발한다는 이야기를 듣고선 시작했다.

Intellij를 개발한 Jetbrains공식홈페이지에 들어가면 Intellij의 설명을 볼 수 있다.

![인텔리제이 홈페이지](/assets/img/ssb/5/homepage.png)


사진처럼 설명이 있는데 자세한건 강의를 듣고 코드를 치면서 확인해보자.

Intellij를 설치하고 실행시키면 아래와 같은 사진이 나온다.

![인텔리제이](/assets/img/ssb/5/intellij.png)

> 강의를 보면서 진행했기에 여러 추가파일이 생겼다.

# 실행 과정

![동작 화면](/assets/img/ssb/5/excute.png)
 [출처 : https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
사용자가 웹 브라우저를 통해 `localhost:8080/hello`를 접근하면 내장 톰캣 서버를 먼저 거치고 그것을 스프링에게 전달한다. 스프링 내부에서는 컨트롤러가 `hello`와 매핑이되는 메소드를 호출하면서 data로 `hello!!`를 전달한다. 마지막으로 viewResolver를 통해 html로 변환 후 웹 브라우저로 보여준다.

🔍 그러면 컨트롤러가 없으면 어떻게 될까?

![동작 화면](/assets/img/ssb/5/no.png)

동작은 비슷한데 만약에 컨트롤러가 없다면 스프링이 내부 resources안에 매핑이되는 값을 찾고 리턴해준다. 

