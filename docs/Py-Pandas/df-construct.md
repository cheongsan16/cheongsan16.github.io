---
layout: default
title: 데이터프레임의 구조
parent: Py-Pandas
nav_order: 1
---

# 데이터프레임의 구조

데이터프레임은 클래스로 만들어진다. 클래스에는 데이터프레임의 크기, 구성 항목, 자료형, 통계 수치 등의 정보를 확인할 수 있는 속성과 메소드가 포함된다.

## 데이터 내용 미리보기

?

**head(), tail()**

head() 메소드는 데이터프레임의 앞부분 일부 내용을 출력한다. 데이터셋의 내용과 구조를 개략젹으로 볼 수 있어 분석 방향을 정하는데 필요한 정보를 얻을 수 있다. 데이터프레임이 너무 커서 한 화면에 출력하기 어려울 때도 사용한다.<br>
tail() 메소드는 뒷부분의 일부 내용을 출력한다.

head(), tail() 함수의 매개변수로 정수 n을 넣으면 처음 또는 마지막 n개 행을 보여준다. 디폴트값은 n=5이다.

```
import pandas as pd

# read_csv() 함수로 df 생성
df = pd.read_csv('./auto-mpg.csv', header=None)

# 열 이름을 지정
df.columns = ['mpg','cylinders','displacement','horsepower','weight',
              'acceleration','model year','origin','name']

# 데이터프레임 df의 내용을 일부 확인 
print(df.head())     # 처음 5개의 행
print('\n')
print(df.tail())     # 마지막 5개의 행

# df의 모양과 크기 확인: (행의 개수, 열의 개수)를 투플로 반환 
print(df.shape)
print('\n')

# 데이터프레임 df의 내용 확인 
print(df.info())
print('\n')

# 데이터프레임 df의 자료형 확인 
print(df.dtypes)
print('\n')

# 시리즈(mog 열)의 자료형 확인 
print(df.mpg.dtypes)
print('\n')

# 데이터프레임 df의 기술통계 정보 확인 
print(df.describe())
print('\n')
print(df.describe(include='all'))
```
