---
layout: post
title: "Tensorflow Serving, Who are You?"
categories: DEV
date: 2019-08-22
lastmod : 2019-09-05 14:00:00
sitemap :
changefreq : hourly
priority : 1.0
---



안녕하세요. 코딩벅스입니다.   



## 도입

  단순히 개인 연구 목적으로 텐서플로우를 사용하는 분들께는 이번 포스트에 큰 관심이 가지 않을 수도 있습니다. 왜냐하면 이번에 다루는 내용은 텐서플로우 머신러닝 모델을 어떻게 **배포**하고 **서비스**할지에 대한 부분이니까요. 내가 만든 텐서플로우 모델을 **배포하고 서비스** 한다? 멋지죠? 그런데 간단한 문제는 아닙니다. 여러 **고객(Client)**들의 요청에 대한 응답도 해야하고, 서비스 중간에 개선한 신규 버전의 모델도 **매끄럽게(Seamless)** 적용시켜야 하는 등, 여러 부분들을 고려하고 이를 구현해야하기 때문이죠. 텐서플로우를 만든 구글(Google)님께서는 이러한 텐서플로우 사용자들의 고민을 덜어주기위해 <u>머신러닝 모델을 운영환경에서 **배포하고 서비스**할 수 있도록</u> 도움을 주는 라이브러리를 제공하고, 이를 **텐서플로우 서빙(이하 TF Serving)**라고 명명하였습니다. 이번 포스트는 **TF Serving**의 구조와 동작흐름, 그리고 **실제** 사용예를 보여드리고자 합니다. 본 포스트는 [관련 블로그][1]를 간략히 요약한 것임을 알려드립니다.



## 본문

#### TF Serving 구조

구조는 크게 다음과 같이 네가지 클래스로 구성됩니다. 

* **Source**
* **Loader** 
* **DynamicManager**
* **ServableHandle**

<img src="https://cdn-media-1.freecodecamp.org/images/1*TwfOoS3M8DaUiB7ntP07_w.png" alt="img" style="zoom:80%;" />

####  동작흐름

* 우선 **Source**는 운영환경의 **FileSystem**에서 지속적 모니터링을 통해 **SavedModel Format**의 머신러닝 모델을 탐색하고, 발견시 이를 검증합니다. 그리고 대한 검증을 마치면, 해당 모델에 대한 **Loader**를 생성합니다. 

* **Loader**는 주어진 머신러닝 모델에 대한 모든 정보를 가지는 클래스입니다. 하지만 본인 스스로 모델을 로딩할 수 없고, 이러한 부분은 **DynamicManager(이하 Manager)**의 통제를 받습니다.  

* **Source**는 **Loader**를 생성한 후,  이 **Loader**에 대한 **Handle**을 **Manager**에게 전달합니다. 

* **Manager**는 전달받은 **Loader**의 **Handle**을 가지고 **서빙 프로세스**를 시작합니다. 이후 상황에따라 두가지 시나리오가 발생할 수 있습니다. 먼저, 최초로 머신러닝 모델이 로드되는 상황입니다. 이때는 **Manager**는 **Loader**를 통해 머신러닝 모델에 요구되는 리소스와 현 운영환경의 가용리소스를 확인하여, 문제없으면 **Loader**가 모델을 로드하도록 권한을 줍니다. 문제는 다음 시나리오입니다. 바로 기존 모델이 서비스 되는 상황에서, 신규 버전의 모델에 대한 로드가 이루어져야하는 상황이죠. 이미 기존 머신러닝 모델이 서비스 되는 상황에서,  **Source**는 상위 버전 모델을 발견하고, 역시 생성한 **Loader**의 **Handle**을 Manager에게 전달하죠. 이때 **Manager**가 이후 행동을 결정하는데 두가지 기준을 두고있습니다. 첫번째는 **이용성(Availability)**이고, 두번째는 **리소스(Resource)**입니다. **이용도**를 중요시한다면, 상위 버전 모델에 대한 로드가 완전히 이루어진 상태에서만, 이전 버전 모델을 해제하죠. 즉, 두가지 모델이 로드된 상태가 발생하고, 이 때문에 서비스 흐름이 끊기지 않고 매끄럽게(Seamless)게 이어지죠. 하지만 추가적인 리소스가 필요하다는 단점이 있습니다. 만약 **리소스**를 중요한 부분으로 고려한다면, 기존 모델의 해제가 완전히 이루어진 이후, 새로운 모델을 로드함을 통해 리소스 부족에 대한 걱정을 덜 수 있도록 해줍니다. 머신러닝 모델의 사이즈가 큰 경우 서비스의 매끄러운 면에서 약간 손해를 보더라도 전체적인 운영 관점에서는 좋은 대안이 될 수 있습니다. 

* **Manager**는 **Loader**에게 딥러닝 모델을 로드할 수 있도록 허락한 이후, 해당 **Loader**에 대한 **Handle**을 **Serverable**에게 전달합니다. 



#### 도전!

그럼 지금부터 실제로 **TF Serving**을 사용해보죠! 



1. **SavedModel 생성**  

   당연히 배포 할 머신러닝 모델이 필요하겠죠? 제일 만만한 **Tensorflow**의 **HelloWorld**인 ***MNIST 모델***로 하겠습니다.  **TF Serving**에서 모델을 인식하게 하기위해서는 해당 모델을 **SavedModel**포맷으로 저장해야합니다. **SavedModel**에 대한 자세한 내용은 [이전 포스트][https://junimnjw.github.io/dev/2019/08/14/what-is-saved-model.html]를 참고해주세요. 실습 속도를 내기위해 MNIST에 데이터 셋에 대한 머신러닝 모델 생성, 그리고 **SavedModel Format**으로 저장하는 내용의 코드는 [깃헙](https://github.com/junimnjw/codes/blob/master/python/mnist_tflite_project/export_mnist_to_savedmodel.py)에 올려두었으니 참고해주세요. 

   

2. **TF Serving 구동**

   이제 저장한 모델을 가지고 운영환경에서 **TF Serving**을 구동하는데는 아래 명령어가 전부입니다. 

   ```
   $ tensorflow_model_server --port=9000 --model_name=deeplab --model_base_path=<full/path/to/serving/versions/>
   ```

   model_base_path에 이전에 생성해두었던 **SavedModel**의 위치만 입력해주는게 끝입니다. 어떤 버전을 어떻게 로드하는지 어떻게 아는건가요? 걱정마세요. **TF Serving**이 해당 Path내에 존재하는 모델들의 버전 정보를 보고 제일 상위버전을 알아서 로드해줍니다. 

   

3. **클라이언트 요청부**

   서버에 수기로 작성한 숫자 이미지 하나(물론 MNIST에서 제공하는 TEST이미지)를 보내고, 이것에 대한 결과를 받는 요청을 작성하겠습니다. 

   

## 결론

여기까지, 텐서플로우 서빙이 무엇인지 간략히 알아보았습니다. 딥러닝 모델에 대한 실제 양산화를 생각하고 계신 분들은 틈틈이 배워 놓는 것도 도움이 될 것입니다. 감사합니다. 



## 참고문헌

[1]:https://www.freecodecamp.org/news/how-to-deploy-tensorflow-models-to-production-using-tf-serving-4b4b78d41700/	"How to deploy a Tensorlofw Model"

