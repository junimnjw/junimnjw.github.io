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

이전 포스트에 이어,  **(2) 텐서플로우 라이트 모델로 변환하기**를 다루겠습니다. 

어떻게 보면 이번 연재의 핵심이 되는 부분입니다. 

<br>

변환하는 과정에 꼭 필요한 것이 바로 **Tensorflow Lite Converter(이하 컨버터)**입니다. 

컨버터는 기존 텐서플로우에서 학습한 모델을 텐서플로우 라이트에서 인식 가능한 포맷(.tflite)로

변환해 주는 툴 입니다. 변환 과정에서 기존 텐서플로우 모델에서 추론에 사용되는 정보만 쏙쏙 빼내어 경량화된

형태로 만들어 냅니다. **컨버터(tflite_convert)**는 참고로 파이썬 패키지를 설치할 때 기본으로 포함됩니다. 

<br>

<br>

변환 전에 염두해 둘 사항이 있습니다. 

모든 텐서플로우 모델이  그럼 변환 가능할까?

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
