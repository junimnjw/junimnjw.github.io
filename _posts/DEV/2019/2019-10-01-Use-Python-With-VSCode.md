---
layout: post
title: '파이썬 VSCode 코딩하기'
categories: DEV
date: 2019-10-01
lastmod : 2019-10-17 05:00:00
sitemap :
changefreq : daily
priority : 1.0
---



<br>

### 들어가며

<br>

 올해 초 파이썬으로 작업하면서, 선택했던 **IDE**는 [Pycharm](https://www.jetbrains.com/pycharm/)이었습니다. 근데 어느날 컴퓨터에 원인 모를 문제로 Pycharm이 중간에 죽는 문제가 발생되었죠. 원인 분석에 오랜 시간을 보내다.. 결국 MS 형님이 만드셨다는 보다 가벼운 툴인 Visual Studio Code로 개발환경을 구축하기로 마음을 먹었습니다. 근데 그렇게 간단치가 않더군요.. 그래서 몇 시간동안 구글링을 통해 환경 설정에 성공했고 이 경험을 기록으로 남겨 훗날 필요할 때 참고하려고 합니다. 

 <br>

### 본문

#### 1. VS Code 최신 버전 설치

[이 곳](https://code.visualstudio.com/Download)에서 플랫폼에 맞는 녀석을 다운로드 받아서 설치합니다.



#### 2. Anaconda 설치

 파이썬 개발 환경을 설치하는 방법 가운데 초보자님들에게 권하는 방식은 Anaconda 패키지를 이용하는 것입니다. 파이썬 인터프리터 뿐만 아니라 다양한 개발 패키지들이 포함되어있고, **conda**를 사용한 가상환경 설정이 가능하다는 장점도 있기 때문이죠. 

[이 곳](https://www.anaconda.com/distribution/)에서 역시나 플랫폼에 맞는 녀석을 다운로드 받아서 설치합니다.



#### 3. VS Code 실행 

* 파이썬 연관 플러그인 설치 : Python, Python for VSCode, Python Extension Pack

  각자가 뭘 하는지 설명 참고하세요. 저도 자세하게는 모릅니다. 그냥 설치하세요 ㅎ

  <center><img src="/assets/img/vscode1.png"></center>
<br>
  
* 인터프리터 설정

  팔렛트 창 활성화(Crtl + Shift + P)하고, **"Python: Select Intepreter"**를 검색합니다. 리스트에서 원하시는 파이썬 환경을 선택하세요. (자신이 생성해두었던 conda 가상환경이 바로 반영되지는 않는 것 같아요. 이때는 좀 기다려리셔야되요. 이런 경우는 그냥 **base**로 우선 선택하고 나중에settings.json 파일에서 수동으로 수정해주시면 됩니다.)

  <center><img src="/assets/img/vscode2.png"></center>
<br>
  
* 터미널 설정

  VS Code는 터미널에 대해 기본으로 PowerShell이 동작하도록 지정되어있습니다. 근데 저도 사실 PowerShell을 잘 안써봤고 여전히 윈도우즈에서 제공하던 기본 도스 터미널에 익숙해져 있습니다. 그래서 저는 PowerShell 대신 윈도우즈 터미널을 VS Code의 기본 터미널로 지정하겠습니다. 

  팔렛트 창 활성화(Crtl + Shit + P)하고, **"Terminal: Select Default Shell"**을 검색합니다. 

  <center><img src="/assets/img/vscode3.png"></center>


<br>

이제 기본적인 것은 마쳤습니다. 파이썬 코드를 만들어서 돌려보면 됩니다. 

**Hello World**를 출력하는 파이썬 프로그램을 만들고, VSCode의 Explorer 창에서 해당 파일을 선택하고, 마우스 오른쪽 버튼 메뉴에서 **Run Python File in Terminal**을 선택합니다. 

<center><img src="/assets/img/vscode4.png"></center>
<br>

짜잔! 

<center><img src="/assets/img/vscode5.png"></center>

<br>

 

감사합니다!!!



## 참고문헌

[1]:https://excelsior-cjh.tistory.com/79	"EXCELSIOR 블로그"