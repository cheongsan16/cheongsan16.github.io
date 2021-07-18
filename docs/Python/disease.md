---
layout: default
title: 지역별 시기별 대표질병 웹서비스
parent: Python
nav_order: 6
---

**국가데이터포털 국가중점데이터**

대표질병 천식, 감기, 눈병, 식중독
국가데이터포털에서 최근 5년동안의 각 지역별(시도별, 시군구별) 대표질병의 발생빈도 데이터를 활용하였다.
각 데이터를 DB에 저장하고, 저장된 데이터를 불러와 웹서비스하는 형태.
지역/날짜/질병 을 입력하면 해당 데이터를 테이블 형태로 웹서비스한다.

(아쉽지만 데이터를 DB에 업로드하는 코드가 사라졌다.. 어디 있는데 안 보인다..)

```
import pymysql.cursors
from flask import Flask, json, Response, jsonify, render_template

app = Flask(__name__)
app.config['JSON_AS_ASCII'] = False

@app.route("/<loc>/<date>/<dis>")
def view(loc, date, dis):
    re = db(loc, date, dis)
    return render_template("dis_table.html", values=re)

def db(loc, date, dis):
    re = []
    with conn.cursor() as cursor:
        sql = "SELECT s.biglocatecode, i.smalllocatecode, s.smalllocate, i.date, i.`NO`, i.dis_id, d.dis_name FROM d_big b, d_info i, d_isease d, d_small s WHERE b.biglocatecode = s.biglocatecode and s.smalllocatecode=i.smalllocatecode and d.dis_id=i.dis_id and i.date = %s AND d.dis_name = %s AND b.biglocate = %s"
        cursor.execute(sql, (date, dis, loc))
        t_re = cursor.fetchall()
        for l in t_re:
            q=dict()
            q["locat"]=l[2]
            q["No"]=l[4]
            re.append(q)
    return re

conn = pymysql.connect(host='host',
user='user',
password='****',
db='DB정보',
charset='utf8')

if __name__ == "__main__":
    app.run(host="127.0.0.1", port="5000")
```
