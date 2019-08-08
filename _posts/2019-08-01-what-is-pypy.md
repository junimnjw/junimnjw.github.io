﻿---
layout: post
title:  "PyPy - 보다빠른 파이썬을 위해!"
---

안녕하세요. 코딩벅스입니다. 

**파이썬**, 참 매력적인 언어입니다.   

제가 회사에 와서 실무에 사용한 언어들(C/C++, C#, Python)  

가운데 가장 흥미로운 언어입니다. 

컴퓨터 전공이 아닌 분들도 몇 시간 투자하면 

금방 간단한 프로그램 정도는 돌릴 수 있을만큼   

초기 진입장벽이 낮은 프로그래밍 언어이죠.  

그런데 쉬운 문법과 환경을 제공하는데,  

약간의 희생도 있습니다. 

바로 **속도**입니다. 


파이썬은 인터프리터 기반의 언어입니다. 

컴파일 기반의 다른 언어들에 비해 **비교적** 수행속도가 느립니다. 

자세한 이유는 관련 [포스트](https://blog.naver.com/junimnje/221567394433)를 참고해주세요. 


물론 파이썬에서 사용되는 다양한 라이브러리들은 

실제로는 껍데기만 파이썬 언어이고 내부적으로는 C로 로직이 

작성되어있는 것이 다수입니다. 

하지만 순수하게 파이썬으로만 작성한 부분은, C/C++과 같은 

컴파일 언어에 비해서 느릴 수 밖에 없죠. 


이에 대한 대안 가운데 하나로 오늘 포스트에서는 **PyPy**를  

소개하려고 합니다. 

![PyPy로고/출처:PyPy 공식홈페이지](http://pypy.org/image/pypy-logo.png){:height="100"}






 
