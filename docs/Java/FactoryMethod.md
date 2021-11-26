---
layout: default
title: Factory Method Pattern 코드 설명
parent: Java
nav_order: 2
---

**Factory Method Pattern 코드 설명**

코드에 설명달기! 임.<br>
먼저, Factory Method Pattern은 객체지향 디자인패턴으로, 부모 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며. 자식 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이기도 하다.<br>
위키백과 설명은 그렇다고 함..<br>
나는 인터페이스와 추상 클래스의 성질과 클래스 간 부모-자식 관계에서의 형변환 등을 있는대로 사용한 패턴이라고 느꼈다. 이 패턴을 들여다보면 코딩이 굉장히 입체적인 것이라는 알게 됨..
<br>
설명은 주석으로 달아두었다. 넘버링이 안되어있는 것들은 변수, 클래스 선언 등이고 코드가 실행되는 순서대로 넘버링을 해두었다.<br>

- 코드 출처 : 를 원래 써야하나 했는데.. 지금보니 검색하면 잘 나오는 코드라 생략

**코드와 코드 설명**

추상클래스 Factory 클래스와 그의 자식 IDCardFactory클래스
```
public abstract class Factory { //추상클래스 Factory
	public final Product create(String owner){ //순서② : 구현된 함수 create 호출!
		Product p = createProduct(owner); //순서③ : createProduct 함수 호출    ->순서⑥-2 : 돌아와서
		registerProduct(p); //순서⑦ : registerProduct함수 호출   ->순서⑨-2 : 돌아와서
		return p; //순서⑨ : p를 return
	}
	protected abstract Product createProduct(String owner); //추상메소드   
	//순서④ : createProduct함수는 추상메소드. 또한, Main클래스의 변수 factory는 본래 IDCardFactory형이므로 부모를 이용해 IDCardFactory에서 구현된 함수를 호출
	protected abstract void registerProduct(Product product); //추상메소드  //매개변수 == Product클래스 객체
	//순서⑦-1 : 순서④와 동일
}
```
```
import java.util.Vector;

@SuppressWarnings({ "unchecked", "rawtypes" }) //? vector 관련 어쩌고
public class IDCardFactory extends Factory { //추상클래스 Factory의 자식 IDcardFactory
	private Vector owners = new Vector();

	@Override
	protected Product createProduct(String owner){ //추상클래스 Factory의 추상메소드 구현
		return new IDCard(owner); //순서⑤ : 부모를 거쳐 함수 호출, return으로 IDcard클래스의 생성자 호출  ->순서⑥-1 : 돌아왔다가
	}
	@Override
	protected void registerProduct(Product product){ //추상클래스 Factory의 추상메소드 구현
		owners.add(((IDCard)product).getOwner()); //12
		//순서⑧ : 부모 거쳐 함수 호출, (add가 Vector클래스의 함수라 잘 모르겠지만 느낌상..) IDcard의 getOwner함수를 호출   ->순서⑨-1 : return받은 this.owner로 Vector의 어떤 작용을 거치고
	}
	
	public Vector getOwners(){ //??얘는 뭐지
		return owners;
	}
}
```
Product 인터페이스와 그의 자식 IDcard 클래스
```
public abstract class Product { //추상클래스 Product //순서⑤-1? : 거쳐가야 함???
	public abstract void use(); //추상메소드    //순서⑫ : use함수는 추상메소드
}
```
```
public class IDCard extends Product { //추상클래스 Product의 자식 IDcard
	private String owner; //owner 변수 선언

	public IDCard(String owner){ //IDCard 생성자! -> 카드 생성
		System.out.println(owner + "의 카드를 만듭니다.");
		this.owner = owner; //순서⑥ : 출력, this.owner에 매개변수 owner저장
	}
	@Override
	public void use(){ //추상클래스 Product의 추상메소드 구현
		//순서⑫-1 : 그래서 자식인 IDCard에서 구현
		System.out.println(this.owner + "의 카드를 사용합니다."); //this.owner == IDcard함수의 매개변수   //순서⑫-2 : 출력
	}
	public String getOwner(){
		return this.owner; //순서⑨ : this.owner를 return
	}
}
```
이들을 실행하는 Main 클래스
```
public class Main {
	public static void main(String[] args) {
		Factory factory = new IDCardFactory(); //Factory를 자식인 IDcardFactory로 형변환
		Product card1 = factory.create("코야"); //create 함수의 return타입이 Product이므로 Product형?으로 변수 선언
		Product card2 = factory.create("레종"); //(다똑같으니 같은 말 밑으로) 변수 factory는 Facatory형이므로 Factory클래스로 이동
		Product card3 = factory.create("유키"); // 순서① : Factory클래스의 create함수 호출   //순서⑩ : card3변수에 return된 p를 저장
		card1.use();  //순서⑪ : use함수 실행  ->Product card1 = ~IDCard형 (★근데 이유를 모르겄네)? 이라서 일단 Product로.
		card2.use();  //똑같지
		card3.use();
	}
}
```

