---
layout: default
title: Discord Bot Project
parent: Java
nav_order: 3
---

# Discord Bot Project

디스코드 봇 프로젝트

## 프로젝트 목적 및 목표

**사용자가 디스코드 내에서 등록한 일정에 대한 알람 서비스를 제공하는 봇 제작<br>
java를 사용한 팀프로젝트 경험**

기획 단계에서 여러 의견이 나왔으나, 팀프로젝트에 맞는 규모의 의견을 정하기까지 오랜 시간이 걸렸다. 처음에는 습관을 만들어주는 봇을 만드려고 했으나, 루틴을 구현할 때 고려할 점들이 늘어나 진행에 지장이 가게 되면서 등록한 일정의 알람을 제공하는 것으로 변경했다.

## 개발환경

|사용 언어|IDE|빌드 도구|
|:------:|:---:|:---:|
|JAVA|Eclipse|Maven|

사용한 Maven Repository
- Javacord
- Hibernate
- Mysql
- Quartz
- Sparkjava
- Freemarker

## 프로젝트 내용 및 동작도

![디코 플젝 발표](https://user-images.githubusercontent.com/57765638/134550860-32bd7e65-8ccf-4d12-848a-5cf46d89fe7a.png)

## 진행 과정

나의 초기 역할은 알람 내용을 음성채널을 통해 전달하는 것이었다. 그러나 음성채널로 보내는 방식은 알람을 받을 당시 사용자가 음성채널에 있어야하기 때문에, 디스코드에서 제공하는 tts 기능을 사용하게 되었다. tts 실행은 메시지를 send할 때 true/false 옵션을 전달하면 되는 간단한 일이었기에, 며칠만에 실업(?)을 했다.<br>
곧이어 알람을 등록할 때 채널 내에서 명령어로 하는 것보다 별도의 웹페이지를 사용하는 것이 좋다는 의견이 나와, 웹페이지를 제작하고 데이터를 받아오는 일을 맡아했다.(웹페이지 이전에, 여러 형식으로 입력된 날짜를 한 가지 포맷으로 통일하기 위한 작업을 진행했지만 HTML 기능으로 간단히 해결된다는 것을 알아버린 후^^ 사용하지 않게 되었다.) 자바 웹프레임워크 sparkjava를 사용했다. 간단한 예제로 get, post가 동작하는 것을 확인한 후, 페이지에 어떤 정보를 어떻게 제공해야할지, 페이지를 통해서 어떤 데이터를 어떻게 받아올지를 구상했다. sesh봇을 레퍼런스 삼아 이것저것 시도해보고 메모를 하며 구상한 끝에 이 작업에서 집중해야하는 것을 아래 네 가지로 정리했다.<br>
1. 웹서비스 전반
2. 데이터 가공
3. DB에 insert
4. 봇으로 전달 (임베드)
추가로, sesh봇 등록페이지에서 미리 작성자와 서버를 인식하고 있는 것을 보고 선순위 작업은 아니지만 중요한 작업으로 정리해뒀다.<br>

sesh봇은 페이지에서 디스코드 계정과 연결하여 이미 사용자가 누구인지 알고 있다. 그러나 나는 디스코드씩이나 연결할 권한이 없기 때문에... (AWS도 찾아봤지만 방향성이 맞지않았다) sparkjava 내에서 라우터 태그를 사용하였다. <+>



코드를 모두 합친 마지막에, 문제가 생겼다. 이전까지는 프로그램을 한번 실행할 때 여러 개를 추가한 적이 없기 때문에 auto increment가 제대로 동작하지 않는다는 걸 알지 못했다. 그런데 다 합치고 테스트하려고 보니 auto increment가 1에서 안 넘어가는 것이다. (당시에는 당황해서 잘 몰랐는데, 생각해보면 한 번호에 덮어쓰기됐던 듯) 처음에는 hibernate의 문제인 줄 알았다. 물론 그쪽에도 문제가 있었다. hbm.xml파일에서 schedule의 id 옵션이 increment어야하나 assigned(default)으로 두었다. 그러나 수정을 해도 결과가 바뀌지 않았다. 옵션값도 다양하게 넣어보고, 
며칠을 hibernate부터 logging까지 다양한 애꿎은 친구들을 의심하며 서치를 했다. 물론 안 됐다. 정확한 문제는 게터세터 초기화를 하지 않아서 였다. 혹시나 싶은 마음에 함께 한 다른 친구의 코드를 다시 살피던 도중, DB 관련 함수에서 게터세터를 초기화하는 것을 보았고 나랑 다른 게 이것 뿐이니 적용해보니 어처구니없게도 잘 돌아갔다. 세상 사람들 hibernate에서 auto increment를 쓸 때는 객체를 초기화해줘야합니다... 몇 날을 고생했는데 별 거 아니라 황당했지만, 데이터 정규화에 이어 변수 초기화에도 각성하게 되었다..
<+>



## 느낀 점

힘들었다면 정말 힘들었고, 재밌었다면 나름 재밌었던 길고 긴 프로젝트였다. 프로젝트를 진행하면 딱 두 가지를 느꼈다. 첫째는 "역시 팀플은 할 게 못 되는구나!" 고, 둘째는 "나 좀 잘한다" 이다. 이번 학기부터 프로젝트가 왕창 있기 시작했는데 갠플일 것 같긴 하지만 제발 교수님 마음 안 바뀌시길..^^

프로젝트를 진행한 기간이 방학이다보니 바른 어린이 생활따위 개줬기 때문에, 보통 밤 10시에서 새벽 3시 사이에 가장 활발하게 작업을 했다. 그렇다보니 새벽감성이 올라와 작업 끝내고 자기 전에 이런저런 생각을 하게 됐고, 그 생각들의 결과가 두번째 느낀 점이다.<br>
파이썬을 배울 때는 아무것도 모르는 상태에서 시작한 것이라 조금만 배워도 뭔가를 해봤어서 내 실력이 늘고있다는 걸 체감할 수 있었다. 그러나 자바는 실습없이 문법만 쭉 나가서 왜인지 모를 불안감이 있었다. 수업 때 이해가 되어도, 이해와 완전학습에 대한 나의 기준이 높아서 확신할 수 없었다. 프로젝트를 시작하고 코딩을 하면서도, 마음 한 구석엔 늘 불안이 있었다. 꽤 그럴듯한 걸 만들어도 (솔직히 뼈대는 구글링으로 잡으니까) 복붙만 하는 게 아닌가 싶었고, 본의 아니게 조장이 되어버리고 꽤 비중있는 파트를 맡게 되면서 내 옷이 맞나 싶었다.(물론 막상 할 일이 생기면 방법이 어느 정도 그려지고 할 수 있을 것 같아서 한 거긴 하다. 지금 보면 좀 모순인데;) 그러다가 언제 "나 좀 하네?"가 되었냐면, 몇몇 파트에서 워낙 진도가 안 나가니 내가 전체적인 클래스와 함수 틀을 잡아서 던져주라는 임무를 받았을 때이다. 클래스는 내가 고전하는 부분이라 자신이 없었다. 근데 생각한 것보다 너무 쉽게 되는 거였다. 어떻게 돌아야 하는지 품이 나오니까 코드 짜는 건 2시간도 안 걸렸다. 어렵다고 생각한 일이 너무나도 쉽게 풀리는 경험을 하니, 어느 정도 자신감이 생겼다. 그리고 "코드를 많이 보면 실력이 는다"는 말을 체감했다. 왜냐하면 내가 클래스 구성할 때 내가 봤던 클래스들을 생각하면서 그 틀대로 만들었거든. 불안 하나가 해결되니 전부 이해가 되고 해결이 되었다. 곧 복붙을 하는 것도 결국 실력이 있어서 가능한 일이라는 걸 깨달았다. 내가 이 상황에서 어떤 것이 필요한지 검색하고, 알맞는 걸 찾아 적용할 수 있는 것은 상당한 능력이다.<br>
프로젝트를 하면서 근 한 달을 거의 매일 코딩을 했기 때문에 당연히 실력이 늘었을 것이다. 그러나 가장 큰 성취는 자신감을 가지게 된 것이 아닐까. 좋아하는 유튜버가 그런 말을 했다. "내가 나를 인정하지 않으면 아무도 날 인정하지 않고, 모두가 날 인정해도 내가 날 인정하지 않으면 쭉정이같은 인생을 산다." 적어도 쭉정이는 아니게 된 것이니!
