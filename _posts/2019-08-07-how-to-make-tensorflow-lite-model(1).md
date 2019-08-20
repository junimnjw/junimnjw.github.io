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



 **텐서플로우 라이트**, 텐서플로우의 경량화 버전입니다. 서버나 PC와 같은 고성능 장치에서 학습하고, 여기서 생성한 모델을 텐서플로우 라이트 포맷(*.tflite)으로 변환하고, 이 변환된 모델은 텐서플로우 라이트를 통해서 안드로이드 앱에서 사용할 수 있습니다. 이는 클라우드 서비스가 아닌, 온디바이스(즉 모바일장치내)에서 실행되는 딥러닝 구현입니다. 백문이 불여일행!!! 직접, 내가 학습한 텐서플로우 모델을 안드로이드 앱에서 실행해보는 과정을 나누려고 합니다. 내용이 긴 만큼, 다음과 같이 나누어 연재하겠습니다.  

* 텐서플로우 모델 생성
* 텐서플로우 라이트 모델(**.tflite**)로 변환하기
* 안드로이드 앱에서 사용하기

## 본문



#### 텐서플로우 모델 생성

 먼저 학습 모델이 필요합니다. 음...대상이 필요하니.. 당연한 말이죠. 근데 어떤 모델을 만들어볼까요? AlexNet, GoogleNet? 이미 만들어진 아주 유명한 딥러닝 모델도 있지만.. 아무래도 직접 만들어보는게 좋으니.. 쉬운 모델을 예시로 드는게 좋겠죠? 딥러닝에서의 HelloWorld라 불리는, MNIST 데이터에 대한 Fully Connected Layers학습 모델을 텐서플로우로 생성해보도록 하겠습니다. 생성 후, 해당 모델을 저장해보도록 하겠습니다.

##### 모델 저장

 이제 위에서 정의한 모델에 대한 학습 결과를 저장하겠습니다. 텐서플로우 모델을 저장한다는 건 어떤 의미일까요? 텐서플로우 모델은 **그래프(Graph)**입니다. 딥러닝모델은, 여러개의 노드들이 하나의 층(Layer)을 이루고, 그리고 수많은 선들이 이러한 층과 층을 연결하는 형태의 그래프인 것이죠.  시각적으로 볼 때 그렇다는 거죠. 그래서 텐서플로우 모델을 저장하면, 이러한 그래프정보를 저장하게 되고, 각 선(에지)들의 가중치 값을 별도로 저장하게 됩니다. 저장의 최종 포맷은 몇가지가 있지만 보통은 tf.train.saver API를 사용하면, 이러한 가중치는 체크포인트(checkpoint)라는 포맷으로 저장됩니다. 그리고 그래프 정보는 meta라는 형태로 저장됩니다. 

#####Frozen Graph로 변환

 위에서 저장한 텐서플로우 모델을, 텐서플로우 라이트 포맷(tflite)로 바꾸기 위해서, 사전 작업이 필요합니다. 바로 프리징(freezing)입니다. 텐서플로우 라이트 모델은 오로지 추론(inference)을 위해서 사용되는 그래프입니다. 추론을 위한 그래프 정보에는 학습에 사용되는 그래프의 불필요한 정보들은 버리고, 또한 가중치값도 더이상 변경이 없기때문에, 이를 상수화 시켜 저장합니다. 이렇게 그래프에 상수화된 가중치값을 포함한 형태로 굳히는 작업을 하고, 이를 가지고 tflite로 바꾸게 됩니다. 이러한 프리징(Freezing)을 돕기위해, tensorflow에서 제공하는 [freez_graph.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py) 사용하면 됩니다. 

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

 자! 여기까지, 텐서플로우 모델을 직접 생성하고, 이를 저장하고, 또 Frozen Graph를 얻는 짧지만 아주 고된 과정을 살펴보았습니다. 아주 고생많으셨습니다. 다음 연재에서는 이렇게 확보한 Frozen Graph를, 드디어 우리가 원하는, 텐서플로우 라이트에서 인식가능한 형태인 tflite로 변환해보는 시간을 가져보겠습니다. 감사합니다!~

## 참고문헌

[1]https://medium.com/@prasadpal107/saving-freezing-optimizing-for-inference-restoring-of-tensorflow-models-b4146deb21b5

[2]https://eehoeskrap.tistory.com/343

[3]https://gusrb.tistory.com/21