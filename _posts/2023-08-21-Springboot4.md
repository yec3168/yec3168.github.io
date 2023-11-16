---
title: Spring Boot 4일차
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-20
categories: [SpringBoot, basis]
tags: [SpringBoot, learning]
---

# 서비스
우리는 지금까지 Controller로부터 Repository를 직접 접근하여 사용하였다.

![직접사용](/assets/img/ssb/4/1.png)

하지만 대부분의 스프링부트 프로젝트는 중간에 `서비스`를 두어 데이터를 처리한다.

![직접사용](/assets/img/ssb/4/2.png)

서비스를 구현하게되면 Controller들이 서비스에 접근하여 기능을 사용할 수 있지만, 서비슬를 구현을 안하면 모든 컨트롤러에 동일한 기능을 중복으로 작성해야한다.

또 다른 이유로는 보안상의 문제도 있는데 컨트롤러로 직접 접근하는것보다 서비스를 통해 Repository에 접근하는것이 더 안전하다.

# 앤티티객체와 DTO 객체 
Question, Answer에 작성한 자바파일은 `앤티티 클래스`이다. 앤티티 클래스는 데이터베이스와 직접 맞닿는 클래스이기 때문에 컨트롤러와 템플릿 엔진에서 사용하는것은 좋지 않다.

그래서 사용하는 것이 `DTO 클래스`이다. Question, Answer같은 앤티티 객체를 DTO 객체로 변환하는 작업이 필요하다.


## 코드

### DataNotFoundException

```java
package com.mysite.community_site;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.NOT_FOUND, reason = "entity not found")
public class DataNotFoundException extends RuntimeException{
	private static final long serialVersionUID = 1L;
	public DataNotFoundException(String msg) {
		super();
	}
}


```
### QuestionService
```java
package com.mysite.community_site.question;

import java.util.List;
import java.util.Optional;
import com.mysite.community_site.DataNotFoundException;

import org.springframework.stereotype.Service;

import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
@Service
public class QuestionService {

	private final QuestionRepository questionrepository;

	public List<Question> getList(){
		return this.questionrepository.findAll();
	}

	public Question getQuestion(Integer id) {
		Optional<Question> question = this.questionrepository.findById(1);

		if(question.isPresent())
			return question.get();
		else {
			throw new DataNotFoundException("Question not found");
		}
	}
}
```

### QuestionController 
```java
package com.mysite.community_site.question;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor  //필드로 구성된 생성자를 re~~가 자동으로 코드를 생성해줌.
@Controller
public class QuestionController {

	private final QuestionService questionservice;
	
	@GetMapping("/question/list")
	public String list(Model model) {
		List<Question> questionList = questionservice.getList();
		model.addAttribute("questionList", questionList);
		
		return "question_list";	
	}
	
	 @GetMapping(value = "/question/detail/{id}")
	public String detail(Model model, @PathVariable("id") Integer id) {
		 Question question = this.questionservice.getQuestion(id);
	     model.addAttribute("question", question);
		 //{id} 와("id")가 이름이 같아야함.
		 return "question_detail";
	}
	
}
```

### question_detail

```html
<h1 th:text="${question.subject}"></h1>
<div th:text="${question.content}"></div>
```