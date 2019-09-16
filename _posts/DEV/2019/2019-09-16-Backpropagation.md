---
layout: post
title: "Backpropagation, Who are You?"
categories: DEV
date: 2019-09-16
lastmod : 2019-09-16 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---



<span style="font-size:11pt;color:blue">*오랜만에 네이버 블로그에 작성해 두었던 글들을 재연재하려고 합니다. 지난 블로그 글들을 보니 제가 쓴 글들이지만 허접(?)하네요. 물론 지금 쓰는 글들도 그렇게 고퀄리티는 아니긴 하지만요...그래도 유익하길 바라며, 많은 피드백 부탁드립니다.*</span>



 인공 신경망에서 **좋은 성능(Performance)**을 얻는데 중요한 몇가지 요소들이 있습니다. 

* 뛰어난 컴퓨팅 파워(막강한 GPU 등)
* 엄청난 양의 고품질 Labeled 데이터 (이것도 다 돈이죠.. )
* **학습 알고리듬**

위 요소들 가운데 외부적 요인이 아닌, 기술과 직접 연관된 것이 바로 **학습 알고리듬** 입니다. 즉, 어떻게 하면 <u>컴퓨터(또는 머신)에게 더 빠르고 효과적으로 공부를 시킬 수 있을까</u>에 대한 고민이죠. 오늘 언급할 내용도 그 고민에 대한 고전적 해법으로 불리는 **역전파(일명 Backpropagation) 알고리듬**을 간략히 알아보는 시간을 가지겠습니다. 



## 본문

 바야흐로 1986년, 제가 엉금엉금 기어다닐 시기에.. 제프리 힌튼(G. Hinton) 교수 등은 인공 신경망 연구에서 획기적인 논문을 발표하죠. 바로 그 논문의 핵심이 바로 소개할 **역전파 알고리듬(Backpropagation)**입니다. 



 우선, 역전파(?)라는 이름에서 힌트를 얻을 수 있습니다. 간단히 역전파의 개념을 설명 드리면, 여러 층으로 이루어진 인공 신경망에서 학습할 때, 주어진 학습데이터를 입력으로 넣고, 이에 대해서 구한 최종 출력값과 주어진 학습 데이터에 대한 기대값이 다른 경우 오차(Error)가 발생합니다. 이 오차에 대한 부분을 역으로(Inversely) 전파하는 것이죠. 학습데이터를 입력으로 해서 출력을 구하는 방향을 순 방향(Foward)이라고 했을 때, 오차에 대한 전파가 반대로 이루어지기 때문에 역방향 (Backward)이라고 하죠. 그리고 역방향에 대한 전파이기 때문에 줄여서 역 전파(Back-propagation)이라고 일컫습니다. 



개념은 이정도로 하고, 좀더 자세하게 설명 드리겠습니다. 



![img1](/assets/img/backpropagation1.png)







## 결론

ㅇㄹ






## 참고문헌

[1]:https://bcho.tistory.com/1182 "조대협의 블로그"
[2]: https://developers.google.com/protocol-buffers/docs/pythontutorial?hl=ko "Protocol Buffers Basic for Python"