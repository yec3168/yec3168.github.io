---
title: Data Analysis 2(API)
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-07-04
categories: [Data_Analysis, basis]
tags: [Data Analysis, API]
---

# API
- 인증된 URL만 있으면 언제든지 필요한 데이터에 접근할 수 있는 방식.
- 보통 `CSV`, `JSON`, `XML`을 사용한다. 


# JSON
파이썬의 `Dictionary` + `List` 해놓은 것

```python
d ={"name": "Eungchan",
    "age": "25"}

print(d["age"])
```
-> 25

## json.dumps()
파이썬의 json패키지를 사용해서 dictionary를 json형식에 맞는 문자열로 바꿔줌.

아까만든 dictionary인 d를 JSON 형식에 맞는 문자열로 변환해보자.

1. json 패키지 import

```python
import json
```

2. json.dumps사용

```python
d_str = json.dumps(d, ensure_ascii = False)

print(d_str)
```
-> {"name": "Eungchan", "age": "25"}

결과 처럼 `JSON`형식의 문자열이 된 것을 볼 수 있다.

> `ensure_ascii`를 False로 한 이유는 json은 아스키코드값만 출력하기 때문에 한글이 제대로 보이지 않는다. ensure_ascii값을 False로 준다면, 원래 저장된 문자 그대로 출력하게 해준다.
{: .prompt-tip}

## type && json.loads
```python
print(type(d_str))
```
-> <class 'str'>

type을 보니 str로 문자열이 된 것을 볼 수 있다.

```python
load_str = json.loads(d_str)
print(load_str["name"])
```
-> Eungchan

`json.loads`를 이용하여 d_str문자열을 다시 `dictionary`형태로 바꿀 수 있다.

```python
print(type(load_str))
```
-> <class 'dict'>


## Code
dictionary를 만들고 json.dumps를 사용해서 문자열로 바꾸고 다시 json.load로 바꾸니 굉장히 복잡하니 json 문자열을 바로 `json.loads`에 전달해보자.

```python

d_str2 = json.loads('{"name": "Eungchan", "age" : 25}')

print(d_str2["name"])
print(d_str2["age"])
```
-> Eungchan<br>
   25


## JSON 배열

`세겹따옴표(""")`를 사용해서 여러 줄에 걸친 문자열을 만들 수 있다.

```python
d_str3 = """
[
    {"name": "Eungchan", "age" : 25},
    {"name": "Data", "age" : 99}
]
"""

d4 = json.loads(d_str3)
print(d4[1]["name"])
```
-> Data

## JSON -> DataFrame (pd.read_json)

```python
import pandas as pd
df2 = pd.read_json(d_str3)

df2.head(2)
```

# XML
```xml
<book>
    <name>혼자 공부하는 데이터분석</name>
    <author>박해선</author>
</book>
```
계층 구조를 이루면서 정보를 표현한다. 시작 태그와 종료 태그로 감싼다. 

## XML을 python으로 표현

```python
xml_str ="""
<book>
    <name>혼자 공부하는 데이터분석</name>
    <author>박해선</author>
</book>
"""
```

## XML 패키지 && fromstring

```python
import xml.etree.ElementTree as et
book = et.fromstring(xml_str)

print(book)
```

fromstring으로 ElementTree 객체를 반환한다.

## 부모 element (tag) && 자식 element(findtext)
```python
print(book.tag)
```
-> book

```python
book_childs = list(book)
print(book_childs)
```
-> [<Element 'name' at 0x0000019D006B7770>, <Element 'author' at 0x0000019D006B7CC0>]


<br><br>

# 데이터 찾기
[정보나루](https://www.data4library.kr)에 들어가서 API를 다운받기 위해 인증키 신청을 해보자.

회원가입하고 원하는