---
title: Spring Boot 6일차(simple project)
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-11-20
categories: [SpringBoot, project]
tags: [SpringBoot, project, user_management]
---

# 프로젝트

이번 프로젝트로는 회원을 관리하는 홈페이지를 개발하려고 한다. 

1. 요구사항

 - 데이터 : 회원 ID, 이름, 전화번호, 가입날짜
 - 기능 : 회원 등록, 조회, 삭제
 - DB : 미정

> 완전 간단히 만들어보고 나중에 조금씩 추가해보자.

2. 구조

 ![구조](/assets/img/ssb/6/big.png)

  - 컨트롤러 : MVC의 컨트롤러 역할
  - 서비스 : 어떠한 서비스를 제공할건지 구현 ( 중복 가입x, 동일 아이디 생성 x 등 )
  - 도메인 : 객체를 정의 (like 회원, 주문, 음식 )
  - 레포지토리 : 도메인으로 생성된 객체를 DB에 저장하고 관리함.

3. 의존
 
 ![의존](/assets/img/ssb/6/의존.png)

 아직 DB가 미정이기 때문에 데이터를 저장할 DB가 따로 없다. 그래서 서비스를 구현할 때 레포지토리에 저장하는 방식으로 설계했다.


# 구현

 1. Memeber

    src/main/java/domain

 ```java
 package springboot_pracitce.hello.domain;

import java.util.Date;

public class Member {

    private Long id; //id
    private String name; //이름
    private String phone_number; //전화번호
    private Date date; //가입날짜

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhone_number() {
        return phone_number;
    }

    public void setPhone_number(String phone_number) {
        this.phone_number = phone_number;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

}

 ```

2. MemberRepository

src/main/java/repository

```java
package springboot_pracitce.hello.repository;

import springboot_pracitce.hello.domain.Member;

import java.util.Date;
import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member); //저장
    Member delete(Member memeber); // 삭제

    Optional<Member> findById(Long id);

    Optional<Member> findByName(String name);

    Optional<Member> findByPhone_number(String phone_number);

    Optional<Member> findByDate(Date date);

    List<Member> findAll();
}
```

3. MemoryMemberRepository

src/main/java/repository

```java
package springboot_pracitce.hello.repository;

import springboot_pracitce.hello.domain.Member;

import java.lang.reflect.Array;
import java.util.*;

public class MemoryMemberRepository implements MemberRepository{
    // 기본키
    // ctrl+space누르면 자동오퍼라이드
    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L; // 인데스 키값을 생성해줌
    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Member delete(Member memeber) {
        store.remove(memeber.getId(), memeber);
        return memeber;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        // 반복문을 돌려서 필터를 걸어 name이 같은 것을 가져옴
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public Optional<Member> findByPhone_number(String phone_number) {
        return store.values().stream()
                .filter(member -> member.getPhone_number().equals(phone_number))
                .findAny();

    }

    @Override
    public Optional<Member> findByDate(Date date) {
        return store.values().stream()
                .filter(member -> member.getDate().equals(date))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore(){
        store.clear();
    }
}


```

# 테스트 케이스 작성

내가 개발한 기능이 정상적으로 실행이 되는지 확인해보기 위해 테스트 케이스를 작성한다. 원래는 JUint라는 것을 사용한다고 하는데 간단하게 컨트롤러를 통해 해당 기능을 수행하도록 작성해보자.

이 방법은 실행하고 준비하는데 오래걸리고 반복 실행과 여러 테스트를 한번에 실행시키기 어렵다는 단점이 있다.


 ## 구현(MemoryMemberRepositoryTest)

 src/test/java/repository


```java
package repository;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import springboot_pracitce.hello.domain.Member;
import springboot_pracitce.hello.repository.MemoryMemberRepository;

import java.util.Date;
import java.util.List;
import java.util.Optional;

import static org.assertj.core.api.AssertionsForClassTypes.assertThat;

public class MemeoryMemberRepositoryTest {
    //콜백 함수
    @AfterEach
    public void afterEach(){
        repository.clearStore();
    }
    MemoryMemberRepository repository = new MemoryMemberRepository();
    @Test
    public void save(){
        Member member = new Member();
        member.setName("Spring!!");
        member.setPhone_number("010-0000-0000");
        member.setDate(new Date());

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        System.out.println(result.getDate());
        System.out.println(result==member);

        // jupiter api
        // 따로 출력되는것은 없고 초록불로 같다고 알려줌
        // 틀리면 에러 빨간불
        Assertions.assertEquals(member, result);

        assertThat(member).isEqualTo(result);
    }

    @Test
    public void findByName(){
        Member member_name1  = new Member();
        member_name1.setName("member12");
        repository.save(member_name1);

        Member member_name2  = new Member();
        member_name2.setName("member22");
        repository.save(member_name2);


        Member result = repository.findByName("member22").get();

        assertThat(result).isEqualTo(member_name2);
    }
    @Test
    public void findByPhone_number(){
        Member phone1  = new Member();
        phone1.setPhone_number("010");
        repository.save(phone1);

        Member phone2  = new Member();
        phone2.setName("033");
        repository.save(phone2);


        Member result = repository.findByPhone_number("010").get();

        assertThat(result).isEqualTo(phone1);
    }
    @Test
    public void findAll(){
        Member member_name1  = new Member();
        member_name1.setName("member1");
        repository.save(member_name1);

        Member member_name2  = new Member();
        member_name2.setName("member2");
        repository.save(member_name2);

        List<Member> all = repository.findAll();

        assertThat(all.size()).isEqualTo(2);
    }


}


```
