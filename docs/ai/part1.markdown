---
layout: default
title: 판다스 자료구조
parent: Python Pandas
grand_parent: AI
permalink: /docs/ai/py-pandas/part1/
nav_order: 1
---

# 판다스 자료구조

판다스는 시리즈와 데이터프레임이라는 구조화된 데이터 형식을 제공한다. 이 둘은 파이썬 클래스로 만들어지고, 서로 다른 종류의 데이터를 한 곳에 담는 역할을 한다. 둘의 차이점은 시리즈는 1차원 배열이고, 데이터프레임은 2차원 배열이라는 것이다.

## 시리즈

시리즈는 데이터가 순차적으로 나열된 1차원 배열의 형태를 갖는다.<br>
시리즈의 인덱스는 데이터 값의 위치를 나타내는 데이터 주소이다. 따라서 '인덱스1'이라는 주소를 안다면 'Data1'이라는 데이터 값에 바로 접근이 가능하다.

<img src = "https://media.vlpt.us/images/dustin/post/59f9cae3-be4e-4d80-b429-6057b590d481/img1.daumcdn.png" width="500px">

### 시리즈 만들기

