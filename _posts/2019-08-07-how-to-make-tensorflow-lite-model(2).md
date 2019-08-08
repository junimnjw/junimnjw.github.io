---
layout: post
title: "Tensorflow Lite 모델 만들기(2)"
date: 2019-08-08
lastmod : 2019-08-08 14:20:00
sitemap :
changefreq : daily
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 



~~(1) 텐서플로우에서 학습한 모델 저장하기~~

**(2) 확보한 모델을 텐서플로우 라이트 모델로 변환하기**

(3) 변환된 모델을 안드로이드 앱에서 로드하기



이전 포스트에 이어서,  **확보한 모델을 텐서플로우 라이트 모델로 변환하기**에 대해서 다루겠습니다. 

이번 연재의 가장 핵심이 되는 포스트입니다. 



텐서플로우 라이트 모델(.tflite)은 고정된 바이너리입니다. 

기존 텐서플로우에서 학습한 모델에서 추론에만 사용되는 

정보만을 쏙쏙 빼내어 최대한 경량화된 형태의 모델 타입으로 만들어 냅니다. 



텐서플로우에서 기존 학습 모델을 저장할 때 다음과 같은 두가지 타입 형태로 저장해야합니다. 

바로 **SavedModel**과 **GraphDef**입니다. 



[SavedModel 빌드방법](https://medium.com/@jsflo.dev/saving-and-loading-a-tensorflow-model-using-the-savedmodel-api-17645576527)

[GraphDef 빌드방법]()



각자의 모델 타입을 빌드하는 방법은 [텐서플로우 공식사이트][https://www.tensorflow.org/guide/saved_model#using_savedmodel_with_estimators]를 참고해주세요. 

본 포스트는 위 두가지 모델 중 하나가 이미 확보가 되었다는 전제에서 시작하려고 합니다. 



변환 과정은 크게 다음으로 구분됩니다. 

* **SavedModel**를 사용하는 경우

* **GraphDef**를 사용하는 경우



### Saved Model을 사용하는 경우

* 생성하기
* 변환하기

### GraphDef를 사용하는 경우 

* 생성하기
  
확보한 GraphDef 모델(**.pb**)을 텐서보드(Tensorboard)로 시각화하고, 
  
여기서 **Input Node**와 **Output Node** 명을 정확히 기록해 놓습니다. 
  

  * GraphDef 구하기

  * 텐서보드를 통한 GraphDef 시각화

    *$* tensorboard --logdir=./pb_logs/ --host localhost --port 8088

    ![텐서보드 캡쳐 화면](../assets/tensorboard-capture.jpg)
  
    
  
* 변환하기
  
  **tflite_convert**를 사용해서 **.pb** 파일을 **.tflite**로 변환합니다. 
  *$* tflite_convert \
  --saved_model_dir=(기존 saved model 명) \
  --output_file=(원하는 ouput 모델명) \
  --input_arrays="myInput"
  --output_arrays="myOutput"