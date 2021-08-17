---
layout: default
title: 데이터 입출력
parent: Python Pandas
nav_order: 2
---

# 데이터 입출력

판다스에서는 다양한 형식의 외부 파일을 읽어와 데이터프레임으로 변환하는 함수를 제공한다. 외부 파일을 데이터프레임으로 변환하면 판다스의 모든 기능을 사용할 수 있다. 반대로 데이터프레임을 여러 형식의 파일로 저장할 수도 있다.<br>
아래 표는 판다스에서 제공하는 입출력 도구이다.

|FileFormat|Reader|Writer|
|------|---|---|
|CSV|read_csv|to_csv|
|JSON|read_json|to_json|
|HTML|read_html|to_html|
|Local clipboard|read_clipboard|to_clipboard|
|MS Excel|read_excel|to_excel|
|HDF5 Format|read_hdf|to_hdf|
|SQL|read_sql|to_sql|

(마음 같아선 데이터셋이랑 결과랑 촥 예쁘게 하고싶은데, 하 너무 힘들다. 최대한 내 말로 푸는 데에 집중하겠삼)

## CSV

csv 파일은 데이터 값의 열을 쉼표로 구분하고 줄바꿈으로 행을 구분한다.

### CSV 파일 -> 데이터프레임 : read_csv

```py
pandas.read_csv("파일 경로")
```

header 옵션은 데이터프레임의 열 이름으로 사용할 행을 지정한다.
```py
# 데이터 중 '열 이름'이 되는 행이 없음
pandas.read_csv("파일경로", header=None)

# 1번째 행을 '열 이름'으로 지정 (0번째 행은 사라짐)
pandas.read_csv("파일경로", header=1)
```

index_col 옵션은 데이터프레임의 행 인덱스가 되는 열을 지정한다.
```py
# 인덱스 지정하지 않음 (= default)
pandas.read_csv("파일경로", index_col=None)

# "c0"열을 행 인덱스로 지정
pandas.read_csv("파일경로", index_col="c0")
```

csv파일에 따라 쉼표 대신 탭이나 공백으로 텍스트를 구분하기도 한다. 이때 구분자 옵션(sep 또는 delimiter)을 변경해야 한다.<br>
이 외에도 names(열 이름으로 사용할 문자열 리스트), skiprows(처음 몇 줄을 건너 뛸건지 설정, 행 번호 담긴 리스트로 설정 가능) 등의 옵션들이 있다.

## JSON

json파일은 데이터 공유 목적으로 개발된 특수한 파일 형식이다. 파이썬 딕셔너리와 유사한 'key : value' 구조를 갖는다.

### JSON 파일 -> 데이터프레임 : read_json

```py
pandas.read_json("파일 경로")
```

## Excel

excel파일의 행과 열은 데이터프레임의 행, 열로 일대일 대응된다.

### Excel 파일 -> 데이터프레임 : read_excel

read_excel() 함수 사용법은 read_csv() 함수와 비슷하다. header, indec_col 등 대부분의 옵션을 그대로 사용할 수 있다.

```py
pandas.read_excel("파일 경로")
pandas.read_excel("파일 경로", header=None)
```

# Web에서 데이터 수집하기

## HTML 웹 페이지에서 표 속성 가져오기

```py
pandas.read_html("url 또는 html 파일 경로")
```

## 웹 스크래핑

BeautifulSoup 등 웹 스크래핑 도구로 수집한 데이터를 파이썬 리스, 딕셔너리 등으로 정리한 뒤 DataFrame() 함수에 전달하여 데이터프레임으로 변환한다.
  
  
  
  
