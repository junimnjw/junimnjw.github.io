---
layout: post
title: "Tensorflow Lite 모델 만들기(1)"
date: 2019-08-07
lastmod : 2019-08-08 09:16:00
sitemap :
changefreq : daily
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 

Tensorflow Lite(이하 텐서플로우 라이트)는 스마트폰과 같은 임베디드 시스템에서, 

텐서플로우 학습 모델을 실행 가능하도록 하는 **경량화된 프레임워크**입니다. 

스마트 폰은 PC나 서버에 비해 하드웨어 성능이 열세하기 때문에,  

보통은 서버나 PC와 같은 고 성능 장치에서 학습을 진행하고, 여기서 생성한 

텐서플로우 모델을 텐서플로우 라이트 모델 포맷(*.tflite)으로 변환하는 과정을 거쳐, 

이를 추론(Inference)에 사용합니다. 



위에서 언급한 과정들을 다음과 같은 세 단계로 나누어 연재하려고 합니다. 

(1) 텐서플로우 모델 생성하기

(2) 텐서플로우 라이트 모델로 변환하기 

(3) 변환한 모델을 안드로이드 앱에 적용하기

우선, 첫 시간으로 **(1) 텐서플로우 모델 생성하기**를 다루고자 합니다. 

여기서 언급하는 텐서플로우 모델의 포맷은 **SavedModel** 또는 **GraphDef**를 말합니다.

**SavedModel**은 텐서플로우 학습 모델의 한 포맷으로 Graph, Weights, Variables를 모두 가지고 있는 모델입니다. 

**GraphDef**는 앞선 SavedModel의 일부분인 Graph정보를 가지는 파일 포맷입니다.
텐서플로우 라이트 포맷으로 변환하려면 Graph 정보에 최종 Weights 값에 대한 
정보도 같이 있어야합니다. 이를위해 freeze_graph.py와 같은 유틸성 스크립트를 통해 이 두가지 정보를 하나의 파일 포맷(.pb)로 만들어줍니다.  

이번 포스트에서는 SavedModel을 생성하고, 이를 가지고 텐서플로우 라이트 포맷 변환에 활용하겠습니다. 

[SavedModel 생성방법](https://medium.com/@jsflo.dev/saving-and-loading-a-tensorflow-model-using-the-savedmodel-api-17645576527)

