---
title: Spring Boot 3일차
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-20
categories: [SpringBoot, basis]
tags: [SpringBoot, learning]
---

# 데이터 CRUD
저번에 JpaRepository를 이용하여 데이터베이스에 저장하는 법을 배웠다. 다음으로는 해당 데이터베이스의 값을 조회, 수정, 삭제, 저장하는 법을 배워보도록 하겠다. 

CRUD는 Create, Read, Update, Delete이다. 


## 데이터 조회하기

### findAll 
questionrepository의 findAll 메소드를 사용해보자.
findAll은 말 그대로 모든 데이터를 조회하는 메소드이다.

```java
void testJpa() {
		List<Question> all = this.questionrepository.findAll();
        assertEquals(2, all.size());

        Question q = all.get(0);
        assertEquals("생성자가 무엇인가요?", q.getSubject());
	}
```

`assertEquals(기대값, 실제값)`와 같이 사용하며 기대값과 실제값이 동일한지 조사한다. 만약에 동일하지 않으면 실패로 끝난다.


### findId
id 값을 조회하기 위해 findById메소드를 사용한다.

```java 
//Optional은 null을 우연하게 처리하기 위해 사용하는 것.
		Optional<Question> q = this.questionrepository.findById(1);
		
		if(q.isPresent()) {
			Question temp = q.get();
			assertEquals("생성자가 무엇인가요?", temp.getSubject());

```

isPresent는 null인지 아닌지 확인하는 것.


## 데이터 수정하기

```java
void testJpa() {
		//Optional은 null을 우연하게 처리하기 위해 사용하는 것.
		Optional<Question> q = this.questionrepository.findById(1);

		assertTrue(q.isPresent());
		Question temp = q.get();
		temp.setSubject("수정 제목");
		this.questionrepository.save(temp);
}
```
`set`메소드를 사용해서 수정한다.

## 데이터 삭제하기

```java
void testJpa() {
		assertEquals(2, this.questionrepository.count()); // count 갯수가 2랑 같은지
		
		
		Optional<Question> q = this.questionrepository.findById(1);

		assertTrue(q.isPresent());
		Question temp = q.get();
		
		this.questionrepository.delete(temp); //삭제
		
		assertEquals(1, this.questionrepository.count()); // count 갯수가 1와 같은지
	}
```


## 답변 데이터 생성후 저장하기.

> 저장할때는 `save` <br>
> 삭제할때는 `delete`

```java
	Optional<Question> q = this.questionrepository.findById(1);

		assertTrue(q.isPresent());
		
		Question temp = q.get();
		
		Answer a = new Answer();
		a.setContent("답변의 내용");
		a.setQuestion(temp);
		a.setDate(LocalDateTime.now());
		this.answerRepository.save(a);
		
	}
```

<br>
<br>
<br>

# 템플릿 설정하기
지금까지 브라우저에 응답하는 문자열은 우리가 직접 작성하였는데 실제로는 직접 만들지 않고 `템플릿 방식`을 사용해서 만든다. 템플릿은 자바코드를 삽입할 수 있는 html형식의 파일이다.

템플릿에는 여러가지 종류가 있는데 그중 `Thymleaf`엔진을 사용할 것이다.

설치는 [템플릿 설정](https://wikidocs.net/161186)을 참고했다.

```html
# question_list.html
<table>
    <thead>
        <tr>
            <th>제목</th>
            <th>작성일시</th>
        </tr>
    </thead>
    <tbody>
        <tr th:each="question : ${questionList}">
            <td th:text="${question.subject}"></td>
            <td th:text="${question.createDate}"></td>
        </tr>
    </tbody>
</table>
```
`th:`는 템플릿 엔진에서 사용하는 속성이다. 이 부분이 자바코드와 연결이된다.