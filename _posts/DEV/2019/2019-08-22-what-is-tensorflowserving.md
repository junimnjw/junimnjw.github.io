---
layout: post
title: "Tensorflow Serving, Who are You?"
categories: DEV
date: 2019-08-22
lastmod : 2019-08-22 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---



안녕하세요. 코딩벅스입니다.   



## 도입

 단순히 개인적인 연구 목적으로 텐서플로우를 사용하는 분들께는 이번 포스트가 큰 관심 거리가 되지 않을 수 있습니다. 왜냐하면 <u>텐서플로우 모델을 **배포**하고 타인에게 **서비스**하는 방법에 대한 내용</u>이니까요. 꽤 긴 시간에 걸쳐학습을 통해 얻은 텐서플로우 모델을, 배포하고 또 이를 서비스(또는 서빙) 한다는 건, 단순한 문제가 아닙니다. 여러 **고객(Client)**들이 요청하는 건들에 대한 **스케쥴링**, 그리고 업데이트된 모델을 **매끄럽게(Seamless) 교체**하는 일 등, 원활한 서비스 제공에는 다양한 컴포넌트들이 유기적으로 동작해야 가능한 것이죠. 구글(Google)은 이러한 텐서플로우 모델에 대한 제품환경에서의 **서비스(서빙)**이 보다 쉽게 가능하도록 하는 프레임워크를 제공하고있고, 이를 **텐서플로우 서빙(Tensorflow Serving)**이라고 합니다. 이번 포스트는 [관련 블로그][1]를 간략하게 요약한 것임을 미리 알려드립니다.



## 본문

#### Tensorflow Serving LifeCycle

텐서플로우 서빙의 구조는 직관적으로 볼 때, 다음과 같이 크게 네가지 컴포넌트로 구성됩니다. 

* Loader
* Source
* Manager
* ServableHandle

<img src="https://cdn-media-1.freecodecamp.org/images/1*TwfOoS3M8DaUiB7ntP07_w.png" alt="img" style="zoom:80%;" />

**Source Class**는 딥러닝 모델 파일을 읽어 들이고, 올바른 형태인지 검증하는 컴포넌트입니다. 올바른 포맷으로 저장된 모델임을 확인한 후, Loader 객체를 생성하고 Loader에게 해당 딥러닝 모델을 넘깁니다. 

**Loader Class**는 딥러닝 모델에 대한 정보를 가지고 있는 객체입니다. 필요한 리소스(모델 사이즈, 필요한 메모리 등)에 대한 정보는 물론이고, 모델에 대한 메타 그래프와 가중치를 포함한 전반적인 정보를 가지게 됩니다. 다만, 이러한 정보를 가지게 되는 시점은 Loader가 해당 모델을 로드하고 난 이후인데, Loader는 스스로 주어진 딥러닝 모델을 로드하는 것이 아니고, Manager의 통제에 의해서만 가능합니다.

**Manager Class**는 프로세스입니다. 하나의 모델에 대해서 프로세스를 할당하고, Loader가 주어진 딥러닝 모델을 로드할 수 있도록 허락합니다.  



#### Tensorflow Serving & SavedModel

텐서플로우 모델을  ***Serve*** 하기 위해서는 **SavedModel**로 저장해야합니다. 

**SavedModel**은 이전 포스트에서 자세히 다루었으니 참고해주세요. 

## 결론

여기까지, 텐서플로우 서빙이 무엇인지 간략히 알아보았습니다. 딥러닝 모델에 대한 실제 양산화를 생각하고 계신 분들은 틈틈이 배워 놓는 것도 도움이 될 것입니다. 감사합니다. 

## 참고문헌

[how to deploy tensorflow models to production using tensorflow serving](https://www.freecodecamp.org/news/how-to-deploy-tensorflow-models-to-production-using-tf-serving-4b4b78d41700/) 