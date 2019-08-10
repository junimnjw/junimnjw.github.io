---
layout: post
title: "Tensorflow Lite 모델 만들기(2)"
date: 2019-08-07
lastmod : 2019-08-08 09:16:00
sitemap :
changefreq : daily
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 

<br>

~~(1) 텐서플로우에서 학습한 모델 저장하기~~

**(2) 텐서플로우 라이트 모델로 변환하기**

<br>

이전 포스트에 이어, 이번 연재의 중심이 되는 부분입니다. 

<br>

우선, 기존 텐서플로우 모델을 텐서플로우 라이트 포맷으로 변환하는 과정에 필요한 것이 

있는데 바로 **Tensorflow Lite Converter(이하 컨버터)**입니다. 

변환 과정에서 기존 텐서플로우 모델에서 추론에 사용되는 정보만 쏙쏙 빼내어 경량화된

형태로 만들어 냅니다. 이러한 **컨버터**는 파이썬 패키지를 설치할 때 기본으로 포함됩니다. 

<br>

<br>

변환에 대한 예시를 들기 전에 질문할 사항이 있습니다. 

모든 텐서플로우 모델이  그럼 변환 가능할까?

<br>

### 정답은 No

<br>

<br>

왜일까요?  텐서플로우 라이트는 경량화 프레임워크입니다. 

즉, 표준 텐서플로우의 모든 오퍼레이션을 지원하지는 않고, 

스마트 폰이나 태플릿과 같은 저 사양 장치에 맞게 제약을 둔 것이죠. 

<br>

<br>

### 변환하기 

그럼 본격적으로 변환하는 방법을 배워보도록 하겠습니다. 

우선 **컨버터**에서 변환을 지원하는 모델의 포맷은 두가지 입니다. 

텐서플로우 모델을 저장할 때 아래와 같은 포맷으로 지정해 줍니다.

* SavedModel
* GraphDef

<br><br>

### Saved Model에 대한 변환

$ tflite_convert  --output_file=my_tflite_model.tflite  --saved_model_dir=saved_model_ouput

<br><br><br>

### Trouble Shooting (on going...)

* not supported operators ~~~
* etc
