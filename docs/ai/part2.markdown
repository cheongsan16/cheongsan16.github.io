---
layout: default
title: 데이터 입출력
parent: Python Pandas
grand_parent: AI
permalink: /docs/ai/py-pandas/part2/
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

### 데이터프레임 -> CSV 파일 : to_csv

```py
df.to_csv("파일 이름(경로)")
```

## JSON

json파일은 데이터 공유 목적으로 개발된 특수한 파일 형식이다. 파이썬 딕셔너리와 유사한 'key : value' 구조를 갖는다.

### JSON 파일 -> 데이터프레임 : read_json

```py
pandas.read_json("파일 경로")
```

### 데이터프레임 -> JSON 파일 : to_json

```py
df.to_json("파일 이름(경로)")
```

## Excel

excel파일의 행과 열은 데이터프레임의 행, 열로 일대일 대응된다.

### Excel 파일 -> 데이터프레임 : read_excel

read_excel() 함수 사용법은 read_csv() 함수와 비슷하다. header, indec_col 등 대부분의 옵션을 그대로 사용할 수 있다.

```py
pandas.read_excel("파일 경로")
pandas.read_excel("파일 경로", header=None)
```

### 데이터프레임 -> Excel 파일 : to_excel

to_excel() 함수를 사용하려면 openpyxl 라이브러리를 설치해야한다.

```py
df.to_excel("파일 이름(경로)")
```

여러 개의 데이터프레임을 하나의 Excel 파일로 저장할 수도 있다.<br>
판다스 ExcelWriter() 함수는 Excel 워크북 객체(Excel파일)를 생성한다. 데이터프레임에 to_excel() 함수를 적용할 때 삽입하려는 워크북 객체(Excel파일)를 인자로 전달한다.
sheet_name 옵션에 Excel 파일의 시트 이름을 입력하면 삽입되는 시트 위치를 지정할 수 있다. 시트 이름을 다르게 설정하면, 같은 Excel 파일의 서로 다른 시트에 여러 데이터프레임이 구분하여 저장된다.

```py
df1 = pandas.DataFrame(data)
df2 = pandas.DataFrame(data)
# data 및 과정 생략
writer = pandas.ExcelWriter("excel 파일 경로")
df1.to_excel(writer, sheet_name="sheet1")
df2.to_excel(writer, sheet_name="sheet2")
writer.save()
```

# Web에서 데이터 수집하기

## HTML 웹 페이지에서 표 속성 가져오기

read_html() 함수는 html 웹 페이지에 있는 <table> 태그에서 표 형식의 데이터를 모두 찾아 데이터프레임으로 변환한다. 표 데이터들은 각각 별도의 데이터프레임으로 변환되기 때문에 여러 개의 데이터프레임을 원소로 갖는 리스트가 반환된다.

```py
pandas.read_html("url 또는 html 파일 경로")
```

## 웹 스크래핑, 데이터베이스 가져오기

BeautifulSoup 등 웹 스크래핑 도구로 수집한 데이터를 파이썬 리스트, 딕셔너리 등으로 정리한 뒤 DataFrame() 함수에 전달하여 데이터프레임으로 변환한다.

데이터베이스에서 판다스 데이터프레임으로 변환은 read_sql() 함수를 이용하면 가능하다. 읽어온 데이터는 데이터프레임 포맷으로 저장된다.
  
## API 활용
  
대부분의 API는 csv, json, xml 등 판다스에서 쉽게 읽어올 수 있는 파일 형식을 지원한다. 따라서 API를 통해 가져온 데이터를 손쉽게 데이터프레임으로 변환할 수 있다.
