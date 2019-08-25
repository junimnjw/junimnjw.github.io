---
layout: post
title: "SavedModel은 무엇인가?"
date: 2019-08-14
lastmod : 2019-08-14 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.   



## 배경설명

 텐서플로우로 학습한 모델을 저장해본 적 있으신가요? 있으시다면 주로 tf.train.Saver를 사용하셨을 것으로 생각됩니다. 물론, 잘못된 방븝은 아닙니다. [텐서플로우 가이드][https://www.tensorflow.org/guide/saved_model]에서도 그렇게 가이드 하고있습니다. 그런데 가이드를 자세히 보시면 **SavedModel**이라는 다른 저장 포맷도 소개하고 있습니다. 이녀석은 당췌 무엇이고, 어디에 쓰는 녀석일까요? 이번 포스트에서 다루겠습니다 .



## 본문



### SavedModel과 Tensorflow Serving

 우선 **SavedModel**의 존재 목적을 알려면 **Tensorflow Serving(이하 텐서플로우 서빙)**과 **Cloud ML**을 아는게 도움이 됩니다. 각 키워드에 연결된 링크를 통해서 미리 이해하시길 권장드립니다. 텐서플로우 서빙(Tensorflow Serving)은 구글에서 개발 및 제안한 것으로, 기존 텐서플로우 모델을 각 제품 운영환경에서 서비스(또는 서빙)하는데 도움을 주는 라이브러리입니다. 

 사실 텐서플로우를 공부하면, 주로 PC에서 텐서플로우 라이브를 통해 모델을 만들고, 학습시키고, 그리고 이렇게 학습한 모델을 가지고 테스트하고.. 하는 작업만 주로 하게됩니다. 하지만, 실제로, 해당 텐서플로우 모델을 어떤 디바이스에 올려서 서빙하려고 할 때, 이전 방식처럼 그래프를 생성하고, 저장해두었던 checkpoint를 로딩해서 사용하는 방법은 너무 불편하고, 비 효율적입니다. 

 즉, 모델을 배포(deploy)하고, 이 배포된 모델을 가지고 실제로 서비스 하는 과정을 도와줄 천사같은 시스템(system like angel?)이 절실히 필요했고, 이러한 요구에 어떻게 보면 텐서플로우를 개발 유지하는 기업인 구글이 대안을 제시한것이죠. 텐서플로우 서빙에 대한 간략한 소개를 마무리하고, 그럼 **Saved Model**과 텐서플로우 서빙은 무슨 관계냐? 정답은, 텐서플로우 서빙이 지원하는 학습 모델 포맷이 바로 Saved Model입니다. 

 근래 들어서 텐서플로우 가이드에서 저장과 복원(Save & Restore) 파트를 보면, **Saved Model**을 권장하는 늬앙스로 작성되어있습니다. 그런데 생각보다 자료가 많지는 않네요. 권장하는 이유는 자신들이 밀고있는 텐서플로우 서빙에 힘을 실어주기 위함이라고 생각합니다. 뭐 실제로 편하기도 하구요. 





### Checkpoint와 뭐가 다른가?

 tf.train.saver API를 사용해서 학습한 모델을 저장하게 되면 다음과 같은 결과로 나타납니다. 

* check.index : 
* check.data : weight 값들
* check.meta : 모델 그래프에 대한 메타정보









 텐서플로우로 학습을 진행하고 난 이후, 해당 모델을 배포(deploy)할 때, 여기서 배포할 그래프는 학습용 그래프와  다릅니다. 아래에서 언급된 학습용 그래프가 가지는 특징들은 사실상 서비스 단계에 이용되는 추론용 그래프에서는 **필요없는** 존재들이죠.

* loss function
* optimizer
* dropout layers
* input queues
* file readers



 SavedModel은 그나마 근래에 들어 Tensorflow에서 모델 저장 시 가장 권장하는 포맷입니다. 

SavedModel 디렉토리가 내부적으로 포함하는 정보는 다음과 같습니다. 

* 학습 모델의 그래프 (pb에 포함)
* 학습 모델의 값 (checkpoint files 내)



### 코드

~~~python
import tensorflow as tf
~~~







## 결론







## 참고문헌

[1]:https://bcho.tistory.com/tag/savedmodel "조대협의 블로그"
[2]: https://www.tensorflow.org/guide/saved_model "텐서플로우 공식 가이드"

