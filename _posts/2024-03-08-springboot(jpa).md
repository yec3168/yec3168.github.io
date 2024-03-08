---
title: JPA 공지글 우선 정렬
author:
name: Eungchan
link: https://github.com/yec3168
date: 2024-03-08
categories: [SpringBoot, basis]
tags: [SpringBoot, jpa, query, querydsl]
---

# JPA

JPA(Java Persistence API)란 자바에서 사용하는 ORM(Object-Relational Mapping)으로 인터페이스를 제공해준다.

> 여기서 `ORM`은 "객체를 연결을 해준다" 라는 의미로 어플리케인션과 데이터베이스 연결시 SQL언어가 아닌 해당 개발언어로 데이터베이스를 접근할 수 있도록 도와주는 툴이다.


# @Query
JPA에서 **사용자 정의 쿼리**를 사용하는 방법은

1. Named Query

2. 쿼리 메소드

3. @Query 어노테이션

이 있다.

그 중 이번에는 @Query 어노테이션을 사용할 것이다. `@Query`어노테이션은 SQL과 유사한 JPQL(Java Persistence Query Language)라는 객체지향 쿼리 언어로 복잡한 쿼리 처리를 지원해준다.

즉 @Query는 개발자가 직접 복잡한 쿼리문을 작성할 때 필요한 어노테이션이다. 이때 사용되는 것이 `JPQL`라는 쿼리가 들어간다.

> 쿼리 메소드는 메소드의 이름으로 우리가 원하는 기능을 수행할 쿼리가 자동으로 생성되게 도와준다.


# JPQL
JPQL은 JPA가 지원하는 다양한 방법 중하나로 `엔티티 객체`를 대상으로 쿼리문을 작성한다.

