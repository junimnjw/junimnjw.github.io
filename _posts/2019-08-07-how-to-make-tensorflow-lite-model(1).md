---
layout: post
title: "Tensorflow Lite 모델 만들기(1)"
date: 2019-08-07
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 



**Tensorflow Lite**(이하 텐서플로우 라이트)는 기존 텐서플로우의 경량화 버전입니다. 텐서플로우 라이트를 사용해서 스마트폰과 같은 임베디드 시스템에서 기존 텐서플로우 모델을 사용할 수 있도록 합니다. 스마트 폰은 하드웨어 성능이 비교적 열세하기 때문에, 서버나 PC은 고성능 장치에서 텐서플로우로 학습을 진행하고, 여기서 생성한 학습 모델을 텐서플로우 라이트에서 인식이 가능한 포맷(*.tflite)으로 변환 후, 이를 추론(Inference)에 활용합니다. 



위 내용에 대한 설명은 다음 세 단계로 연재하려고 합니다. 



(1) 텐서플로우 모델(**SavedModel**) 생성하기

(2) 텐서플로우 라이트 모델(**.tflite**)로 변환하기 

(3) 안드로이드 앱에 적용하기



첫번째 순서로 이번 포스트에서는 **(1) 텐서플로우 모델 생성하기**를 다루고자 합니다. 저장해야 할 텐서플로우 모델의 포맷은 **SavedModel** 또는 **GraphDef**입니다. 텐서플로우 학습 모델은 **그래프**로 표현됩니다. 결국, 이러한 그래프의 노드와 에지 정보가 어떻게 보면 해당 모델을 표현한다고 볼 수 있죠.

<br>

<br>

#### CheckPoint Vs. SavedModel

텐서플로우로 학습한 모델을 임시로 저장할 때, 보통 **Checkpoint(.ckpt)**형태로 저장합니다. 그리고 이를 가지고 추후 복원(Restore)해서 다시 학습을 재개합니다. 다만, Checkpoint는 저장 시점에서의 Variable 정보만을 가지고 있기 때문에, 학습을 지속하려면 기존 Checkpoint를 복원 하기 전, 학습 모델의 그래프를 다시 정의해주어야 합니다. 



 이와는 달리 **SavedModel**은 Variables 뿐 아니라 학습 모델의 그래프 정보를 같이 포함하고 있습니다. 사실상, 학습 모델에 대한 모든 정보를 가지고 있기 때문에, 모델 자체를 온전히 배포하는데 주로 활용됩니다. 텐서플로우에서 **SavedModel**에 대한 자세한 설명은 차후 별도로 다루겠습니다. **SavedModel**을 배포하는 간단한 예제를 다음 링크에서 공유합니다. 



[MNIST 학습 모델에 대한 SavedModel 생성](/assets/main.py)

![SavedModel](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/savedmodel_captured.JPG?raw=true)

