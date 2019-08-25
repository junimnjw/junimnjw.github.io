---
layout: post
title: "텐서플로우 라이트 모델 생성(2)"
categories: ML
date: 2019-08-09
lastmod : 2019-08-11 09:16:00
sitemap :
changefreq : hourly
priority : 1.0
---

안녕하세요. 코딩벅스입니다.



## 배경설명



 앞서 얻은 Frozen Graph를 텐서플로우 라이트 포맷(**.tflite**)으로 변환하는 과정을 다루는 차례입니다. 여기에 필요한 것이 있습니다. 바로 **TOCO(Tensorflow Lite Optimized Converter)**입니다. **TOCO 컨버터(tflite_convert)**는 파이썬 설치시 기본 포함됩니다. 



## 본문



 잠깐! 과연, 모든 텐서플로우 모델이 변환이 가능할까요? 가능은 합니다. 다만 경우에 따라 추가적인 노력이 발생할 수 있습니다. 경우에 따라 어떤 노력이 발생할까요? 텐서플로우 라이트는 아시다시피 경량화 프레임워크입니다. 그래서 표준 텐서플로우의 모든 사양들을 지원하지 않습니다. 물론 아직도 버전업이 열심히 이루어지고 있지만, 아직은 필수적인 소수의 기능들ㄴ 지원되고 있죠. 이들 가운데에서도, 변환시킬 대상 모델에서 사용된 오퍼레이션들(Operations)에 대해서 텐서플로우 라이트는 일부만 지원하고, 이들을 제외하고는 사용시, 사용자가 직접 정의 해주어야합니다. 여기에 대한 방법은 추후 별도 공유하겠습니다. 

 그럼 본격적으로 변환하는 방법을 배워보도록 하겠습니다. 확보한 텐서플로우 모델 파일에 대한 변환 과정을 포맷에 따라 구분해서 언급하도록 하겠습니다 .

* 변환

  * 대상 모델의 Frozen GraphDef (.pb) 파일 위치 확인 

  * 터미널에서 다음 입력 

    `$ tflite_convert --output_file=.\model\my_model.tflite --graph_def_file=.\model\frozen_graph.pb --input_arrays=Placeholder --output_arrays=Mean --allow_custom_ops`

  * 생성결과

    ![결과](https://github.com/junimnjw/junimnjw.github.io/blob/master/assets/img/tflite_outputfile.JPG?raw=true)
  



## 결론

 여기까지 잘 실행되셨나요? 변환과정에 문제는 없으셨나요? 앞서, 텐서플로우 라이트는 텐서플로우 오퍼레이션들 가운데 일부만 지원한다고 하였습니다. 저희가 예제로 사용한 MNIST 학습 모델에서 loss function을 정의하기 위해 사용한 함수 ` softmax_cross_entropy_logit`는 텐서플로우 라이트에서 지원되지 않는 오퍼레이션인데, 왜 변환과정에서 아무런 에러메시지가 없을 까요? 궁금하지 않으셨나요?답은... Frozen Graph를 생성할 때, 제외하였기 때문입니다. Frozen Graph생성시, Output Node를 선택할 때, `softmax_cross_entropy_logit`를 이용한 노드를 선택하지 않고, tf.nn.softmax를 사용한 별도의 노드를 선택하였기 때문입니다. softmax_cross_entropy_logit는 학습의 효율성 때문에 학습 모델에서만 사용하고, 추론에서는 tf.nn.softmax를 사용하도록 하였습니다. 이처럼 학습용과 추론용 그래프를 구분함을 통해서, 텐서플로우 라이트에서 미지원하는 오퍼레이션에 대한 어느정도의 대처가 가능합니다. 

## 참고문헌

