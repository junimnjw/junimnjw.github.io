---
layout: post
title: "텐서보드를 사용한 학습 모델 구조 파악하기"
categories: 개발
date: 2019-08-13
lastmod : 2019-08-13 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.   



### 배경설명

 **텐서보드**. 아주 유용한 툴이죠...라고 합니다.. 저도 많이 써보지 않아서 이렇다 저렇다 할 레벨은 아닙니다. 화이트보드도 아니고 텐서보드, 그냥 칠판같은 역할을 하는 정도로 알고있죠. 칠판에 텐서플로우 모델을 그래프처럼 그려서 보여주는 그런 역할을 하는 툴입니다. 



 이번 포스트에서는 **텐서보드**로 할 수 있는 일 가운데 **이미 학습된 모델**을 시각화(Visualization)하는 방법에 대해서 알아보려고 합니다. 시각화를 통해서, `출력층`과 `입력층`의 노드 정보를 파악하겠습니다. 왜? 안드로이드 앱에서 사용하려면, 해당 모델을 텐서플로우 라이트 포맷(.tflite)로 변경해야하는데, 이때 `출력층`과 `입력층`정보가 필요하기때문이죠!!!!!



### 준비물 

* 텐서보드 - 텐서보드를 실행하려면 당연히 텐서보드가 설치되어있어야겠죠?
  * 설치법
  * pip install tensorboard 
* [import_pb_to_tensorboard.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/import_pb_to_tensorboard.py)  
* 텐서보드 대상 모델 (.pb 파일)



### 사용법

1. 대상 텐서플로우 모델을 텐서보드로 가져오기 

   `(my-env)$ python import_pb_to_tensorboard.py --model_dir my_train_model.pb \		`

   `--log_dir \tensorboard\`

   * 결과화면 

     ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorflow_logdir.JPG?raw=true)

     

2. 텐서보드 실행

   `(my-env)$ tensorboard --logdir=/tensorboard/`

   출력결과 : *`TensorBoard 1.14.0 at http://DO-JJW-NAM03:6006/ (Press CTRL+C to quit)`* 

   * 크롬 또는 인터넷 익스플로러에서 http://DO-JJW-NAM03:6006/ 또는 http://localhost:6066으로 접속

   * *`경우에 따라 접속이 안되는 경우 tensorboard --logdir=\tensorboard\ --host localhost --port 8088 로 실행합니다.`*

   * 결과화면

     ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tensorboard-full-captured.JPG?raw=true){:width="50%" height="50%"}

   













[1]: https://medium.com/@daj/how-to-inspect-a-pre-trained-tensorflow-model-5fd2ee79ced0 "How to inspect a pre-trained TensorFlow model"

