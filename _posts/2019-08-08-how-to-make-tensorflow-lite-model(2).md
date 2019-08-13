---
layout: post
title: "텐서플로우 라이트 모델 생성(2)"
date: 2019-08-09
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.

 이번 연재의 중심이 되는 부분입니다. 앞선 얻은 텐서플로우 모델(**SavedModel 또는 GraphDef**)을 텐서플로우 라이트 포맷(**.tflite**)으로 변환하는 과정을 다루어볼려고 합니다. 우선, 여기에 필요한 것이 있습니다. 바로 **Tensorflow Lite Converter(이하 컨버터)**입니다. **컨버터**는 기존 모델에서 **추론**에 사용되는 정보만 쏙쏙 빼내어 압축된 형태로 만들어 냅니다. 이러한 **컨버터(tflite_convert)**는 파이썬 패키지를 설치할 때 기본으로 포함됩니다.

 변환에 대한 예를 들기 전에 의문이 드는 부분이 있습니다. 과연, 모든 텐서플로우 모델이  변환이 가능할까요? **정답은.. 가능은 하다.. 다만 노력이 들어갈뿐** 

 왜 별도의 노력이 들어갈까요?  텐서플로우 라이트는 경량화 딥러닝 프레임워크입니다. 즉, PC에서 구동하는 기존 텐서플로우의 모든 오퍼레이션들을 포함하지 않고있고, 스마트 폰이나 태플릿과 같은 저 사양 장치에 맞게 제약을 두었습니다. 그래서 지원하지 않고있는 오퍼레이터들에 대해서는 사용자가 직접 정의를 해주어야합니다. 이러한 사용자 정의 오퍼레이션을 사용할 때 컨버터를 통한 변환과정에서 **--allow_custom_ops** 옵션을 활성하해야합니다. 여기에 대한 내용은 별도로 추후 공유하겠습니다. 약속드립니다!



### 변환하기 

 그럼 본격적으로 변환하는 방법을 배워보도록 하겠습니다. 앞선 연재에서 확보한 텐서플로우 모델 파일에 대한 변환 과정을 포맷에 따라 구분해서 언급하도록 하겠습니다 .



#### GraphDef

* 변환

  * 대상 모델 파일(GraphDef) 경로 확인 

  * 터미널에서 다음과 같이 입력 

    `tflite_convert --output_file=.\model\my_model.tflite --graph_def_file=.\model\frozen_graph.pb --input_arrays=Placeholder --output_arrays=Mean --allow_custom_ops`

    ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tflite_outputfile.JPG?raw=true)

    

#### SavedModel

* 변환

  * 터미널에서 다음 명령어를 입력합니다. 

    tflite_convert  --output_file=my_tflite_model.tflite  --saved_model_dir=saved_model_ouput



### Trouble Shooting (on going...)

* not supported operators ~~~
* etc
