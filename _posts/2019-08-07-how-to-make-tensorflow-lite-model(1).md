---
layout: post
title: "텐서플로우 라이트 모델 생성(1)"
date: 2019-08-07
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.

## 배경설명

**텐서플로우 라이트**는 텐서플로우의 경량화 버전입니다. 서버나 PC와 같은 고성능 장치에서 학습을 진행하고, 여기서 생성한 텐서플로우 모델을 텐서플로우 라이트 포맷(*.tflite)으로 변환 후, 안드로이드 앱에서 사용할 수 있습니다.

PC에서 학습한 텐서플로우 모델을 안드로이드 앱에서 사용하는 전체과정을 전체적으로 살펴보는 시간을 가지려고 합니다. 이번 주제에 대한 연재는 다음과 같습니다.

* 텐서플로우 모델 생성
* 텐서플로우 라이트 모델(**.tflite**)로 변환하기
* 안드로이드 앱에서 사용하기

## 본문


### 텐서플로우 모델 생성

먼저 학습 모델이 필요합니다. 음... 아무래도 좀 쉬운 모델을 예시로 드는게 좋겠죠? 딥러닝에서의 HelloWorld로 불리는, MNIST 데이터셋에 대한 학습 모델을 텐서플로우를 사용해서 생성해보도록 하겠습니다. 생성후에는 해당 모델을 저장해보도록 하겠습니다.

### 모델 저장

학습 모델을 저장하겠습니다. 텐서플로우 모델을 저장한다는 건 어떤 의미일까요? 텐서플로우 모델은 그래프로 표현됩니다. 그래프, 즉 여러개의 층으로 이루어져있고 각 층과 층에는 여러개의 노드가 존재하죠. 그리고 이러한 노드들은 다른 층의 노드들과 에지(선)으로 연결되어있습니다. 시각적으로 볼 때 그렇다는 거죠. 이러한 정보를 그래프로 나타낼 수 있고, 그러기에 이러한 그래프정보를 저장함으로써, 테서플로우 모델에 대한 정보를 저장한다고 볼 수 있는 것입니다. 또한, 학습한 당시의 각 에지의 가중치정보도 저장이 됩니다. 이러한 가중치가 저장되는 것을 텐서플로우에서는 체크포인트(checkpoint)라는 형태의 포맷으로 저장됩니다. 그래프 정보는 meta라는 형태로 저장됩니다. 

#### Frozen Graph

저장한 학습 모델을, 텐서플로우 라이트 포맷(tflite)로 바꾸어주기 위해서, 추가적인 작업이 필요합니다. 바로 freezing입니다. 텐서플로우 라이트 모델은 학습이 아닌 오로지 추론(inference)만을 위해서 사용되는 그래피이기 때문에, 그래프 정보에 가중 치 정보를 함께 상수화 시켜서 저장합니다. 즉 추론에서 사용될 딥러닝 모델은, 더이상 학습 될 필요없는 굳어진 가중치 값을 사용한다는 의미이죠. 
이렇게 그래프에 상수화된 가중치값을 포함한 형태를 얼음화된(Frozen) 그래프라고 합니다. 이를 가지고 tflite로 바꾸는 것이죠. 이러한 프리징(Freezing)작업은 [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 스크립트가 그 역할을 수행합니다. 

* Frozen Graph 생성

  * 기존에 생성해 두었던 ProtocolBuffer파일(pb)를 가지고 tensorboard에서 visualize하기

    * `python import_pb_to_tensorboard.py --model_dir model\my_train_model.pb --log_dir .\tensorboard\`

      (import_pb_to_tensorboard.py는 tensorflow github code를 통해 배포됩니다.)

  * Tensoboard에서 Output Node Name을 확인 & 기록 

    * `tensorboard --logdir=.\tensorboard\ --host localhost --port 8088`

      ![tensorboard_1](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_1.JPG?raw=true)

      ![tensorboard2](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard_2.JPG?raw=true)

      **텐서보드를 사용해서 추론(Inference)단계에서 Output Node Name이 `Mean` 임을 확인하였습니다.**


  * freeze_graph.py를 사용한 **GraphDef 객체** 파일 획득

    * `python freeze_graph.py --input_graph=model\my_train_model.pbtxt --input_checkpoint=model\my_train_model.ckpt --output_graph=model\frozen_graph.pb --output_node_names=Mean`

      ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/freezed.JPG?raw=true)


![freezing 이전의 저장된 checkpoint 파일들](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/ckptfile.JPG?raw=true)

* meta: 그래프들(GraphDef, SaverDef)에 대한 정보
* xxx-data-xxx : 가중치, 바이어스, 플레이스 홀더등에 대한 값
* xxx-index-xxx: 각 텐서와 값에 대한 테이블 정보

## 결론

 여기까지, 텐서플로우 모델을 생성하고 이를 가지고 텐서플로우 라이트 포맷으로 변경하기 직전까지의 작업과정을 살펴보았습니다. 다음시간에서는 이렇게 확보한 Frozen Graph를 사용해서, tflite형태의 포맷으로 변경하는 방법을 살펴보겠습니다.

## 참고문헌

!["생성결과"](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/savedmodel_captured.JPG?raw=true)



참고문헌

[1]:https://medium.com/@prasadpal107/saving-freezing-optimizing-for-inference-restoring-of-tensorflow-models-b4146deb21b5 "How to store, save and freeze a model"
[2]: https://eehoeskrap.tistory.com/343 "ckpt, pb 그리고 pbtxt의 차이점"
[3]: https://gusrb.tistory.com/21 "ckpt를 pb로 변환하는 방법"