JPQL의 문법은 SQL가 매우 유사하여 SQL을 알고 있다면 보다 쉽게 이해할 수 있다.[참고](https://wonit.tistory.com/470)

```sql
select _ from _ [where] _ [groupby] _ [having] _ [orderby] _

update _ [where] _

delete _ [where] 
```

@Query는 JpaRepository를 상속 받는 인터페이스에서 사용한다. 

```java
@Repository
public interface CommentBoardRepository extends JpaRepository<CommentBoard, Long> {
    Optional<CommentBoard> findById(Long id);
    @Query("쿼리문")
    Page<CommentBoard> findAllByBoard(Board board, Pageable pageable); // 페이지 정렬
}
```

예를 들어보면 

CommentBoard라는 Entity에는 아래와 같은 속성이 존재한다.

```java
public class CommentBoard extends BaseTimeEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "command_id")
    private Long id;

    private String content;

    @ManyToOne(fetch = FetchType.LAZY) // detail의 openPop때문에 eager로 바꿈
    @JoinColumn(name = "user_id")
    private Member writer;

    @ManyToOne(fetch =  FetchType.LAZY)
    @JoinColumn(name = "board_id")
    private Board board;
            
    @ManyToMany
    private Set<Member> vote;
}
```

```java
public abstract class BaseTimeEntity {
    // 수정시간, 생성시간.

    @CreatedDate //엔티티가 생성되어 저장할때 자동으로 저장.
    @Column(updatable = false) //업데이트를 안함
    private LocalDateTime createDate;

    @LastModifiedDate // 변경할때 시간을 자동으로 저장.
    private LocalDateTime updateDate;
}

```

가 있다고 했을 때, 만약 해당 정보를 createDate에서 최신순으로 정렬을 하고 싶다고 한다면, 다음 JPQL은 다음과 같이 사용될 것이다.

```java
@Repository
public interface CommentBoardRepository extends JpaRepository<CommentBoard, Long> {
    Optional<CommentBoard> findById(Long id);
    @Query("SELECT c "
            + "FROM CommentBoard as c "
            + "ORDER BY c.createDate DESC ")
    Page<CommentBoard> findAllByBoard(Board board, Pageable pageable); // 페이지 정렬
}
```


# 공지글 부분

현재 내 게시글 목록 부분은 자유와 공지글이 최신순으로 정렬되어있다.

![1](/assets/img/springboot/basis/1.png)

해당 공지글을 우선적으로 정렬하기 위해서는 

1. 공지글끼리 최신 순으로 정렬 

2. 자유글끼리 최신 순으로 정렬

3. 1번과 2번 `UNION`

4. Service에서 3번을 반환

라고 생각 하고 진행했다.

## Board

```java

public class Board extends BaseTimeEntity{
    
    (생략 ...)

    private Boolean noticeYn;

    (생략 ...)
}
```

```java
public abstract class BaseTimeEntity {
    // 수정시간, 생성시간.

    @CreatedDate //엔티티가 생성되어 저장할때 자동으로 저장.
    @Column(updatable = false) //업데이트를 안함
    private LocalDateTime createDate;

    @LastModifiedDate // 변경할때 시간을 자동으로 저장.
    private LocalDateTime updateDate;
}

```

`noticeYn`은 공지글이면 False 그것이 아니면 True의 값을 가지고 있다. 


## BoardRepository

앞서 언급했다시피 @Query는 JpaRepository에서 사용하기 때문에 게시글 정보를 가지고 있는 `BoardRepository`에서 쿼리문을 작성한다.

```java
@Repository
public interface BoardRepository extends JpaRepository<Board, Long> {
    
    (생략 ...)

     @Query(" SELECT b "
            + "FROM Board as b "
            + "WHERE b.noticeYn = true "
            + "order by b.createDate desc  union "
            + "SELECT b "
            + "from Board as b "
            + "WHERE b.noticeYn =false "
            + "order by b.createDate desc "
    )
    Page<Board> findAll(Pageable pageable);
    (생략 ...)
}
```

![2](/assets/img/springboot/basis/2.png)

<br><br>

???

<br><br>

At 1:90 and token 'descunion', mismatched input 'descunion', expecting one of the following tokens 라는 오류가 발생했다.

찾아보니 `UNION`은 지원을 안한다고 한다. ㅠㅠ

그래서 다른 방법으로 공지글, 자유글 따로 쿼리문을 작성하고 두 개의 list문을 합치는 방법을 적용해보자.

```java

@Repository
public interface BoardRepository extends JpaRepository<Board, Long> {
    
    (생략 ...)

    @Query(" SELECT b "
            + "FROM Board as b "
            + "WHERE b.noticeYn = true "
            + "order by b.createDate desc  "
    )
    Page<Board> findByNoticeYnTrueOrderByCreateDateDesc(Pageable pageable);

    @Query(  "SELECT b "
            + "from Board as b "
            + "WHERE b.noticeYn =false "
            + "order by b.createDate desc ")
    Page<Board> findByNoticeYnFalseOrderByCreateDateDesc(Pageable pageable);

    (생략 ...)
}
```
먼저, `findByNoticeYnTrueOrderByCreateDateDesc`는 공지글만 따로 가져온다. 그리고 해당 글을 `createData`속성 기준으로 최신 순으로 정렬하는 쿼리문이고

`findByNoticeYnFalseOrderByCreateDateDesc`는 자유글 기준으로 최신순을 정렬한 쿼리문이다.

## BoardService
```java
 public Page<Board> getListNoticeTrue(int page){
        List<Sort.Order> sorts = new ArrayList<>();
        sorts.add(Sort.Order.desc("createDate"));
        Pageable pageable = PageRequest.of(page, 10, Sort.by(sorts));

        Page<Board> noticeTrue = boardRepository.findByNoticeYnFalseOrderByCreateDateDesc(pageable);
        return noticeTrue;
    }

    public Page<Board> getListNoticeFalse(int page){
        List<Sort.Order> sorts = new ArrayList<>();
        sorts.add(Sort.Order.desc("createDate"));
        Pageable pageable = PageRequest.of(page, 10, Sort.by(sorts));

        Page<Board> noticeFalse = boardRepository.findByNoticeYnTrueOrderByCreateDateDesc(pageable);
        return noticeFalse;
    }

```
## BoardController

```java

@GetMapping("/list")
    public String getBoardList(Model model,
                               @RequestParam(value = "page", defaultValue = "0")int page) {
        Page<Board> pagingTrue = boardService.getListNoticeFalse(page);
        Page<Board> paging = boardService.getListNoticeTrue(page);
        model.addAttribute("paging", paging);
        model.addAttribute("pagingTrue", pagingTrue);
        return "board/List";
    }
```

## HTML

```html
            (생략 ...)
            <tbody id="list">
                    <tr th:each="board, loop: ${pagingTrue}">
                        <td scope="col"><input type="checkbox"/></td>
                        <td th:if="${board.noticeYn}" th:text="공지"></td>
                        <td class="text-start">
                            <a th:href="@{|/board/detail/${board.id}|}" th:text="${board.subject}"></a>
                            <span th:if="${#lists.size(board.commentBoardList) != 0}" th:text="|+${#lists.size(board.commentBoardList)}|"></span>
                        </td>
                        <td th:text="${board.writer.nickName}"></td>
                        <td th:text="${#temporals.format(board.createDate, 'yyyy-MM-dd HH:mm') }"></td>
                        <td th:text="${board.view}"></td>
                    </tr>

                    <tr th:each="board, loop: ${paging}">
                        <td scope="col"><input type="checkbox"/></td>
                        <td th:unless="${board.noticeYn}" th:text="${paging.getTotalElements - (paging.number * paging.size) - loop.index}"></td>
                        <td class="text-start">
                            <a th:href="@{|/board/detail/${board.id}|}" th:text="${board.subject}"></a>
                            <span th:if="${#lists.size(board.commentBoardList) != 0}" th:text="|+${#lists.size(board.commentBoardList)}|"></span>
                        </td>
                        <td th:text="${board.writer.nickName}"></td>
                        <td th:text="${#temporals.format(board.createDate, 'yyyy-MM-dd HH:mm') }"></td>
                        <td th:text="${board.view}"></td>
                    </tr>
                </tbody>
                 (생략 ...)
```