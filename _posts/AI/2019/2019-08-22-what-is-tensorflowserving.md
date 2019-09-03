---
layout: post
title: "Tensorflow Serving, Who are You?"
categories: AI
date: 2019-08-22
lastmod : 2019-08-22 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---



안녕하세요. 코딩벅스입니다.   



## 도입

 단순히 개인 연구나 학습을 목적으로 텐서플로우를 사용하는 분들에게는 이번 포스트가 큰 관심이 가지 않을 수 있습니다. 왜냐하면 이번 포스트는 텐서플로우로 생성하고 학습한 모델을 어떻게 원활하게 서비스 할 수 있을까에 대한 솔류션이니까요. 머리속으로 예상되겠지만 열심히 만들고 학습한 모델을, 실제로 서비스 한다는게 그렇게 단순한 문제는 아닙니다. 여러 **Client**분들이 요청하는 서비스에 대한 관리와, 신규 모델이 생성되었을 때의 버전 관리등, 다양한 컴포넌트들이 유기적으로 연결되었을 때만 가능한 것이죠. 구글이 이런 고민을 덜어주기위해 텐서플로우 모델에  대한 서비스(즉, 서빙)를 유기적으로 가능하도록 서비스 프레임워크를 만들었고 제공하였고 이를 **Tensorflow Serving**이라고 합니다. 



## 본문



#### Tensorflow Serving LifeCycle

<img src="https://cdn-media-1.freecodecamp.org/images/1*TwfOoS3M8DaUiB7ntP07_w.png" alt="img" style="zoom:80%;" />



#### Tensorflow Serving & SavedModel



텐서플로우 모델을  ***Serve*** 하기 위해서는 모델을 **SavedModel**로 저장해야합니다. 

하나의 약속이죠.





## 결론



## 참고문헌

[1] https://www.freecodecamp.org/news/how-to-deploy-tensorflow-models-to-production-using-tf-serving-4b4b78d41700/ "how to deploy tensorflow models to production using tensorflow serving"