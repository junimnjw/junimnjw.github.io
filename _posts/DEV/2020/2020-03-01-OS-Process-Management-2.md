---
layout: post
title: "[운영체제] 프로세스 관리[2]"
categories: DEV
date: 2020-03-01
lastmod : 2020-03-01 14:00:00
sitemap :
changefreq : daily
priority : 1.0
---

<br>

복기

Backing Store == Swap Device 

: 하드의 일정 공간을 Midium Scheduler가 Swap In/Out 할 프로그램들의 예비 저장 공간. 

<br>

Context Switching in OS

: CPU 실행이 프로세스 1에서 2로 전환 발생.

<br>

Dispatcher: CPU Scheduler가 프로세스간 전환 실행을 할 때, 

현 프로세스의 상태를 각 프로세스당 할당된 PCB에 저장해두어야한다. 

즉 기존 Context 를 저장하고, 새로운 Context를 가져온다. 

이 역할을 Dispatcher가 맡는다. 

P1에서 P2로 넘어갈 때마다. Context Switching Overhead 발생. 

Overhead는 속도를 위해 어셈블리어같은 Low Level Lang 작성. 

<br>



CPU Scheduling

: 



방식 

Preemptive Vs. Non-preemptive.

Preemptive - 기존의 실행중인 프로세스가 한창 진행중인 상황에서도 강제로 끊고, 

Ready Queue의 다음 프로세스를 실행. 

(응급상황 병원진료 -- 갑자기 응급환자가 들어오면.. 일반 환자는 놔두고 응급환자를 본다.)

Non-preemptive - 실행중인 프로세스가 실행을 마치지 않는 한, 끊지 않는다. 

(일반적인 병원 진료--- 진료 받는 도중에 환자를 쫓아내지 않는다.)

<br>

Scheduling Criteria (척도)

1. CPU 이용률 -- CPU가 놀지 않는지.. - % 
2. Throughput (처리율) -- 단위 시간당 일 처리량. - Jobs / sec
3. Turn around(반환시간) -- 
4. Wating Time(대기시간) -- CPU 서비스를 위해 Ready Queue에서 대기한 시간 - sec
5. Response Time(응답시간) -- 



CPU Scheduling 종류들

1. FCFS - First-Come, First-Served

   - 간단하고, 공평하다. 하지만 최선일까??

   - 3명의 친구가, 은행에 방문.. 번호표 뽑는다. 

     P1: 24msec, P2:3msec, P3:3msec

     P1 - P2 - P3 순서인 경우.

     AVG Waiting Time: (0 + 24 + 27)/3 = 17.xx 

     더 줄일 방법이 없나??

     P3 - P2 - P1 을 하면?

     AVG Waiting Time: (0 + 3 + 6) /3= 3

     후자가 낫네??? 

   - 평균대기시간 입자에서 보면 짧은 일을 우선 처리하는게 낫다.

   - Convoy Effect: 오래걸리는 Job이 앞설 때, 작은일들이 대기하면서 뒤따르는 모양새.

   - Non-preemptive 유형. 

     

2. SJF - Shortest - Job - First

   * 

3. 



 







## References

