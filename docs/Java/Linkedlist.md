---
layout: default
title: linked list 구현
parent: Java
nav_order: 1
---

**서론**

Linkedlist를 JAVA로 구현한 코드 설명<br>
우선, Linkedlist, 연결리스트는 각 노드가 데이터와 포인터를 가지고 일렬로 연결된 방식으로 데이터를 저장하는 자료구조이다.<br>
연결리스트는 연결된 노드 사이에 새로운 노드를 추가하거나, 기존 노드를 삭제할 수 있다는 특징을 가진다.<br>
연결리스트에서 노드는 데이터와 포인터를 가진 덩어리이다.

**노드 생성**

노드를 생성한다.

```
public class Node {
	public int data; //데이터
	public Node next; //포인터

	public Node(int data) { //생성자 - 매개변수를 노드의 데이터로
		this.data = data;
	}
	public void print() {
		System.out.print("{" + data + "}");
	}
}
```

**기능 구현 - 추가, 삭제 등**

```
import java.util.ArrayList;
import java.util.Iterator;

public class LinkedList {
	private Node head; //노드의 데이터 시작지점
	
	//생성자 - 
	public LinkedList(){
		head = null;
	}
	
	//null인지 물어보는 함수
	public boolean isEmpty(){
		return head == null; //-> T or F
	}

	//?
	public Node get(int index) {
	    Node node = head;
	    for (int i = 0; i < index; i++)
	        node = node.next;
	    return node;
	}
	
	//? 추가
	public void addFirst(int value){
		Node link = new Node(value);
		link.next = head;		//새로 추가하는 노드의 next를 앞 노드로 지정
		head = link;			//새로 추가하는 노드를 first로 지정
	}
	public void addLast(int value){
		Node link = new Node(value);

		//마지막까지 보내는 구문
		Node tmpLink = head;
		Node lastLink = null;
		while(tmpLink != null) {
			lastLink = tmpLink;
			tmpLink = tmpLink.next;
		}
		if(lastLink == null) head = link;
		else lastLink.next = link;			//새로 추가하는 노드를 first로 지정
	}
	public void add(int index, int value){
		// index가 0이면 첫번째 노드에 추가
		if(index == 0){
			addFirst(value);
		} else {
			Node previosNode = get(index-1);	//추가할 인덱스 앞 요소(이전노드)
			Node nextNode = previosNode.next;	//이전노드의 링크 노드는 새로운 노드의 링크가 되어야 함
			Node newNode = new Node(value);
			previosNode.next = newNode;		//이전노드의 링크 노드는 새로운 노드
			newNode.next = nextNode;		//새로운 노드의 링크는 이전노드가 가리켰던 노드
		}
	}
	public int size(){
		int count = 0;
		while(!isEmpty()) {
			deleteFirst();
			count++;
		}
		return count;
	}
	public Iterator<Integer> listIterator(){
		ArrayList<Integer> list = new ArrayList<Integer>(); //<제네릭>
		while(!isEmpty()) {
			Node delLink = deleteFirst();
			list.add(delLink.data);
		}
		return list.iterator();
	}
	public Node deleteFirst(){
		Node link = head;	//제일 앞의 first부터 값을 리턴
		if(head == null){
			return null;
		}
		head = head.next;
		return link;
	}
	public Object delete(int index){
	    if(index == 0)
	        return deleteFirst();
	    Node previosNode = get(index-1);				//삭제할 인덱스 앞 요소(이전노드)
	    Node deleteNode = previosNode.next;				//이전노드의 링크 노드는 삭제할 노드, 지금 삭제하면 노드를 연결할 수 없다. 
	    previosNode.next = deleteNode.next;				//삭제할 노드의 링크노드가 이전노드의 링크노드가 되어야 삭제할 노드와의 연결이 끊어진다.
	    Object returnValue = deleteNode.data; 			//삭제할 노드의 값을 리턴하기 위해 저장
	    return returnValue;
	}
	public void print() {
		Node link = head;
		while(link != null) {
			link.print();
			link = link.next;
		}
		System.out.println();
	}
}
```
