---
layout: post
title: "SavedModel 이란 무엇인가?"
categories: DEV
date: 2019-08-14
lastmod : 2020-01-06 14:00:00
sitemap :
changefreq : daily
priority : 1.0
---



안녕하세요. 코딩벅스입니다.   



 **Tensorflow**로 만든 딥러닝 모델, 어떻게 저장하시나요? 보통은 **tf.train.Saver**를 사용해서 **\*.ckpt**라는 **Checkpoint** 포맷으로 저장하실 겁니다. 하지만 이건 모델 그래프가 아닌, 변수의 가중치 값들을 <span style="color:red;font-weight:bold">임시 저장</span>해두는 것입니다. 이 때문에 **Checkpoint** 포맷은 추가 학습을 진행할 때, 필수적으로 코드를 통한 딥러닝 모델 복원이 선행되어야합니다. 

<br>

그런데, 학습한 결과를 다른 환경이나 사람에게 배포할 때를 생각해보겠습니다. 이 방식을 따른다면 **Checkpoint** 파일뿐 아니라 딥러닝 모델 복원을 위한 모델 생성 알고리즘 코드까지 전달해야하는 번거로움도 생기기 마련이죠. 거기다가, 모델 생성 알고리즘 코드는 Java로 작성되었는데, 전달 받는 환경은 Python 기반이라면, 또 한번의 어려움을 겪게 됩니다. 

<br>

위와 같은 고민을 덜 수 있도록, 학습 완료된 모델을 패키지 형태로 저장 및 배포할 수는 없을까요?

<br>

 있습니다. 바로 **SavedModel**이죠. 학습 모델의 변수에 저장된 가중치 값 뿐 아니라, 모델 그래프도 같이 포함된 **"패키지"**형태의 포맷이죠. 또한, 특정한 코딩 언어에 종속되지 않는 포맷이기 때문에, Tensorflow Serving, Tensorflow Lite 등의 다른 플랫폼 환경으로도 배포해야하는 경우에 **SavedModel** 포맷이 더욱 권장됩니다. 

<br>

[텐서플로우 가이드][https://www.tensorflow.org/guide/saved_model]는 기존 **tf.train.Saver()**를 통해 저장하고 복원하는 방식과 별개로, **SavedModel** 포맷의 저장 방식도 소개합니다.  구체적으로 이 포맷은 무엇이고, 어디에 쓰는 걸까요? 



## 본문



#### 알아두면 유익한 용어들

* MetaGraphDef
* SignatureDef



#### SavedModel은 Tensorflow Serving이 지원하는 포맷이다

<br>

 **SavedModel**의 존재 목적을 알려면 **Tensorflow Serving**과 **Cloud ML**을 아는게 도움이 됩니다. **Tensorflow Serving**은 구글에서 개발한 것으로, 기존 텐서플로우로 제작한 딥러닝 모델을 각 제품환경에서 인퍼런싱 서비스 하도록 돕는 시스템입니다. 

<br>

 **Tensorflow**를 처음 배우면, 자신의 PC에서 모델을 만들고, 학습시키고, 그리고 테스트하고.. 하는 작업만 주로 하게됩니다. 하지만, 실제로 해당 텐서플로우 모델을 어떤 제품에 올려서 서비스하려고 할 때, 직접 모델을 로딩하고 이를 기반으로 서비스를 제공하는 시스템을 구축하는 것은 쉽지않습니다. 

<br>

 즉, 모델을 배포(deploy)하고, 이 배포된 모델을 가지고 실제로 서비스 하는 과정을 도와줄 시스템이 필요한 상황에서, 구글이 대안을 제시한것이죠. 그럼 **Saved Model**과 **Tensorflow Serving**은 무슨 관계냐?  텐서플로우 서빙이 지원하는 딥러닝 모델 포맷이 **Saved Model**입니다. 

<br>



### 실습











## 참고문헌

[1]:https://bcho.tistory.com/tag/savedmodel "조대협의 블로그"
[2]: https://www.tensorflow.org/guide/saved_model "텐서플로우 공식 가이드"
