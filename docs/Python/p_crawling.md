---
layout: default
title: python 크롤링 모음
parent: Python
nav_order: 2
---

**웹크롤링 -네이버 실시간 검색어**

웹크롤링은 어렵지만 제일 재밌는 것 같다.
네이버 실검을 뽑아오는 코드.

```ruby
import requests
from bs4 import BeautifulSoup

response=requests.get('https://www.naver.com/')
html=response.text
soup=BeautifulSoup(html, 'html.parser')
ranking=1

for a in soup.select('span[class=ah_k]'):
    if ranking == 21:
        break
    print(str(ranking) + '위 : ' + a.text)
    ranking = ranking + 1
```

**웹크롤링 -실시간 네이버 영화 순위**

네이버 영화 순위를 뽑아오는() 코드

```ruby
import requests
from bs4 import BeautifulSoup

response=requests.get('https://movie.naver.com/movie/sdb/rank/rmovie.nhn')
html=response.text
soup=BeautifulSoup(html, 'html.parser')
ranking=1
for tag in soup.select('div[class=tit3] a'):
    print(str(ranking) + '위 : ' + tag.text)
    # print(tag)
    url = tag.get('href')
    print('https://movie.naver.com' + url)

    answer = requests.get('https://movie.naver.com'+url)
    lmth = answer.text
    suop = BeautifulSoup(lmth, 'html.parser')
    for gat in suop.select('div[class=star_score] p'):
        a=gat.text.strip()
        print(a)
    ranking=ranking+1
```

**xml 기상청 날씨**

(사실 웹크롤링인지는 잘 모르겠는데)
기상청의 자료를 가져와서 입력한 지역의 날씨 예측과 예측의 신뢰도를 출력하는 코드.

```ruby
import requests
from bs4 import BeautifulSoup

response=requests.get('http://www.weather.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=109')
html=response.text
soup=BeautifulSoup(html, 'html.parser')

y=input("지역")
for z in soup.select('location'):
    if z.city.text == y:
        print(z.city.text)
        print("==========")
        for a in z.select('data'):
            print('시간:' + a.tmef.text, end="  ")
            print('날씨:' + a.wf.text, end="  ")
            print('최저:' + a.tmn.text, end="  ")
            print('최고:' + a.tmx.text, end="  ")
            print('신뢰도:' + a.rnst.text)
``` 
