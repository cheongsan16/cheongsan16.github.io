---
layout: default
title: Robot System
parent: Robot
nav_order: 1
---

# 로봇이란?

세상의 거의 모든 기계는 로봇!!

## 로봇의 정의

- 지적인 기계, 부드러운 기계, 인간과 유사한 기계
- sensor(감각기), effector(효과기)가 있고, brain(두뇌부)에서 판단하여 환경에 적응할 수 있는 행동을 취하는 기계
- 센서, 지능/제어시스템, 구동계의 세 가지 기술요소를 갖춘 지능화된 기계시스템
- ...등

-> 똑똑한 기계! (추가하면,) 센서부, 제어부, 구동부를 갖춘 똑똑한 기계

## Manipulator

산업용 로봇. 자재, 부품, 공구 등 특별한 장치들을 프로그램대로 운반하도록 설계된, 재프로그램이 가능하고 여러 기능을 가진 매니퓰레이터.<br>
Manipulator는 팔같이 생긴 로봇을 말함.<br>
작업에 알맞는 도구가 손(end-effector)에 부착되어, 제어 장치에 내장된 프로그램 순서에 따라 작업을 수행함.<br>
장점 : 인건비 감소, 작업 정밀도/생산성 향상, 작업 환경 개선

## 산업용 로봇의 분류

### 일반적 분류

수동 로봇, 시퀀스 로봇, 플레이백 로봇, 수치제어 로봇, 지능 로봇, 감각제어 로봇, 적응제어 로봇, 학습제어 로봇 등

### 제어방식에 따른 분류

- **비서보제어** (open-loop) : feedback 無
- **서보제어** (closed-loop) : feedback 有

제어 시 피드백의 유무에 따라 분류.<br>
피드백 과정이 없는 비서보제어의 경우, 프로그램대로만 움직임. 외부의 방해에 의해 주어진 task를 완벽히 수행하지 못하더라도, 프로그램대로 움직였다면 작동을 중지한다.<br>
반대로 피드백 과정이 있는 서보제어는, [Actuator](https://cheongsan16.github.io/docs/robot/actuator/)에 [sensor](https://cheongsan16.github.io/docs/robot)를 추가해
외부의 방해로 task를 완벽히 수행하지 못하더라도 환경 감지와 피드백으로 task를 완벽하게 관철시킨다.<br>
예를 들어 90º 회전 이동하는 task가 있을 때, 비서보제어 로봇은 실제 회전한 각도와 무관하게 로봇이 90º 회전하는 프로그램을 수행했다면 작동을 중지한다. 
서보제어 로봇은 센서를 통해 정확히 90º 회전할 때까지 현재 상태를 확인하며 회전하고 완벽하게 수행했을 때 작동을 중지한다.

### 구조적 동작 특성에 따른 분류

여기서 중요한 개념은?<br>
<mark>**workspace**</mark> → 작업반경. manipulator의 end-effector가 도달할 수 있는 범위를 말한다.

**직교좌표로봇**<br>
직선운동을 수행하는 축으로 구성.<br>일반적으로 x,y,z 3축으로 구성.<br>
workspace는 직육면체. (3 DoF)

**원통좌표로봇**<br>
원통의 길이와 반지름 방향으로 움직이는 2개의 직선축과 원주 방향으로 움직이는 하나의 회전축(바닥 회전)으로 구성.<br>
workspace는 휴지와 같은 원통모양. 바닥의 회전축의 회전 각도에 따라 셀러리 모양일 수도. (3 DoF)

**극좌표로봇**<br>
한 개의 직선축과 2개의 회전축으로 구성.<br>
workspace는 중간이 빈 반구모양. (3 DoF)

**수평다관절로봇**<br>
SCARA(Selective Compliance Assembly Robot Arm)라고도 불림.<br>2개의 회전축과 1개의 직선축으로 이루어져 있음.<br>
workspace는 원통을 여러 개 붙인 모양 추정. 추상적이어짐. 그림을 그리거나 글씨를 쓰는 로봇에 주로 활용 (3 DoF)

**수직다관절로봇**<br>
모든 관절이 회전축으로 이루어짐.<br>
workspace는 상당히 추상적. (3 DoF)

## 로봇의 구성요소

- <mark>기구부</mark> : 링크와 관절로 구성된 로봇의 몸체. [Mechanism](https://cheongsan16.github.io/docs/robot/mechanism/). ex) Manipulator
- <mark>말단장치 (end-effector)</mark> : 로봇의 마지막 관절에 연결된 로봇 손. Gripper.
- <mark>구동기</mark> : 기구를 실질적으로 움직이도록 하는 장치. ex) 스탭 모터, 유압 실린더
- <mark>센서</mark> : 로봇관절의 위치를 제어기에 보냄. ex) encoder, camera
- 전원공급기 : 로봇 동작에 필요한 에너지 공급장치.
- 제어부 (추후 업로드)

## 로봇의 기본적인 해석

**정기구학**<br>
모터의 위치(각도)를 이용하여 end-effector의 위치 좌표를 구함.<br>
end-effector의 위치 좌표 (x,y)에 대하여<br>
x = l * cosΘ , y = l * sinΘ

**역기구학**<br>
end-effector의 위치를 이용하여 모터의 위치(각도)를 구함. 해가 없거나 하나 이상 존재할 수 있다.