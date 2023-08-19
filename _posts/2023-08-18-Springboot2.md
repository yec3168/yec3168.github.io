---
title: Spring Boot 2일차
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-18
categories: [SpringBoot, basis]
tags: [SpringBoot, learning]
---

# **JPA**
자바의 ORM 기술의 표준이다.

# **ORM**
Object Relation Mapping<br>
객체와 관계형 데이터베이스를 매핑해준다. 이 방식은 데이터베이스의 접근할 때 프로그래밍 언어 관점으로 볼 수 있고, SQL문을 직접 작성하지 않고 `엔티티`를 객체로 표현할 수 있다.

>Info 테이블

|ID|Name|Phone|
|:---:|:---:|:---:|
|1111|김xx|010-0000-0000|
|2222|박xx|010-1111-1111|
|...|...|...|

ORM을 사용하면 쿼리 대신 자바코드로 작성할 수 있다.

```java
Info i1 = new Info();
i1.setId(1111);
i1.setName("김xx");
i1.setPhone("010-0000-0000")

Info i2 = new Info();
i2.setId(1111);
i2.setName("김xx");
i2.setPhone("010-0000-0000")
```

# **H2 데이터베이스**
h2 데이터베이스는 설치도 쉽고 사용하기 간편하다. [H2설치방법](https://wikidocs.net/161164#orm)홈페이지에서 설치해보자.


설치가 다됐으면 [http://localhost:8080/h2-console](http://localhost:8080/h2-console) 홈페이지에 접속하면 아래와 같은 사진이 나온다.

![h2](/assets/img/ssb/2/h2.png)

JDBC URL 값을 `jdbc:h2:~/local`로 바꿔주고  연결하자.

![h2](/assets/img/ssb/2/h2-1.png)

>나머지 환경변수 설정은 위에 링크한 홈페이지 참조 바람.


# **엔티티**
엔티티는 데이터베이스 테이블과 매핑되는 자바 클래스를 말한다. 혹은 모델 또는 도메인 모델이라고 부른다.

## **속성 구상하기**
이번에 만들 질문과 답변 엔티티를 만들어보자.

먼저, `질문 엔티티`는 아래와 같은 속성이 필요하고

|속성명|설명|
|:---:|:---:|
|id|고유 번호|
|subject|제목|
|content|내용|
|date|작성날짜|

`답변 엔티티`는 다음과 같은 속성이 필요하다.

|속성명|설명|
|:---:|:---:|
|id|고유 번호|
|question|답변 제목|
|content|내용|
|date|작성날짜|


## 질문 엔티티 작성
`src/main/java`폴더에 `Question.java`를 만들어 아래 내용을 입력하자.

```java
package com.mysite.community_site;

import java.time.LocalDateTime;
import java.util.List;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@Entity
public class Question {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Integer id;
	
	@Column(length = 200)
	private String subject;
	
	@Column( columnDefinition = "TEXT")
	private String content;
	
	private LocalDateTime createDate;
	
	@OneToMany(mappedBy = "question", cascade = CascadeType.REMOVE)
	private List<Answer> list;
}

```


### 질문 애너테이션

1. @Entity <br>
 JPA가 엔티티로 인식한다.
2. @Id <br>
 id속성을 기본키로 지정한다. 중복이 안된다.
3. @GeneratedValue<br>
 데이터를 저장할때마다 자동으로 1씩 증가하여 저장된다. `strategy`는 고유번호를 생성하는 옵션으로 `GenerationType.IDENTITY`는 해당 컬럼을 독립적인 시퀀스를 생성하여 번호를 증가시킬때 사용.
4. @Column
 각 컬럼의 세부 설정을 해준다. 



 ## 답변 엔티티 작성

 똑같은 위치에 `Answer.java`파일을 만들자.


 ```java
package com.mysite.community_site;


import java.time.LocalDateTime;

import org.springframework.data.annotation.CreatedDate;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.ManyToOne;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@Entity
public class Answer {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Integer id;
	
	@ManyToOne
	private Question question;
	
	@Column(columnDefinition = "TEXT")
	private String content;
	
	@CreatedDate
	private LocalDateTime date;
}
 ```



# Repository 생성

`src/main/java/com/mysite/community_site`부분에 `QuestionRepository.java`파일을 생성한다.

> QuestionRepository.java

```java
package com.mysite.community_site;

import org.springframework.data.jpa.repository.JpaRepository;

public interface  QuestionRepository extends JpaRepository<Question, Integer>{

}
```

JpaRepository인터페이스를 상속하여 Repository를 생성했다. 상속할 때 `Generic` 타입으로 <앤티티타입, PK(기본키)속성>을 지정했다.

똑같이 AnswerRepository도 만들자.

```java
package com.mysite.community_site;

import org.springframework.data.jpa.repository.JpaRepository;

public interface AnswerRepository extends JpaRepository<Answer, Integer>{

}
```

Repository를 만들었으니 Test를 해보자. `src/test/java/com/mysite/community_site`의 `CommunitySiteApplicationTests.java`파일을 아래와 같이 수정하자.

```java
package com.mysite.community_site;

import java.time.LocalDateTime;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class CommunitySiteApplicationTests {

	@Autowired
	private QuestionRepository questionrepository;
	
	@Test
	void testJpa() {
		Question q1 = new Question();
		q1.setSubject("생성자가 무엇인가요?");
		q1.setContent("제곧내");
		q1.setCreateDate(LocalDateTime.now());
		this.questionrepository.save(q1);
		
		Question q2 = new Question();
		q2.setSubject("java와 javascript의 차이점이 뭔가요?");
		q2.setContent("제곧내");
		q2.setCreateDate(LocalDateTime.now());
		this.questionrepository.save(q2);
	}

}
```

Question q1, q2 객체를 생성하여 제목과 내용, 날짜를 적은 뒤 값을 QuestionRepository 데이터베이스의 저장한다.

실행은 `Run -> Run As -> JUnit Test`으로 실행한다.

> local서버가 가동중이면 중지하고 실행해야한다.


# 결과 확인

`localhost:8080/h2-console`로 들어가 작성했던 데이터가 무사히 들어갔는지 확인하자.

```sql
SELECT * 
FROM Question
```

![결과](/assets/img/ssb/2/h2-result.png)