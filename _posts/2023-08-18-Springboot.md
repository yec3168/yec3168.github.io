---
title: Spring Boot
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-18
categories: [SpringBoot, basis]
tags: [SpringBoot, learning]
---
# 시작
 [스프링부트 위키독스](https://wikidocs.net/book/7601)를 보고 공부했다.
# **스프링부트**
자바의 웹 프레임워크로 기존 스프링 프레임워크에 톰캣 서버를 내장하고 여러 편의 기능들을 추가한 프레임워크이다. 간단히 말하자면 웹 프로그램을 간단하게 빠르게 개발할 수 있도록 도와주는 웹 프레임 워크이다.

> 웝 프레임워크 : 웹을 이용할 때 사용하는 로그인, 로그아웃, 데이터베이스 처리, 쿠키 처리 등 만들어야할 프로그램을 일일히 만들지 않고 이미 만들어져있는 것을 이용하여 사용하는 것.
{: .prompt-info}


# **구조**

![구조](/assets/img/ssb/1/home.png)

## **src/main/java디렉토리**
자바 파일을 작성하는 공간이다. 파일로는 컨트롤러, 폼, DTO, 데이터베이스 처리를 위한 엔티티, 서비스 파일이 존재한다.

### **CommunitySiteApplication.java파일**

`<프로젝트명>`+`Application.java`파일이다. 프로젝트 생성시 자동으로 생성된다.

## **src/main/resources디렉토리**
자바를 제외한 `CSS`, `javascript`등을 작성하는 공간이다.

### **template디렉토리**
템플릿 파일을 저장한다. 템플릿 파일은 html파일 형태로 자바 객체와 연동되는 파일이다.

### **static디렉토리**
`.css`, `.js`, `.png`같은 이미지 파일을 저장하는 공간이다.

### **application.properties**
sbs 프로젝트의 환경을 설정한다.

## **build.gradle** 
프로젝트를 위한 플러그인과 라이브러리 등을 기술한다.



# **실습 (URL 매핑)**
`https://localhost:8080/sbb`페이지 접속시 "8월 18일 Spring boot 공부 1일차" 문자열을 출력하도록 작성해보자.

위 구조를 보면 알 수 있듯이 해당 페이지 코드는 자바파일로 작성하기 때문에 `src/main/java디렉토리`에 작성하면 된다.

```java
package com.mysite.community_site;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MainController {

	@GetMapping("/sbb")
	@ResponseBody
	public String index() {
		return "8월 18일 Spring boot 공부 1일차";
	}
}
```

**@Controller**: 스프링부터의 컨트롤러로 지정.<br>
**@GetMapping("/값")**: 요청된 URL과의 매핑을 담당. <br>
**@ResponseBody** : GetMapping의 URL요청에 대한 응답으로 문자열을 리턴하라는 의미. 즉 index()함수가 Response된다.<br>


