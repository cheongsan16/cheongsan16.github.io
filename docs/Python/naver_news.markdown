---
layout: default
title: 날짜별 네이버뉴스 내용 가져오기
parent: Python
nav_order: 4
---

**python bs4를 이용한 HTML 웹크롤링. 시작일과 종료일, 가져올 뉴스의 분야를 입력받아 해당 기간에 올라온 해당 분야의 모든 뉴스기사를 제목과 함께 출력하는 코드**

python bs4를 이용한 HTML 웹크롤링. 시작일과 종료일, 가져올 뉴스의 분야를 입력받아 해당 날짜에 올라온 해당 분야의 모든 뉴스기사를 제목과 함께 출력하는 코드를 제작하였다. 예전에 시작일과 종료일을 입력받아 그 사이의 모든 날짜를 출력하는 코드를 짜두었기 때문에 그 코드를 활용하였다. 해당 코드의 링크는 [이곳](https://cheongsan16.github.io/2019/11/01/date/).
처음으로 100줄이 넘어가는 코드를 짜보는 것이라 어렵기도 하였다. (하지만 뭐... 그 중 50줄은 시작일 종료일 코드가 차지했다.) 이번 활동으로 def 함수에 대해 조금 더 이해하게 되었다. python에서 함수와 클래스, 딕셔너리는 아직은 많이 사용해보지 않아 낯설지만 (나의 체감상) 어느 정도 규모가 있는 프로젝트를 진행할 때마다 찾아가며 코드를 짜는 재미가 있는 것 같다.

```ruby
import requests
from bs4 import BeautifulSoup
import re

def totalDayFromCalendar(year, month, day):
    dayOfMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30]
    i=1
    totaldays = 365 * (int(year) - 1)
    while i<year:
        if i % 4 == 0 and i % 100 != 0 or i % 400 == 0:
            totaldays+=1
        i += 1
    premonth = month - 1
    for b in range(len(dayOfMonth)):
        if premonth >= (b+1):
            totaldays += dayOfMonth[b]
    if (month > 2 and year % 4 == 0 and year % 100 != 0 or year % 400 == 0):
            totaldays +=1
    totaldays +=1
    totaldays = totaldays + day
    return totaldays

def endDayFromTotalDay(year, month):
    if month == 2:
        lastday = 28
        if (year % 4 == 0 and year % 100 != 0 or year % 400 == 0):
            lastday = 29
        else:
            lastday = 28
    else:
        if month == 4 or month == 6 or month == 9 or month == 11:
            lastday = 30
        else:
            lastday = 31
    return lastday

#날짜 시작

a=int(input('시작년도'))
b=int(input('시작월수'))
c=int(input('시작일수'))
d=int(input('끝일년도'))
e=int(input('끝일월수'))
f=int(input('끝일일수'))
print('시작일:'+str(a)+str(b)+str(c))
print('끝일:'+str(d)+str(e)+str(f))

z=totalDayFromCalendar(d, e, f) - totalDayFromCalendar(a, b, c)
y=endDayFromTotalDay(a, b)
w=0
file=[]
while w<z+1:
    if b<10:
        if c<10:
            s=str(a) + '0' + str(b) + '0' + str(c)
        else:
            s=str(a) + '0' + str(b) + str(c)
    else:
        if c<10:
            s=str(a) + str(b) + '0' + str(c)
        else:
            s=str(a) + str(b) + str(c)
    print(s)
    c = c + 1
    w = w + 1
    y = endDayFromTotalDay(a, b)
    if y + 1 == c:
        b = b + 1
        c = 1
        if b == 13:
            b = 1
            a = a + 1
  #  rink='https://news.naver.com/main/list.nhn?sid1=100&sid2=269&mid=shm&mode=LS2D&date=%s&page=' % s
    file.append(s)
#foler에 날짜가 들어간 링크 저장 -> 각 링크 당 페이지 코드

folder=[]
def field(j,sid1,sid2):
    while j<len(file):
        rink = 'https://news.naver.com/main/list.nhn?sid1=%s&sid2=%s&mid=shm&mode=LS2D&date=%s&page=' % (sid1,sid2,file[j])
        folder.append(rink)
        j=j+1

area = input('분야?')
if area == '정치':
    field(0,'100','269')
if area == '경제':
    field(0,'101','263')
if area == '사회':
    field(0,'102','257')
if area == '문화':
    field(0,'103','245')
if area == '세계':
    field(0,'104','322')
if area == '과학':
    field(0,'105','228')
if area == 'IT':
    field(0,'105','230')

j=0
print(folder)
while j<len(folder):
    o=0
    while o<1000:
        o=o+1
        rink=folder[j] + str(o)
        print(rink)
        response = requests.get(rink)
        html = response.text
        soup = BeautifulSoup(html, 'html.parser')
        direc = []
        n=[]
        for page in soup.select('div[class=paging]'):
            direc.append(page.text)
            n.append(page.text)
        n = n[0].split('\n')
        #print(n)
        if '다음' not in n:
            if o > int(n[-2]):
                break
        nu=[]
        for tag in soup.select('dt a'):
            print(tag.text)  # 제목
            text = tag.get('href')
            pattern = '(?<=(&oid=|\?oid=)|(&aid=|\?aid=))[\d]+'
            au = re.finditer(pattern, text)
            bu = ""
            for match in au:
                # print(match.group())
                bu = bu + str(match.group()) + "-"
            if bu not in nu:
                print(tag.get('href'))
                substance = requests.get(tag.get('href'))
                lmth = substance.text
                suop = BeautifulSoup(lmth, 'html.parser')
                for date in suop.select('span[class=t11]'):
                    print('기사입력일시 : '+date.text)
                for content in suop.select('div[id=articleBodyContents][class=_article_body_contents]'):
                    print(content.text)
            nu.append(bu)
    j=j+1
```

아직 def를 사용하는 것이 많이 헷갈리는 것 같다. 
