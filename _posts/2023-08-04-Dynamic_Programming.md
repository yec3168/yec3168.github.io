---
title: Dynamic Programming
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-08-04
categories: [algorithm, Dynamic_programming]
tags: [Data algorithm, Dynamic_programming]
---


# **Dynamic Programming**
큰 문제를 작은 문제로 나누어서 푸는 문제로 한번 계산한 문제는 다시 계산하지 않도록 하는 알고리즘이다.

`Dynamic Programming`에는 2가지 조건을 만족해야 사용할 수 있다.
  1. **중복되는 부분 문제**<br>
  동일한 작은 문제가 반복적으로 해결해야하는 경우.
  
  예시를 들어보면 피보나치 수열문제가 있다.

  ![피보나치](/assets/img/algorithm/dp/피보나치.png)

  그림으로 보면 f(3), f(2), f(1)같이 중복되어 계산되는 문제가 나타나게된다. 

  2. **최적 부분 구조**<br>
  부분문제의 최적 결과 값을 사용하여 전체 문제의 최적인 결과를 낼 수 있어야한다.

 