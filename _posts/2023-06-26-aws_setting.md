---
title: AWS Setting
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-06-26
categories: [Project, tacotron2]
tags: [tacotron2, Proeject, AWS]
math: true  # mathematical 기능
mermaid: true  # 표생성 도구
pin: true  # 홈페이지 메인화면에 게시물 고정
#image : /path/to/image-file 포스트 최상단에 이미지를 넣고 싶을때
---


## AWS
AWS(Amazon Web Services)란 [Amazon.com](https://www.amazon.com/)에서 제공하는 '클라우딩 컴퓨터'이다.
네트워킹을 기반으로 가상 컴퓨터와 스토리징 등 다양한 서비스를 누릴 수 있으며, 웹 서비스를 사용하여 기업이 애플리케이션 및 서비스를 쉽게 확장하고 실행할 수 있도록 해준다.

<br>

## 클라이딩 컴퓨터
컴퓨터 리소스를 인터넷을 통해 서비스로 사용할 수 있는 주문형 서비스이다. 직접 관리할 필요 없이 사용한만큼 비용만 지불하면 자유롭게 사용가능하다.

간단하게 말하면 사용자가 대여 컴퓨팅 서비스를 요청하고 접근하는 클라우딩 플랫폼에 연결한다. `중앙서버`는 클라이언트와 서버의 모든 통신을 처리하며 데이터 교환을 가능하게 한다.  

<br>

## AWS Instance Create
인스턴스 생성과 보안그룹 생성은 [AWS 인스턴스 생성](https://velog.io/@yec3168/AWS)과[AWS 보안그룹 생성](https://velog.io/@yec3168/AWS-%EB%B3%B4%EC%95%88%EA%B7%B8%EB%A3%B9)을 보기를 권한다.


<br>

## AWS 연결
### 1. EC2 인스턴스 연결
임시 SSH키를 생성하여 인스턴스에 연결하는 방식

자신의 인스턴스를 마우스에 올려 우클릭을 누른다.
그러면 아래 사진처럼 나올탠데 여기서 `연결`을 눌러준다.

![AWS_Access](/assets/img/tacotron2/aws/aws_access.png);


누르게되면 아래 사진처럼 나올 것이다.
마찬가지로 `연결`을 눌러준다

![AWS_Access](/assets/img/tacotron2/aws/aws_access1.png)

>사용자 이름에 `ec2-user`는 인스턴스를 시작할때 AMI에 정의된 사용자 이름으로 ssh로 연결시 `ec2-user`를 입력하여 연결한다.
{: .prompt-tip }

![AWS_Access](/assets/img/tacotron2/aws/aws_access2.png)

위와 같은 화면이 나오면 성공이다.

<br>
### 2. ssh 연결

이 방식은 ssh 키페어로 인증하는 방식이다. 이 방식을 사용하기 위해서는 보안그룹의 수정이 필요한데 [ssh 보안그룹 설정](https://leveloper.tistory.com/17)를 참고해서 작성하면 된다.


그 외 접속하는 방법은 [putty를 이용한 접속 방법](https://bbeomgeun.tistory.com/73)을 참고해서 해보자.


<br>
<br>

## AWS 환경설정

### apt-get update

```bash
~$ sudo apt-get update
```

### python install

```bash
~$ sudo apt-get upgrade python3
```

```bash
~$ sudo apt install python3-pip
```


### pip install

플라스크 웹 어플리케이션을 만들기 위한 가상환경 설치
```bash
~$ sudo apt-get install python3-virtualenv
```

### FLASK install

```bash
~$ pip3 install Flask

~$ python3 -m flask --vesion
```

### flask-ngork install
```bash
~$ pip3 install flask-ngrok
```


### python3-numpy install
```bash
~$ sudo apt-get install python3-numpy
```

### pyTorch
```bash
~$ pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

>[torch]는 홈페이지에 자세히 나와있음.
{: .prompt-tip}


### 권장드라이버 자동설치
```bash
~$ sudo ubuntu-drivers autoinstall

~$ pip3 install numpy scipy librosa unidecode inflect librosa
```


## 내 PC -> EC2에 파일을 업로드 하는법

아래 명령어는 ec2에서 입력하는 것이 아닌 `cmd`에 입력하면 된다.
```bash
scp -i [키위치] [파일명] [EC2 Host이름]@[EC2 Public ip]:[받는 위치]

```

