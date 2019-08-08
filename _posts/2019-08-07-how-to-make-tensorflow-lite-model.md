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

이미 텐서플로우로 학습한 모델을 다시 사용할 수 있도록 해주는 **경량화된 프레임워크**입니다. 



스마트 폰은 PC나 고가의 서버에 비해서, 하드웨어 성능이 열세하기때문에, 

주로 PC와 서버에서 이미 학습한 텐서플로우 모델을 텐서플로우 라이트에서 

인식 가능한 형태(*.tflite)로 변환하고 이를 추론(Inference)사용합니다. 



이번 포스트에서는 이미 학습을 통해 얻은 텐서플로우 모델을 텐서플로우 라이트 포맷(.tflite)

으로 변환하는 방법과, 이를 사용하는 과정을 정리해볼까 합니다. 아주 긴 내용이 될 것 같은데요. 



과정을 크게 세가지로 구분하고 각 구분에 대한 포스트를 정리하고자 합니다. 

(1) 텐서플로우에서 학습한 모델 저장하기

(2) 확보한 모델을 텐서플로우 라이트 모델로 변환하기 

(3) 변환된 모델을 안드로이드 앱에서 로드하기





오늘은 첫 시간으로 **텐서플로우에서 학습한 모델 저장하기**를 다루고자 합니다. 

텐서플로우 라이트 모델이 필요로 하는 정보를 얻기 위해서는 텐서플로우에서 기존 학습 모델을 저장할 때 

다음과 같은 두가지 타입 형태로 저장해야합니다. 바로 **SavedModel**과 **GraphDef**입니다. 

이 두가지 모델 타입에 따른 변환 방법이 약간 상이합니다. 



[SavedModel 빌드방법][https://medium.com/@jsflo.dev/saving-and-loading-a-tensorflow-model-using-the-savedmodel-api-17645576527]

[GraphDef 빌드방법][]



각자의 모델 타입을 빌드하는 방법은 [텐서플로우 공식사이트][https://www.tensorflow.org/guide/saved_model#using_savedmodel_with_estimators]를 참고해주세요. 

본 포스트에서는 위 두가지 모델 중 적어도 한가지 타입은 확보가 되었다는 전제에서 시작하려고 합니다. 







우선 변환 과정은 크게 다음으로 구분됩니다. 

1. 텐서플로우로 학습해서 얻은 모델 **.pb**를 텐서보드(Tensorboard)로 시각화하고, 
   **Input Node**와 **Output Node** 명을 정확히 기록해 놓습니다. 
   1. **.pb**를 얻는 방법은 tf.io.write_graph API 사용법을 참고하세요. 
   2. **.pb**를 텐서보드로 시각화하는 스크립트의 예는 아래와 같습니다. 
      (tensorboard --logdir=./pb_logs/ --host localhost --port 8088)
2. **tflite_convert**를 사용해서 **.pb** 파일을 **.tflite**로 변환합니다. 
   1. tflite_convert --output_file=(원하는 ouput 모델명)  --saved_model_dir=(기존 saved model 명)
3. **.tflite** 를 텐서플로 라이트를 사용해서 실행한다.
