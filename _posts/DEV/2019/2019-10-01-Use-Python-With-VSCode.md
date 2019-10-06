---
layout: post
title: 'Use Python with VSCode'
categories: DEV
date: 2019-10-01
lastmod : 2019-10-01 05:00:00
sitemap :
changefreq : daily
priority : 1.0
---



<br>

### 들어가며

<br>

올해 초 Python으로 작업을 하게 되면서, 제가 선택한 **IDE**는 [Pycharm](https://www.jetbrains.com/pycharm/)이었습니다. 근데 어느날 컴퓨터가 문제가 생겼는지 Pycharm을 동작시키면 버벅대고 잘 동작하지도 않게되었죠. 시간이 촉박해 원인 분석에 날밤을 보낼 수 없어서.. 결국 MS 형님께서 만드신 Visual Studio Code를 깔고, Python 개발환경을 셋팅하기로 했죠.. 근데 이것도 그렇게 간단치가 않더군요.. 그래서 몇 시간동안 구글링 한 끝에 개발 환경 셋팅했는데 이 경험을 기록으로 남겨 훗날에 언젠가 필요할 때 참고할려고 합니다. 

 <br>

### 본문



#### 1. VS Code 최신 버전 설치

[이 곳](https://code.visualstudio.com/Download)에서 플랫폼 버전에 맞는 녀석 다운로드 받아서 설치



#### 2. Anaconda 설치

Python을 컴퓨터에 설치하는 방법 가운데 초보자님들에게 권하는 방식은 바로 Anaconda한 패키지 배포를 이용하는 것입니다. Python 뿐만 아니라 필요한 여러 패키지와 함께 가상환경 설정이 가능한 장점이 있기 때문이죠. 

[이 곳](https://www.anaconda.com/distribution/)에서 역시나 플랫폼 버전에 맞는 녀석 다운로드 받아서 설치



#### VS Code 실행 

* Python 관련 플러그인 설치 : Python, Python for VSCode, Python Extension Pack

  각자가 뭘 하는지 설명 참고하세요. 저도 자세하게는 모릅니다. 그냥 설치하세요 ㅎ

  <center><img src="/assets/img/vscode1.png"></center>
<br>
  
* 인터프리터 세팅

  팔렛트 창 활성화(Crtl + Shift + P)하고, **"Python: Select Intepreter"**를 검색합니다. 선택가능한 리스트 가운데 원하시는 파이썬 환경을 선택하세요. (자신이 생성한 Conda 가상환경이 바로 반영되지는 않는 것 같아요. 좀 기다려리셔야되요. 이런 경우는 그냥 **base**로 우선 선택하고 settings.json에서 수정해주셔야 해요)

  <center><img src="/assets/img/vscode2.png"></center>
<br>
  
* 디폴트 터미널 설정

  VS Code는 기본으로 터미널에 대해 PowerShell을 지정합니다. 근데 저도 사실 잘 안써본 거구요. 전 여전히 윈도우 기본 도스 터미널에 익숙해져있습니다. 그래서 저는 윈도우 기본 터미널을 VS Code의 기본 터미널로 지정해보려고 합니다. 

  팔렛트 창 활성화(Crtl + Shit + P)하고, **"Terminal: Select Default Shell"**을 검색합니다. 

  <center><img src="/assets/img/vscode3.png"></center>

  
<br>

이제 기본적인 것은 마쳤습니다. 코드를 하나 만들어서 돌려보면 됩니다. 

**Hello World**를 출력하는 파이썬 프로그램을 만들고, VSCode의 Explorer에서 해당 파일을 선택하고, 마우스 오른쪽 버튼 메뉴에서 **Run Python File in Terminal**을 선택합니다. 

<center><img src="/assets/img/vscode4.png"></center>
<br>

짜잔! 

<center><img src="/assets/img/vscode5.png"></center>


<br>

 



## 참고문헌

[1]:https://excelsior-cjh.tistory.com/79	"EXCELSIOR 블로그"