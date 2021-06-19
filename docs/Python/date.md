---
layout: default
title: python으로 시작일 종료일 코드 만들기
parent: Python
nav_order: 1
---

**python으로 시작일 종료일 코드 만들기**

시작일과, 종료일을 입력받아 시작일부터 종료일까지의 모든 날짜를 출력하는 코드이다.

```
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


def ooo(a,b,c):
    if b<10:
        if c<10:
            print(str(a) + '0' + str(b) + '0' + str(c))
        else:
            print(str(a) + '0' + str(b) + str(c))
    else:
        if c<10:
            print(str(a) + str(b) + '0' + str(c))
        else:
            print(str(a) + str(b) + str(c))


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
while w<z+1:
    ooo(a,b,c)
    c=c+1
    w=w+1
    y = endDayFromTotalDay(a, b)
    if y+1==c:
        b = b + 1
        c = 1
        if b==13:
            b=1
            a=a+1
```
