---
layout: post
title: "Tensorflow Lite 모델 만들기(3)"
date: 2019-08-09
lastmod : 2019-08-09 16:00:00
sitemap :
	changefreq : hourly
	priority : 1.0
---

안녕하세요. 코딩벅스입니다. 

<br>

~~(1) 텐서플로우에서 학습한 모델 저장하기~~

~~(2) 텐서플로우 라이트 모델로 변환하기~~

**(3) 안드로이드 앱에 적용하기**

<br>

이번 포스트에서는 앞서 변환을 통해 얻은 **텐서플로우 라이트 모델**을 가지고 앱에서 사용하는 과정을

다루어 보겠습니다. 

<br>

우선 준비물을 살펴보겠습니다. 

* 안드로이드 스튜디오 설치
* 텐서플로우 라이트 모델(.tflite)
* [텐서플로우 라이트 예제 앱](https://github.com/tensorflow/examples)

<br>

<br>

_안드로이드 설치 부터 세세하게 적기에는 내용도 많아지고, 깊어지기에, 앱 동작에 필요한 환경설정들은 이미 정상적으로 마쳤다라고 전제합니다._



### 텐서플로우 라이트 예제 앱 실행

우선 [참고자료](https://github.com/tensorflow/examples/blob/master/lite/examples/image_classification/android/README.md)을 읽고 이미 완성된 예제 샘플 앱을 다운로드 합니다. 





<br>

### 정답은 No

<br>

<br>

왜일까요?  텐서플로우 라이트는 경량화된 프레임워크입니다. 

그렇기에 텐서플로우의 모든 오퍼레이션을 지원하지는 않고, 

스마트 폰이나 태플릿과 같은 비교적 저 사양 장치에 맞게 제약을 둔 것이죠. 

<br>

<br>

### 변환하기 

본격적으로 변환하는 방법을 배워보도록 하겠습니다. 

우선 **컨버터**에서 변환을 지원하는 대상 모델은 두가지 입니다. 

* SavedModel
* GraphDef

<br>

<br>

### Saved Model이 변환 대상인 경우

$ tflite_convert  --output_file=my_tflite_model.tflite  --saved_model_dir=saved_model_ouput

<br>

이렇게 하고 에러가 없으면 .tflite 가 생성됩니다. 

<br>

<br>

### Trouble Shooting (on going...)

* not supported operators ~~~
* etc