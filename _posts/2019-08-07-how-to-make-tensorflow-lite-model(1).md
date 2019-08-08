---
layout: post
title: "Tensorflow Lite 모델 만들기"
date: 2019-08-07
lastmod : 2019-08-08 09:16:00
sitemap :
changefreq : daily
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 

Tensorflow Lite(이하 텐서플로우 라이트)는 스마트폰과 같은 임베디드 시스템에서, 

텐서플로우로 학습 모델을 실행 가능하도록 하는 **경량화된 프레임워크**입니다. 



스마트 폰은 PC나 서버에 비해, 하드웨어 성능이 열세합니다. 

그래서 주로 서버와 PC같은 고 성능 장치에서 학습을 진행하고, 여기서 확보한 텐서플로우 모델을 

가지고 텐서플로우 라이트 모델(*.tflite)로 변환 후, 이를 추론(Inference)에 사용합니다. 



크게 다음과 같은 세 단계로 나누어 포스트를 작성하려고 합니다. 



(1) 텐서플로우 모델 저장하기

(2) 확보한 모델을 텐서플로우 라이트 모델로 변환하기 

(3) 변환된 모델을 안드로이드 앱에서 로드하기



우선, 오늘은 첫 시간으로 **(1)텐서플로우 모델 저장하기**를 다루고자 합니다. 

나중에 텐서플로우 라이트 모델로 변환하려면, 텐서플로우에서 기존 학습 모델을 저장할 때 

**SavedModel**또는 **GraphDef**로 저장해야합니다. 



**SavedModel**과 **GraphDef**는 무엇이고 둘은 어떤 차이가 있을 까요?



[SavedModel 빌드방법](https://medium.com/@jsflo.dev/saving-and-loading-a-tensorflow-model-using-the-savedmodel-api-17645576527)

[GraphDef 빌드방법]()