---
layout: post
title: "Tensorflow Lite 제대로 이해하기(3)"
categories: 개발
date: 2019-08-09
lastmod : 2020-01-06 16:00:00
sitemap :
changefreq : daily
priority : 1.0
---

안녕하세요. 코딩벅스입니다. 



## 들어가며

이번 포스트에서는 앞서 변환을 통해 얻은 **텐서플로우 라이트 모델**을 가지고 앱에서 사용하는 과정을

다루어 보겠습니다. 



우선 준비물을 살펴보겠습니다. 

* 안드로이드 스튜디오 설치
* 텐서플로우 라이트 모델(.tflite)
* [텐서플로우 라이트 예제 앱](https://github.com/tensorflow/examples)



## 본문



_안드로이드 설치 부터 세세하게 적기에는 내용도 많아지고, 깊어지기에, 앱 동작에 필요한 환경설정들은 이미 정상적으로 마쳤다라고 전제합니다._



### 텐서플로우 라이트 예제 앱 실행

우선 [참고자료](https://github.com/tensorflow/examples/blob/master/lite/examples/image_classification/android/README.md)을 읽고 이미 완성된 예제 샘플 앱을 다운로드 합니다. 


