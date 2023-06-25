---
title: Chirpy에 utterances 댓글 연동하기
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-06-21
categories: [Blog, ufferance]
tags: [blogging, guide]
math: true  # mathematical 기능
mermaid: true  # 표생성 도구
pin: true  # 홈페이지 메인화면에 게시물 고정
#image : /path/to/image-file 포스트 최상단에 이미지를 넣고 싶을때
---
  

## 사전 준비
  

> Github 계정
> 자신의  Gitblog

  
## 설명

 우리가 사용하는 Chirpy는 [Disqus](https://disqus.com/)를 기본적으로 지원한다.
  
 계정정보만 연동하면 잘 나오지만, 무료 기능들이 유로화되어 무료로 지원해주는 [Utterances](https://utteranc.es/)를 이용하여 댓글 기능을 만들어보자.

 
## Utterance 설치
 

[Utterance](https://github.com/apps/utterances)홈페이지에 접속하면 아래와 같은 사진이 나온다.

![utterance](/assets/img/utterance/utterance.png)
 

오른쪽 위 `install`버튼을 클릭하여 설치를 진행한다.

그러면 아래 사진 처럼 옵션이 두 개가 나오는데, 그중 `Only select repositories`버튼을 클릭한다.
  

![utterance](/assets/img/utterance/utterance2.png)

그 다음 자신의 `Repository`를 선택하고 install 버튼을 클릭한다.

## Configuration and Enable Utterances
`install`이 끝나면 홈페이지가 나오는데 아래로 내려보면, 중간에 `Configuration` 섹션이 나온다.

여기에 나의 저장소를 입력하는 부분이 있는데 
입력 형식은 `[저장소ID]/[저장소명]`이다.

아래의 자신의 것을 입력한다.
저는 `yec3168/yec3168.github.io`를 입력하였습니다.

![utterance](/assets/img/utterance/utterance3.png)

그 후 조금 더 내려보면 `Enable Utterance`섹션이 나온다.
여기에 자바스크립트 섹션에서 `repo`에 적힌 주소가 자신의 저장소가 맞으면 이를 복사한다.
![utterance](/assets/img/utterance/utterance4.png)


## Chirpy에 복사한 자바스크립트 입력

아까 복사한 `Scripts`를 `_layouts/post.html`'에 그대로 넣어준다.

>위치는 파일 맨 아래에 추가한다.
{: .prompt-info }

![utterance](/assets/img/utterance/utterance5.png)


## 결과
마지막으로  댓글 시스템이 각 post에 추가가 됐는지 확인해 보자.

Ruby를 통해 로컬로 확인해보자
<http://127.0.0.1:4000>
![utterance](/assets/img/utterance/utterance_result.png)

문제 없이 잘 나오는것을 확인할 수 있고,
![utterance](/assets/img/utterance/utterance_preview.png)

`Preview`도 지원하는 것을 볼 수 있다.


# 출처
위 정보는 [하얀눈길](https://www.irgroup.org/posts/utternace-comments-system/) 와 [jaejae0015](https://jaejae0015.github.io/)블로그를 참고해서 만들어졌습니다.