**과정**

-편의상 어미가 명사형 어미로 끝나는 경우가 있습니다.-<br>
일단 변수 선언, 클래스 선언, 누가 추상이고 누가 자식이고 부모고 바로 눈에 보이는 것들부터 순조롭게 시작함.<br>
이걸 다하고나니 어떻게 시작을 할지 고민하던 중.. 실행 순서대로 넘버링을 하기로 마음먹음.<br>
시작은 당연히 Main이니 차근차근 진행해나감.<br>
create 함수 호출 -> 추상 클래스의 create를 거쳐 구현된 함수를 통해~<br>
그렇게 9번째까지 넘버링을 했는데 어느 순간 이상함을 느낌.<br>
IDcardfactory에서 Factory의 createProduct 함수를 구현하는 부분에서 아직 IDcard클래스는 근처도 안갔는데 갑자기 그 클래스의 변수를 매개변수로 받는 거임???<br>
잘 가다가(그런 것 같았는데) 갑자기 아직 정의도 안된 변수를 데려가야겠다고 하니.. 당황할 수밖에<br>
그때가 밤 12시였기 때문에 아무리 들여다봐도 이유를 알 수 없어서 덮고 자러갔다.
<다음날><br>
원래 하던 일을 덮어두면 (무의식적으로 계속 그걸 생각하니 때문이라던데) 새롭게 보이는 게 있는 법.<br>
그제서야 return new IDCard(owner); (<- IDcardfactory 클래스의 createProduct 함수의 리턴) 얘가 매개변수를 가지고 있다는 걸 발견했다.<br>
클래스 객체를 생성할 때는 매개변수가 없잖아?<br>
정신차리고 구글을 두들겼다. 그때는 생성자가 return에 있을 수 있는지 몰랐지..<br>
return에 new가 들어가는 것조차 전부 수상해서 new를 쓰는 경우까지 전부 찾아보니<br>
"생성자를 호출할 때는 new를 사용합니다"<br>
허허;; 잠시 잊혀졌었지만 평생 못 잊게 되었음.<br>
드디어 저 친구가 생성자였음을 깨달았다.<br>

그 다음은 그닥 시행착오는? 없었다. 뭔가 요상하게 느껴지면 디버그를^^ 이용해서 해결했다.<br>

<br>
★Main에 근데 잘모르겄네? 해둔 것에 대한 답 (card1이 왜 IDCard형? 인지를 모른다는 말)<br>
저걸 왜 몰랐을까? 하고 들여다보다 알게 됨.<br>

순서8에서 IDcard의 getOwner함수를 호출하여 IDcard의 클래스 변수 this.owner를 return받음. IDcard의 클래스변수가 Vector로 어떤 작용을 받은 결과값이 p로 return받고 그 p가 곧 card1이기 때문이다!

**후기**

Factory Method Pattern 자체가 어렵다고 느껴지진 않았다. 오히려 설명하려고 하나하나 뜯어보니까 이 친구자체가 어렵진 않았는데, 설명하는 게 힘들었다.<br>
그냥 공부할 떄는 용어 하나하나까지 고민하진 않았는데, 글로 설명을 해야하니 용어 하나라도 틀린 정보가 있을까 싶어서 내가 아는 단어가 맞는 표현인지도 검색해봐야했다.<br>
예를 들어 클래스 객체가 내가 생각하는 거시기가 맞는지... 함수 호출이라는 게 맞는 표현인건지... 등<br>
그래도 지난 17년을 문과로 굳게 믿어 온 경력(?)덕분에 주저리주저리 말을 길게 쓰는 건 잘할 자신이 있어서 다행이다.<br>
뭐라도 글로 쓰려고하면 쓸 순 있음.. 가독성은 보장 못하지만<br>

여하튼! Factory Method Pattern 코드에 대한 설명달기 였습니다 _-_<br>


<br>
<!-- <br>
<br>
+ 근황이 궁금하신 것 같아.. (?)<br>
초등학생은 아니지만 단 두 과목, 중간고사 첫 날에 모든 시험이 끝나버린 대학생은 집에서 과제하면서 노는 중입니다<br>
파이썬이 한 과목이 있어서 아주 힐링이어요<br>
얘처럼 설명을 달아야하는 인공지능 과제가 있어서 돌아버리겠지만.. 아직 일주일 남았으니 언젠간 하겠지요^^<br> -->
