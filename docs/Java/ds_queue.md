---
layout: default
title: Queue
parent: 22-1 자료구조
grand_parent: Java
permalink: /docs/Java/datastructure/queue/
nav_order: 5
---

# Queue

큐! FIFO<br>
enqueue dequeue, front rear

초기 생성 시 : front = rear = -1
공백 큐 : front = rear
포화 큐 : front % n = (rear+1) % n

## 원형 큐

배열에서 데이터가 뒤로 몰려서 포화 큐가 아닌데 포화 큐로 인식함...
이를 막기위해 순환구조의 원형 큐 사용!

인덱스 0번을 사용하지 않으므로 위 문제를 해결
데이터 삽입 위치 : 데이터삽입순서 mod 배열길이

초기 생성 시 : front = rear = 0
공백 큐 : front = rear
포화 큐 : front % n = (rear+1) % n
