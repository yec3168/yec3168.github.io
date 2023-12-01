---
title: Spring Boot 7일차(simple project2)
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-11-24
categories: [SpringBoot, project]
tags: [SpringBoot, project, user_management]
---

다음으로는 회원 레포지토리와 도메인을 활용해서 실제 비즈니스 로직을 구현하려고 한다.

# 회원 서비스

src/main/java/service

 1. Join 메소드

 이 메소드는 회원가입을 담당하는 로직이다. 파라미터로 Member를 받아오고 해당 Member을 레포지토리에 등록(save)하는 메소드.

 ```java
/**
     * 회원가입 담당 로직
     * @param member
     * @return memver.getId()s
     */
    public Long join(Member member){
        // 중복 memeber(name)가 레포지토리에 존재 하면 안된다.
        // ctrl+alt +v 누르면 자동 완성
        Optional<Member> result = memberRepository.findByName(member.getName());
        // isPresent는 result값이 null이 아니라 다른 값이 존재한다면 아래 lambda 함수를 실행함.
        result.ifPresent(m ->{
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });

    //    // 추천하는 방식 get()으로 가져오는 것은 추천하지 않음
    //    memberRepository.findByName(member.getName()).ifPresent(m -> {
    //        throw new IllegalStateException("get사용하지 않고 바로 exception");
    //    });


        memberRepository.save(member);
        return member.getId();

    }

 ```


# 회원 조회


 회원 전체 조회 

 ```java
  /**
     * 전체 회원 조회
     * @return
     */

    public List<Member> findAllMember(){
        return memberRepository.findAll();
    }
 ```

 원하는 회원 조회

```java
/**
     * 원하는 회원 조회
     * @param memberId
     * @return
     */
    public Optional<Member> findOneMember(Long memberId){
        return memberRepository.findById(memberId);
    }

```


# 회원 삭제

```java
    /**
     * 해당 맴버 제거
     * @param member
     */
    public void deleteMember(Member member){
        memberRepository.findByName(member.getName()).ifPresent(m->{
            memberRepository.delete(member);
        });
    }
```



# 서비스 테스트

src/main/test/spring.boot.test/service

```java

```