---
layout: post
title: "SavedModel이란 무엇인가?"
date: 2019-08-14
lastmod : 2019-08-14 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.   



### 배경설명

 **SavedModeld**이 뭐지? 텐서플로우로 학습한 모델을 저장해본 경험이 있으신가요? 

딥잘알못(딥러닝 잘 알지 못하는)인 저는 tf.train.Saver 라는 API를 사용해서 주로 저장했었습니다. 

그런데 **SavedModel**이라는 전혀 알지 못했던 저장 포맷이 있다는 것을 알게되었습니다. 

이번 포스트에서는 이게 당췌 뭐인지.. 그리고 어디에 필요한지 알아보는 시간을 가지도록 하겠습니다. 



#### 학습용 그래프 vs. 추론용 그래프

 텐서플로우로 학습을 진행하고 난 이후, 해당 모델을 배포(deploy)한다고 생각해보겠습니다. 

그리고 배포한 모델을 가지고, 실제 서비스를 제공한다고 가정하겠습니다. 여기서 서비스에 제공할 그래프는

학습용 그래프와 엄밀하게 말하면 완전히 다릅니다. 



학습용 그래프에 포함된 아래 특징들은 사실 서비스에 사용되는 추론용 그래프에서는 **필요없는** 존재들이죠.

* loss function
* optimizer
* dropout layers
* input queues
* file readers



 SavedModel은 그나마 근래에 들어 Tensorflow에서 모델 저장 시 가장 권장하는 포맷입니다. 

SavedModel 디렉토리가 내부적으로 포함하는 정보는 다음과 같습니다. 

* 학습 모델의 그래프 (pb에 포함)
* 학습 모델의 값 (checkpoint files 내)







### 준비물 

* 
  
  



### 사용법

1. 22222

   














[1]: https://medium.com/@daj/how-to-inspect-a-pre-trained-tensorflow-model-5fd2ee79ced0 "How to inspect a pre-trained TensorFlow model"

