---
layout: default
title: 명령 프롬포트 구현
parent: Python
nav_order: 3
---

**python 으로 명령 프롬포트 만들기**

os 모듈을 이용하여 명령 프롬포트의 기능 중 dir (파일 목록 출력), cd .. (상위 디렉토리로 이동), cd 파일명 (하위 디렉토리로 이동), cat 파일명 (리눅스 운영체제에서 지원함. 파일 내용을 읽기), exit (명령 프롬포트-프로그램-종료) 를 구현해 보았다.


```ruby
import os

while 1:
    d = os.getcwd()
    z=input(d+'>')

    if z=='dir':
        a = os.popen(z)
        print(a.read())

    if 'cat' in z:
        y = z.split()
        n=d.replace("\\", "/")
        v = open("%s/%s.txt" % (n, y[1]), 'r')
        print(v.readline())
        v.close()

    if z=='exit':
        break

    if 'cd' in z:
        y=z.split()
        if y[1]=='..':
            w=d.split('\\')
            l=w[-1]
            v=d.replace(l,"")
            os.chdir(v)
        else :
            os.chdir(d+'\\'+"%s" % y[1])
```
