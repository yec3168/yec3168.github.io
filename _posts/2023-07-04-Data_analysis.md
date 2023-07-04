---
title: Data Analysis 1
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-07-04
categories: [Data_Analysis, basis]
tags: [Data Analysis, encoding, chardet]
math: true  # mathematical 기능
mermaid: true  # 표생성 도구
pin: true  # 홈페이지 메인화면에 게시물 고정
#image : /path/to/image-file 포스트 최상단에 이미지를 넣고 싶을때
---

# Information
시작하기 앞서 [혼자 공부하는 데이터분석](https://hongong.hanbit.co.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B6%84%EC%84%9D-with-%ED%8C%8C%EC%9D%B4%EC%8D%AC/)책을 보고 공부했습니다.

`목표`로는 이 책을 어느정도 공부하면서 **스스로 데이터를 찾아서 분석해보는것**으로 세우고 시작하겠습니다.

# 데이터 분석(Data Analysis)
데이터 분석은 단순히 한 마디로 정의하기는 어렵다. 물론 사전적인 의미로는 데이터는 자료, 정보를 의미하고 분석은 복자한 대상을 정확하게 이해하기위해 단순한 요소로 나누어 설명하는것을 의미하게 된다.

위키피디아에서 정의된 `데이터 분석`은 **유용한 정보를 발견하고 결론을 유추하거나, 의사 결정을 돕기 위해 데이터를 조사, 정제, 변환하여 모델링 하는 과정**을 말한다.

데이터 분석과 함께 자주 언급되는 용어에는 `데이터 과학`이다. 보통은 데이터 분석과 데이터 과학은 동일시 취급되지만, 정확히는 데이터 과학은 `통계학`, `머신러닝`, `데이터마이닝`등 같은 큰 개념으로 이루어져있다.


# 데이터 과학(Data Science)
데이터 과학자 `지 리`가 말하길 데이터 과학은 **데이터 세계와 비즈니스 세계를 이어주는 다리 역할**이다. 데이터 과학을 통해 소프트웨어나 제품은 개발할 수 있지만 이것이 전부가 아니며, 머신러닝, 통계학, 그래프 그리기 등 많은 것을 포함하는 분야이다.

>데이터 분석
: 올바른 의사결정을 돕기위한 `통찰`에 집중함
{: .prompt-info}
>데이터 과학
: 한 걸음 더나아가 문제 해결을 돕기아한 최선의 `솔루션`을 제공해준다.
{: .prompt-info}

# 언어
데이터 분석에 사용되는 언어는 `Python`, `R` 이다. 물론 데이터가 `데이터베이스` 형태로 존재한다면, `SQL`도 사용할 수 있지만 SQL은 시각화나 통계적인 분석에 어려움이 존재한다.

이 책에서 사용되는 언어는 `Python`이며, 사용되는 프로그램은 `구글 Colab`을 사용하지만 `Visual studio Code`가 편하기에 Code로 사용하기로 한다.

# 환경만들기
[Visual Studio Code](https://code.visualstudio.com/)홈페이지에 들어가면 아래와 같은 사진이 나와 다운로드를 눌러 설치를 진행하면된다.

![install 사진](/assets/img/data_analysis/basis/1/visual_install.png)


제대로 다운이 완료되고 실행시켜보면 사진처럼 나오게 된다.

![install 사진](/assets/img/data_analysis/basis/1/visual_install2.png)

여기서 폴더열기를 누르고 `C:\Users\사용자 이름\Documents\`위치에  `Data_Analysis`라는 새로운 폴더를 만들고 `폴더 선택`을 누르면 된다.

![install 사진](/assets/img/data_analysis/basis/1/visual_install3.png)


무사히 마치면 확장으로 들어가 마켓플레이스에서 python을 설치하자 가장 최신버전으로 설치하면된다.

# 데이터 세트 받아오기
폴더를 생성했으면 데이터 분석에 사용할 데이터를 받아와야한다. 책에서 사용하는 데이터 세트는 [정보나루](https://www.data4library.kr)에 있는 `서울특별시교육청남산도서관 장서/대출 데이터`이다.

![데이터 세트 사진](/assets/img/data_analysis/basis/1/data_set.png)

위 사진처럼 데이터 제공에 들어가 검색창에 `남산` 검색하고 해당 데이터를 `CSV`형태로 다운받자.

다운받은 파일은 이전에 만들었던 `Data_Analysis`폴더에 넣자.

> 더 많은 데이터들을 찾고 싶다면 [공개 데이터 세트](#공개-데이터-세트) 참고.
{: .prompt-info}

# Test.ipynb
이제 Data_Analysis폴더에 파일을 하나 만들어보자. 이름은 `test.ipynb`로해서 만들면 된다.

![test 사진](/assets/img/data_analysis/basis/1/test.png)

## 파일 열기
CSV 파일은 텍스트 파일이므로 `open()`함수를 통해 읽을 수 있다.

```python
with open('./data/서울특별시교육청남산도서관 장서 대출목록 (2023년 06월).csv') as f:
    print(f.readline())
```
open 파일은 `UTF-8` 형식으로 저장되어있다고 가정하지만, 다운받은 CSV 파일의 인코딩 방식이 다른거 같다. 

## 파일 인코딩 확인하기
`chardet 패키지`의 `chardet_detect()`함수는 파일의 인코딩 방식을 확인한다.

```python
import chardet
with open('./data/서울특별시교육청남산도서관 장서 대출목록 (2023년 06월).csv', mode='rb') as f:
    d = f.readline()
print(chardet.detect(d))
```
{'encoding': 'EUC-KR', 'confidence': 0.99, 'language': 'Korean'}

인코딩 결과를 보면 `EUC-KR`로 되어있는걸로 볼 수 있다.

## 인코딩 형식 저장하기

```python
with open('./data/서울특별시교육청남산도서관 장서 대출목록 (2023년 06월).csv', encoding="EUC_KR") as f:
    print(f.readline())
```

# 마무리
다음으로는 판다스를 통해 CSV 파일을 데이터프레임이라는 표 형식 형태로 만든 것과 사용법에 대해 알아보도록 하겠다.

# 공개 데이터 세트
## 국내 사이트
- [공공데이터](https://www.data.go.kr)
: 행정 안전부가 제공하는 공공데이터 통합 제공시스템
- [통합데이터 지도](https://bigdata-map.kr)
: 국가와 민간분야에서 운영 중인 여러 빅데이터 플랫폼의 데이터를 한곳에서 검색할 수 있는 서비스
- [AI 허브](https://aihub.or.kr)
: AI 통합 플랫폼으로 다양한 AI학습용 데이터를 제공
- [국가통계포털](https://kosis.kr)
: 경제, 사회, 환경 등 30개의 분야의 국내외 통계 데이터를 찾을 수 있음.

## 해외사이트
- [구글 데이터 세트 검색](https://datasetsearch.research.google.com)
: 무료로 사용할 수 있는 데이터를 제공하는 검색엔진
- [캐글 데이터 세트](https://kaggle.com/datasets)
: 머신러닝 경진 대회 플랫폼으로, 경진대회에서 사용한 데이터셋을 확인할 수 있다.
- [위키피디아 머신러닝데이터 세트](https://en.wikipedia.org/wiki/List_of_datasets_for_machine_learning-research)
: 머신러닝 연구에 널리 사용되는 데이터 세트가 주제별로 정리되어 있다.
- [아마존 웹서비스 오픈데이터](https://registry.opendata.aws)
- [UCI 머신러닝 데이터 저장소](https://archive.ics.uci.edu/ml)

## 온라인포털
- [데이터 분석 커뮤니티](https://www.facebook.com/groups/datacommunity)
- [캐글 코리아](https://www.facebook.com/groups/KagglekoreaOpenGroup)
- [탠서플로 코리아](https:///www.facebook.com/groups/TensorFlowKR)
- [파이토치 코리아](https://www.facebook.com/groups/PyTorchKR)
