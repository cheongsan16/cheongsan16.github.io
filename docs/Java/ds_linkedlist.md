---
layout: default
title: Linked List
parent: 22-1 자료구조
grand_parent: Java
permalink: /docs/Java/datastructure/linkedlist/
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
public class Node<E> {
	private E item;
	private Node<E> next;

	public Node(E newItem, Node<E> node) {
		// TODO Auto-generated constructor stub
		item = newItem;
		next = node;
	}

	public E getItem() {
		return item;
	}

	public void setItem(E item) {
		this.item = item;
	}

	public Node<E> getNext() {
		return next;
	}

	public void setNext(Node<E> next) {
		this.next = next;
	}
}
```

#### SList.java

```{java}
import java.util.NoSuchElementException; //inderflow 방지

public class SList<E> {

	protected Node head;
	private int size;

	public SList() {
		// TODO Auto-generated constructor stub
		head = null;
		size = 0;
	}

	public int search(E target) {
		Node p = head;
		for (int i = 0; i < size; i++) {
			if (target == p.getItem()) {
				return i;
			}
			p = p.getNext();
		}
		return -1; //탐색 실패
	}

	public void insertFront(E newItem) {
		head = new Node(newItem, head);
		size++;
	}

	public void insertAfter(E newItem, Node p) {
		p.setNext(new Node(newItem, p.getNext()));
		size++;
	}

	public void deleteFront(E newItem) {
		head = new Node(newItem, head);
		size++;
	}

	public void deleteAfter(Node p) {
		if(p ==null) throw new NoSuchElementException();
		Node t = p.getNext();
		p.setNext(t.getNext());
		t.setNext(null);
		size--;
	}

	public void print() {
		Node p = head;
		for (int i = 0; i < size; i++) {
			System.out.print(p.getItem()+"\t");
			p = p.getNext();
		}
	}

	public int size() {
		return size;
	}
}
```

#### Main.java

```{java}
public class Main{
	public static void main(String[] args) {
		SList<String> s = new SList<String>();
		s.insertFront("orange");
		s.insertFront("apple");
		s.insertAfter("cherry", s.head.getNext());
		s.insertFront("pear");

		s.print();
		System.out.println(": s의 길이 = " + s.size()+"\n");
		System.out.println("체리가 \t"+s.search("cherry")+"번째에 있다.");
		System.out.println("키위가 \t"+s.search("kiwi")+"번째에 있다.\n");
		s.deleteAfter(s.head);
		s.print();
		System.out.println(": s의 길이 = " + s.size()+"\n"); //size 구현

		SList<Integer> t = new SList<Integer>();
		t.insertFront(500);
		t.insertFront(200);
		t.insertAfter(400, t.head);
		t.insertFront(100);
		t.insertAfter(300, t.head.getNext());
		t.print();
		System.out.println(": t의 길이 = " + t.size());
	}
}
```

## Circular Linked List

원형연결리스트<br>
순환 구조의 연결리스트. head가 따로 없고, 마지막 노드를 last라 부른다.<br>
last의 포인터는 첫 노드를 가리킨다.

코드는 총 3개의 클래스로 이루어져있다.<br>
Node.java : 노드의 정보를 담는 클래스. 데이터와 포인터를 가지고 있음. (단순연결리스트의 Node와 동일)<br>
CList.java : 기능을 구현한 클래스. 리스트 내 데이터 삽입, 삭제 등의 기능을 가지고 있다.<br>
Main.java : 실행 클래스.

#### CList.java

```{java}
import java.util.NoSuchElementException; //underflow 방지

public class CList<E> {
	protected Node last; //last의 next는 첫 노드
	private int size;

	public CList() {
		last = null;
		size = 0;
	}

	public void insert(E newItem) {
		Node newNode = new Node<E>(newItem, null);
		if (last==null) {
			newNode.setNext(newNode);
			last = newNode;
		} else {
			newNode.setNext(last.getNext());
			last.setNext(newNode);
		}
		size++;
	}

	public Node delete() {
		if (isEmpty()) throw new NoSuchElementException();
		Node x = last.getNext();
		if(x==last) last=null;
		else{
			last.setNext(x.getNext());
			x.setNext(null);
		}
		size--;
		return x;
	}

	public void print() {
		if(size>0){//size 0이면
			//		Node p = last.getNext();
			//		for (int i = 0; i < size; i++) {
			//			System.out.print(p.getItem()+"\t");
			//			p = p.getNext();
			//		}

			int i = 0;
			for(Node p = last.getNext(); i<size; p=p.getNext(), i++){
				//for 안에서 매번하는 짓이라면 변경부에서 변경하면 된다.
				// ; 전에 여러 개를 넣으면 여러 개도 가능. 초기값 정의하는 부분은 자료형이 다르면 한 번에 안 됨
				System.out.print(p.getItem()+"\t");
			}
		}
		else{
			System.out.println("리스트가 없습니다.");
		}
	}

	public int size() {
		return size;
	}

	public boolean isEmpty() {return size==0;}
}
```

#### Main.java

```{java}
public class Main{
	public static void main(String[] args) {
		CList<String> s = new CList<String>();
		s.insert("pear");
		s.insert("cherry");
		s.insert("orange");
		s.insert("apple");
		s.print();
		System.out.println(": s의 길이 = " + s.size()+"\n");
		
		s.delete();
		s.print();
		System.out.println(": s의 길이 = " + s.size()+"\n"); //size 구현
	}
}
```
