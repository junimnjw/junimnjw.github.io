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



이전 포스트에 이어,  **확보한 모델을 텐서플로우 라이트 모델로 변환하기**에 대해서 다루겠습니다. 

이번 연재의 핵심이 되는 포스트입니다. 시작하겠습니다!



텐서플로우 라이트 모델(.tflite)은 변환과정에서 소스(Source)가 되는 기존 텐서플로우 모델에서 추론에만 사용되는 정보만 쏙쏙 빼내어 최대한 경량화된 형태의 모델로 만들어 냅니다. 본 포스트는 텐서플로우 모델이 확보되었다는 전제에서 출발합니다. 



변환 전에 염두해 둘 사항이 있습니다. 

모든 텐서플로우 모델이 변환 가능할까?

### 정답은 No

왜요? 

텐서플로우 라이트는 경량화된 프레임워크입니다. 

이때문에 모든 텐서플로우 모델의 오퍼레이션을 지원하지 않습니다. 







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

    ![텐서보드 캡쳐 화면](https://t1.daumcdn.net/cfile/tistory/232187485816EB1F31)
  
    
  
* 변환하기
  
  **tflite_convert**를 사용해서 **.pb** 파일을 **.tflite**로 변환합니다.  
  *$* tflite_convert \  
  --saved_model_dir=(기존 saved model 명) \  
  --output_file=(원하는 ouput 모델명) \  
  --input_arrays="myInput"  
  --output_arrays="myOutput"