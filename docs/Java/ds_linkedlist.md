---
layout: default
title: Linked List
parent: 22-1 자료구조
grand_parent: AI
permalink: /docs/ai/py-pandas/part1/
nav_order: 1
---

# Linked List

연결리스트 음

## Singly Linked List

단순연결리스트<br>
각 노드에 데이터와 포인터를 가지고 있다. 포인터에는 다음 노드의 주소가 저장되어 있다.<br>
각 노드의 순서를 통해 <br>

코드는 총 3개의 클래스로 이루어져있다.<br>
Node.java : 노드의 정보를 담는 클래스. 데이터와 포인터를 가지고 있음.<br>
SList.java : 연결리스트의 기능을 구현한 클래스. 리스트 내 데이터 삽입, 삭제, 검색 등의 기능을 가지고 있다.<br>
Main.java : 실행 클래스.

#### Node.java

```{java}

```

#### SList.java

```{java}

```

#### Main.java

```{java}

```

## Circular Linked List

원형연결리스트<br>
순환 구조의 연결리스트. head가 따로 없고, 마지막 노드를 last라 부른다.<br>
last의 포인터는 첫 노드를 가리킨다.

코드는 총 3개의 클래스로 이루어져있다.
Node.java : 노드의 정보를 담는 클래스. 데이터와 포인터를 가지고 있음. (단순연결리스트의 Node와 동일)
CList.java : 기능을 구현한 클래스. 리스트 내 데이터 삽입, 삭제 등의 기능을 가지고 있다.
Main.java : 실행 클래스.

#### CList.java

```{java}

```

#### Main.java

```{java}

```
