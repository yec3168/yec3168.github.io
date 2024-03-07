---
title: "Failed to execute 'send' on 'XMLHttpRequest': Failed to load Error 해결"
author:
name: Eungchan
link: https://github.com/yec3168
date: 2024-03-07
categories: [SpringBoot, Error]
tags: [SpringBoot, BoardProject, Error, ajax, XMLHttpRequest]
---

# Failed to execute 'send' on 'XMLHttpRequest'
Springboot 프로젝트 진행 중 `ajax`를 이용하여 통신을 진행하다 아래와 같은 오류가 발생했다.

![오류메세지](/assets/img/springboot/error/1/errormsg.png)


## 1. async:false

`XMLHttpRequest` 오류는 일부 브라우저 업데이트 후 며칠 후에 발생하는 오류라고 한다. [참고](https://stackoverflow.com/questions/32878613/networkerror-failed-to-execute-send-on-xmlhttprequest)

최신 브라우저가 비동기가 아닌 요청을 거부하는 것으로 ajax통신시 `async:false`로 설정하면 된다고 한다. 

![solve1](/assets/img/springboot/error/1/solve1.png)


하지만, 내 경우는 해결되지 않았다. 다른 방법을 찾아보자.

## 2. ErrorMsg check

![solve2](/assets/img/springboot/error/1/solve2.png)

controller를 확인해보면 `CommentBoardFormDto`를 return 하게 되는데, CommentBoardFormDto에는 Member객체인 writer가 있다. 

![solve2-1](/assets/img/springboot/error/1/solve2-1.png)

이렇게 보면 큰 문제가 없어보이지만, console창에 오류를 확인해보면

> 2024-03-07T22:23:13.753+09:00 ERROR 5164 --- [nio-9090-exec-3] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed: org.springframework.http.converter.HttpMessageNotWritableException: Could not write JSON: `Infinite recursion `(StackOverflowError)] with root cause

아래와 같이 무한반복하는 부분이 존재한다고 나온다. debug를 통해 객체에는 값이 아래 사진과 같이 무한반복하다보니 `stack overflow`가 발생하여 값이 null 나오는 문제가 발생했다.

![middle](/assets/img/springboot/error/1/middle.png)

그래서 Member객체를 사용하는 것이 아닌 String으로 교체해야한다.

## 해결

새로 Member객체를 사용하는 것이 아닌 String을 사용하는 CommentUpdateForm을 생성하고, 해당 정보를 controller를 통해 저장하여 사용한다.

### CommentUpdateForm

```java
@Getter
@Setter
public class CommentUpdateForm {

    private String content;

    private String nickname;
}
```

### Controller

```java
    @GetMapping("/update/{id}")
    @ResponseBody
    public Object updateComment(@PathVariable("id") Long id){
        CommentBoardFormDto commentBoardFormDto = CommentBoardFormDto.toDto(commentBoardService.getComment(id));
        CommentUpdateForm commentUpdateForm = new CommentUpdateForm();
        commentUpdateForm.setContent(commentBoardFormDto.getContent());
        commentUpdateForm.setNickname(commentBoardFormDto.getWriter().getNickName());
        return commentUpdateForm;
    }
```

Controller에서는 CommentUpdateForm에 FormDto값을 넣어준다.

![result2](/assets/img/springboot/error/1/result2.png)


# 정리

2시간동안 async 동기 오류인 줄 알고 구글에 검색하면서 삽질했지만 진짜 오류는 console에 자세히 나와있었다. 다음에는 debug를 좀 더 활용하고 console을 중심적으로 확인하는 방향으로 나아가보자.